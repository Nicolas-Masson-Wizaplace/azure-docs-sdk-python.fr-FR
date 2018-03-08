---
title: "Bibliothèques Azure Server Manager pour Python"
description: "Référence sur les bibliothèques Azure Server Manager pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, Server Manager"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 480513a76683c97fdac8d2a65b38fde0fffb6dcd
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="azure-server-manager-libraries-for-python"></a><span data-ttu-id="aff6f-104">Bibliothèques Azure Server Manager pour Python</span><span class="sxs-lookup"><span data-stu-id="aff6f-104">Azure Server Manager libraries for python</span></span>

## <a name="management-apipythonapioverviewazureservermanagermanagement"></a>[<span data-ttu-id="aff6f-105">API de gestion</span><span class="sxs-lookup"><span data-stu-id="aff6f-105">Management API</span></span>](/python/api/overview/azure/servermanager/management)

```bash
pip install azure-mgmt-servermanager
```

## <a name="create-the-management-client"></a><span data-ttu-id="aff6f-106">Créer le client de gestion</span><span class="sxs-lookup"><span data-stu-id="aff6f-106">Create the management client</span></span>

<span data-ttu-id="aff6f-107">Le code suivant permet de créer une instance du client de gestion.</span><span class="sxs-lookup"><span data-stu-id="aff6f-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="aff6f-108">Vous devrez fournir votre identifiant ``subscription_id``, qui peut être récupéré à partir de votre [liste d’abonnements](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span><span class="sxs-lookup"><span data-stu-id="aff6f-108">You will need to provide your ``subscription_id`` which can be retrieved from your [subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="aff6f-109">Consultez la section relative à [l’authentification de la gestion de ressources](/python/azure/python-sdk-azure-authenticate) pour en savoir plus sur la gestion de l’authentification d’Azure Active Directory avec le Kit de développement logiciel (SDK) Python et la création d’une instance ``Credentials``.</span><span class="sxs-lookup"><span data-stu-id="aff6f-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.servermanager import ServerManagement
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

servermanager_client = ServerManagement(
    credentials,
    subscription_id
)
``` 

## <a name="create-gateway"></a><span data-ttu-id="aff6f-110">Créer une passerelle</span><span class="sxs-lookup"><span data-stu-id="aff6f-110">Create gateway</span></span>
```python
gateway_async = servermanager_client.gateway.create(
    'MyResourceGroup',
    'MyGateway',
    'centralus'
)
gateway = gateway_async.result() # Blocking wait
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="aff6f-111">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="aff6f-111">Explore the Management APIs</span></span>](/python/api/overview/azure/servermanager/management)