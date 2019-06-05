---
title: Bibliothèques Azure Web Apps pour Python
description: ''
keywords: Azure, Python, Kit de développement logiciel (SDK), API, web apps, App Service
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 4870394c6ee39cde546d090fa1a0d136609851b3
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376692"
---
# <a name="azure-web-apps-libraries-for-python"></a><span data-ttu-id="3fadc-103">Bibliothèques Azure Web Apps pour Python</span><span class="sxs-lookup"><span data-stu-id="3fadc-103">Azure Web Apps libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="3fadc-104">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="3fadc-104">Overview</span></span>

<span data-ttu-id="3fadc-105">Déployez et mettez à l’échelle des sites web, des applications web, des services et des API REST avec [Azure App Service](/azure/app-service).</span><span class="sxs-lookup"><span data-stu-id="3fadc-105">Deploy and scale websites, web applications, services, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="3fadc-106">Pour découvrir Azure App Service, consultez la section [Créer une application web Python dans Azure](/azure/app-service-web/app-service-web-get-started-python).</span><span class="sxs-lookup"><span data-stu-id="3fadc-106">To get started with Azure App Service, see [Create a Python web app in Azure](/azure/app-service-web/app-service-web-get-started-python).</span></span>

## <a name="management-api"></a><span data-ttu-id="3fadc-107">API de gestion</span><span class="sxs-lookup"><span data-stu-id="3fadc-107">Management API</span></span>

<span data-ttu-id="3fadc-108">Déployez, gérez et mettez à l’échelle des éléments hébergés dans Azure App Service avec l’API de gestion.</span><span class="sxs-lookup"><span data-stu-id="3fadc-108">Deploy, manage, and scale elements hosted in the Azure App Service with the management API.</span></span>

<span data-ttu-id="3fadc-109">Installez la bibliothèque via pip.</span><span class="sxs-lookup"><span data-stu-id="3fadc-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-web
```

### <a name="example"></a><span data-ttu-id="3fadc-110">Exemples</span><span class="sxs-lookup"><span data-stu-id="3fadc-110">Example</span></span>

<span data-ttu-id="3fadc-111">Déployer une application web à partir d’un référentiel GitHub dans Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="3fadc-111">Deploy a webapp from a GitHub repository into Azure Web App.</span></span>

```python
siteConfiguration = SiteConfig(
    python_version='3.4'
)

# create a web app
web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=SERVICE_PLAN_ID,
        site_config=siteConfiguration
    )
)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url='https://github.com/lisawong19/python-docs-hello-world',
        branch='master'
    )
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3fadc-112">Explorer les API de gestion</span><span class="sxs-lookup"><span data-stu-id="3fadc-112">Explore the Management APIs</span></span>](/python/api/overview/azure/webapps/management)

## <a name="samples"></a><span data-ttu-id="3fadc-113">Exemples</span><span class="sxs-lookup"><span data-stu-id="3fadc-113">Samples</span></span>

* <span data-ttu-id="3fadc-114">[Gérer des sites web Azure avec Python][1]</span><span class="sxs-lookup"><span data-stu-id="3fadc-114">[Manage Azure websites with python][1]</span></span>
* <span data-ttu-id="3fadc-115">[Créer un flux de travail d’application logique][2]</span><span class="sxs-lookup"><span data-stu-id="3fadc-115">[Create a Logic App workflow][2]</span></span>

<span data-ttu-id="3fadc-116">Affichez la [liste complète](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app) des exemples d’application web.</span><span class="sxs-lookup"><span data-stu-id="3fadc-116">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app) of web application samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md
