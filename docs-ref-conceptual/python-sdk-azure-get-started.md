---
title: "Prise en main des bibliothèques Azure pour Java"
description: "Prise en main des bibliothèques Azure pour Java"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: get-started
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 848ca9dc40000e68e5e3cea3af8b8a0d22747881
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-azure-libraries-for-python"></a>Prise en main des bibliothèques Azure pour Java

Ce guide illustre l’utilisation de plusieurs bibliothèques Azure pour Python.  Vous devrez configurer l’authentification, créer et utiliser un compte de stockage Azure et une base de données Azure SQL Database, ainsi que déployer des machines virtuelles et une application web Azure App Service à partir de GitHub.

## <a name="prerequisites"></a>Composants requis

- Un compte Azure. Si vous n’en avez pas, inscrivez-vous pour un [essai gratuit](https://azure.microsoft.com/free/).
- [Python](https://www.python.org/downloads/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) ou [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

[!INCLUDE [azure-cloud-shell](../docs-ref-conceptual/includes/cloud-shell-try-it.md)]

## <a name="set-up-authentication"></a>Configurer l’authentification
> [!IMPORTANT]
> Cela doit être utilisé comme guide de démarrage rapide pour une expérience de développement. À des fins de production, utilisez [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) ou votre propre système d’informations d’identification.
> Toute modification apportée à votre configuration CLI impactera l’exécution du kit de développement logiciel (SDK).

Le kit de développement logiciel (SDK) est en mesure de créer un client à l’aide de l’abonnement de votre CLI.

Pour définir des informations d’identification actives, utilisez la commande [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).
L’ID d’abonnement par défaut est le seul dont vous disposez ou celui que vous définissez via la commande [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli).

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
## <a name="create-a-virtual-environment"></a>Créer un environnement virtuel

> [!IMPORTANT]
> La création d’un environnement virtuel est facultative, mais nous vous recommandons fortement de le faire.

Créez un environnement virtuel dans Bash (Linux ou [Bash pour Windows](https://msdn.microsoft.com/commandline/wsl/about)) :
```bash
pip install virtualenv
virtualenv mytestenv
cd mytestenv
source ./bin/activate
```

Créez un environnement virtuel dans Powershell :
```powershell
pip install virtualenv
virtualenv mytestenv
cd mytestenv
.\Scripts\activate
```

> [!IMPORTANT]
> Remarque : Si vous utilisez [VSCode](https://code.visualstudio.com/) et l’[extension Python](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python), vous pouvez le démarrer à l’aide de `code .` et obtenir votre environnement configuré.

## <a name="create-a-linux-virtual-machine"></a>Créer une machine virtuelle Linux
Ce code crée une nouvelle machine virtuelle Linux avec le nom `testLinuxVM` dans un groupe de ressources `sampleVmResourceGroup` qui s’exécute dans la région Azure Est des États-Unis.

Authentifiez :
```azcli
az login
```
Créez un groupe de ressources :
```azurecli-interactive
az group create -l eastus --n sampleVmResourceGroup
```

Créez un réseau virtuel et un sous-réseau :
```azurecli-interactive
az network vnet create -g sampleVmResourceGroup -n azure-sample-vnet --address-prefix 10.0.0.0/16 --subnet-name azure-sample-subnet --subnet-prefix 10.0.0.0/24
```

Créez une adresse IP publique :
```azurecli-interactive
az network public-ip create -g sampleVmResourceGroup -n azure-sample-ip --allocation-method Dynamic --version IPv6
```
Créez un client d’interface réseau :
```azurecli-interactive
az network nic create -g sampleVmResourceGroup --vnet-name azure-sample-vnet --subnet azure-sample-subnet -n azure-sample-nic --public-ip-address azure-sample-ip
```

```python
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
from azure.common.client_factory import get_client_from_cli_profile

# Azure Datacenter
LOCATION = 'eastus'

# Resource Group
GROUP_NAME = 'sampleVmResourceGroup'

# Network
VNET_NAME = 'azure-sample-vnet'
SUBNET_NAME = 'azure-sample-subnet'

# VM
NIC_NAME = 'azure-sample-nic'
VM_NAME = 'testLinuxVM'

USERNAME = 'userlogin'
PASSWORD = 'Pa$$w0rd91'

IP_ADDRESS_NAME='azure-sample-ip'

VM_REFERENCE = {
    'linux': {
        'publisher': 'Canonical',
        'offer': 'UbuntuServer',
        'sku': '16.04.0-LTS',
        'version': 'latest'
    },
    'windows': {
        'publisher': 'MicrosoftWindowsServerEssentials',
        'offer': 'WindowsServerEssentials',
        'sku': 'WindowsServerEssentials',
        'version': 'latest'
    }
}


def run_example():

    compute_client = get_client_from_cli_profile(ComputeManagementClient)
    network_client = get_client_from_cli_profile(NetworkManagementClient)

    # get nic id
    nic = network_client.network_interfaces.get(GROUP_NAME, NIC_NAME)

    # Create Linux VM
    print('\nCreating Linux Virtual Machine')
    vm_parameters = create_vm_parameters(nic.id, VM_REFERENCE['linux'])
    print(vm_parameters)
    async_vm_creation = compute_client.virtual_machines.create_or_update(
        GROUP_NAME, VM_NAME, vm_parameters)
    async_vm_creation.wait()


def create_vm_parameters(nic_id, vm_reference):
    """Create the VM parameters structure.
    """
    return {
        'location': LOCATION,
        'os_profile': {
            'computer_name': VM_NAME,
            'admin_username': USERNAME,
            'admin_password': PASSWORD
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': vm_reference['publisher'],
                'offer': vm_reference['offer'],
                'sku': vm_reference['sku'],
                'version': vm_reference['version']
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': nic_id,
            }]
        },
    }


if __name__ == "__main__":
    run_example()
```

Lorsque le programme se termine, vérifiez la machine virtuelle de votre abonnement avec Azure CLI 2.0 :

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

Après avoir vérifié que le code a fonctionné, utilisez l’interface CLI pour supprimer la machine virtuelle et ses ressources.

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>Déployer une application web à partir d’un référentiel GitHub
Ce code déploie une application web Flask à partir de la branche `master` d’un référentiel GitHub vers une nouvelle [application web Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) s’exécutant au niveau gratuit. 

Avant de commencer : Répliquer https://github.com/Azure-Samples/python-docs-hello-world

Authentifiez :
```azcli
az login
```
Créez un groupe de ressources :
```azurecli-interactive
az group create -l eastus -n sampleWebResourceGroup
```

Créez un plan App Service gratuit :
```azurecli-interactive
az appservice plan create -g sampleWebResourceGroup -n sampleFreePlan  --sku Free
```

```python
from azure.mgmt.web import WebSiteManagementClient
from azure.mgmt.web.models import Site, SiteSourceControl, SiteConfig
from azure.common.client_factory import get_client_from_cli_profile

RESOURCE_GROUP_NAME = 'sampleWebResourceGroup'
SERVICE_PLAN_NAME = 'sampleFreePlanName'
WEB_APP_NAME = 'sampleflaskapp123'
REPO_URL = 'GitHub_Repository_URL'

#log in
web_client = get_client_from_cli_profile(WebSiteManagementClient)

# get service plan id
service_plan = web_client.app_service_plans.get(RESOURCE_GROUP_NAME, SERVICE_PLAN_NAME)

# create a web app
siteConfiguration = SiteConfig(
    python_version='3.4',
)
site_async_operation = web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=service_plan.id,
        site_config=siteConfiguration
    ),
)

site = site_async_operation.result()
print('created webapp: ' + site.default_host_name)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url= REPO_URL,
        branch='master'
    )
)

source_control = source_control_async_operation.result()
print("added source control to: " + source_control.name + "azurewebsites.net")
```

Ouvrez un navigateur menant à l’application à l’aide de l’interface CLI :
```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

Supprimez l’application web et le plan de votre abonnement après vérification du déploiement. 
```azurecli-interactive
az group delete --name sampleWebResourceGroup
```


## <a name="connect-to-a-sql-database"></a>Connexion à une base de données SQL
Ce code crée une nouvelle base de données SQL avec une règle de pare-feu autorisant l’accès à distance, puis se connecte à l’aide du pilote ODBC de Microsoft. Pyodbc utilise le pilote ODBC Microsoft sous Linux pour se connecter aux bases de données SQL. 

Authentifiez :
```azcli
az login
```
Créez un groupe de ressources :
```azurecli-interactive
az group create -l eastus -n azure-sample-group
```

Créez un serveur SQL :
```azurecli-interactive
az sql server create --admin-password HusH_Sec4et --admin-user mysecretname -l eastus -n samplesqlserver123 -g azure-sample-group
```

Ajoutez une règle de pare-feu :
```azurecli-interactive
az sql server firewall-rule create --end-ip-address 167.220.0.235 --name firewall_rule_name_123.123.123.123 --resource-group azure-sample-group --server samplesqlserver123 --start-ip-address 123.123.123.123
```

Créez une base de données SQL :
```azurecli-interactive
az sql db create --name sample-db --resource-group azure-sample-group --server samplesqlserver123
```


```python
from azure.mgmt.sql import SqlManagementClient
from azure.common.client_factory import get_client_from_cli_profile
import pyodbc

LOCATION = 'eastus'
GROUP_NAME = 'azure-sample-group'
SERVER_NAME = 'samplesqlserver123'
DATABASE_NAME = 'sample-db'
USER_NAME ='mysecretname'
PASSWORD='HusH_Sec4et'

# authenticate
sql_client = get_client_from_cli_profile(SqlManagementClient)


def run_example():
    # Get SQL database
    print('Get SQL database')
    database = sql_client.databases.get(
        GROUP_NAME,
        SERVER_NAME,
        DATABASE_NAME
    )
    print(database)
    print("\n\n")


def create_and_insert():
    server = SERVER_NAME+'.database.windows.net'
    database = DATABASE_NAME
    username = USER_NAME
    password = PASSWORD
    driver = '{ODBC Driver 13 for SQL Server}'
    cnxn = pyodbc.connect(
        'DRIVER=' + driver + ';PORT=1433;SERVER=' + server + ';PORT=1443;DATABASE=' + database + ';UID=' + username + ';PWD=' + password)
    cursor = cnxn.cursor()
    tsql = "CREATE TABLE CLOUD (name varchar(255), code int);"
    tsqlInsert = "INSERT INTO CLOUD (name, code) VALUES ('Azure', 1); "
    selectValues = "SELECT * FROM CLOUD"
    with cursor.execute(tsql):
        print('Successfully created table!')
    with cursor.execute(tsqlInsert):
        print('Successfully inserted data into table')
    cursor.execute(selectValues)
    row = cursor.fetchone()
    while row:
        print(str(row[0]) + " " + str(row[1]))
        row = cursor.fetchone()
    cnxn.commit()

if __name__ == "__main__":
    run_example()
    create_and_insert()
```

Après avoir vérifié que le code a fonctionné, utilisez l’interface CLI pour supprimer la base de données SQL et ses ressources.

```azurecli-interactive
az group delete --name azure-sample-group
```

## <a name="write-a-blob-into-a-new-storage-account"></a>Écrire un objet blob dans un nouveau compte de stockage

Authentifiez :
```azcli
az login
```
Créez un groupe de ressources :
```azurecli-interactive
az group create -l eastus -n sampleStorageResourceGroup
```

Créez un compte de stockage :
```azurecli-interactive
az storage account create -n samplestorageaccountname -g sampleStorageResourceGroup -l eastus --sku Standard_RAGRS
```

Ce code crée un [compte de stockage Azure](https://docs.microsoft.com/azure/storage/storage-introduction), avant d’utiliser les bibliothèques de stockage Azure pour Python pour créer un fichier HTML dans le cloud. 
```python
from azure.storage import CloudStorageAccount
from azure.storage.blob import PublicAccess
from azure.storage.blob.models import ContentSettings
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.storage import StorageManagementClient

RESOURCE_GROUP = 'sampleStorageResourceGroup'
STORAGE_ACCOUNT_NAME = 'samplestorageaccountname'
CONTAINER_NAME = 'samplecontainername'

# log in
storage_client = get_client_from_cli_profile(StorageManagementClient)

# create a public storage container to hold the file
storage_keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP, STORAGE_ACCOUNT_NAME)
storage_keys = {v.key_name: v.value for v in storage_keys.keys}

storage_client = CloudStorageAccount(STORAGE_ACCOUNT_NAME, storage_keys['key1'])
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(CONTAINER_NAME, public_access=PublicAccess.Container)

blob_service.create_blob_from_bytes(
    CONTAINER_NAME,
    'helloworld.html',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url(CONTAINER_NAME, 'helloworld.html'))
```
Pour vérifier que le contenu a bien été chargé, accédez à l’URL imprimée. 

Effacez le compte de stockage à l’aide de l’interface CLI :
```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>Explorez d’autres exemples

Pour savoir comment utiliser les bibliothèques de gestion Azure pour Python afin de gérer les ressources et d’automatiser des tâches, consultez notre exemple de code pour les [machines virtuelles](python-sdk-azure-web-apps-samples.md), [les applications web](python-sdk-azure-web-apps-samples.md) et [SQL Database](python-sdk-azure-sql-database-samples.md).


## <a name="reference"></a>Référence

Il existe une [référence](/python/api/overview/azure/?view=azure-python) pour tous les packages.
