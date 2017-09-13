---
title: "Bibliothèques Azure SQL Database pour Python"
description: 
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, SQL, base de données, pyodbc"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: b580c5011412bc77fd8fd55b709a305be07e2316
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-sql-database-libraries-for-python"></a>Bibliothèques Azure SQL Database pour Python

## <a name="overview"></a>Vue d'ensemble

Utilisez les données stockées dans [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) à partir de Python avec le pilote ODBC de Microsoft et pyodbc. 

## <a name="client-odbc-driver-and-pyodbc"></a>Pilote ODBC du client et pyodbc

```bash
pip install pyodbc
```
Pour en savoir plus sur l’installation des bibliothèques de communication Python et SQL Database, rendez-vous à [cette adresse](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).

### <a name="example"></a>Exemple

Connectez-vous à la base de données SQL et sélectionnez tous les enregistrements dans une table.

```python
import pyodbc 

SERVER = 'YOUR_SERVER_NAME.database.windows.net'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_DB_USERNAME'
PASSWORD = 'YOUR_DB_PASSWORD'

DRIVER= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER=' + DRIVER + ';PORT=1433;SERVER=' + SERVER +
    ';PORT=1443;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>API de gestion

Créez et gérez des ressources Azure SQL Database dans votre abonnement avec l’API de gestion. 

```bash
pip install azure-mgmt-sql
```

### <a name="example"></a>Exemple

Créez une ressource de base de données SQL et restreignez l’accès à une plage d’adresses IP à l’aide d’une règle de pare-feu.

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/sql/managementlibrary)

## <a name="samples"></a>Exemples

* [Créer et gérer des bases de données SQL][1]    
* [Utiliser Python pour se connecter et interroger des données][2]   

[1]: https://github.com/Azure-Samples/sql-database-python-manage
[2]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python

Affichez la [liste complète](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL) d’exemples d’Azure SQL Database. 