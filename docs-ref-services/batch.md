---
title: "Bibliothèques Azure Batch pour Python"
description: "Références sur les bibliothèques Batch pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, Batch, traitement, planification, longue durée"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: f954499888cbc3dfe4793a3e769b85ceb5de71d2
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-batch-libraries-for-python"></a>Bibliothèques Azure Batch pour Python

## <a name="overview"></a>Vue d'ensemble

Exécutez efficacement des applications de calcul haute performance en parallèle et à grande échelle dans le cloud avec [Azure Batch](/azure/batch/batch-technical-overview).   

Pour découvrir Azure Batch, consultez [Créer un compte Batch avec le portail Azure](/azure/batch/batch-account-create-portal).

## <a name="install-the-libraries"></a>Installer les bibliothèques

## <a name="client-library"></a>Bibliothèque cliente
Les bibliothèques de client Azure Batch vous permettent de configurer des nœuds et des pools de calcul, de définir des tâches et de les configurer pour s’exécuter dans les travaux. Vous pouvez configurer un gestionnaire de travaux pour contrôler et surveiller l’exécution du travail. [En savoir plus](/azure/batch/batch-api-basics) sur l’utilisation de ces objets pour exécuter des solutions de calcul parallèles à grande échelle.

```bash
pip install azure-batch
```
### <a name="example"></a>Exemple

Configurez un pool de nœuds de calcul Linux dans un compte Batch :

```python
# create the batch client for an account using its URI and keys
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

## <a name="management-api"></a>API de gestion
Utilisez les bibliothèques de gestion Azure Batch pour créer et supprimer des comptes Batch, lire et régénérer les clés de compte Batch, et gérer le stockage de comptes Batch.

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [Explorer les API clientes](/python/api/overview/azure/batch/clientlibrary)

### <a name="example"></a>Exemple
Créez un compte Azure Batch, puis configurez pour ce compte une nouvelle application et un compte de stockage Azure.

```python
from azure.mgmt.batch import BatchManagementClient
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

LOCATION ='eastus'
GROUP_NAME ='batchresourcegroup'
STORAGE_ACCOUNT_NAME ='batchstorageaccount'

# Create Resource group
print('Create Resource Group')
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})

# Create a storage account
storage_async_operation = storage_client.storage_accounts.create(
    GROUP_NAME,
    STORAGE_ACCOUNT_NAME,
    StorageAccountCreateParameters(
        sku=Sku(SkuName.standard_ragrs),
        kind=Kind.storage,
        location=LOCATION
    )
)
storage_account = storage_async_operation.result()

# Create a Batch Account, specifying the storage account we want to link
storage_resource = storage_account.id
batch_account = azure.mgmt.batch.models.BatchAccountCreateParameters(
    location=LOCATION,
    auto_storage=azure.mgmt.batch.models.AutoStorageBaseProperties(storage_resource)
)
creating = batch_client.account.create('MyBatchAccount', LOCATION, batch_account)
creating.wait()
```

> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/batch/managementlibrary)