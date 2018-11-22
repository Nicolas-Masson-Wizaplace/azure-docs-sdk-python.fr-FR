---
title: S’authentifier avec les bibliothèques de gestion Azure pour Python
description: S’authentifier avec un principal de service dans les bibliothèques de gestion Azure pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, authentification, active directory, principal de service
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5011d36f9258fb7c06a8b1d6a689e3b5058360bb
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52273045"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>S’authentifier avec les bibliothèques de gestion Azure pour Python

Vous pouvez authentifier votre application avec Azure de plusieurs façons, lorsque vous utilisez les bibliothèques de gestion Python pour créer et gérer des ressources.

## <a name="mgmt-auth-token"></a>S’authentifier à l’aide d’informations d’identification de jeton

Stockez les informations d’identification en sécurité dans un fichier de configuration, dans le registre ou dans Azure Key Vault.

L’exemple suivant utilise un [principal de service](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) pour l’authentification.

> [!NOTE]
> Vous pouvez créer un principal de service à l’aide d’Azure CLI 2.0
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```

```python
    from azure.common.credentials import ServicePrincipalCredentials

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID
    )
```

> REMARQUE : pour vous connecter à un des clouds souverains Azure, utilisez le paramètre `cloud_environment`.

```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID,
        cloud_environment = AZURE_CHINA_CLOUD
    )
```

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
    "clientId": "ad735158-65ca-11e7-ba4d-ecb1d756380e",
    "clientSecret": "b70bb224-65ca-11e7-810c-ecb1d756380e",
    "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
    "tenantId": "c81da1d8-65ca-11e7-b1d1-ecb1d756380e",
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

## <a name="mgmt-auth-msi"></a>Authentifier avec Managed Service Identity (MSI) 
MSI est un moyen simple pour une ressource sous Azure d’utiliser le Kit de développement logiciel (SDK)/l’interface de ligne de commande sans avoir besoin de créer d’informations d’identification spécifiques.

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

Le kit de développement logiciel (SDK) est en mesure de créer un client à l’aide de l’abonnement de votre CLI.

> [!IMPORTANT]
> Cela doit être utilisé comme guide de démarrage rapide pour une expérience de développement. À des fins de production, utilisez [ADAL](#authenticate-with-token-credentials) ou votre propre système d’informations d’identification.
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
