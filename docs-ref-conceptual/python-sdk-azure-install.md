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
ms.openlocfilehash: 5ce4ef27667d45697200eef67be92c62812b3809
ms.sourcegitcommit: 66e112df9be660354e23955b0adf3efd784ba739
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2018
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
