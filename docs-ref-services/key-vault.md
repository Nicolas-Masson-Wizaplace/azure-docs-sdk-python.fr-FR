---
title: "Bibliothèques Azure Key Vault pour Python"
description: "Consulter la documentation sur les bibliothèques de client Python pour Azure Key Vault"
author: lisawong19
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, clés, Key Vault, authentification, secret, clé, sécurité"
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 3eac46eb4d5d19273ead9f19b739f6fb6d72e5cc
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="a52ec-104">Bibliothèques Azure Key Vault pour Python</span><span class="sxs-lookup"><span data-stu-id="a52ec-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="a52ec-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a52ec-105">Overview</span></span>

<span data-ttu-id="a52ec-106">Créez, mettez à jour et supprimez des clés et des secrets dans Azure Key Vault avec les bibliothèques clientes.</span><span class="sxs-lookup"><span data-stu-id="a52ec-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="a52ec-107">Utilisez les bibliothèques de gestion Azure Key Vault pour créer des coffres de clé, autoriser des applications et gérer les autorisations.</span><span class="sxs-lookup"><span data-stu-id="a52ec-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="a52ec-108">En savoir plus sur [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="a52ec-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="a52ec-109">Installer les bibliothèques</span><span class="sxs-lookup"><span data-stu-id="a52ec-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="a52ec-110">Bibliothèque cliente</span><span class="sxs-lookup"><span data-stu-id="a52ec-110">Client library</span></span>
```bash
pip install azure-keyvault
```

## <a name="example"></a><span data-ttu-id="a52ec-111">Exemple</span><span class="sxs-lookup"><span data-stu-id="a52ec-111">Example</span></span>
<span data-ttu-id="a52ec-112">Récupérez une [clé web JSON](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) à partir de Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a52ec-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callack(server, resource, scope):
    credentials = credentials or ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = resource
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callack))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```
[!div class="nextstepaction"]
[<span data-ttu-id="a52ec-113">Explorer les API clientes</span><span class="sxs-lookup"><span data-stu-id="a52ec-113">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/clientlibrary)

### <a name="management-api"></a><span data-ttu-id="a52ec-114">API de gestion</span><span class="sxs-lookup"><span data-stu-id="a52ec-114">Management API</span></span>
```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="a52ec-115">Exemple</span><span class="sxs-lookup"><span data-stu-id="a52ec-115">Example</span></span>
<span data-ttu-id="a52ec-116">L’exemple suivant montre comment créer un coffre Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a52ec-116">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="a52ec-117">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="a52ec-117">Explore the Management APIs</span></span>](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [<span data-ttu-id="a52ec-118">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="a52ec-118">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/managementlibrary)

## <a name="samples"></a><span data-ttu-id="a52ec-119">Exemples</span><span class="sxs-lookup"><span data-stu-id="a52ec-119">Samples</span></span>
* <span data-ttu-id="a52ec-120">[Gérer les coffres de clés][1]</span><span class="sxs-lookup"><span data-stu-id="a52ec-120">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="a52ec-121">[Récupération d’un coffre Key Vault][2]</span><span class="sxs-lookup"><span data-stu-id="a52ec-121">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="a52ec-122">Affichez la [liste complète](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) des exemples de coffres Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a52ec-122">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 