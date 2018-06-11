---
title: Bibliothèques Azure Data Factory pour Python
description: Référence pour les bibliothèques Azure Data Factory pour Python
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 05/10/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 05d93f7d1838c110c3ba77f7abd3967f7870774b
ms.sourcegitcommit: d65549030a0edb50d75482f79aac0962d1dacb57
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34759046"
---
# <a name="azure-data-factory-libraries-for-python"></a><span data-ttu-id="cce07-103">Bibliothèques Azure Data Factory pour Python</span><span class="sxs-lookup"><span data-stu-id="cce07-103">Azure Data Factory libraries for Python</span></span>

<span data-ttu-id="cce07-104">Composition de services de stockage, de déplacement et de traitement des services au sein de pipelines de données automatisés avec [Azure Data Factory](/azure/data-factory/)</span><span class="sxs-lookup"><span data-stu-id="cce07-104">Compose data storage, movement, and processing services into automated data pipelines with [Azure Data Factory](/azure/data-factory/)</span></span>

<span data-ttu-id="cce07-105">[En savoir plus](/azure/data-factory/introduction) sur Data Factory et la prise en main en consultant [Créer une fabrique de données et un pipeline à l’aide du démarrage rapide de Python](/azure/data-factory/quickstart-create-data-factory-python).</span><span class="sxs-lookup"><span data-stu-id="cce07-105">[Learn more](/azure/data-factory/introduction) about Data Factory and get started with the [Create a data factory and pipeline using Python quickstart](/azure/data-factory/quickstart-create-data-factory-python).</span></span> 

## <a name="management-module"></a><span data-ttu-id="cce07-106">Module de gestion</span><span class="sxs-lookup"><span data-stu-id="cce07-106">Management module</span></span>

<span data-ttu-id="cce07-107">Créez et gérez des instances Data Factory dans votre abonnement avec le module de gestion.</span><span class="sxs-lookup"><span data-stu-id="cce07-107">Create and manage Data Factory instances in your subscription with the management module.</span></span>

### <a name="installation"></a><span data-ttu-id="cce07-108">Installation</span><span class="sxs-lookup"><span data-stu-id="cce07-108">Installation</span></span>

<span data-ttu-id="cce07-109">Installez le package avec [pip](https://pip.pypa.io/en/stable/quickstart/) :</span><span class="sxs-lookup"><span data-stu-id="cce07-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a><span data-ttu-id="cce07-110">Exemples</span><span class="sxs-lookup"><span data-stu-id="cce07-110">Example</span></span> 

<span data-ttu-id="cce07-111">Créer une fabrique de données dans votre abonnement sur la région Est des États-Unis.</span><span class="sxs-lookup"><span data-stu-id="cce07-111">Create a Data Factory in your subscription on the East US region.</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.datafactory import DataFactoryManagementClient
from azure.mgmt.datafactory.models import *
import time

#Create a data factory
subscription_id = '<Specify your Azure Subscription ID>'
credentials = ServicePrincipalCredentials(client_id='<Active Directory application/client ID>', secret='<client secret>', tenant='<Active Directory tenant ID>')
adf_client = DataFactoryManagementClient(credentials, subscription_id)

rg_params = {'location':'eastus'}
df_params = {'location':'eastus'}  

df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="cce07-112">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="cce07-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datafactory/management)