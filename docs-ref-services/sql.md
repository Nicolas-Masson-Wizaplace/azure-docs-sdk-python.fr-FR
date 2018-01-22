---
title: "Bibliothèques Azure SQL Database pour Python"
description: "Connectez-vous à Azure SQL Database à l’aide du pilote ODBC et pyodbc ou gérez des instances Azure SQL avec l’API de gestion."
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 01/09/2018
ms.topic: reference
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: baa0e53a77d18dc93241135b5b0fecff5786114c
ms.sourcegitcommit: ab96bcebe9d5bfa5f32ec5a61b79bd7483fadcad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2018
---
# <a name="azure-sql-database-libraries-for-python"></a>Bibliothèques Azure SQL Database pour Python

## <a name="overview"></a>Vue d’ensemble

Utilisez les données stockées dans [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) à partir de Python avec le pilote de base de données [ODBC pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers). Consultez notre [guide de démarrage rapide](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) pour en savoir plus sur la connexion à une base de données SQL Azure et l’utilisation d’instructions Transact-SQL pour interroger des données et obtenir des [exemples](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) de mise en route avec pyodbc.

## <a name="install-odbc-driver-and-pyodbc"></a>Installer le pilote ODBC et pyodbc

```bash
pip install pyodbc
```
Pour [en savoir plus](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) sur l’installation des bibliothèques de communication Python et SQL Database.

## <a name="connect-and-execute-a-sql-query"></a>Se connecter et exécuter une requête SQL

### <a name="connect-to-a-sql-database"></a>Connexion à une base de données SQL

```python
import pyodbc

server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'

cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
```

### <a name="execute-a-sql-query"></a>Exécuter une requête SQL

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [exemple de pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a>Connexion aux ORM

pyodbc fonctionne avec les autres ORM tel que [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) et [Django](https://github.com/lionheart/django-pyodbc/). 

## <a name="management-apipythonapioverviewazuresqlmanagementlibrary"></a>[API de gestion](/python/api/overview/azure/sql/managementlibrary)

Créez et gérez des ressources Azure SQL Database dans votre abonnement avec l’API de gestion. 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a>exemples

Créez une ressource de base de données SQL et restreignez l’accès à une plage d’adresses IP à l’aide d’une règle de pare-feu.

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

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

