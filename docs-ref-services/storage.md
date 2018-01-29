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
ms.openlocfilehash: e00e821ff3e806a994fa8d96aae50c35eeeb8392
ms.sourcegitcommit: 5ab15a7214082d16f339a13e4ae7735b3a57a9aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="azure-storage-libraries-for-python"></a>Bibliothèques de stockage Azure pour Python

## <a name="overview"></a>Vue d’ensemble
- Lire et écrire des objets et des fichiers à partir du [Stockage Blob Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)
- Envoyer et recevoir des messages entre des applications connectées par le cloud avec le [stockage de files d’attente Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)
- Lire et écrire des données structurées volumineuses avec le [stockage de tables Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) 
- Partager le stockage entre des applications avec [stockage de fichiers Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)

Créer, mettre à jour et gérer les requêtes et les comptes de stockage Azure et régénérer les clés d’accès à partir de votre code Python avec les bibliothèques de gestion.

## <a name="install-the-libraries"></a>Installer les bibliothèques

### <a name="client"></a>Client

Les bibliothèques clientes de stockage Azure sont constituées de 4 packages : objet blob, fichier, file d’attente et Table. Pour installer le package de l’objet blob, exécutez :

```bash
pip install azure-storage-blob
```

### <a name="management"></a>gestion

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>exemples
```python
blob_service = BlockBlobService(account_name, account_key)

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
| [Prise en main du stockage Blob Azure dans Python](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | Créer, lire, mettre à jour, limiter l’accès et supprimer des fichiers et des objets dans le stockage Azure. |
| [Prise en main du stockage de file d’attente Azure dans Python](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | Insérez, Affichez un aperçu, récupérez et supprimez des messages à partir du stockage de files d’attente Azure. | 
| [Gérer des comptes de stockage Azure](https://azure.microsoft.com/resources/samples/storage-python-manage) | Créer, mettre à jour et supprimer des comptes de stockage. Récupérer et régénérer des clés d’accès de compte de stockage.

Découvrez d’autres [exemples de code Python](https://azure.microsoft.com/resources/samples/?platform=python) à utiliser dans vos applications.
