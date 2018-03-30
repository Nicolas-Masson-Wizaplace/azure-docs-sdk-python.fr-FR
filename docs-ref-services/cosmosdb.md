---
title: Bibliothèques Azure Cosmos DB pour Python
description: Consulter la documentation sur les bibliothèques de client Python pour Azure Cosmos DB
keywords: Azure, Python, Kit de développement logiciel (SDK), API, SQL, base de données, Postgres, Cosmos DB, NoSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 03/20/2018
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 391b556ece7d818406fa501763814eb7f0d50d22
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-libraries-for-python"></a>Bibliothèques Azure Cosmos DB pour Python

## <a name="overview"></a>Vue d'ensemble

Utilisez Azure Cosmos DB dans vos applications Python pour stocker et interroger des documents JSON dans un magasin de données NoSQL.

Apprenez-en davantage sur [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).

## <a name="client-library"></a>Bibliothèque cliente
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>Bibliothèque de gestion
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>Exemples

Trouver des documents correspondants dans Azure Cosmos DB à l’aide d’une interface de requête de type SQL :

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python Azure Cosmos DB client
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
# Create a database
db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })

# Create collection options
options = {
    'offerEnableRUPerMinuteThroughput': True,
    'offerVersion': "V2",
    'offerThroughput': 400
}

# Create a collection
collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)

# Create some documents
document1 = client.CreateDocument(collection['_self'],
    { 
        'id': 'server1',
        'Web Site': 0,
        'Cloud Service': 0,
        'Virtual Machine': 0,
        'name': 'some' 
    })

# Query them in SQL
query = { 'query': 'SELECT * FROM server s' }    

options = {} 
options['enableCrossPartitionQuery'] = True
options['maxItemCount'] = 2

result_iterable = client.QueryDocuments(collection['_self'], query, options)
results = list(result_iterable)

print(results)
```
> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a>Exemples

[Développer une application Python à l’aide d’Azure Cosmos DB](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


