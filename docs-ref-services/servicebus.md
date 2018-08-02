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
ms.openlocfilehash: 02c172ff1a54d060c6af36a5a5daa5dcbff8795c
ms.sourcegitcommit: e35ec475d4b9d8061d0528a93aa8e1c4b7db6c0d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39418954"
---
# <a name="service-bus-libraries-for-python"></a>Bibliothèques Service Bus pour Python

## <a name="overview"></a>Vue d’ensemble

Microsoft Azure Service Bus prend en charge un ensemble de technologies middleware Cloud orientées messages, notamment la mise en file d’attente de messages fiable et la messagerie de publication/abonnement durable. 

## <a name="install-the-libraries"></a>Installer les bibliothèques
```bash
pip install azure-servicebus
```

## <a name="servicebus-queues"></a>Files d’attente ServiceBus
Les files d’attente ServiceBus sont une alternative aux files d’attente de stockage qui peuvent être utiles dans les scénarios requérant des fonctionnalités de messagerie plus avancées (messages plus volumineux, classement des messages, lectures destructives à opération unique, fourniture planifiée) via une fourniture de style push (interrogation sur le long terme).

Le service peut utiliser l’authentification de signature d’accès partagé ou l’authentification ACS.

Les espaces de noms Service Bus créés via le portail Azure après le mois d’août 2014 ne prennent plus en charge l’authentification ACS. Vous pouvez créer des espaces de noms compatibles avec ACS grâce au Kit de développement logiciel (SDK) Microsoft Azure.

### <a name="shared-access-signature-authentication"></a>Authentification avec une signature d’accès partagé

Pour utiliser l’authentification par signature d’accès partagé, créez le service Service Bus avec les éléments suivants :

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a>Authentification ACS

Pour utiliser l’authentification ACS, créez le service Service Bus avec les éléments suivants :

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a>Envoi et réception de messages

La méthode **create\_queue** peut être utilisée pour garantir l’existence d’une file d’attente :

```python
sbs.create_queue('taskqueue')
```
Vous pouvez ensuite appeler la méthode **send\_queue\_message** pour insérer le message dans la file d’attente :

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
La méthode **send\_queue\_message_batch** peut ensuite être appelée pour envoyer plusieurs messages en même temps :

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
Il est ensuite possible d’appeler la méthode **receive\_queue\_message** pour retirer le message de la file d’attente.

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a>Rubriques ServiceBus

Les rubriques ServiceBus sont une abstraction placée sur les files d’attente ServiceBus, qui facilite l’implémentation de scénarios de publication/d’abonnement.

La méthode **create\_topic** peut ensuite servir à créer une rubrique côté serveur :

```python
sbs.create_topic('taskdiscussion')
```
Vous pouvez utiliser la méthode **send\_topic\_message** pour envoyer un message à une rubrique :

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

Vous pouvez utiliser la méthode **send\_topic\_message_batch** pour envoyer plusieurs messages en même temps :

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

N’oubliez pas que, dans Python 3, un message str présente le codage utf-8. Vous devez gérer votre codage vous-même, dans Python 2.

Un client peut ensuite créer un abonnement et commencer à consommer des messages en appelant la méthode **create\_subscription**, puis la méthode **receive\_subscription\_message**. N’oubliez pas que les messages envoyés avant la création de l’abonnement n’arriveront pas à destination.

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a>Event Hub

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

## <a name="advanced-features"></a>Fonctionnalités avancées

### <a name="broker-properties-and-user-properties"></a>Propriétés du répartiteur et de l’utilisateur

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

> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/servicebus/management)
