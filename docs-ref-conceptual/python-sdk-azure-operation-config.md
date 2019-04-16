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
ms.openlocfilehash: d7be7cd76d019c6741d93c04458376a9352e363b
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30204258"
---
# <a name="operation-config"></a><span data-ttu-id="ce54b-104">Configuration de l’opération</span><span class="sxs-lookup"><span data-stu-id="ce54b-104">Operation config</span></span> 

<span data-ttu-id="ce54b-105">Les méthodes concernant les opérations ont des paramètres supplémentaires qui peuvent être fournis dans les arguments de paramètres.</span><span class="sxs-lookup"><span data-stu-id="ce54b-105">Methods on operations have extra parameters which can be provided in the kwargs.</span></span> <span data-ttu-id="ce54b-106">Il s’agit d’operation_config.</span><span class="sxs-lookup"><span data-stu-id="ce54b-106">This is called operation_config.</span></span>

<span data-ttu-id="ce54b-107">Les options de configuration de l’opération sont :</span><span class="sxs-lookup"><span data-stu-id="ce54b-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="ce54b-108">Nom du paramètre</span><span class="sxs-lookup"><span data-stu-id="ce54b-108">Parameter name</span></span>|<span data-ttu-id="ce54b-109">type</span><span class="sxs-lookup"><span data-stu-id="ce54b-109">Type</span></span>|<span data-ttu-id="ce54b-110">Rôle</span><span class="sxs-lookup"><span data-stu-id="ce54b-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="ce54b-111">verify</span><span class="sxs-lookup"><span data-stu-id="ce54b-111">verify</span></span> |`bool`|<span data-ttu-id="ce54b-112">Indique s’il faut vérifier le certificat SSL.</span><span class="sxs-lookup"><span data-stu-id="ce54b-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="ce54b-113">La valeur par défaut est True.</span><span class="sxs-lookup"><span data-stu-id="ce54b-113">Default is True.</span></span>|
|  <span data-ttu-id="ce54b-114">cert</span><span class="sxs-lookup"><span data-stu-id="ce54b-114">cert</span></span> |`str`| <span data-ttu-id="ce54b-115">Chemin d’accès du certificat local pour la vérification du côté client.</span><span class="sxs-lookup"><span data-stu-id="ce54b-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="ce54b-116">timeout</span><span class="sxs-lookup"><span data-stu-id="ce54b-116">timeout</span></span> |`int`| <span data-ttu-id="ce54b-117">Délai d’expiration pour l’établissement d’une connexion au serveur en secondes.</span><span class="sxs-lookup"><span data-stu-id="ce54b-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="ce54b-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="ce54b-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="ce54b-119">Indique s’il faut autoriser les redirections.</span><span class="sxs-lookup"><span data-stu-id="ce54b-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="ce54b-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="ce54b-120">max_redirects</span></span>  |`int`| <span data-ttu-id="ce54b-121">Le nombre maximal de redirections autorisées.</span><span class="sxs-lookup"><span data-stu-id="ce54b-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="ce54b-122">proxies</span><span class="sxs-lookup"><span data-stu-id="ce54b-122">proxies</span></span>  |`dict` |<span data-ttu-id="ce54b-123">Paramètres du serveur proxy.</span><span class="sxs-lookup"><span data-stu-id="ce54b-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="ce54b-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="ce54b-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="ce54b-125">Indique s’il faut lire les paramètres de proxy à partir de variables d’environnement locales.</span><span class="sxs-lookup"><span data-stu-id="ce54b-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="ce54b-126">retries</span><span class="sxs-lookup"><span data-stu-id="ce54b-126">retries</span></span>  |`int` | <span data-ttu-id="ce54b-127">Nombre total de nouvelles tentatives.</span><span class="sxs-lookup"><span data-stu-id="ce54b-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="ce54b-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="ce54b-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="ce54b-129">Active les journaux HTTP en mode débogage (False par défaut).</span><span class="sxs-lookup"><span data-stu-id="ce54b-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
