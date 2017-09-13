---
title: "Bibliothèques Service Bus pour Python"
description: "Consulter la documentation sur les bibliothèques de client et de gestion Python pour Service Bus"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, messagerie, pubsub, pub-sub, répartiteur de messages"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: bf7be945f2c7a3daea93ff4e5b770459c00632c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="cfcea-104">Bibliothèques Service Bus pour Python</span><span class="sxs-lookup"><span data-stu-id="cfcea-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="cfcea-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="cfcea-105">Overview</span></span>

<span data-ttu-id="cfcea-106">Microsoft Azure Service Bus prend en charge un ensemble de technologies middleware Cloud orientées messages, notamment la mise en file d’attente de messages fiable et la messagerie de publication/abonnement durable.</span><span class="sxs-lookup"><span data-stu-id="cfcea-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="cfcea-107">Installer les bibliothèques</span><span class="sxs-lookup"><span data-stu-id="cfcea-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a><span data-ttu-id="cfcea-108">Exemple</span><span class="sxs-lookup"><span data-stu-id="cfcea-108">Example</span></span>
<span data-ttu-id="cfcea-109">Envoyez des messages à une file d’attente.</span><span class="sxs-lookup"><span data-stu-id="cfcea-109">Send messages to a queue.</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="cfcea-110">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="cfcea-110">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/managementlibrary)

