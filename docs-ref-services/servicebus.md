---
title: Bibliothèques Service Bus pour Python
description: Consulter la documentation sur les bibliothèques de client et de gestion Python pour Service Bus
keywords: Azure, Python, Kit de développement logiciel (SDK), API, messagerie, pubsub, pub-sub, répartiteur de messages
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 6c0bc66fbe8194b5b8f34ee8e29945b03ba8c242
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551592"
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="b9a83-104">Bibliothèques Service Bus pour Python</span><span class="sxs-lookup"><span data-stu-id="b9a83-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="b9a83-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="b9a83-105">Overview</span></span>

<span data-ttu-id="b9a83-106">Microsoft Azure Service Bus prend en charge un ensemble de technologies middleware Cloud orientées messages, notamment la mise en file d’attente de messages fiable et la messagerie de publication/abonnement durable.</span><span class="sxs-lookup"><span data-stu-id="b9a83-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="b9a83-107">Installer les bibliothèques</span><span class="sxs-lookup"><span data-stu-id="b9a83-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="servicebus-queues"></a><span data-ttu-id="b9a83-108">Files d’attente ServiceBus</span><span class="sxs-lookup"><span data-stu-id="b9a83-108">ServiceBus Queues</span></span>
<span data-ttu-id="b9a83-109">Les files d’attente ServiceBus sont une alternative aux files d’attente de stockage qui peuvent être utiles dans les scénarios requérant des fonctionnalités de messagerie plus avancées (messages plus volumineux, classement des messages, lectures destructives à opération unique, fourniture planifiée) via une fourniture de style push (interrogation sur le long terme).</span><span class="sxs-lookup"><span data-stu-id="b9a83-109">ServiceBus Queues are an alternative to Storage Queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

<span data-ttu-id="b9a83-110">Le service peut utiliser l’authentification de signature d’accès partagé ou l’authentification ACS.</span><span class="sxs-lookup"><span data-stu-id="b9a83-110">The service can use Shared Access Signature authentication, or ACS authentication.</span></span>

<span data-ttu-id="b9a83-111">Les espaces de noms Service Bus créés via le portail Azure après le mois d’août 2014 ne prennent plus en charge l’authentification ACS.</span><span class="sxs-lookup"><span data-stu-id="b9a83-111">Service bus namespaces created using the Azure portal after August 2014 no longer support ACS authentication.</span></span> <span data-ttu-id="b9a83-112">Vous pouvez créer des espaces de noms compatibles avec ACS grâce au Kit de développement logiciel (SDK) Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b9a83-112">You can create ACS compatible namespaces with the Azure SDK.</span></span>

### <a name="shared-access-signature-authentication"></a><span data-ttu-id="b9a83-113">Authentification avec une signature d’accès partagé</span><span class="sxs-lookup"><span data-stu-id="b9a83-113">Shared Access Signature Authentication</span></span>

<span data-ttu-id="b9a83-114">Pour utiliser l’authentification par signature d’accès partagé, créez le service Service Bus avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b9a83-114">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a><span data-ttu-id="b9a83-115">Authentification ACS</span><span class="sxs-lookup"><span data-stu-id="b9a83-115">ACS Authentication</span></span>

<span data-ttu-id="b9a83-116">Pour utiliser l’authentification ACS, créez le service Service Bus avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b9a83-116">To use ACS authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a><span data-ttu-id="b9a83-117">Envoi et réception de messages</span><span class="sxs-lookup"><span data-stu-id="b9a83-117">Sending and Receiving Messages</span></span>

<span data-ttu-id="b9a83-118">La méthode **create\_queue** peut être utilisée pour garantir l’existence d’une file d’attente :</span><span class="sxs-lookup"><span data-stu-id="b9a83-118">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="b9a83-119">Vous pouvez ensuite appeler la méthode **send\_queue\_message** pour insérer le message dans la file d’attente :</span><span class="sxs-lookup"><span data-stu-id="b9a83-119">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="b9a83-120">La méthode **send\_queue\_message_batch** peut ensuite être appelée pour envoyer plusieurs messages en même temps :</span><span class="sxs-lookup"><span data-stu-id="b9a83-120">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="b9a83-121">Il est ensuite possible d’appeler la méthode **receive\_queue\_message** pour retirer le message de la file d’attente.</span><span class="sxs-lookup"><span data-stu-id="b9a83-121">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a><span data-ttu-id="b9a83-122">Rubriques ServiceBus</span><span class="sxs-lookup"><span data-stu-id="b9a83-122">ServiceBus Topics</span></span>

