---
title: "Bibliothèques de machines virtuelles Azure pour Python"
description: 
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, Calcul, Machines virtuelles"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: c25665e19adb44c7112bf1533097ce1e6c739cb8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="e4f8f-103">Bibliothèques de machines virtuelle Azure</span><span class="sxs-lookup"><span data-stu-id="e4f8f-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="e4f8f-104">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e4f8f-104">Overview</span></span>

<span data-ttu-id="e4f8f-105">Des ressources de calcul à la demande et évolutives s’exécutant sous Linux et Windows.</span><span class="sxs-lookup"><span data-stu-id="e4f8f-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="e4f8f-106">Pour découvrir les machines virtuelles Azure, consultez la section [Créer une machine virtuelle Linux avec le portail Azure](/azure/virtual-machines/linux/quick-create-portal).</span><span class="sxs-lookup"><span data-stu-id="e4f8f-106">To get started with Azure Virtual Machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="e4f8f-107">API de gestion</span><span class="sxs-lookup"><span data-stu-id="e4f8f-107">Management API</span></span>

<span data-ttu-id="e4f8f-108">Créez, configurez et mettez à l’échelle des machines virtuelles Windows et Linux dans Azure à partir de votre code avec l’API de gestion.</span><span class="sxs-lookup"><span data-stu-id="e4f8f-108">Create, configure, manage and scale Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="e4f8f-109">Installez la bibliothèque via pip.</span><span class="sxs-lookup"><span data-stu-id="e4f8f-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a><span data-ttu-id="e4f8f-110">Exemple</span><span class="sxs-lookup"><span data-stu-id="e4f8f-110">Example</span></span>

<span data-ttu-id="e4f8f-111">Créez une machine virtuelle Linux dans un groupe de ressources Azure existant.</span><span class="sxs-lookup"><span data-stu-id="e4f8f-111">Create a new Linux virtual machine in an existing Azure resource group.</span></span>

```python
VM_PARAMETERS={
        'location': 'LOCATION',
        'os_profile': {
            'computer_name': 'VM_NAME',
            'admin_username': 'USERNAME',
            'admin_password': 'PASSWORD'
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': 'Canonical',
                'offer': 'UbuntuServer',
                'sku': '16.04.0-LTS',
                'version': 'latest'
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': 'NIC_ID',
            }]
        },
    }

def create_vm()
    compute_client.virtual_machines.create_or_update(
        'RESOURCE_GROUP_NAME', 'VM_NAME', VM_PARAMETERS)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e4f8f-112">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="e4f8f-112">Explore the Management APIs</span></span>](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a><span data-ttu-id="e4f8f-113">Exemples</span><span class="sxs-lookup"><span data-stu-id="e4f8f-113">Samples</span></span>

* <span data-ttu-id="e4f8f-114">[Gérer des machines virtuelles][1]</span><span class="sxs-lookup"><span data-stu-id="e4f8f-114">[Manage virtual machines][1]</span></span>
* <span data-ttu-id="e4f8f-115">[Gérer un équilibreur de charge][2]</span><span class="sxs-lookup"><span data-stu-id="e4f8f-115">[Manage a load balancer][2]</span></span>
* <span data-ttu-id="e4f8f-116">[Créer et configurer des disques gérés][3]</span><span class="sxs-lookup"><span data-stu-id="e4f8f-116">[Create and configure managed disks][3]</span></span>
* <span data-ttu-id="e4f8f-117">[Répertorier des images][4]</span><span class="sxs-lookup"><span data-stu-id="e4f8f-117">[List images][4]</span></span> 
* <span data-ttu-id="e4f8f-118">[Surveiller les machines virtuelles][5]</span><span class="sxs-lookup"><span data-stu-id="e4f8f-118">[Monitor virtual machines][5]</span></span>

<span data-ttu-id="e4f8f-119">Afficher la [liste complète](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) des exemples de machines virtuelles.</span><span class="sxs-lookup"><span data-stu-id="e4f8f-119">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) of virtual machine samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[3]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[4]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md