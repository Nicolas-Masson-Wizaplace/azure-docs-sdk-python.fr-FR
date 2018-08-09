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
ms.openlocfilehash: e3cd84288ad8d49cfcd673506e2db150c02e7918
ms.sourcegitcommit: 16eecfc4ed0e2a8344ce5887327cdf2619ba89e4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/21/2018
ms.locfileid: "39189589"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a><span data-ttu-id="00fee-103">Bibliothèques Azure MySQL/PostgreSQL pour Python</span><span class="sxs-lookup"><span data-stu-id="00fee-103">Azure MySQL/PostgreSQL libraries for Python</span></span>

## <a name="mysql"></a><span data-ttu-id="00fee-104">MySQL</span><span class="sxs-lookup"><span data-stu-id="00fee-104">MySQL</span></span>

<span data-ttu-id="00fee-105">Utilisez des ressources et des données stockées dans la [base de données Azure MySQL](/azure/mysql/overview) à partir de Python avec le Gestionnaire MySQL et pyodbc.</span><span class="sxs-lookup"><span data-stu-id="00fee-105">Work with resources and data stored in [Azure MySQL Database](/azure/mysql/overview) from python with the MySQL manager and pyodbc.</span></span>

### <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="00fee-106">Pilote ODBC du client et pyodbc</span><span class="sxs-lookup"><span data-stu-id="00fee-106">Client ODBC driver and pyodbc</span></span>

<span data-ttu-id="00fee-107">Nous recommandons le [pilote ODBC](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft comme bibliothèque client pour accéder à la base de données Azure pour MySQL.</span><span class="sxs-lookup"><span data-stu-id="00fee-107">The recommended client library for accessing Azure Database for MySQL is the Microsoft [ODBC driver](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span> <span data-ttu-id="00fee-108">Utilisez le pilote ODBC pour vous connecter à la base de données et exécuter les instructions SQL directement.</span><span class="sxs-lookup"><span data-stu-id="00fee-108">Use the ODBC driver to connect to the database and execute SQL statements directly.</span></span>

#### <a name="example"></a><span data-ttu-id="00fee-109">Exemples</span><span class="sxs-lookup"><span data-stu-id="00fee-109">Example</span></span>

<span data-ttu-id="00fee-110">Connectez-vous à la base de données Azure pour MySQL et sélectionnez tous les enregistrements dans le tableau des ventes.</span><span class="sxs-lookup"><span data-stu-id="00fee-110">Connect to a Azure Database for MySQL and select all records in the sales table.</span></span> <span data-ttu-id="00fee-111">Il est possible d’obtenir la chaîne de connexion ODBC de la base de données à partir du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="00fee-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

### <a name="management-api"></a><span data-ttu-id="00fee-112">API de gestion</span><span class="sxs-lookup"><span data-stu-id="00fee-112">Management API</span></span>

<span data-ttu-id="00fee-113">Créez et gérez des ressources MySQL dans votre abonnement avec l’API de gestion.</span><span class="sxs-lookup"><span data-stu-id="00fee-113">Create and manage MySQL resources in your subscription with the management API.</span></span>

#### <a name="requirements"></a><span data-ttu-id="00fee-114">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="00fee-114">Requirements</span></span>
<span data-ttu-id="00fee-115">Vous devez installer les bibliothèques de gestion MySQL pour Python.</span><span class="sxs-lookup"><span data-stu-id="00fee-115">You must install the MySQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="00fee-116">Veuillez consulter la page sur l’[l’authentification du Kit de développement logiciel (SDK) Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)pour en savoir plus sur l’obtention des informations d’identification pour l’authentification avec le client de gestion.</span><span class="sxs-lookup"><span data-stu-id="00fee-116">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

#### <a name="example"></a><span data-ttu-id="00fee-117">Exemples</span><span class="sxs-lookup"><span data-stu-id="00fee-117">Example</span></span>

<span data-ttu-id="00fee-118">Créez une ressource de base de données MySQL 5.7 et restreignez l’accès à une plage d’adresses IP à l’aide d’une règle de pare-feu.</span><span class="sxs-lookup"><span data-stu-id="00fee-118">Create a MySQL 5.7 Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

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
> [<span data-ttu-id="00fee-119">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="00fee-119">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a><span data-ttu-id="00fee-120">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="00fee-120">PostgreSQL</span></span>
<span data-ttu-id="00fee-121">Utilisez le pilote ODBC et pyodbc pour vous connecter à la base de données et exécuter les instructions SQL directement.</span><span class="sxs-lookup"><span data-stu-id="00fee-121">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="00fee-122">En savoir plus sur les [bases de données Azure pour PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span><span class="sxs-lookup"><span data-stu-id="00fee-122">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

### <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="00fee-123">Pilote ODBC du client et pyodbc</span><span class="sxs-lookup"><span data-stu-id="00fee-123">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="00fee-124">Nous recommandons [pyodbc et le pilote ODBC](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft comme bibliothèque client pour accéder à la base de données Azure pour PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="00fee-124">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

#### <a name="example"></a><span data-ttu-id="00fee-125">Exemples</span><span class="sxs-lookup"><span data-stu-id="00fee-125">Example</span></span> 

<span data-ttu-id="00fee-126">Connectez-vous à la base de données Azure pour PostgreSQL et sélectionnez tous les enregistrements dans le tableau `SALES`.</span><span class="sxs-lookup"><span data-stu-id="00fee-126">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="00fee-127">Il est possible d’obtenir la chaîne de connexion ODBC de la base de données à partir du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="00fee-127">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

### <a name="management-api"></a><span data-ttu-id="00fee-128">API de gestion</span><span class="sxs-lookup"><span data-stu-id="00fee-128">Management API</span></span>
#### <a name="requirements"></a><span data-ttu-id="00fee-129">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="00fee-129">Requirements</span></span>
<span data-ttu-id="00fee-130">Vous devez installer les bibliothèques de gestion PostgreSQL pour Python.</span><span class="sxs-lookup"><span data-stu-id="00fee-130">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="00fee-131">Veuillez consulter la page sur l’[l’authentification du Kit de développement logiciel (SDK) Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)pour en savoir plus sur l’obtention des informations d’identification pour l’authentification avec le client de gestion.</span><span class="sxs-lookup"><span data-stu-id="00fee-131">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

#### <a name="example"></a><span data-ttu-id="00fee-132">Exemples</span><span class="sxs-lookup"><span data-stu-id="00fee-132">Example</span></span>
<span data-ttu-id="00fee-133">Dans cet exemple, nous allons créer une base de données Postgres sur notre serveur Postgres existant.</span><span class="sxs-lookup"><span data-stu-id="00fee-133">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
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
> [<span data-ttu-id="00fee-134">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="00fee-134">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/mysql/management)