<span data-ttu-id="b9a83-123">Les rubriques ServiceBus sont une abstraction placée sur les files d’attente ServiceBus, qui facilite l’implémentation de scénarios de publication/d’abonnement.</span><span class="sxs-lookup"><span data-stu-id="b9a83-123">ServiceBus topics are an abstraction on top of ServiceBus Queues that make pub/sub scenarios easy to implement.</span></span>

<span data-ttu-id="b9a83-124">La méthode **create\_topic** peut ensuite servir à créer une rubrique côté serveur :</span><span class="sxs-lookup"><span data-stu-id="b9a83-124">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="b9a83-125">Vous pouvez utiliser la méthode **send\_topic\_message** pour envoyer un message à une rubrique :</span><span class="sxs-lookup"><span data-stu-id="b9a83-125">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="b9a83-126">Vous pouvez utiliser la méthode **send\_topic\_message_batch** pour envoyer plusieurs messages en même temps :</span><span class="sxs-lookup"><span data-stu-id="b9a83-126">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="b9a83-127">N’oubliez pas que, dans Python 3, un message str présente le codage utf-8. Vous devez gérer votre codage vous-même, dans Python 2.</span><span class="sxs-lookup"><span data-stu-id="b9a83-127">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="b9a83-128">Un client peut ensuite créer un abonnement et commencer à consommer des messages en appelant la méthode **create\_subscription**, puis la méthode **receive\_subscription\_message**.</span><span class="sxs-lookup"><span data-stu-id="b9a83-128">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="b9a83-129">N’oubliez pas que les messages envoyés avant la création de l’abonnement n’arriveront pas à destination.</span><span class="sxs-lookup"><span data-stu-id="b9a83-129">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a><span data-ttu-id="b9a83-130">Event Hub</span><span class="sxs-lookup"><span data-stu-id="b9a83-130">Event Hub</span></span>

<span data-ttu-id="b9a83-131">Les hubs Event Hubs permettent la collecte de flux d’événements à débit élevé, depuis un ensemble varié d’appareils et de services.</span><span class="sxs-lookup"><span data-stu-id="b9a83-131">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="b9a83-132">La méthode **create\_event\_hub** peut permettre de créer un hub Event Hub :</span><span class="sxs-lookup"><span data-stu-id="b9a83-132">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="b9a83-133">Pour envoyer un événement, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="b9a83-133">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="b9a83-134">Le contenu de l’événement correspond au message d’événement ou à une chaîne codée au format JSON qui contient plusieurs messages.</span><span class="sxs-lookup"><span data-stu-id="b9a83-134">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

## <a name="advanced-features"></a><span data-ttu-id="b9a83-135">Fonctionnalités avancées</span><span class="sxs-lookup"><span data-stu-id="b9a83-135">Advanced features</span></span>

### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="b9a83-136">Propriétés du répartiteur et de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9a83-136">Broker Properties and User Properties</span></span>

<span data-ttu-id="b9a83-137">Cette section explique comment utiliser les propriétés du répartiteur et de l’utilisateur définies [ici](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties) :</span><span class="sxs-lookup"><span data-stu-id="b9a83-137">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                    broker_properties={'Label': 'M3'},
                    custom_properties={'Priority': 'Medium',
                                        'Customer': 'ABC'}
            )
```
<span data-ttu-id="b9a83-138">Vous pouvez utiliser les valeurs datetime, int, float ou boolean.</span><span class="sxs-lookup"><span data-stu-id="b9a83-138">You can use datetime, int, float or boolean</span></span>

```python
props = {'hello': 'world',
            'number': 42,
            'active': True,
            'deceased': False,
            'large': 8555111000,
            'floating': 3.14,
            'dob': datetime(2011, 12, 14),
            'double_quote_message': 'This "should" work fine',
            'quote_message': "This 'should' work fine"}
sent_msg = Message(b'message with properties', custom_properties=props)
```
<span data-ttu-id="b9a83-139">Pour des raisons de compatibilité avec l’ancienne version de cette bibliothèque, l’élément `broker_properties` peut également être défini comme une chaîne JSON.</span><span class="sxs-lookup"><span data-stu-id="b9a83-139">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="b9a83-140">Si tel est le cas, vous devez écrire une chaîne JSON valide. Python n’effectuera aucune vérification avant l’envoi à l’API RestAPI.</span><span class="sxs-lookup"><span data-stu-id="b9a83-140">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                    broker_properties = broker_properties
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="b9a83-141">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="b9a83-141">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/management)