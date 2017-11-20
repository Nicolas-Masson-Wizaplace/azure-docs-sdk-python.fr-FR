---
title: "Bibliothèques Azure DNS pour Python"
description: "Références sur les bibliothèques Azure DNS pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, DNS"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 59ae3628b4a810db8fee21aacf46c13054dc8cd3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="2e381-104">Bibliothèques Azure DNS pour Python</span><span class="sxs-lookup"><span data-stu-id="2e381-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="2e381-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="2e381-105">Overview</span></span>

<span data-ttu-id="2e381-106">[Azure DNS](/azure/dns/dns-overview) est un service d’hébergement pour les domaines DNS, offrant une résolution de noms à l’aide de l’infrastructure Azure.</span><span class="sxs-lookup"><span data-stu-id="2e381-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="2e381-107">Pour découvrir Azure DNS, consultez la section [Prise en main d’Azure DNS à l’aide du portail Azure](/azure/dns/dns-getstarted-portal).</span><span class="sxs-lookup"><span data-stu-id="2e381-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apis"></a><span data-ttu-id="2e381-108">API de gestion</span><span class="sxs-lookup"><span data-stu-id="2e381-108">Management APIs</span></span>

<span data-ttu-id="2e381-109">Créez et gérez des zones DNS et des enregistrements avec l’API de gestion.</span><span class="sxs-lookup"><span data-stu-id="2e381-109">Create and manage DNS zones and records with the management API.</span></span>

<span data-ttu-id="2e381-110">Installez le package de gestion avec pip.</span><span class="sxs-lookup"><span data-stu-id="2e381-110">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a><span data-ttu-id="2e381-111">Exemple</span><span class="sxs-lookup"><span data-stu-id="2e381-111">Example</span></span>

<span data-ttu-id="2e381-112">Créez une zone DNS.</span><span class="sxs-lookup"><span data-stu-id="2e381-112">Create a new DNS zone.</span></span>

```python
from azure.mgmt.dns import DnsManagementClient

dns_client = DnsManagementClient(credentials, 'your-subscription-id')
zone = dns_client.zones.create_or_update('resource-group',
                                         'zone_name_no_dot',
                                         {
                                            "location": "global"
                                         })

```

> [!div class="nextstepaction"]
> [<span data-ttu-id="2e381-113">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="2e381-113">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/managementlibrary)
