---
title: Bibliothèques Azure PostgreSQL pour Python
description: ''
keywords: Azure, Python, Kit de développement logiciel (SDK), API, SQL, base de données, Postgres, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: cad5995072d5040764986765d9a900f46f5141ec
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478932"
---
#<a name="azure-postgresql-libraries-for-python"></a>Bibliothèques Azure PostgreSQL pour Python

## <a name="overview"></a>Vue d'ensemble
Utilisez le pilote ODBC et pyodbc pour vous connecter à la base de données et exécuter les instructions SQL directement.

En savoir plus sur les [bases de données Azure pour PostgreSQL](https://docs.microsoft.com/azure/postgresql/).

## <a name="client-odbc-driver-and-pyodbc"></a>Pilote ODBC du client et pyodbc
Nous recommandons [pyodbc et le pilote ODBC](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft comme bibliothèque client pour accéder à la base de données Azure pour PostgreSQL

### <a name="example"></a>exemples 

Connectez-vous à la base de données Azure pour PostgreSQL et sélectionnez tous les enregistrements dans le tableau `SALES`. Il est possible d’obtenir la chaîne de connexion ODBC de la base de données à partir du portail Azure.

```python
import pyodbc

SERVER = 'YOUR_SERVER_NAME.postgres.database.azure.com'
DATABASE = 'YOUR_DB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

DRIVER = '{PostgreSQL ODBC Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=5432;SERVER=' + SERVER +
    ';PORT=5432;DATABASE=' + DATABASE + ';UID=' + USERNAME +
    ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES" # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>API de gestion
### <a name="requirements"></a>Configuration requise
Vous devez installer les bibliothèques de gestion PostgreSQL pour Python.
```bash
pip install azure-mgmt-rdbms
```

Veuillez consulter la page sur l’[l’authentification du Kit de développement logiciel (SDK) Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)pour en savoir plus sur l’obtention des informations d’identification pour l’authentification avec le client de gestion.

### <a name="example"></a>exemples
Dans cet exemple, nous allons créer une base de données Postgres sur notre serveur Postgres existant.
```python
from azure.mgtm.rdbms.postgresql import PostgreSQLManagementClient

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE_GROUP_WITH_POSTGRES"
POSTGRES_SERVER = "YOUR_POSTGRES_SERVER_NAME"
DB_NAME = "YOUR_DESIRED_DATABASE_NAME"

client = PostgreSQLManagementClient(credentials, SUBSCRIPTION_ID)

db_creation_poller = client.databases.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=POSTGRES_SERVER, database_name=DB_NAME)
db = db_creation_poller.result()
```

> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/postgresql/management)

