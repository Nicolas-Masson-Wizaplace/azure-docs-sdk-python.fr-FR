---
title: Bibliothèques Service Bus pour Python
description: Consulter la documentation sur les bibliothèques de client et de gestion Python pour Service Bus
keywords: Azure, Python, Kit de développement logiciel (SDK), API, messagerie, pubsub, pub-sub, répartiteur de messages
author: annatisch
ms.author: antisch
manager: mayurid
ms.date: 01/15/2019
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 540a2a248f7a2abcc83517bc4a4ef96ef03e8a9f
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747739"
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="fa4a5-104">Bibliothèques Service Bus pour Python</span><span class="sxs-lookup"><span data-stu-id="fa4a5-104">Service Bus libraries for Python</span></span>

<span data-ttu-id="fa4a5-105">Microsoft Azure Service Bus prend en charge un ensemble de technologies middleware Cloud orientées messages, notamment la mise en file d’attente de messages fiable et la messagerie de publication/abonnement durable.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-105">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span>

* [<span data-ttu-id="fa4a5-106">Code source du SDK</span><span class="sxs-lookup"><span data-stu-id="fa4a5-106">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="fa4a5-107">Documentation de référence du SDK</span><span class="sxs-lookup"><span data-stu-id="fa4a5-107">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a><span data-ttu-id="fa4a5-108">Nouveautés de la version 0.50.0</span><span class="sxs-lookup"><span data-stu-id="fa4a5-108">What's new in v0.50.0?</span></span>
<span data-ttu-id="fa4a5-109">La version 0.50.0 fournit une nouvelle API basée sur AMQP pour l’envoi et la réception des messages.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-109">As of version 0.50.0 a new AMQP-based API is available for sending and receiving messages.</span></span> <span data-ttu-id="fa4a5-110">Cette mise à jour introduit des **changements cassants**.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-110">This update involves **breaking changes**.</span></span>
<span data-ttu-id="fa4a5-111">Lisez la section [Migration de la version 0.21.1 vers la version 0.50.0](#migration-from-v0211-to-v0500) pour déterminer si vous avez ou non intérêt à faire la mise à niveau maintenant.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-111">Please read [Migration from v0.21.1 to v0.50.0](#migration-from-v0211-to-v0500) to determine if upgrading is right for you at this time.</span></span>

<span data-ttu-id="fa4a5-112">La nouvelle API basée sur AMQP améliore la fiabilité, les performances et la prise en charge des fonctionnalités de la transmission des messages.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-112">The new AMQP-based API offers improved message passing reliability, performance and expanded feature support going forward.</span></span>
<span data-ttu-id="fa4a5-113">Cette API permet également d’utiliser des opérations asynchrones (basées sur asyncio) pour l’envoi, la réception et le traitement des messages.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-113">The new API also offers support for asynchronous operations (based on asyncio) for sending, receiving and handling messages.</span></span>

<span data-ttu-id="fa4a5-114">Pour plus d’informations sur les opérations HTTP héritées, consultez [Utilisation d’opérations HTTP de l’API héritée](#using-http-based-operations-of-the-legacy-api).</span><span class="sxs-lookup"><span data-stu-id="fa4a5-114">For documentation on the legacy HTTP-based operations please see [Using HTTP-based operations of the legacy API](#using-http-based-operations-of-the-legacy-api).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fa4a5-115">Prérequis</span><span class="sxs-lookup"><span data-stu-id="fa4a5-115">Prerequisites</span></span>

* <span data-ttu-id="fa4a5-116">Abonnement Azure - [Créer un compte gratuit](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="fa4a5-116">Azure subscription - [Create a free account](https://azure.microsoft.com/free/)</span></span>
* <span data-ttu-id="fa4a5-117">[Informations d’identification de gestion et d’espace de noms Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span><span class="sxs-lookup"><span data-stu-id="fa4a5-117">Azure [Service Bus namespace and management credentials](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span></span>
* [<span data-ttu-id="fa4a5-118">Python 2.7-3.7</span><span class="sxs-lookup"><span data-stu-id="fa4a5-118">Python 2.7-3.7</span></span>](https://www.python.org/downloads/)


## <a name="installation"></a><span data-ttu-id="fa4a5-119">Installation</span><span class="sxs-lookup"><span data-stu-id="fa4a5-119">Installation</span></span>
```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a><span data-ttu-id="fa4a5-120">Se connecter à Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="fa4a5-120">Connect to Azure Service Bus</span></span>

### <a name="get-credentials"></a><span data-ttu-id="fa4a5-121">Récupérer les informations d’identification</span><span class="sxs-lookup"><span data-stu-id="fa4a5-121">Get credentials</span></span>
<span data-ttu-id="fa4a5-122">Utilisez l’extrait de code Azure CLI ci-dessous pour remplir une variable d’environnement avec la chaîne de connexion Service Bus (vous pouvez également trouver cette valeur dans le portail Azure).</span><span class="sxs-lookup"><span data-stu-id="fa4a5-122">Use the Azure CLI snippet below to populate an environment variable with the Service Bus connection string (you can also find this value in the Azure portal).</span></span> <span data-ttu-id="fa4a5-123">L’extrait de code est mis en forme pour l’interpréteur de commandes Bash.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-123">The snippet is formatted for the Bash shell.</span></span>

```Bash
RES_GROUP=<resource-group-name>
NAMESPACE=<servicebus-namespace>

export SB_CONN_STR=$(az servicebus namespace authorization-rule keys list \
 --resource-group $RES_GROUP \
 --namespace-name $NAMESPACE \
 --name RootManageSharedAccessKey \
 --query primaryConnectionString \
 --output tsv)
```

### <a name="create-client"></a><span data-ttu-id="fa4a5-124">Créer un client</span><span class="sxs-lookup"><span data-stu-id="fa4a5-124">Create client</span></span>
<span data-ttu-id="fa4a5-125">Une fois que vous avez rempli la variable d’environnement `SB_CONN_STR`, vous pouvez créer le ServiceBusClient.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-125">Once you've populated the `SB_CONN_STR` environment variable, you can create the ServiceBusClient.</span></span>
```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```
<span data-ttu-id="fa4a5-126">Si vous souhaitez utiliser des opérations asynchrones, utilisez l’espace de noms `azure.servicebus.aio`.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-126">If you wish to use asynchronous operations, please use the `azure.servicebus.aio` namespace.</span></span>
```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```


## <a name="service-bus-queues"></a><span data-ttu-id="fa4a5-127">Files d’attente Service Bus</span><span class="sxs-lookup"><span data-stu-id="fa4a5-127">Service Bus queues</span></span>
<span data-ttu-id="fa4a5-128">Les files d’attente Service Bus sont une alternative aux files d’attente de stockage qui peuvent être utiles dans les scénarios requérant des fonctionnalités de messagerie plus avancées (messages plus volumineux, classement des messages, lectures destructives à opération unique, remise planifiée) via une remise de type push (interrogation sur le long terme).</span><span class="sxs-lookup"><span data-stu-id="fa4a5-128">Service Bus queues are an alternative to Storage queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

### <a name="create-queue"></a><span data-ttu-id="fa4a5-129">Créer la file d’attente</span><span class="sxs-lookup"><span data-stu-id="fa4a5-129">Create queue</span></span>
<span data-ttu-id="fa4a5-130">Cette opération crée une file d’attente dans l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-130">This creates a new queue within the Service Bus namespace.</span></span> <span data-ttu-id="fa4a5-131">Une erreur se produit si une file d’attente du même nom existe déjà dans l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-131">If a queue of the same name already exists within the namespace an error will be raised.</span></span> 
```python
sb_client.create_queue("MyQueue")
```
<span data-ttu-id="fa4a5-132">Vous pouvez également définir des paramètres facultatifs pour configurer le comportement de la file d’attente.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-132">Optional parameters to configure the queue behaviour can also be specified.</span></span>
```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a><span data-ttu-id="fa4a5-133">Obtenir un client de file d'attente</span><span class="sxs-lookup"><span data-stu-id="fa4a5-133">Get a queue client</span></span>
<span data-ttu-id="fa4a5-134">Un QueueClient peut être utilisé pour la transmission des messages avec la file d’attente, ainsi que d’autres opérations.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-134">A QueueClient can be used to send and receive messages from the queue, along with other operations.</span></span>
```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a><span data-ttu-id="fa4a5-135">Envoi de messages</span><span class="sxs-lookup"><span data-stu-id="fa4a5-135">Sending messages</span></span>
<span data-ttu-id="fa4a5-136">Le client de file d’attente peut envoyer un ou plusieurs messages à la fois :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-136">The queue client can send one or more messages at a time:</span></span>
```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```
<span data-ttu-id="fa4a5-137">Chaque appel à QueueClient.send crée une connexion au service.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-137">Each call to QueueClient.send will create a new service connection.</span></span> <span data-ttu-id="fa4a5-138">Si vous souhaitez réutiliser la même connexion pour plusieurs appels d’envoi, vous pouvez ouvrir un expéditeur (sender) :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-138">To reuse the same connection for multiple send calls, you can open a sender:</span></span>
```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```
<span data-ttu-id="fa4a5-139">Si vous utilisez un client asynchrone, les opérations ci-dessus doivent respecter la syntaxe async :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-139">If you are using an asynchronous client, the above operations will use async syntax:</span></span>
```python
from azure.servicebus.aio import Message

message = Message("Hello World")
await queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
async with queue_client.get_sender() as sender:
    await sender.send(message_one)
    await sender.send(message_two)
```


### <a name="receiving-messages"></a><span data-ttu-id="fa4a5-140">Réception de messages</span><span class="sxs-lookup"><span data-stu-id="fa4a5-140">Receiving messages</span></span>
<span data-ttu-id="fa4a5-141">Les messages peuvent être reçus d’une file d’attente en tant qu’itérateur continu.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-141">Messages can be received from a queue as a continuous iterator.</span></span> <span data-ttu-id="fa4a5-142">Le mode par défaut pour la réception des messages est [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), qui nécessite que chaque message ait été traité explicitement avant d’être supprimé de la file d’attente.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-142">The default mode for message receiving is [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), which requires each message to be explicitly completed in order that it be removed from the queue.</span></span>
```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```
<span data-ttu-id="fa4a5-143">La connexion au service reste ouverte durant l’intégralité de l’itérateur.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-143">The service connection will remain open for the entirety of the iterator.</span></span>
<span data-ttu-id="fa4a5-144">Si vous voyez que le flux des messages est seulement itéré partiellement, vous devez exécuter le récepteur (receiver) dans une instruction `with` pour vous assurer que la connexion est fermée :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-144">If you find yourself only partially iterating the message stream, you should run the receiver in a `with` statement to ensure the connection is closed:</span></span>
```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
<span data-ttu-id="fa4a5-145">Si vous utilisez un client asynchrone, les opérations ci-dessus doivent respecter la syntaxe async :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-145">If you are using an asynchronous client, the above operations will use async syntax:</span></span>
```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a><span data-ttu-id="fa4a5-146">Rubriques et abonnements Service Bus</span><span class="sxs-lookup"><span data-stu-id="fa4a5-146">Service Bus topics and subscriptions</span></span>

<span data-ttu-id="fa4a5-147">Les rubriques et abonnements Service Bus constituent une couche d’abstraction au-dessus des files d’attente Service Bus afin de fournir une forme de communication un-à-plusieurs dans un modèle de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-147">Service Bus topics and subscriptions are an abstraction on top of Service Bus queues that provide a one-to-many form of communication, in a publish/subscribe pattern.</span></span> <span data-ttu-id="fa4a5-148">Les messages sont envoyés à une rubrique et sont remis à un ou plusieurs abonnements associés, ce qui facilite l’envoi à un grand nombre de destinataires.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-148">Messages are sent to a topic and delivered to one or more associated subscriptions, which is useful for scaling to large numbers of recipients.</span></span>

### <a name="create-topic"></a><span data-ttu-id="fa4a5-149">Créer une rubrique</span><span class="sxs-lookup"><span data-stu-id="fa4a5-149">Create topic</span></span>
<span data-ttu-id="fa4a5-150">Cette opération crée une rubrique dans l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-150">This creates a new topic within the Service Bus namespace.</span></span> <span data-ttu-id="fa4a5-151">Une erreur se produit si une rubrique du même nom existe déjà dans l’espace de noms.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-151">If a topic of the same name already exists an error will be raised.</span></span> 
```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a><span data-ttu-id="fa4a5-152">Obtenir un client de rubrique</span><span class="sxs-lookup"><span data-stu-id="fa4a5-152">Get a topic client</span></span>
<span data-ttu-id="fa4a5-153">Un TopicClient peut être utilisé pour l’envoi de messages à la rubrique, ainsi que d’autres opérations.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-153">A TopicClient can be used to send messages to the topic, along with other operations.</span></span>
```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a><span data-ttu-id="fa4a5-154">Créer un abonnement</span><span class="sxs-lookup"><span data-stu-id="fa4a5-154">Create subscription</span></span>
<span data-ttu-id="fa4a5-155">Cette opération crée un abonnement pour la rubrique spécifiée dans l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-155">This creates a new subscription for the specified topic within the Service Bus namespace.</span></span>
```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a><span data-ttu-id="fa4a5-156">Obtenir un client d’abonnement</span><span class="sxs-lookup"><span data-stu-id="fa4a5-156">Get a subscription client</span></span>
<span data-ttu-id="fa4a5-157">Un SubscriptionClient peut être utilisé pour la réception de messages de la rubrique, ainsi que d’autres opérations.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-157">A SubscriptionClient can be used to receive messages from the topic, along with other operations.</span></span>
```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a><span data-ttu-id="fa4a5-158">Migration de la version 0.21.1 vers la version 0.50.0</span><span class="sxs-lookup"><span data-stu-id="fa4a5-158">Migration from v0.21.1 to v0.50.0</span></span>
<span data-ttu-id="fa4a5-159">Des changements cassants ont été introduits dans la version 0.50.0.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-159">Major breaking changes were introduced in version 0.50.0.</span></span>
<span data-ttu-id="fa4a5-160">L’API HTTP initiale est toujours disponible dans la version 0.50.0, mais elle est maintenant implémentée dans le nouvel espace de noms `azure.servicebus.control_client`.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-160">The original HTTP-based API is still available in v0.50.0 - however it now exists under a new namesapce: `azure.servicebus.control_client`.</span></span>

### <a name="should-i-upgrade"></a><span data-ttu-id="fa4a5-161">Dois-je effectuer la mise à niveau ?</span><span class="sxs-lookup"><span data-stu-id="fa4a5-161">Should I upgrade?</span></span>
<span data-ttu-id="fa4a5-162">Le nouveau package (version 0.50.0) n’apporte pas d’améliorations pour les opérations HTTP par rapport à la version 0.21.1.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-162">The new package (v0.50.0) offers no improvements in HTTP-based operations over v0.21.1.</span></span> <span data-ttu-id="fa4a5-163">L’API HTTP est identique, à ceci près qu’elle est maintenant implémentée dans un nouvel espace de noms.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-163">The HTTP-based API is identical except that it now exists under a new namespace.</span></span> <span data-ttu-id="fa4a5-164">Pour cette raison, si vous souhaitez uniquement utiliser des opérations HTTP (`create_queue`, `delete_queue`, etc.), il n’y a pas d’intérêt particulier à faire cette mise à niveau.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-164">For this reason if you only wish to use HTTP-based operations (`create_queue`, `delete_queue` etc) - there will be no additional benefit in upgrading at this time.</span></span>


### <a name="how-do-i-migrate-my-code-to-the-new-version"></a><span data-ttu-id="fa4a5-165">Comment migrer mon code vers la nouvelle version ?</span><span class="sxs-lookup"><span data-stu-id="fa4a5-165">How do I migrate my code to the new version?</span></span>
<span data-ttu-id="fa4a5-166">Le code écrit pour la version 0.21.0 peut être migré vers la version 0.50.0 simplement en changeant l’espace de noms import :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-166">Code written against v0.21.0 can be ported to version 0.50.0 by simply changing the import namespace:</span></span>

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a><span data-ttu-id="fa4a5-167">Utilisation d’opérations HTTP de l’API héritée</span><span class="sxs-lookup"><span data-stu-id="fa4a5-167">Using HTTP-based operations of the legacy API</span></span>
<span data-ttu-id="fa4a5-168">Les sections suivantes décrivent l’API héritée et sont destinées à ceux qui souhaitent migrer du code existant vers la version 0.50.0 sans apporter d’autres changements.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-168">The following documentation describes the legacy API and should be used for those wishing to port existing code to v0.50.0 without making any additional changes.</span></span> <span data-ttu-id="fa4a5-169">Ces informations de référence peuvent également apporter des conseils utiles aux utilisateurs de la version 0.21.1.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-169">This reference can also be used as guidance by those using v0.21.1.</span></span>
<span data-ttu-id="fa4a5-170">Nous recommandons à ceux qui écrivent du code nouveau d’utiliser la nouvelle API décrite ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-170">For those writing new code, we recommend using the new API described above.</span></span>

### <a name="service-bus-queues"></a><span data-ttu-id="fa4a5-171">Files d’attente Service Bus</span><span class="sxs-lookup"><span data-stu-id="fa4a5-171">Service Bus queues</span></span>

#### <a name="shared-access-signature-sas-authentication"></a><span data-ttu-id="fa4a5-172">Authentification SAS (par signature d’accès partagé)</span><span class="sxs-lookup"><span data-stu-id="fa4a5-172">Shared Access Signature (SAS) authentication</span></span>

<span data-ttu-id="fa4a5-173">Pour utiliser l’authentification par signature d’accès partagé, créez le service Service Bus avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-173">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a><span data-ttu-id="fa4a5-174">Authentification ACS (Access Control Service)</span><span class="sxs-lookup"><span data-stu-id="fa4a5-174">Access Control Service (ACS) authentication</span></span>
<span data-ttu-id="fa4a5-175">ACS n’est plus pris en charge dans les nouveaux espaces de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-175">ACS is no longer supported on new Service Bus namespaces.</span></span> <span data-ttu-id="fa4a5-176">Nous vous recommandons de [migrer vos applications vers l’authentification SAS](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span><span class="sxs-lookup"><span data-stu-id="fa4a5-176">We recommend [migrating applications to SAS authentication](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span></span>
<span data-ttu-id="fa4a5-177">Pour utiliser l’authentification ACS dans un espace de noms Service Bus d’une version antérieure, créez ServiceBusService comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-177">To use ACS authentication within an older Service Bus namesapce, create the ServiceBusService with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
#### <a name="sending-and-receiving-messages"></a><span data-ttu-id="fa4a5-178">Envoi et réception des messages</span><span class="sxs-lookup"><span data-stu-id="fa4a5-178">Sending and receiving messages</span></span>

<span data-ttu-id="fa4a5-179">La méthode **create\_queue** peut être utilisée pour garantir l’existence d’une file d’attente :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-179">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="fa4a5-180">Vous pouvez ensuite appeler la méthode **send\_queue\_message** pour insérer le message dans la file d’attente :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-180">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="fa4a5-181">La méthode **send\_queue\_message_batch** peut ensuite être appelée pour envoyer plusieurs messages en même temps :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-181">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="fa4a5-182">Il est ensuite possible d’appeler la méthode **receive\_queue\_message** pour retirer le message de la file d’attente.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-182">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a><span data-ttu-id="fa4a5-183">Rubriques Service Bus</span><span class="sxs-lookup"><span data-stu-id="fa4a5-183">Service Bus topics</span></span>

<span data-ttu-id="fa4a5-184">La méthode **create\_topic** peut ensuite servir à créer une rubrique côté serveur :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-184">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="fa4a5-185">Vous pouvez utiliser la méthode **send\_topic\_message** pour envoyer un message à une rubrique :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-185">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="fa4a5-186">Vous pouvez utiliser la méthode **send\_topic\_message_batch** pour envoyer plusieurs messages en même temps :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-186">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="fa4a5-187">N’oubliez pas que, dans Python 3, un message str présente le codage utf-8. Vous devez gérer votre codage vous-même, dans Python 2.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-187">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="fa4a5-188">Un client peut ensuite créer un abonnement et commencer à consommer des messages en appelant la méthode **create\_subscription**, puis la méthode **receive\_subscription\_message**.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-188">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="fa4a5-189">N’oubliez pas que les messages envoyés avant la création de l’abonnement n’arriveront pas à destination.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-189">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a><span data-ttu-id="fa4a5-190">Event Hub</span><span class="sxs-lookup"><span data-stu-id="fa4a5-190">Event Hub</span></span>

<span data-ttu-id="fa4a5-191">Les hubs Event Hubs permettent la collecte de flux d’événements à débit élevé, depuis un ensemble varié d’appareils et de services.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-191">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="fa4a5-192">La méthode **create\_event\_hub** peut permettre de créer un hub Event Hub :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-192">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="fa4a5-193">Pour envoyer un événement, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-193">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="fa4a5-194">Le contenu de l’événement correspond au message d’événement ou à une chaîne codée au format JSON qui contient plusieurs messages.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-194">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

### <a name="advanced-features"></a><span data-ttu-id="fa4a5-195">Fonctionnalités avancées</span><span class="sxs-lookup"><span data-stu-id="fa4a5-195">Advanced features</span></span>

#### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="fa4a5-196">Propriétés du répartiteur et de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="fa4a5-196">Broker properties and user properties</span></span>

<span data-ttu-id="fa4a5-197">Cette section explique comment utiliser les propriétés du répartiteur et de l’utilisateur définies [ici](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties) :</span><span class="sxs-lookup"><span data-stu-id="fa4a5-197">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```
<span data-ttu-id="fa4a5-198">Vous pouvez utiliser les valeurs datetime, int, float ou boolean.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-198">You can use datetime, int, float or boolean</span></span>

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
<span data-ttu-id="fa4a5-199">Pour des raisons de compatibilité avec l’ancienne version de cette bibliothèque, l’élément `broker_properties` peut également être défini comme une chaîne JSON.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-199">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="fa4a5-200">Si tel est le cas, vous devez écrire une chaîne JSON valide. Python n’effectuera aucune vérification avant l’envoi à l’API RestAPI.</span><span class="sxs-lookup"><span data-stu-id="fa4a5-200">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a><span data-ttu-id="fa4a5-201">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fa4a5-201">Next Steps</span></span>
* [<span data-ttu-id="fa4a5-202">documentation de Service Bus</span><span class="sxs-lookup"><span data-stu-id="fa4a5-202">Service Bus documentation</span></span>](https://docs.microsoft.com/azure/service-bus-messaging)
* [<span data-ttu-id="fa4a5-203">Code source du SDK</span><span class="sxs-lookup"><span data-stu-id="fa4a5-203">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="fa4a5-204">Documentation de référence du SDK</span><span class="sxs-lookup"><span data-stu-id="fa4a5-204">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [<span data-ttu-id="fa4a5-205">Exemples supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fa4a5-205">Additional samples</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
