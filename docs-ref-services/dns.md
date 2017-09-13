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
# <a name="azure-dns-libraries-for-python"></a>Bibliothèques Azure DNS pour Python

## <a name="overview"></a>Vue d'ensemble

[Azure DNS](/azure/dns/dns-overview) est un service d’hébergement pour les domaines DNS, offrant une résolution de noms à l’aide de l’infrastructure Azure.

Pour découvrir Azure DNS, consultez la section [Prise en main d’Azure DNS à l’aide du portail Azure](/azure/dns/dns-getstarted-portal).

## <a name="management-apis"></a>API de gestion

Créez et gérez des zones DNS et des enregistrements avec l’API de gestion.

Installez le package de gestion avec pip.

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a>Exemple

Créez une zone DNS.

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
> [Explorer les API de gestion](/python/api/overview/azure/dns/managementlibrary)
