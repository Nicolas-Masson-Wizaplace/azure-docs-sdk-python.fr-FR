---
title: "Bibliothèques de ressources Azure pour Python"
description: 
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, ressources"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: 32e13bee27db091f0bca12c7d9ae4fc62165f4c0
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="43cd3-103">Bibliothèques de ressources Azure pour Python</span><span class="sxs-lookup"><span data-stu-id="43cd3-103">Azure Resources libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="43cd3-104">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="43cd3-104">Overview</span></span> 
<span data-ttu-id="43cd3-105">Gérez des ressources Azure dans des groupes de ressources.</span><span class="sxs-lookup"><span data-stu-id="43cd3-105">Manage Azure resources in resource groups.</span></span>

| <span data-ttu-id="43cd3-106">Package</span><span class="sxs-lookup"><span data-stu-id="43cd3-106">Package</span></span>  |  <span data-ttu-id="43cd3-107">Description</span><span class="sxs-lookup"><span data-stu-id="43cd3-107">Description</span></span> |
|---|---|
|<span data-ttu-id="43cd3-108">[azure.mgmt.resource.features][1]</span><span class="sxs-lookup"><span data-stu-id="43cd3-108">[azure.mgmt.resource.features][1]</span></span>|<span data-ttu-id="43cd3-109">Le contrôle d’exposition aux fonctionnalités d’Azure (AFEC) fournit un mécanisme permettant aux fournisseurs de ressources de contrôler l’exposition aux fonctionnalités des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="43cd3-109">Azure Feature Exposure Control (AFEC) provides a mechanism for the resource providers to control feature exposure to users.</span></span>|
|<span data-ttu-id="43cd3-110">[azure.mgmt.resource.links][2]</span><span class="sxs-lookup"><span data-stu-id="43cd3-110">[azure.mgmt.resource.links][2]</span></span>|<span data-ttu-id="43cd3-111">Les ressources Azure peuvent être liées pour établir des relations logiques.</span><span class="sxs-lookup"><span data-stu-id="43cd3-111">Azure resources can be linked together to form logical relationships.</span></span> <span data-ttu-id="43cd3-112">Vous pouvez établir des liens entre les ressources de différents groupes de ressources.</span><span class="sxs-lookup"><span data-stu-id="43cd3-112">You can establish links between resources belonging to different resource groups.</span></span>|
|<span data-ttu-id="43cd3-113">[azure.mgmt.resource.locks][3]</span><span class="sxs-lookup"><span data-stu-id="43cd3-113">[azure.mgmt.resource.locks][3]</span></span>|<span data-ttu-id="43cd3-114">Les ressources Azure peuvent être verrouillées pour empêcher les autres utilisateurs de votre organisation de supprimer ou modifier des ressources.</span><span class="sxs-lookup"><span data-stu-id="43cd3-114">Azure resources can be locked to prevent other users in your organization from deleting or modifying resources.</span></span>|
|<span data-ttu-id="43cd3-115">[azure.mgmt.resource.managedapplications][4]</span><span class="sxs-lookup"><span data-stu-id="43cd3-115">[azure.mgmt.resource.managedapplications][4]</span></span>|<span data-ttu-id="43cd3-116">Applications gérées ARM (appliances).</span><span class="sxs-lookup"><span data-stu-id="43cd3-116">ARM managed applications (appliances).</span></span>|
|<span data-ttu-id="43cd3-117">[azure.mgmt.resource.policy][5]</span><span class="sxs-lookup"><span data-stu-id="43cd3-117">[azure.mgmt.resource.policy][5]</span></span>|<span data-ttu-id="43cd3-118">Pour gérer et contrôler l’accès à vos ressources, vous pouvez définir des stratégies personnalisées et leurs portées.</span><span class="sxs-lookup"><span data-stu-id="43cd3-118">To manage and control access to your resources, you can define customized policies and assign them at a scope.</span></span>|
|<span data-ttu-id="43cd3-119">[azure.mgmt.resource.resources][6]</span><span class="sxs-lookup"><span data-stu-id="43cd3-119">[azure.mgmt.resource.resources][6]</span></span>| <span data-ttu-id="43cd3-120">Fournit des opérations utiliser des ressources et des groupes de ressources.</span><span class="sxs-lookup"><span data-stu-id="43cd3-120">Provides operations for working with resources and resource groups.</span></span>|
|<span data-ttu-id="43cd3-121">[azure.mgmt.resource.subscriptions][7]</span><span class="sxs-lookup"><span data-stu-id="43cd3-121">[azure.mgmt.resource.subscriptions][7]</span></span>|<span data-ttu-id="43cd3-122">Tous les groupes de ressources et les ressources existent dans les abonnements.</span><span class="sxs-lookup"><span data-stu-id="43cd3-122">All resource groups and resources exist within subscriptions.</span></span> <span data-ttu-id="43cd3-123">Ces opérations vous permettent d’obtenir des informations sur vos abonnements et vos clients.</span><span class="sxs-lookup"><span data-stu-id="43cd3-123">These operation enable you get information about your subscriptions and tenants.</span></span>|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a><span data-ttu-id="43cd3-124">Installer les bibliothèques</span><span class="sxs-lookup"><span data-stu-id="43cd3-124">Install the libraries</span></span> 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="43cd3-125">Exemple</span><span class="sxs-lookup"><span data-stu-id="43cd3-125">Example</span></span>
<span data-ttu-id="43cd3-126">L’exemple suivant montre comment créer un groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="43cd3-126">The following is an example of how to create a resource group.</span></span> 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

<span data-ttu-id="43cd3-127">Découvrez d’autres [exemples de code Python](https://azure.microsoft.com/resources/samples/?platform=python) à utiliser dans vos applications.</span><span class="sxs-lookup"><span data-stu-id="43cd3-127">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span> 