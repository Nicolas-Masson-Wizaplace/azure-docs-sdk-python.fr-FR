---
title: Bibliothèques de ressources Azure pour Python
description: Références sur les bibliothèques de ressources Azure pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, ressources
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f331a88ceac73449c9c1aff7bccbbaff4c4ba7be
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277376"
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="927dd-104">Bibliothèques de ressources Azure pour Python</span><span class="sxs-lookup"><span data-stu-id="927dd-104">Azure Resources libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="927dd-105">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="927dd-105">Overview</span></span> 
<span data-ttu-id="927dd-106">Déployez, surveillez et gérez des ressources dans des groupes avec [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="927dd-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="927dd-107">API de gestion</span><span class="sxs-lookup"><span data-stu-id="927dd-107">Management API</span></span>
<span data-ttu-id="927dd-108">Utilisez l’API de gestion pour créer des groupes de ressources et déployer des ressources à partir de modèles.</span><span class="sxs-lookup"><span data-stu-id="927dd-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a><span data-ttu-id="927dd-109">Exemples</span><span class="sxs-lookup"><span data-stu-id="927dd-109">Example</span></span> 
<span data-ttu-id="927dd-110">Créer un groupe de ressources dans la région Azure Est des États-Unis.</span><span class="sxs-lookup"><span data-stu-id="927dd-110">Create a new resource group in the Azure Eastern US region.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="927dd-111">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="927dd-111">Explore the Management APIs</span></span>](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a><span data-ttu-id="927dd-112">Exemples</span><span class="sxs-lookup"><span data-stu-id="927dd-112">Samples</span></span>
[<span data-ttu-id="927dd-113">Manage Azure resources and resource groups with .NET (Gérer des ressources et des groupes de ressources Azure avec .NET)</span><span class="sxs-lookup"><span data-stu-id="927dd-113">Manage Azure resources and resource groups</span></span>](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

<span data-ttu-id="927dd-114">Affichez la [liste complète](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) d’exemples Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="927dd-114">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) of Azure Resource Manager samples.</span></span>
