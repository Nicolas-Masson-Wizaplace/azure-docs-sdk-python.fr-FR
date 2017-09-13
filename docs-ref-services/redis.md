---
title: "Bibliothèques Redis Azure pour Python"
description: "Consulter la documentation sur les bibliothèques de client Python pour Redis"
keywords: "Azure, Python, Redis, API, Kit de développement logiciel (SDK), base de données, NoSQL"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: redis
ms.openlocfilehash: 3ba8d972e91fad229c1489800edeca08760254e6
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-redis-cache-libraries-for-python"></a><span data-ttu-id="6fbef-104">Bibliothèques Cache Redis Azure pour Python</span><span class="sxs-lookup"><span data-stu-id="6fbef-104">Azure Redis Cache libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="6fbef-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="6fbef-105">Overview</span></span>

<span data-ttu-id="6fbef-106">Le Cache Redis Azure se base sur le projet Redis open source connu.</span><span class="sxs-lookup"><span data-stu-id="6fbef-106">Azure Redis Cache is based on the popular open source Redis project.</span></span> <span data-ttu-id="6fbef-107">Il vous permet d’accéder à une instance Redis sécurisée et dédiée, gérée par Microsoft et accessible depuis vos applications Azure.</span><span class="sxs-lookup"><span data-stu-id="6fbef-107">It gives you access to a secure, dedicated Redis instance, managed by Microsoft and accessible from your Azure apps.</span></span>

<span data-ttu-id="6fbef-108">Redis est un stockage clé/valeur avancé, dont les clés peuvent contenir des structures de données telles que des chaînes, des hachages, des listes, des groupes et des groupes triés.</span><span class="sxs-lookup"><span data-stu-id="6fbef-108">Redis is an advanced key-value store, where keys can contain data structures such as strings, hashes, lists, sets, and sorted sets.</span></span> <span data-ttu-id="6fbef-109">Redis prend en charge un ensemble d'opérations atomiques pour ces types de données.</span><span class="sxs-lookup"><span data-stu-id="6fbef-109">Redis supports a set of atomic operations on these data types.</span></span>

<span data-ttu-id="6fbef-110">En savoir plus sur le [Cache Redis Azure](https://docs.microsoft.com/azure/redis-cache/).</span><span class="sxs-lookup"><span data-stu-id="6fbef-110">Learn more about [Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/).</span></span>

## <a name="management-api"></a><span data-ttu-id="6fbef-111">API de gestion</span><span class="sxs-lookup"><span data-stu-id="6fbef-111">Management API</span></span>

<span data-ttu-id="6fbef-112">Créez et gérez vos ressources Redis dans votre abonnement avec l’API de gestion de Redis.</span><span class="sxs-lookup"><span data-stu-id="6fbef-112">Create and manage your Redis resources in your subscription with the Redis management API.</span></span>

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a><span data-ttu-id="6fbef-113">Exemple</span><span class="sxs-lookup"><span data-stu-id="6fbef-113">Example</span></span>

<span data-ttu-id="6fbef-114">L’exemple suivant crée un Cache Redis :</span><span class="sxs-lookup"><span data-stu-id="6fbef-114">The following example creates a new Redis cache:</span></span>

```python
from azure.mgmt.redis import RedisManagementClient
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

redis_client = RedisManagementClient(
    credentials,
    subscription_id
)
group_name = 'myresourcegroup'
cache_name = 'mycachename'
redis_cache = redis_client.redis.create_or_update(
    group_name,
    cache_name,
    RedisCreateOrUpdateParameters(
        sku = Sku(name = 'Basic', family = 'C', capacity = '1'),
        location = "East US"
    )
)
# redis_cache is a RedisResourceWithAccessKey instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="6fbef-115">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="6fbef-115">Explore the Management APIs</span></span>](/python/api/overview/azure/redis/managementlibrary)

