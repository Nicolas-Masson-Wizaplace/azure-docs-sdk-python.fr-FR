---
title: Bibliothèques Azure MySQL/PostgreSQL pour Python
description: ''
keywords: Azure, Python, Kit de développement logiciel (SDK), API, SQL, base de données, MySQL, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.openlocfilehash: 402e87ae81e6df64b040293992244902313e5b1b
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747719"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a>Bibliothèques Azure MySQL/PostgreSQL pour Python

## <a name="mysql"></a>MySQL

Utilisez des ressources et des données stockées dans la [base de données Azure MySQL](/azure/mysql/overview) à partir de Python avec le Gestionnaire MySQL et pyodbc.

### <a name="client-odbc-driver-and-pyodbc"></a>Pilote ODBC du client et pyodbc

Nous recommandons le [pilote ODBC](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft comme bibliothèque client pour accéder à la base de données Azure pour MySQL. Utilisez le pilote ODBC pour vous connecter à la base de données et exécuter les instructions SQL directement.

#### <a name="example"></a>Exemples

Connectez-vous à la base de données Azure pour MySQL et sélectionnez tous les enregistrements dans le tableau des ventes. Il est possible d’obtenir la chaîne de connexion ODBC de la base de données à partir du portail Azure.

```python
SERVER = 'YOUR_SEVER_NAME' + '.mysql.database.azure.com'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_MYSQL_USERNAME'
PASSWORD = 'YOUR_MYSQL_PASSWORD'

DRIVER = '{MySQL ODBC 5.3 UNICODE Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=3306;SERVER=' + SERVER + ';PORT=3306;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

### <a name="management-api"></a>API de gestion

Créez et gérez des ressources MySQL dans votre abonnement avec l’API de gestion.

#### <a name="requirements"></a>Configuration requise
Vous devez installer les bibliothèques de gestion MySQL pour Python.
```bash
pip install azure-mgmt-rdbms
```

Veuillez consulter la page sur l’[l’authentification du Kit de développement logiciel (SDK) Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)pour en savoir plus sur l’obtention des informations d’identification pour l’authentification avec le client de gestion.

#### <a name="example"></a>Exemples

Créez une ressource de base de données MySQL 5.7 et restreignez l’accès à une plage d’adresses IP à l’aide d’une règle de pare-feu.

```python

from azure.mgmt.rdbms.mysql import MySQLManagementClient
from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE-GROUP_WITH_POSTGRES"
MYSQL_SERVER = "YOUR_DESIRED_MYSQL_SERVER_NAME"
MYSQL_ADMIN_USER = "YOUR_MYSQL_ADMIN_USERNAME"
MYSQL_ADMIN_PASSWORD = "YOUR_MYSQL_ADMIN_PASSOWRD"
LOCATION = "westus"  # example Azure availability zone, should match resource group


client = MySQLManagementClient(credentials=creds,
    subscription_id=SUBSCRIPTION_ID)

server_creation_poller = client.servers.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=MYSQL_SERVER,
    ServerForCreate(
        ServerPropertiesForDefaultCreate(
            administrator_login=MYSQL_ADMIN_USER,
            administrator_login_password=MYSQL_ADMIN_PASSWORD,
            version=ServerVersion.five_full_stop_seven
        ),
        location=LOCATION
    )
)

server = server_creation_poller.result()

# Open access to this server for IPs
rule_creation_poller = client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    MYSQL_SERVER,
    "some_custom_ip_range_whitelist_rule_name",  # Custom firewall rule name
    "123.123.123.123",  # Start ip range
    "167.220.0.235"  # End ip range
)

firewall_rule = rule_creation_poller.result()
```

> [!div class="nextstepaction"]
> [Explorer les API de gestion](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a>PostgreSQL
Utilisez le pilote ODBC et pyodbc pour vous connecter à la base de données et exécuter les instructions SQL directement.

En savoir plus sur les [bases de données Azure pour PostgreSQL](https://docs.microsoft.com/azure/postgresql/).

### <a name="client-odbc-driver-and-pyodbc"></a>Pilote ODBC du client et pyodbc
Nous recommandons [pyodbc et le pilote ODBC](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft comme bibliothèque client pour accéder à la base de données Azure pour PostgreSQL

#### <a name="example"></a>Exemples 

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

### <a name="management-api"></a>API de gestion
#### <a name="requirements"></a>Configuration requise
Vous devez installer les bibliothèques de gestion PostgreSQL pour Python.
```bash
pip install azure-mgmt-rdbms
```

Veuillez consulter la page sur l’[l’authentification du Kit de développement logiciel (SDK) Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)pour en savoir plus sur l’obtention des informations d’identification pour l’authentification avec le client de gestion.

#### <a name="example"></a>Exemples
Dans cet exemple, nous allons créer une base de données Postgres sur notre serveur Postgres existant.
```python
from azure.mgmt.rdbms.postgresql import PostgreSQLManagementClient

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
> [Explorer les API de gestion](/python/api/overview/azure/postgresql/mysql/management)
