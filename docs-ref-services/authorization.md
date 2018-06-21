---
title: Bibliothèques d’autorisation Azure pour Python
description: Références sur les bibliothèques d’autorisation Azure pour Python
keywords: Azure, python, Kit de développement logiciel (SDK), API, autorisation
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: a3db490357ec35c0780d7dd16114b9041458373d
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35720040"
---
# <a name="azure-authorization-libraries-for-python"></a>Bibliothèques d’autorisation Azure pour Python

## <a name="management-apipythonapioverviewazureauthorizationmanagement"></a>[API de gestion](/python/api/overview/azure/authorization/management)

```bash
pip install azure-mgmt-authorization
```

## <a name="create-the-management-client"></a>Créer le client de gestion

Le code suivant permet de créer une instance du client de gestion.

Vous devrez fournir votre identifiant ``subscription_id``, qui peut être récupéré à partir de votre [liste d’abonnements](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).

Consultez la section relative à [l’authentification de la gestion de ressources](/python/azure/python-sdk-azure-authenticate) pour en savoir plus sur la gestion de l’authentification d’Azure Active Directory avec le Kit de développement logiciel (SDK) Python et la création d’une instance ``Credentials``.

```python
from azure.mgmt.authorization import AuthorizationManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password'       # Your password
)

authorization_client = AuthorizationManagementClient(
    credentials,
    subscription_id
)
``` 

## <a name="check-permissions-for-a-resource-group"></a>Vérifier les autorisations d’un groupe de ressources

Le code suivant vérifie les autorisations dans un groupe de ressources donné.
Pour créer ou gérer des groupes de ressources, consultez la section relative à la [gestion des ressources](/python/api/overview/azure/azure.mgmt.resource).

```python
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

group_name = 'myresourcegroup'
permissions = self.authorization_client.permissions.list_for_resource_group(
    group_name
)
# permissions is a iterable of Permissions instances
```

> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/authorization/management)

