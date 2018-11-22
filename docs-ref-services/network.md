---
title: Bibliothèques de réseau Azure pour Python
description: Références sur les bibliothèques de réseau Azure pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, réseau
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f468f844becc2f0fe07d892c725c00df4669f4e3
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52273025"
---
# <a name="azure-network-libraries-for-python"></a><span data-ttu-id="ea54d-104">Bibliothèques de réseau Azure pour Python</span><span class="sxs-lookup"><span data-stu-id="ea54d-104">Azure Network libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="ea54d-105">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="ea54d-105">Overview</span></span>

<span data-ttu-id="ea54d-106">Le [réseau virtuel Azure](/azure/virtual-network/virtual-networks-overview) vous permet de connecter des ressources Azure, et de les connecter à votre réseau local.</span><span class="sxs-lookup"><span data-stu-id="ea54d-106">[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) allows you to connect Azure resources, and also connect them to your on-premises network.</span></span>

<span data-ttu-id="ea54d-107">Pour découvrir le réseau virtuel Azure, consultez la section [Créer votre premier réseau virtuel](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span><span class="sxs-lookup"><span data-stu-id="ea54d-107">To get started with Azure Virtual Network, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-apis"></a><span data-ttu-id="ea54d-108">API de gestion</span><span class="sxs-lookup"><span data-stu-id="ea54d-108">Management APIs</span></span>

<span data-ttu-id="ea54d-109">Inspectez, gérez et configurez des réseaux virtuels Azure avec les API de gestion.</span><span class="sxs-lookup"><span data-stu-id="ea54d-109">Inspect, manage, and configure Azure virtual networks with the management APIs.</span></span>

<span data-ttu-id="ea54d-110">Contrairement à d’autres API Azure pour Python, les API de réseau sont clairement divisées en packages séparés.</span><span class="sxs-lookup"><span data-stu-id="ea54d-110">Unlike other Azure python APIs, the networking APIs are explicitly versioned into separage packages.</span></span> <span data-ttu-id="ea54d-111">Vous n’avez pas besoin de les importer individuellement, puisque les informations de package sont détaillées chez le constructeur du client.</span><span class="sxs-lookup"><span data-stu-id="ea54d-111">You do not need to import them individually since the package information is specified in the client constructor.</span></span>

<span data-ttu-id="ea54d-112">Installez le package de gestion avec pip.</span><span class="sxs-lookup"><span data-stu-id="ea54d-112">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-network
```

### <a name="example"></a><span data-ttu-id="ea54d-113">Exemples</span><span class="sxs-lookup"><span data-stu-id="ea54d-113">Example</span></span>

<span data-ttu-id="ea54d-114">Créez un réseau virtuel et un sous-réseau associé.</span><span class="sxs-lookup"><span data-stu-id="ea54d-114">Create a virtual network and an associated subnet.</span></span>

```python
from azure.mgmt.network import NetworkManagementClient

GROUP_NAME = 'resource-group'
VNET_NAME = 'your-vnet-identifier'
LOCATION = 'region'
SUBNET_NAME = 'your-subnet-identifier'

network_client = NetworkManagementClient(credentials, 'your-subscription-id')

async_vnet_creation = network_client.virtual_networks.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    {
        'location': LOCATION,
        'address_space': {
            'address_prefixes': ['10.0.0.0/16']
        }
    }
)
async_vnet_creation.wait()

# Create Subnet
async_subnet_creation = network_client.subnets.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    SUBNET_NAME,
    {'address_prefix': '10.0.0.0/24'}
)
subnet_info = async_subnet_creation.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ea54d-115">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="ea54d-115">Explore the Management APIs</span></span>](/python/api/overview/azure/network/management)

### <a name="samples"></a><span data-ttu-id="ea54d-116">Exemples</span><span class="sxs-lookup"><span data-stu-id="ea54d-116">Samples</span></span>

* <span data-ttu-id="ea54d-117">[Getting started with Azure Resource Manager for load balancers in Python](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)(Prise en main d’Azure Resource Manager pour les équilibreurs de charge de Python)</span><span class="sxs-lookup"><span data-stu-id="ea54d-117">[Getting started with Azure Resource Manager for load balancers in Python](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)</span></span>

<span data-ttu-id="ea54d-118">Affichez la [liste complète](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) des exemples du réseau virtuel Azure.</span><span class="sxs-lookup"><span data-stu-id="ea54d-118">View the [complete list](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) of Azure Virtual Network samples.</span></span>
