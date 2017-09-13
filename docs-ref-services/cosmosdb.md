---
title: "Bibliothèques Azure CosmosDB pour Python"
description: "Consulter la documentation sur les bibliothèques de client Python pour CosmosDB"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, SQL, base de données, Postgres, CosmosDB, NoSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/11/2017
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 5382779d659f7c85e5ecbd304920e00b78a08a49
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmosdb-libraries-for-python"></a>Bibliothèques Azure CosmosDB pour Python

## <a name="overview"></a>Vue d'ensemble

Utilisez CosmosDB dans vos applications Python pour stocker et interroger des documents JSON dans un stockage de données NoSQL.

En savoir plus sur [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction).

## <a name="client-library"></a>Bibliothèque cliente
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a>Bibliothèque de gestion
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a>Exemple

Trouver des documents correspondants dans CosmosDB à l’aide d’une interface de requête de type SQL :

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python DocumentDB client
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
> [Explorer les API de gestion](/python/api/overview/azure/cosmosdb/managementlibrary)

## <a name="samples"></a>Exemples

[Développer une application Python en utilisant l’API DocumentDB d’Azure Cosmos DB](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


