---
title: Bibliothèques Azure DNS pour Python
description: Références sur les bibliothèques Azure DNS pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, DNS
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 294373469b1792821253ae46ab51fa0c06a74ffa
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551562"
---
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="7a150-104">Bibliothèques Azure DNS pour Python</span><span class="sxs-lookup"><span data-stu-id="7a150-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="7a150-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="7a150-105">Overview</span></span>

<span data-ttu-id="7a150-106">[Azure DNS](/azure/dns/dns-overview) est un service d’hébergement pour les domaines DNS, offrant une résolution de noms à l’aide de l’infrastructure Azure.</span><span class="sxs-lookup"><span data-stu-id="7a150-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="7a150-107">Pour découvrir Azure DNS, consultez la section [Prise en main d’Azure DNS à l’aide du portail Azure](/azure/dns/dns-getstarted-portal).</span><span class="sxs-lookup"><span data-stu-id="7a150-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apipythonapioverviewazurednsmanagement"></a>[<span data-ttu-id="7a150-108">API de gestion</span><span class="sxs-lookup"><span data-stu-id="7a150-108">Management API</span></span>](/python/api/overview/azure/dns/management)

```bash
pip install azure-mgmt-dns
```

## <a name="create-the-management-client"></a><span data-ttu-id="7a150-109">Créer le client de gestion</span><span class="sxs-lookup"><span data-stu-id="7a150-109">Create the management client</span></span>

<span data-ttu-id="7a150-110">Le code suivant permet de créer une instance du client de gestion.</span><span class="sxs-lookup"><span data-stu-id="7a150-110">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="7a150-111">Vous devrez fournir votre identifiant ``subscription_id``, qui peut être récupéré à partir de votre [liste d’abonnements](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span><span class="sxs-lookup"><span data-stu-id="7a150-111">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="7a150-112">Consultez la section relative à [l’authentification de la gestion de ressources](/python/azure/python-sdk-azure-authenticate) pour en savoir plus sur la gestion de l’authentification d’Azure Active Directory avec le Kit de développement logiciel (SDK) Python et la création d’une instance ``Credentials``.</span><span class="sxs-lookup"><span data-stu-id="7a150-112">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python 
from azure.mgmt.dns import DnsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

dns_client = DnsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="create-dns-zone"></a><span data-ttu-id="7a150-113">Créer une zone DNS</span><span class="sxs-lookup"><span data-stu-id="7a150-113">Create DNS zone</span></span>
```python
# The only valid value is 'global', otherwise you will get a:
# The subscription is not registered for the resource type 'dnszones' in the location 'westus'.
zone = dns_client.zones.create_or_update(
    'MyResourceGroup',
    'pydns.com',
    {
        'location': 'global'
    }
)
```
    
## <a name="create-a-record-set"></a><span data-ttu-id="7a150-114">Création d’un jeu d’enregistrements</span><span class="sxs-lookup"><span data-stu-id="7a150-114">Create a Record Set</span></span>
```python
record_set = dns_client.record_sets.create_or_update(
    'MyResourceGroup',
    'pydns.com',
    'MyRecordSet',
    'A',
    {
            "ttl": 300,
            "arecords": [
                {
                "ipv4_address": "1.2.3.4"
                },
                {
                "ipv4_address": "1.2.3.5"
                }
            ]
    }
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="7a150-115">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="7a150-115">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/management)
