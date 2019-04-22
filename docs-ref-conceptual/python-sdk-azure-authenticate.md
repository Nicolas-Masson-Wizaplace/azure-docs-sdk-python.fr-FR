---
title: S’authentifier avec les bibliothèques de gestion Azure pour Python
description: S’authentifier avec un principal de service dans les bibliothèques de gestion Azure pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, authentification, active directory, principal de service
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/11/2019
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 51f26b120cefffd2d7f4af9c2b6b2cb532bc6006
ms.sourcegitcommit: 375a1f9180eb1323fe2af0a7e28fd4676973c68e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59586807"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>S’authentifier avec les bibliothèques de gestion Azure pour Python

Vous pouvez authentifier votre application avec Azure de plusieurs façons, lorsque vous utilisez les bibliothèques de gestion Python pour créer et gérer des ressources.

## <a name="mgmt-auth-token"></a>S’authentifier à l’aide d’informations d’identification de jeton

Stockez les informations d’identification en sécurité dans un fichier de configuration, dans le registre ou dans Azure Key Vault.

L’exemple suivant utilise un [principal de service](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) pour l’authentification.

> [!NOTE]
> Pour créer un principal de service avec l’interface Azure CLI, utilisez la commande suivante :
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> Pour en savoir plus sur la configuration de principaux de service avec l’interface CLI, consultez [Créer un principal du service Azure avec Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli).

```python
from azure.common.credentials import ServicePrincipalCredentials

# Tenant ID for your Azure subscription
TENANT_ID = '<Your tenant ID>'

# Your service principal App ID
CLIENT = '<Your service principal ID>'

# Your service principal password
KEY = '<Your service principal password>'

credentials = ServicePrincipalCredentials(
    client_id = CLIENT,
    secret = KEY,
    tenant = TENANT_ID
)
```

> REMARQUE : pour vous connecter à un des clouds souverains Azure, utilisez le paramètre `cloud_environment`.
>
> ```python
> from azure.common.credentials import ServicePrincipalCredentials
> from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
> 
> # Tenant ID for your Azure Subscription
> TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
> 
> # Your Service Principal App ID
> CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'
> 
> # Your Service Principal Password
> KEY = 'password'
> 
> credentials = ServicePrincipalCredentials(
>     client_id = CLIENT,
>     secret = KEY,
>     tenant = TENANT_ID,
>     cloud_environment = AZURE_CHINA_CLOUD
> )
> ```

Si vous avez besoin de plus de contrôle, nous vous recommandons d’utiliser [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) et le Kit de développement logiciel (SDK) ADAL wrapper. Reportez-vous au site Web de la bibliothèque ADAL pour consulter la liste des scénarios et des exemples disponibles. Pour l’authentification du principal du service, par exemple :

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Tenant ID for your Azure Subscription
TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

# Your Service Principal App ID
CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

# Your Service Principal Password
KEY = 'password'

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    RESOURCE,
    CLIENT,
    KEY
)
```

Tous les appels valides ADAL peuvent être utilisés avec la classe `AdalAuthentication`.

Créez ensuite un objet de type `ComputeManagementClient` pour commencer à utiliser l’API :

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> REMARQUE : lors de l’utilisation d’un cloud souverain Azure, vous devez également spécifier l’URL de base appropriée (via les constantes dans `msrestazure.azure_cloud`) lorsque vous créez le client de gestion. Par exemple, pour le cloud Azure Chine :
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a>Authentification basée sur un fichier

La méthode d’authentification la plus simple consiste à créer un fichier JSON qui contient les informations d’identification d’un principal de service Azure. Vous pouvez utiliser la commande CLI suivante pour créer un principal de service et ce fichier en même temps :

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

Enregistrez ce fichier dans un emplacement sécurisé sur votre système que votre code peut lire. Définissez une variable d’environnement avec le chemin complet du fichier dans l’interpréteur de commande :

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

Si vous souhaitez créer le fichier vous-même, veuillez suivre ce format :

```json
{
    "clientId": "<Service principal ID>",
    "clientSecret": "<Service principal secret/password>",
    "subscriptionId": "<Subscription associated with the service principal>",
    "tenantId": "<The service principal's tenant>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Vous pouvez ensuite créer n’importe quel client à l’aide de la fabrique de clients :

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a>S’authentifier avec des identités managées Azure
Azure Managed Identity est un moyen simple pour une ressource dans Azure d’utiliser le kit SDK/l’interface CLI sans devoir créer d’informations d’identification spécifiques.

> [!IMPORTANT]
>
> Pour utiliser des identités managées, vous devez vous connecter à Azure à partir d’une ressource Azure, comme une fonction Azure ou une machine virtuelle exécutée dans Azure. Pour apprendre à configurer une identité managée pour une ressource, consultez [Configurer des identités managées pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) et [Guide pratique pour utiliser des identités managées pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

# Create MSI Authentication
credentials = MSIAuthentication()


# Create a Subscription Client
subscription_client = SubscriptionClient(credentials)
subscription = next(subscription_client.subscriptions.list())
subscription_id = subscription.subscription_id

# Create a Resource Management client
resource_client = ResourceManagementClient(credentials, subscription_id)


# List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
for resource_group in resource_client.resource_groups.list():
    print(resource_group.name)
```

## <a name="mgmt-auth-cli"></a>Authentification basée sur l’interface CLI

Le kit SDK peut créer un client à l’aide de l’abonnement actif de l’interface Azure CLI.

> [!IMPORTANT]
> Cela doit être utilisé comme guide de démarrage rapide pour une expérience de développement. À des fins de production, utilisez [ADAL](#mgmt-auth-legacy) ou votre propre système d’informations d’identification.
> Toute modification apportée à votre configuration CLI impactera l’exécution du kit de développement logiciel (SDK).

Pour définir des informations d’identification actives, utilisez la commande [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).
L’ID d’abonnement par défaut est le seul dont vous disposez ou celui que vous définissez via la commande [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a>S’authentifier à l’aide d’informations d’identification de jeton (anciennes)

Dans la version précédente du kit de développement logiciel (SDK), la bibliothèque ADAL n’était pas encore disponible, et nous fournissions une classe `UserPassCredentials`. Elle est déconseillée et ne doit plus être utilisée.

Cet exemple montre un scénario utilisateur/mot de passe. Il ne prend pas en charge l’authentification à deux facteurs (2FA).

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```
