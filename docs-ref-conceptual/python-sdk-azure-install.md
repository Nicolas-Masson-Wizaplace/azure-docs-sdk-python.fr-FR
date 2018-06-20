---
title: Installation
description: Installation du kit de développement logiciel (SDK) Azure pour Python
keywords: Azure, Python, Kit de développement logiciel (SDK), API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
ms.locfileid: "29565818"
---
# <a name="installation"></a>Installation

## <a name="which-python-and-which-version-to-use"></a>Quelle solution Python et quelle version utiliser ?
Il existe différents interpréteurs Python, par exemple :

* CPython : interpréteur Python standard le plus couramment utilisé
* PyPy : implémentation rapide et conforme autre que CPython
* IronPython : interpréteur Python qui s'exécute sur .Net/CLR
* Jython : interpréteur Python qui s’exécute sur la Machine Virtuelle Java

**CPython** version 2.7 ou 3.4+ et PyPy 5.4.0 sont testés et pris en charge par le kit de développement logiciel Microsoft Azure SDK pour Python.

## <a name="where-to-get-python"></a>Où télécharger Python ?
Il existe plusieurs façons de télécharger CPython :

* Directement à partir de [Python](https://www.python.org/)
* À partir d’un distributeur digne de confiance comme [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) ou [ActiveState](https://www.activestate.com/)
* Génération à partir de la source !

Sauf en cas de nécessité particulière, nous vous recommandons d’opter pour l’une des deux premières options.

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