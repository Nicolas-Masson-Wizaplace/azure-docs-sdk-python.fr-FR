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
# <a name="operation-config"></a>Configuration de l’opération 

Les méthodes concernant les opérations ont des paramètres supplémentaires qui peuvent être fournis dans les `kwargs`. Il s’agit d’operation_config.

Les options de configuration de l’opération sont :

|Nom du paramètre|Type|Rôle|
|----------------------|------|---------------|
| verify |`bool`|Indique s’il faut vérifier le certificat SSL. La valeur par défaut est True.|
|  cert |`str`| Chemin d’accès du certificat local pour la vérification du côté client.|
|  timeout |`int`| Délai d’expiration pour l’établissement d’une connexion au serveur en secondes.|
|  allow_redirects |`bool` | Indique s’il faut autoriser les redirections.|
|  max_redirects  |`int`| Le nombre maximal de redirections autorisées.|
|  proxies  |`dict` |Paramètres du serveur proxy.|
|  use_env_proxies |`bool` |Indique s’il faut lire les paramètres de proxy à partir de variables d’environnement locales.|
|  retries  |`int` | Nombre total de nouvelles tentatives.|
|  enable_http_logger | `bool`| Active les journaux d’activité HTTP en mode débogage (False par défaut).|
