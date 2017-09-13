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
# <a name="azure-storage-libraries-for-python"></a>Bibliothèques de stockage Azure pour Python

## <a name="overview"></a>Vue d'ensemble
- Lire et écrire des objets et des fichiers à partir du [Stockage Blob Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)
- Envoyer et recevoir des messages entre des applications connectées par le cloud avec le [stockage de files d’attente Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)
- Lire et écrire des données structurées volumineuses avec le [stockage de tables Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) 
- Partager le stockage entre des applications avec [stockage de fichiers Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)

Créer, mettre à jour et gérer les requêtes et les comptes de stockage Azure et régénérer les clés d’accès à partir de votre code Python avec les bibliothèques de gestion.

## <a name="install-the-libraries"></a>Installer les bibliothèques

### <a name="client"></a>Client

```bash
pip install azure-storage
```

### <a name="management"></a>Gestion

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>Exemple
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

## <a name="samples"></a>Exemples

| | |
|--|--|
| [Prise en main du stockage Blob Azure dans Python](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | Créer, lire, mettre à jour, limiter l’accès et supprimer des fichiers et des objets dans le stockage Azure. |
| [Prise en main du stockage de file d’attente Azure dans Python](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | Insérez, Affichez un aperçu, récupérez et supprimez des messages à partir du stockage de files d’attente Azure. | 
| [Gérer des comptes de stockage Azure](https://azure.microsoft.com/resources/samples/storage-python-manage) | Créer, mettre à jour et supprimer des comptes de stockage. Récupérer et régénérer des clés d’accès de compte de stockage.

Découvrez d’autres [exemples de code Python](https://azure.microsoft.com/resources/samples/?platform=python) à utiliser dans vos applications.