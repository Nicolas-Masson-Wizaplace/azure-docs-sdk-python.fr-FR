---
title: "Bibliothèques Azure CDN pour Python"
description: "Références sur les bibliothèques Azure CDN pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, CDN"
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: c704b32ff5fd6db922ef9c296142832455088562
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
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

### <a name="example"></a>Exemple

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

endpoint_poller = client.endpoints.create('my-resource-group',
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
> [Explorer les API de gestion](/python/api/overview/azure/cdn/managementlibrary)
