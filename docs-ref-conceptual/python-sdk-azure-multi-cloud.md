---
title: Multi-cloud
description: utilisation d’Azure dans toutes les régions
author: lmazuel
ms.author: lmazuel
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 6d2ba0580f8b6dda857b48ed5235a8c969a051f5
ms.sourcegitcommit: 7066ace94076483bae7d54172605f431e47bd5ee
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="multi-cloud---use-azure-on-all-regions"></a><span data-ttu-id="70c33-103">Multi-cloud : utilisation d’Azure dans toutes les régions</span><span class="sxs-lookup"><span data-stu-id="70c33-103">Multi-cloud - use Azure on all regions</span></span>

<span data-ttu-id="70c33-104">Vous pouvez utiliser le Kit de développement logiciel (SDK) Azure pour Python afin de vous connecter à l’ensemble des régions au sein desquelles Azure est [disponible](https://azure.microsoft.com/regions/services).</span><span class="sxs-lookup"><span data-stu-id="70c33-104">You can use the Azure SDK for Python to connect to all regions where Azure is [available](https://azure.microsoft.com/regions/services).</span></span>

<span data-ttu-id="70c33-105">Par défaut, le Kit de développement logiciel (SDK) Azure pour Python est configuré pour une connexion à l’instance Azure publique.</span><span class="sxs-lookup"><span data-stu-id="70c33-105">By default, the Azure SDK for Python is configured to connect to public Azure.</span></span>

## <a name="using-predeclared-cloud-definition"></a><span data-ttu-id="70c33-106">Utilisation d’une définition de cloud prédéclarée</span><span class="sxs-lookup"><span data-stu-id="70c33-106">Using predeclared cloud definition</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70c33-107">La version du package `msrestazure` doit être supérieure ou égale à 0.4.11 pour cette section.</span><span class="sxs-lookup"><span data-stu-id="70c33-107">The `msrestazure` package must be superior or equals to 0.4.11 for this section.</span></span>

<span data-ttu-id="70c33-108">Vous pouvez utiliser le module `azure_cloud` de `msrestazure`</span><span class="sxs-lookup"><span data-stu-id="70c33-108">You can use the `azure_cloud` module of `msrestazure`</span></span>

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=AZURE_CHINA_CLOUD
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager
)
``` 
  
<span data-ttu-id="70c33-109">Les définitions de cloud disponibles sont :</span><span class="sxs-lookup"><span data-stu-id="70c33-109">Available cloud definition are</span></span>
  - <span data-ttu-id="70c33-110">AZURE_PUBLIC_CLOUD</span><span class="sxs-lookup"><span data-stu-id="70c33-110">AZURE_PUBLIC_CLOUD</span></span>
  - <span data-ttu-id="70c33-111">AZURE_CHINA_CLOUD</span><span class="sxs-lookup"><span data-stu-id="70c33-111">AZURE_CHINA_CLOUD</span></span>
  - <span data-ttu-id="70c33-112">AZURE_US_GOV_CLOUD</span><span class="sxs-lookup"><span data-stu-id="70c33-112">AZURE_US_GOV_CLOUD</span></span>
  - <span data-ttu-id="70c33-113">AZURE_GERMAN_CLOUD</span><span class="sxs-lookup"><span data-stu-id="70c33-113">AZURE_GERMAN_CLOUD</span></span>

## <a name="using-your-own-cloud-definition-eg-azure-stack"></a><span data-ttu-id="70c33-114">Utilisation de votre propre définition de cloud (par exemple, Azure Stack)</span><span class="sxs-lookup"><span data-stu-id="70c33-114">Using your own cloud definition (e.g. Azure Stack)</span></span>
<span data-ttu-id="70c33-115">ARM inclut un point de terminaison de métadonnées, afin de vous aider :</span><span class="sxs-lookup"><span data-stu-id="70c33-115">ARM has a metadata endpoint to help you:</span></span>

```python
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

mystack_cloud = get_cloud_from_metadata_endpoint("https://myazurestack-arm-endpoint.com")
credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=mystack_cloud
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=mystack_cloud.endpoints.resource_manager
)
```
## <a name="using-adal"></a><span data-ttu-id="70c33-116">Utilisation de la bibliothèque ADAL</span><span class="sxs-lookup"><span data-stu-id="70c33-116">Using ADAL</span></span>

<span data-ttu-id="70c33-117">Pour vous connecter à une autre région, vous devez prendre en compte certains éléments :</span><span class="sxs-lookup"><span data-stu-id="70c33-117">To connect to another region, a few things have to be considered:</span></span>

- <span data-ttu-id="70c33-118">Sur quel point de terminaison vais-je demander ce jeton (authentification) ?</span><span class="sxs-lookup"><span data-stu-id="70c33-118">What is the endpoint where to ask for a token (authentication)?</span></span>
- <span data-ttu-id="70c33-119">Dans quel point de terminaison vais-je utiliser ce jeton (utilisation) ?</span><span class="sxs-lookup"><span data-stu-id="70c33-119">What is the endpoint where I will use this token (usage)?</span></span>

<span data-ttu-id="70c33-120">Voici un exemple générique :</span><span class="sxs-lookup"><span data-stu-id="70c33-120">This is a generic example:</span></span>

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Public Azure - default values
authentication_endpoint = 'https://login.microsoftonline.com/'
azure_endpoint = 'https://management.azure.com/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-government"></a><span data-ttu-id="70c33-121">Azure Government</span><span class="sxs-lookup"><span data-stu-id="70c33-121">Azure Government</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Government
authentication_endpoint = 'https://login-us.microsoftonline.com/'
azure_endpoint = 'https://management.usgovcloudapi.net/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-germany"></a><span data-ttu-id="70c33-122">Azure Germany</span><span class="sxs-lookup"><span data-stu-id="70c33-122">Azure Germany</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure Germany
authentication_endpoint = 'https://login.microsoftonline.de/'
azure_endpoint = 'https://management.microsoftazure.de/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-china"></a><span data-ttu-id="70c33-123">Azure China</span><span class="sxs-lookup"><span data-stu-id="70c33-123">Azure China</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure China
authentication_endpoint = 'https://login.chinacloudapi.cn/'
azure_endpoint = 'https://management.chinacloudapi.cn/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```