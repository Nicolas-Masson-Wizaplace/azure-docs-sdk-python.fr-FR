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
ms.openlocfilehash: d56dd69f4fc4513034046f9f721608ad94ff5cfe
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="azure-cosmosdb-libraries-for-python"></a><span data-ttu-id="3356c-104">Bibliothèques Azure CosmosDB pour Python</span><span class="sxs-lookup"><span data-stu-id="3356c-104">Azure CosmosDB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="3356c-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="3356c-105">Overview</span></span>

<span data-ttu-id="3356c-106">Utilisez CosmosDB dans vos applications Python pour stocker et interroger des documents JSON dans un stockage de données NoSQL.</span><span class="sxs-lookup"><span data-stu-id="3356c-106">Use CosmosDB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="3356c-107">En savoir plus sur [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="3356c-107">Learn more about [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="3356c-108">Bibliothèque cliente</span><span class="sxs-lookup"><span data-stu-id="3356c-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="3356c-109">Bibliothèque de gestion</span><span class="sxs-lookup"><span data-stu-id="3356c-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="3356c-110">exemples</span><span class="sxs-lookup"><span data-stu-id="3356c-110">Example</span></span>

<span data-ttu-id="3356c-111">Trouver des documents correspondants dans CosmosDB à l’aide d’une interface de requête de type SQL :</span><span class="sxs-lookup"><span data-stu-id="3356c-111">Find matching documents in CosmosDB using a SQL-like query interface:</span></span>

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
> [<span data-ttu-id="3356c-112">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="3356c-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/management)

## <a name="samples"></a><span data-ttu-id="3356c-113">Exemples</span><span class="sxs-lookup"><span data-stu-id="3356c-113">Samples</span></span>

[<span data-ttu-id="3356c-114">Développer une application Python en utilisant l’API DocumentDB d’Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3356c-114">Develop a Python app using Azure Cosmos DB's DocumentDB API</span></span>](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)


