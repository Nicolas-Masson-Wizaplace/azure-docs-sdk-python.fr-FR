---
title: "Bibliothèques de réseau Azure pour Python"
description: "Références sur les bibliothèques de réseau Azure pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, réseau"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 47252ca3b2f5c6087277bac3735025f0dbabbdd8
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="azure-network-libraries-for-python"></a>Bibliothèques de réseau Azure pour Python

## <a name="overview"></a>Vue d'ensemble

Le [réseau virtuel Azure](/azure/virtual-network/virtual-networks-overview) vous permet de connecter des ressources Azure, et de les connecter à votre réseau local.

Pour découvrir le réseau virtuel Azure, consultez la section [Créer votre premier réseau virtuel](/azure/virtual-network/virtual-network-get-started-vnet-subnet).

## <a name="management-apis"></a>API de gestion

Inspectez, gérez et configurez des réseaux virtuels Azure avec les API de gestion.

Contrairement à d’autres API Azure pour Python, les API de réseau sont clairement divisées en packages séparés. Vous n’avez pas besoin de les importer individuellement, puisque les informations de package sont détaillées chez le constructeur du client.

Installez le package de gestion avec pip.

```bash
pip install azure-mgmt-network
```

### <a name="example"></a>exemples

Créez un réseau virtuel et un sous-réseau associé.

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
> [Explorer les API de gestion](/python/api/overview/azure/network/management)

### <a name="samples"></a>Exemples

* [Getting started with Azure Resource Manager for load balancers in Python](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)(Prise en main d’Azure Resource Manager pour les équilibreurs de charge de Python)

Affichez la [liste complète](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) des exemples du réseau virtuel Azure.
