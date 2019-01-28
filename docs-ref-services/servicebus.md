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
# <a name="service-bus-libraries-for-python"></a>Bibliothèques Service Bus pour Python

Microsoft Azure Service Bus prend en charge un ensemble de technologies middleware Cloud orientées messages, notamment la mise en file d’attente de messages fiable et la messagerie de publication/abonnement durable.

* [Code source du SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [Documentation de référence du SDK](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a>Nouveautés de la version 0.50.0
La version 0.50.0 fournit une nouvelle API basée sur AMQP pour l’envoi et la réception des messages. Cette mise à jour introduit des **changements cassants**.
Lisez la section [Migration de la version 0.21.1 vers la version 0.50.0](#migration-from-v0211-to-v0500) pour déterminer si vous avez ou non intérêt à faire la mise à niveau maintenant.

La nouvelle API basée sur AMQP améliore la fiabilité, les performances et la prise en charge des fonctionnalités de la transmission des messages.
Cette API permet également d’utiliser des opérations asynchrones (basées sur asyncio) pour l’envoi, la réception et le traitement des messages.

Pour plus d’informations sur les opérations HTTP héritées, consultez [Utilisation d’opérations HTTP de l’API héritée](#using-http-based-operations-of-the-legacy-api).


## <a name="prerequisites"></a>Prérequis

* Abonnement Azure - [Créer un compte gratuit](https://azure.microsoft.com/free/)
* [Informations d’identification de gestion et d’espace de noms Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)
* [Python 2.7-3.7](https://www.python.org/downloads/)


## <a name="installation"></a>Installation
```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a>Se connecter à Azure Service Bus

### <a name="get-credentials"></a>Récupérer les informations d’identification
Utilisez l’extrait de code Azure CLI ci-dessous pour remplir une variable d’environnement avec la chaîne de connexion Service Bus (vous pouvez également trouver cette valeur dans le portail Azure). L’extrait de code est mis en forme pour l’interpréteur de commandes Bash.

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

### <a name="create-client"></a>Créer un client
Une fois que vous avez rempli la variable d’environnement `SB_CONN_STR`, vous pouvez créer le ServiceBusClient.
```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```
Si vous souhaitez utiliser des opérations asynchrones, utilisez l’espace de noms `azure.servicebus.aio`.
```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```


## <a name="service-bus-queues"></a>Files d’attente Service Bus
Les files d’attente Service Bus sont une alternative aux files d’attente de stockage qui peuvent être utiles dans les scénarios requérant des fonctionnalités de messagerie plus avancées (messages plus volumineux, classement des messages, lectures destructives à opération unique, remise planifiée) via une remise de type push (interrogation sur le long terme).

### <a name="create-queue"></a>Créer la file d’attente
Cette opération crée une file d’attente dans l’espace de noms Service Bus. Une erreur se produit si une file d’attente du même nom existe déjà dans l’espace de noms. 
```python
sb_client.create_queue("MyQueue")
```
Vous pouvez également définir des paramètres facultatifs pour configurer le comportement de la file d’attente.
```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a>Obtenir un client de file d'attente
Un QueueClient peut être utilisé pour la transmission des messages avec la file d’attente, ainsi que d’autres opérations.
```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a>Envoi de messages
Le client de file d’attente peut envoyer un ou plusieurs messages à la fois :
```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```
Chaque appel à QueueClient.send crée une connexion au service. Si vous souhaitez réutiliser la même connexion pour plusieurs appels d’envoi, vous pouvez ouvrir un expéditeur (sender) :
```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```
Si vous utilisez un client asynchrone, les opérations ci-dessus doivent respecter la syntaxe async :
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


### <a name="receiving-messages"></a>Réception de messages
Les messages peuvent être reçus d’une file d’attente en tant qu’itérateur continu. Le mode par défaut pour la réception des messages est [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), qui nécessite que chaque message ait été traité explicitement avant d’être supprimé de la file d’attente.
```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```
La connexion au service reste ouverte durant l’intégralité de l’itérateur.
Si vous voyez que le flux des messages est seulement itéré partiellement, vous devez exécuter le récepteur (receiver) dans une instruction `with` pour vous assurer que la connexion est fermée :
```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
Si vous utilisez un client asynchrone, les opérations ci-dessus doivent respecter la syntaxe async :
```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a>Rubriques et abonnements Service Bus

Les rubriques et abonnements Service Bus constituent une couche d’abstraction au-dessus des files d’attente Service Bus afin de fournir une forme de communication un-à-plusieurs dans un modèle de publication/abonnement. Les messages sont envoyés à une rubrique et sont remis à un ou plusieurs abonnements associés, ce qui facilite l’envoi à un grand nombre de destinataires.

### <a name="create-topic"></a>Créer une rubrique
Cette opération crée une rubrique dans l’espace de noms Service Bus. Une erreur se produit si une rubrique du même nom existe déjà dans l’espace de noms. 
```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a>Obtenir un client de rubrique
Un TopicClient peut être utilisé pour l’envoi de messages à la rubrique, ainsi que d’autres opérations.
```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a>Créer un abonnement
Cette opération crée un abonnement pour la rubrique spécifiée dans l’espace de noms Service Bus.
```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a>Obtenir un client d’abonnement
Un SubscriptionClient peut être utilisé pour la réception de messages de la rubrique, ainsi que d’autres opérations.
```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a>Migration de la version 0.21.1 vers la version 0.50.0
Des changements cassants ont été introduits dans la version 0.50.0.
L’API HTTP initiale est toujours disponible dans la version 0.50.0, mais elle est maintenant implémentée dans le nouvel espace de noms `azure.servicebus.control_client`.

### <a name="should-i-upgrade"></a>Dois-je effectuer la mise à niveau ?
Le nouveau package (version 0.50.0) n’apporte pas d’améliorations pour les opérations HTTP par rapport à la version 0.21.1. L’API HTTP est identique, à ceci près qu’elle est maintenant implémentée dans un nouvel espace de noms. Pour cette raison, si vous souhaitez uniquement utiliser des opérations HTTP (`create_queue`, `delete_queue`, etc.), il n’y a pas d’intérêt particulier à faire cette mise à niveau.


### <a name="how-do-i-migrate-my-code-to-the-new-version"></a>Comment migrer mon code vers la nouvelle version ?
Le code écrit pour la version 0.21.0 peut être migré vers la version 0.50.0 simplement en changeant l’espace de noms import :

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a>Utilisation d’opérations HTTP de l’API héritée
Les sections suivantes décrivent l’API héritée et sont destinées à ceux qui souhaitent migrer du code existant vers la version 0.50.0 sans apporter d’autres changements. Ces informations de référence peuvent également apporter des conseils utiles aux utilisateurs de la version 0.21.1.
Nous recommandons à ceux qui écrivent du code nouveau d’utiliser la nouvelle API décrite ci-dessus.

### <a name="service-bus-queues"></a>Files d’attente Service Bus

#### <a name="shared-access-signature-sas-authentication"></a>Authentification SAS (par signature d’accès partagé)

Pour utiliser l’authentification par signature d’accès partagé, créez le service Service Bus avec les éléments suivants :

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a>Authentification ACS (Access Control Service)
ACS n’est plus pris en charge dans les nouveaux espaces de noms Service Bus. Nous vous recommandons de [migrer vos applications vers l’authentification SAS](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).
Pour utiliser l’authentification ACS dans un espace de noms Service Bus d’une version antérieure, créez ServiceBusService comme ci-dessous :

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
#### <a name="sending-and-receiving-messages"></a>Envoi et réception des messages

La méthode **create\_queue** peut être utilisée pour garantir l’existence d’une file d’attente :

```python
sbs.create_queue('taskqueue')
```
Vous pouvez ensuite appeler la méthode **send\_queue\_message** pour insérer le message dans la file d’attente :

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
La méthode **send\_queue\_message_batch** peut ensuite être appelée pour envoyer plusieurs messages en même temps :

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
Il est ensuite possible d’appeler la méthode **receive\_queue\_message** pour retirer le message de la file d’attente.

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a>Rubriques Service Bus

La méthode **create\_topic** peut ensuite servir à créer une rubrique côté serveur :

```python
sbs.create_topic('taskdiscussion')
```
Vous pouvez utiliser la méthode **send\_topic\_message** pour envoyer un message à une rubrique :

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

Vous pouvez utiliser la méthode **send\_topic\_message_batch** pour envoyer plusieurs messages en même temps :

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

N’oubliez pas que, dans Python 3, un message str présente le codage utf-8. Vous devez gérer votre codage vous-même, dans Python 2.

Un client peut ensuite créer un abonnement et commencer à consommer des messages en appelant la méthode **create\_subscription**, puis la méthode **receive\_subscription\_message**. N’oubliez pas que les messages envoyés avant la création de l’abonnement n’arriveront pas à destination.

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a>Event Hub

Les hubs Event Hubs permettent la collecte de flux d’événements à débit élevé, depuis un ensemble varié d’appareils et de services.

La méthode **create\_event\_hub** peut permettre de créer un hub Event Hub :

```python
sbs.create_event_hub('myhub')
```
Pour envoyer un événement, procédez comme suit :

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
Le contenu de l’événement correspond au message d’événement ou à une chaîne codée au format JSON qui contient plusieurs messages.

### <a name="advanced-features"></a>Fonctionnalités avancées

#### <a name="broker-properties-and-user-properties"></a>Propriétés du répartiteur et de l’utilisateur

Cette section explique comment utiliser les propriétés du répartiteur et de l’utilisateur définies [ici](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties) :

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```
Vous pouvez utiliser les valeurs datetime, int, float ou boolean.

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
Pour des raisons de compatibilité avec l’ancienne version de cette bibliothèque, l’élément `broker_properties` peut également être défini comme une chaîne JSON.
Si tel est le cas, vous devez écrire une chaîne JSON valide. Python n’effectuera aucune vérification avant l’envoi à l’API RestAPI.

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a>Étapes suivantes
* [documentation de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging)
* [Code source du SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [Documentation de référence du SDK](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [Exemples supplémentaires](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
