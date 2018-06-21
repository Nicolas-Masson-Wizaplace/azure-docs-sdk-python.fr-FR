---
title: Bibliothèques Azure Key Vault pour Python
description: Consulter la documentation sur les bibliothèques de client Python pour Azure Key Vault
author: lisawong19
keywords: Azure, Python, Kit de développement logiciel (SDK), API, clés, Key Vault, authentification, secret, clé, sécurité
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 3e7d9970f5799708c6822493106aec5466de52d9
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35719640"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="6a396-104">Bibliothèques Azure Key Vault pour Python</span><span class="sxs-lookup"><span data-stu-id="6a396-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="6a396-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="6a396-105">Overview</span></span>

<span data-ttu-id="6a396-106">Créez, mettez à jour et supprimez des clés et des secrets dans Azure Key Vault avec les bibliothèques clientes.</span><span class="sxs-lookup"><span data-stu-id="6a396-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="6a396-107">Utilisez les bibliothèques de gestion Azure Key Vault pour créer des coffres de clé, autoriser des applications et gérer les autorisations.</span><span class="sxs-lookup"><span data-stu-id="6a396-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="6a396-108">En savoir plus sur [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="6a396-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="6a396-109">Installer les bibliothèques</span><span class="sxs-lookup"><span data-stu-id="6a396-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="6a396-110">Bibliothèque cliente</span><span class="sxs-lookup"><span data-stu-id="6a396-110">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="6a396-111">Exemples</span><span class="sxs-lookup"><span data-stu-id="6a396-111">Examples</span></span>

<span data-ttu-id="6a396-112">Récupérez une [clé web JSON](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) à partir de Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6a396-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```

<span data-ttu-id="6a396-113">De même, vous pouvez utiliser l’extrait de code suivant pour récupérer un secret à partir du coffre :</span><span class="sxs-lookup"><span data-stu-id="6a396-113">Similarly, you can use the following snippet to retrieve a secret from the vault:</span></span>

```
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '',
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

secret_bundle = client.get_secret("https://VAULT_ID.vault.azure.net/", "SECRET_ID", "SECRET_VERSION")

print(secret_bundle.value)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="6a396-114">Explorer les API clientes</span><span class="sxs-lookup"><span data-stu-id="6a396-114">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a><span data-ttu-id="6a396-115">API de gestion</span><span class="sxs-lookup"><span data-stu-id="6a396-115">Management API</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="6a396-116">Exemples</span><span class="sxs-lookup"><span data-stu-id="6a396-116">Example</span></span>
<span data-ttu-id="6a396-117">L’exemple suivant montre comment créer un coffre Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6a396-117">The following example shows how to create an Azure Key Vault.</span></span> 

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
> [<span data-ttu-id="6a396-118">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="6a396-118">Explore the Management APIs</span></span>](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [<span data-ttu-id="6a396-119">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="6a396-119">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="6a396-120">Exemples</span><span class="sxs-lookup"><span data-stu-id="6a396-120">Samples</span></span>
* <span data-ttu-id="6a396-121">[Gérer les coffres de clés][1]</span><span class="sxs-lookup"><span data-stu-id="6a396-121">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="6a396-122">[Récupération d’un coffre Key Vault][2]</span><span class="sxs-lookup"><span data-stu-id="6a396-122">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="6a396-123">Affichez la [liste complète](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) des exemples de coffres Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6a396-123">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 
