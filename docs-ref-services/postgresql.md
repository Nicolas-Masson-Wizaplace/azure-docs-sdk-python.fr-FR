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
#<a name="azure-postgresql-libraries-for-python"></a><span data-ttu-id="15a35-103">Bibliothèques Azure PostgreSQL pour Python</span><span class="sxs-lookup"><span data-stu-id="15a35-103">Azure PostgreSQL libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="15a35-104">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="15a35-104">Overview</span></span>
<span data-ttu-id="15a35-105">Utilisez le pilote ODBC et pyodbc pour vous connecter à la base de données et exécuter les instructions SQL directement.</span><span class="sxs-lookup"><span data-stu-id="15a35-105">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="15a35-106">En savoir plus sur les [bases de données Azure pour PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span><span class="sxs-lookup"><span data-stu-id="15a35-106">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="15a35-107">Pilote ODBC du client et pyodbc</span><span class="sxs-lookup"><span data-stu-id="15a35-107">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="15a35-108">Nous recommandons [pyodbc et le pilote ODBC](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft comme bibliothèque client pour accéder à la base de données Azure pour PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="15a35-108">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

### <a name="example"></a><span data-ttu-id="15a35-109">exemples</span><span class="sxs-lookup"><span data-stu-id="15a35-109">Example</span></span> 

<span data-ttu-id="15a35-110">Connectez-vous à la base de données Azure pour PostgreSQL et sélectionnez tous les enregistrements dans le tableau `SALES`.</span><span class="sxs-lookup"><span data-stu-id="15a35-110">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="15a35-111">Il est possible d’obtenir la chaîne de connexion ODBC de la base de données à partir du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="15a35-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="15a35-112">API de gestion</span><span class="sxs-lookup"><span data-stu-id="15a35-112">Management API</span></span>
### <a name="requirements"></a><span data-ttu-id="15a35-113">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="15a35-113">Requirements</span></span>
<span data-ttu-id="15a35-114">Vous devez installer les bibliothèques de gestion PostgreSQL pour Python.</span><span class="sxs-lookup"><span data-stu-id="15a35-114">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="15a35-115">Veuillez consulter la page sur l’[l’authentification du Kit de développement logiciel (SDK) Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate)pour en savoir plus sur l’obtention des informations d’identification pour l’authentification avec le client de gestion.</span><span class="sxs-lookup"><span data-stu-id="15a35-115">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="15a35-116">exemples</span><span class="sxs-lookup"><span data-stu-id="15a35-116">Example</span></span>
<span data-ttu-id="15a35-117">Dans cet exemple, nous allons créer une base de données Postgres sur notre serveur Postgres existant.</span><span class="sxs-lookup"><span data-stu-id="15a35-117">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
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
> [<span data-ttu-id="15a35-118">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="15a35-118">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/management)

