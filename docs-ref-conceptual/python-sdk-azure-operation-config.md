---
title: Configuration d’opérations Kit de développement logiciel (SDK) Azure pour Python
description: C levé par le Kit de développement logiciel (SDK) Azure pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API, exceptions
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 03/07/2018
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: adeb6aa8a2c363c3119e97503df9536fb0633b4c
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376862"
---
# <a name="operation-config"></a><span data-ttu-id="2e488-104">Configuration de l’opération</span><span class="sxs-lookup"><span data-stu-id="2e488-104">Operation config</span></span> 

<span data-ttu-id="2e488-105">Les méthodes concernant les opérations ont des paramètres supplémentaires qui peuvent être fournis dans les `kwargs`.</span><span class="sxs-lookup"><span data-stu-id="2e488-105">Methods on operations have extra parameters which can be provided in the `kwargs`.</span></span> <span data-ttu-id="2e488-106">Il s’agit d’operation_config.</span><span class="sxs-lookup"><span data-stu-id="2e488-106">This is called operation_config.</span></span>

<span data-ttu-id="2e488-107">Les options de configuration de l’opération sont :</span><span class="sxs-lookup"><span data-stu-id="2e488-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="2e488-108">Nom du paramètre</span><span class="sxs-lookup"><span data-stu-id="2e488-108">Parameter name</span></span>|<span data-ttu-id="2e488-109">Type</span><span class="sxs-lookup"><span data-stu-id="2e488-109">Type</span></span>|<span data-ttu-id="2e488-110">Rôle</span><span class="sxs-lookup"><span data-stu-id="2e488-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="2e488-111">verify</span><span class="sxs-lookup"><span data-stu-id="2e488-111">verify</span></span> |`bool`|<span data-ttu-id="2e488-112">Indique s’il faut vérifier le certificat SSL.</span><span class="sxs-lookup"><span data-stu-id="2e488-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="2e488-113">La valeur par défaut est True.</span><span class="sxs-lookup"><span data-stu-id="2e488-113">Default is True.</span></span>|
|  <span data-ttu-id="2e488-114">cert</span><span class="sxs-lookup"><span data-stu-id="2e488-114">cert</span></span> |`str`| <span data-ttu-id="2e488-115">Chemin d’accès du certificat local pour la vérification du côté client.</span><span class="sxs-lookup"><span data-stu-id="2e488-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="2e488-116">timeout</span><span class="sxs-lookup"><span data-stu-id="2e488-116">timeout</span></span> |`int`| <span data-ttu-id="2e488-117">Délai d’expiration pour l’établissement d’une connexion au serveur en secondes.</span><span class="sxs-lookup"><span data-stu-id="2e488-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="2e488-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="2e488-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="2e488-119">Indique s’il faut autoriser les redirections.</span><span class="sxs-lookup"><span data-stu-id="2e488-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="2e488-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="2e488-120">max_redirects</span></span>  |`int`| <span data-ttu-id="2e488-121">Le nombre maximal de redirections autorisées.</span><span class="sxs-lookup"><span data-stu-id="2e488-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="2e488-122">proxies</span><span class="sxs-lookup"><span data-stu-id="2e488-122">proxies</span></span>  |`dict` |<span data-ttu-id="2e488-123">Paramètres du serveur proxy.</span><span class="sxs-lookup"><span data-stu-id="2e488-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="2e488-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="2e488-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="2e488-125">Indique s’il faut lire les paramètres de proxy à partir de variables d’environnement locales.</span><span class="sxs-lookup"><span data-stu-id="2e488-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="2e488-126">retries</span><span class="sxs-lookup"><span data-stu-id="2e488-126">retries</span></span>  |`int` | <span data-ttu-id="2e488-127">Nombre total de nouvelles tentatives.</span><span class="sxs-lookup"><span data-stu-id="2e488-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="2e488-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="2e488-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="2e488-129">Active les journaux d’activité HTTP en mode débogage (False par défaut).</span><span class="sxs-lookup"><span data-stu-id="2e488-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
