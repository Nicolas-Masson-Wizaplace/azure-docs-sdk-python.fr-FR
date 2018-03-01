---
title: "Bibliothèques Azure Event Grid pour Python"
description: 
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, Event Grid"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: 299b50ce8366d0c49ade28dfece98d6696a4f9ef
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="2f0d5-103">Bibliothèques Event Grid pour Python</span><span class="sxs-lookup"><span data-stu-id="2f0d5-103">Event Grid libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="2f0d5-104">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="2f0d5-104">Overview</span></span>
<span data-ttu-id="2f0d5-105">Azure Event Grid est un service de routage d’événement intelligent et entièrement géré qui permet une consommation d’événements uniforme à l’aide d’un modèle publication-abonnement.</span><span class="sxs-lookup"><span data-stu-id="2f0d5-105">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

## <a name="management-api"></a><span data-ttu-id="2f0d5-106">API de gestion</span><span class="sxs-lookup"><span data-stu-id="2f0d5-106">Management API</span></span>
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="2f0d5-107">exemples</span><span class="sxs-lookup"><span data-stu-id="2f0d5-107">Example</span></span>
<span data-ttu-id="2f0d5-108">L’exemple suivant crée une rubrique personnalisée, s’abonne à la rubrique et déclenche l’événement pour afficher le résultat.</span><span class="sxs-lookup"><span data-stu-id="2f0d5-108">The following creates a custom topic, subscribes to the topic, and triggers the event to view the result.</span></span> <span data-ttu-id="2f0d5-109">RequestBin est un outil tiers en open-source qui vous permet de créer un point de terminaison et d’afficher les requêtes qui lui sont envoyées.</span><span class="sxs-lookup"><span data-stu-id="2f0d5-109">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="2f0d5-110">Accédez à [RequestBin](https://requestb.in/), puis cliquez sur **Créer un RequestBin**.</span><span class="sxs-lookup"><span data-stu-id="2f0d5-110">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="2f0d5-111">Copiez l’URL du fichier bin, dont vous avez besoin pour vous abonner à la rubrique.</span><span class="sxs-lookup"><span data-stu-id="2f0d5-111">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.eventgrid import EventGridManagementClient
import requests

RESOURCE_GROUP_NAME = 'gridResourceGroup'
TOPIC_NAME = 'gridTopic1234'
LOCATION = 'westus2'
SUBSCRIPTION_ID = 'YOUR_SUBSCRIPTION_ID'
SUBSCRIPTION_NAME = 'gridSubscription'
REQUEST_BIN_URL = 'YOUR_REQUEST_BIN_URL'

# create resource group
resource_client = ResourceManagementClient(credentials, SUBSCRIPTION_ID)
resource_client.resource_groups.create_or_update(
    RESOURCE_GROUP_NAME,
    {
        'location': LOCATION
    }
)

event_client = EventGridManagementClient(credentials, SUBSCRIPTION_ID)

# create a custom topic
event_client.topics.create_or_update(RESOURCE_GROUP_NAME, TOPIC_NAME, LOCATION)

# subscribe to a topic
scope = '/subscriptions/'+SUBSCRIPTION_ID+'/resourceGroups/'+RESOURCE_GROUP_NAME+'/providers/Microsoft.EventGrid/topics/'+TOPIC_NAME
event_client.event_subscriptions.create(scope, SUBSCRIPTION_NAME,
    {
        'destination': {
            'endpoint_url': REQUEST_BIN_URL
        }
    }
)

# send an event to topic
# get endpoint url
url = event_client.event_subscriptions.get_full_url(scope, SUBSCRIPTION_NAME).endpoint_url
# get key
key = event_client.topics.list_shared_access_keys(RESOURCE_GROUP_NAME,TOPIC_NAME).key1
headers = {'aeg-sas-key': key}
s = requests.get('https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json')
r = requests.post(url, data=s, headers=headers)
print(r.status_code)
print(r.content)
```
<span data-ttu-id="2f0d5-112">Accédez à l’URL RequestBin créée précédemment pour voir l’événement envoyé.</span><span class="sxs-lookup"><span data-stu-id="2f0d5-112">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="2f0d5-113">Supprimer des ressources</span><span class="sxs-lookup"><span data-stu-id="2f0d5-113">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="2f0d5-114">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="2f0d5-114">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

