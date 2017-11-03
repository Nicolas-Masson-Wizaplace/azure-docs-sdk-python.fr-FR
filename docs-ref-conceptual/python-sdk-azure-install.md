---
title: Installation
description: "Installation du kit de développement logiciel (SDK) Azure pour Python"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a>Installation

## <a name="installation-with-pip"></a>Installation avec pip

Vous pouvez installer chaque bibliothèque de service Azure individuellement :

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

L’aperçu des packages peut être installé à l’aide de l’indicateur `--pre` :

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Vous pouvez également installer un ensemble de bibliothèques Azure d’une seule ligne à l’aide du méta-package `azure` .

```bash
pip install azure
```

Nous publions une version préliminaire de ce package, auquel vous pouvez accéder à l’aide de l’indicateur --pre :

```bash
pip install --pre azure
```

## <a name="install-from-github"></a>Installer à partir de GitHub

Si vous souhaitez installer `azure` à partir de la source :

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
