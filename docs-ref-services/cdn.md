---
title: Bibliothèques Azure CDN pour Python
description: Références sur les bibliothèques Azure CDN pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, CDN
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 2dd7703e94a814d85716a7b96994666e32f95565
ms.sourcegitcommit: 3db75daa592da90ea9aa8fd17fb99627a30eb4fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66179870"
---
# <a name="azure-cdn-libraries-for-python"></a>Bibliothèques Azure CDN pour Python

## <a name="overview"></a>Vue d'ensemble

Le [réseau de distribution de contenu (CDN) Azure](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) vous permet de mettre en cache du contenu web pour garantir une disponibilité de bande passante élevée dans le monde entier.

Pour découvrir Azure CDN, consultez la section [Prise en main d’Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).

## <a name="management-apis"></a>API de gestion

Créez, interrogez et gérez des réseaux de distribution de contenu (CDN) Azure avec l’API de gestion.

Installez le package de gestion via pip.

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a>Exemples

Création d’un profil CDN avec un seul point de terminaison défini :

```python
from azure.mgmt.cdn import CdnManagementClient

cdn_client = CdnManagementClient(credentials, 'your-subscription-id')
profile_poller = cdn_client.profiles.create('my-resource-group',
                                            'cdn-name',
                                            {
                                                "location": "some_region", 
                                                "sku": {
                                                    "name": "sku_tier"
                                                } 
                                            })
profile = profile_poller.result()

endpoint_poller = cdn_client.endpoints.create('my-resource-group',
                                          'cdn-name',
                                          'unique-endpoint-name', 
                                          { 
                                              "location": "any_region", 
                                              "origins": [
                                                  {
                                                      "name": "origin_name", 
                                                      "host_name": "url"
                                                  }]
                                          })
endpoint = endpoint_poller.result()
```

> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/cdn/management)
