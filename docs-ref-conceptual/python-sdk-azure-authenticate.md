---
title: "S’authentifier avec les bibliothèques de gestion Azure pour Python"
description: "S’authentifier avec un principal de service dans les bibliothèques de gestion Azure pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, authentification, active directory, principal de service"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 5c4cf1dee7d9864e809f2797ad49ce78886a6f66
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2017
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="38df1-104">S’authentifier avec les bibliothèques de gestion Azure pour Python</span><span class="sxs-lookup"><span data-stu-id="38df1-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="38df1-105">Vous pouvez authentifier votre application avec Azure de plusieurs façons, lorsque vous utilisez les bibliothèques de gestion Python pour créer et gérer des ressources.</span><span class="sxs-lookup"><span data-stu-id="38df1-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="38df1-106">S’authentifier à l’aide d’informations d’identification de jeton</span><span class="sxs-lookup"><span data-stu-id="38df1-106">Authenticate with token credentials</span></span>

<span data-ttu-id="38df1-107">Stockez les informations d’identification en sécurité dans un fichier de configuration, dans le registre ou dans Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="38df1-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="38df1-108">L’exemple suivant utilise un [principal de service](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="38df1-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="38df1-109">Vous pouvez créer un principal de service à l’aide d’Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="38df1-109">You can create a Service Principal via the Azure CLI 2.0</span></span>
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

> <span data-ttu-id="38df1-110">REMARQUE : pour vous connecter à un des clouds souverains Azure, utilisez le paramètre `cloud_environment`.</span><span class="sxs-lookup"><span data-stu-id="38df1-110">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>

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

<span data-ttu-id="38df1-111">Si vous avez besoin de plus de contrôle, nous vous recommandons d’utiliser [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) et le Kit de développement logiciel (SDK) ADAL wrapper.</span><span class="sxs-lookup"><span data-stu-id="38df1-111">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="38df1-112">Reportez-vous au site Web de la bibliothèque ADAL pour consulter la liste des scénarios et des exemples disponibles.</span><span class="sxs-lookup"><span data-stu-id="38df1-112">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="38df1-113">Pour l’authentification du principal du service, par exemple :</span><span class="sxs-lookup"><span data-stu-id="38df1-113">For instance for service principal authentication:</span></span>

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

<span data-ttu-id="38df1-114">Tous les appels valides ADAL peuvent être utilisés avec la classe `AdalAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="38df1-114">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="38df1-115">Créez ensuite un objet de client pour commencer à utiliser l’API :</span><span class="sxs-lookup"><span data-stu-id="38df1-115">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="38df1-116">REMARQUE : lors de l’utilisation d’un cloud souverain Azure, vous devez également spécifier l’URL de base appropriée (via les constantes dans `msrestazure.azure_cloud`) lorsque vous créez le client de gestion.</span><span class="sxs-lookup"><span data-stu-id="38df1-116">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="38df1-117">Par exemple, pour le cloud Azure Chine :</span><span class="sxs-lookup"><span data-stu-id="38df1-117">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="38df1-118">Authentification basée sur un fichier</span><span class="sxs-lookup"><span data-stu-id="38df1-118">File based authentication</span></span>

<span data-ttu-id="38df1-119">La méthode d’authentification la plus simple consiste à créer un fichier JSON qui contient les informations d’identification d’un principal de service Azure.</span><span class="sxs-lookup"><span data-stu-id="38df1-119">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="38df1-120">Vous pouvez utiliser la commande CLI suivante pour créer un principal de service et ce fichier en même temps :</span><span class="sxs-lookup"><span data-stu-id="38df1-120">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="38df1-121">Enregistrez ce fichier dans un emplacement sécurisé sur votre système que votre code peut lire.</span><span class="sxs-lookup"><span data-stu-id="38df1-121">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="38df1-122">Définissez une variable d’environnement avec le chemin complet du fichier dans l’interpréteur de commande :</span><span class="sxs-lookup"><span data-stu-id="38df1-122">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="38df1-123">Si vous souhaitez créer le fichier vous-même, veuillez suivre ce format :</span><span class="sxs-lookup"><span data-stu-id="38df1-123">If you want to create the file yourself, please follow this format:</span></span>

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

<span data-ttu-id="38df1-124">Vous pouvez ensuite créer n’importe quel client à l’aide de la fabrique de clients :</span><span class="sxs-lookup"><span data-stu-id="38df1-124">You can then create any client using the client factory:</span></span>
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="38df1-125">Authentifier avec Managed Service Identity (MSI)</span><span class="sxs-lookup"><span data-stu-id="38df1-125">Authenticate with Managed Service Identity(MSI)</span></span> 
<span data-ttu-id="38df1-126">MSI est un moyen simple pour une ressource sous Azure d’utiliser le Kit de développement logiciel (SDK)/l’interface de ligne de commande sans avoir besoin de créer d’informations d’identification spécifiques.</span><span class="sxs-lookup"><span data-stu-id="38df1-126">MSI is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

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

## <a name="mgmt-auth-cli"></a><span data-ttu-id="38df1-127">Authentification basée sur l’interface CLI</span><span class="sxs-lookup"><span data-stu-id="38df1-127">CLI-based authentication</span></span>

<span data-ttu-id="38df1-128">Le kit de développement logiciel (SDK) est en mesure de créer un client à l’aide de l’abonnement de votre CLI.</span><span class="sxs-lookup"><span data-stu-id="38df1-128">The SDK is able to create a client using your CLI active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="38df1-129">Cela doit être utilisé comme guide de démarrage rapide pour une expérience de développement.</span><span class="sxs-lookup"><span data-stu-id="38df1-129">This should be used as quick start developer experience.</span></span> <span data-ttu-id="38df1-130">À des fins de production, utilisez [ADAL](#authenticate-with-token-credentials) ou votre propre système d’informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="38df1-130">For production purposes, use [ADAL](#authenticate-with-token-credentials) or your own credentials system.</span></span>
> <span data-ttu-id="38df1-131">Toute modification apportée à votre configuration CLI impactera l’exécution du kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="38df1-131">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="38df1-132">Pour définir des informations d’identification actives, utilisez la commande [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="38df1-132">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="38df1-133">L’ID d’abonnement par défaut est le seul dont vous disposez ou celui que vous définissez via la commande [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="38df1-133">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="38df1-134">S’authentifier à l’aide d’informations d’identification de jeton (anciennes)</span><span class="sxs-lookup"><span data-stu-id="38df1-134">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="38df1-135">Dans la version précédente du kit de développement logiciel (SDK), la bibliothèque ADAL n’était pas encore disponible, et nous fournissions une classe `UserPassCredentials`.</span><span class="sxs-lookup"><span data-stu-id="38df1-135">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="38df1-136">Elle est déconseillée et ne doit plus être utilisée.</span><span class="sxs-lookup"><span data-stu-id="38df1-136">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="38df1-137">Cet exemple montre un scénario utilisateur/mot de passe.</span><span class="sxs-lookup"><span data-stu-id="38df1-137">This sample shows user/password scenario.</span></span> <span data-ttu-id="38df1-138">Il ne prend pas en charge l’authentification à deux facteurs (2FA).</span><span class="sxs-lookup"><span data-stu-id="38df1-138">This does not support 2FA.</span></span>

```python
    from azure.common.credentials import UserPassCredentials

    credentials = UserPassCredentials(
        'user@domain.com',
        'my_smart_password',
    )
```
