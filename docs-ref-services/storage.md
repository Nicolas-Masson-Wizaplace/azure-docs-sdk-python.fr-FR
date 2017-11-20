---
title: "Bibliothèques de stockage Azure pour Python"
description: 
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, stockage"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 64465964d32934a6a45dec44cb92a0a8a84b9c37
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="c5109-103">Bibliothèques de stockage Azure pour Python</span><span class="sxs-lookup"><span data-stu-id="c5109-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="c5109-104">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c5109-104">Overview</span></span>
- <span data-ttu-id="c5109-105">Lire et écrire des objets et des fichiers à partir du [Stockage Blob Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span><span class="sxs-lookup"><span data-stu-id="c5109-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="c5109-106">Envoyer et recevoir des messages entre des applications connectées par le cloud avec le [stockage de files d’attente Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span><span class="sxs-lookup"><span data-stu-id="c5109-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="c5109-107">Lire et écrire des données structurées volumineuses avec le [stockage de tables Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span><span class="sxs-lookup"><span data-stu-id="c5109-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="c5109-108">Partager le stockage entre des applications avec [stockage de fichiers Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span><span class="sxs-lookup"><span data-stu-id="c5109-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="c5109-109">Créer, mettre à jour et gérer les requêtes et les comptes de stockage Azure et régénérer les clés d’accès à partir de votre code Python avec les bibliothèques de gestion.</span><span class="sxs-lookup"><span data-stu-id="c5109-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="c5109-110">Installer les bibliothèques</span><span class="sxs-lookup"><span data-stu-id="c5109-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="c5109-111">Client</span><span class="sxs-lookup"><span data-stu-id="c5109-111">Client</span></span>

```bash
pip install azure-storage
```

### <a name="management"></a><span data-ttu-id="c5109-112">Gestion</span><span class="sxs-lookup"><span data-stu-id="c5109-112">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="c5109-113">Exemple</span><span class="sxs-lookup"><span data-stu-id="c5109-113">Example</span></span>
```python
storage_client = CloudStorageAccount(storage_account_name, storage_key)
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(
    'mycontainername',
    public_access=PublicAccess.Blob
)

blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url('mycontainername', 'myblobname'))
```

## <a name="samples"></a><span data-ttu-id="c5109-114">Exemples</span><span class="sxs-lookup"><span data-stu-id="c5109-114">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="c5109-115">Prise en main du stockage Blob Azure dans Python</span><span class="sxs-lookup"><span data-stu-id="c5109-115">Get started with Azure Blob Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | <span data-ttu-id="c5109-116">Créer, lire, mettre à jour, limiter l’accès et supprimer des fichiers et des objets dans le stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="c5109-116">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="c5109-117">Prise en main du stockage de file d’attente Azure dans Python</span><span class="sxs-lookup"><span data-stu-id="c5109-117">Get started with Azure Queue Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | <span data-ttu-id="c5109-118">Insérez, Affichez un aperçu, récupérez et supprimez des messages à partir du stockage de files d’attente Azure.</span><span class="sxs-lookup"><span data-stu-id="c5109-118">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="c5109-119">Gérer des comptes de stockage Azure</span><span class="sxs-lookup"><span data-stu-id="c5109-119">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="c5109-120">Créer, mettre à jour et supprimer des comptes de stockage.</span><span class="sxs-lookup"><span data-stu-id="c5109-120">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="c5109-121">Récupérer et régénérer des clés d’accès de compte de stockage.</span><span class="sxs-lookup"><span data-stu-id="c5109-121">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="c5109-122">Découvrez d’autres [exemples de code Python](https://azure.microsoft.com/resources/samples/?platform=python) à utiliser dans vos applications.</span><span class="sxs-lookup"><span data-stu-id="c5109-122">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>