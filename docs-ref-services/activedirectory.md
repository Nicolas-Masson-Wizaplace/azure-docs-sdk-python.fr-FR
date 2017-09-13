---
title: "Bibliothèques Azure Active Directory pour Python"
description: "Consulter la documentation sur les bibliothèques de client Python pour Azure Active Directory"
keywords: "Azure, Python, Kit de développement logiciel (SDK), API, SQL, authentification, AAD, Active Directory, Graph, OAuth 2.0"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/07/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: active-directory
ms.openlocfilehash: 41234fd44fa98c1ff57287193b0437b7caca46c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-libraries-for-python"></a><span data-ttu-id="65801-104">Bibliothèques Azure Active Directory pour Python</span><span class="sxs-lookup"><span data-stu-id="65801-104">Azure Active Directory libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="65801-105">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="65801-105">Overview</span></span>

<span data-ttu-id="65801-106">Authentifiez des utilisateurs et contrôlez l’accès aux applications et aux API avec [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span><span class="sxs-lookup"><span data-stu-id="65801-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

## <a name="client-library"></a><span data-ttu-id="65801-107">Bibliothèque cliente</span><span class="sxs-lookup"><span data-stu-id="65801-107">Client library</span></span>

<span data-ttu-id="65801-108">Configurez l’authentification avec OAuth2, OpenID Connect ou Active Directory Graph et l’authentification unique [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) avec la [bibliothèque d’authentification d’Azure Active Directory (ADAL) pour Python](https://github.com/AzureAD/azure-activedirectory-library-for-python).</span><span class="sxs-lookup"><span data-stu-id="65801-108">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Python](https://github.com/AzureAD/azure-activedirectory-library-for-python).</span></span>

```bash
pip install azure-graphrbac
```

### <a name="example"></a><span data-ttu-id="65801-109">Exemple</span><span class="sxs-lookup"><span data-stu-id="65801-109">Example</span></span>
> [!NOTE]
> <span data-ttu-id="65801-110">Vous devrez modifier le paramètre de ressource à l’adresse https://graph.windows.net lors de la création de l’instance d’informations d’identification</span><span class="sxs-lookup"><span data-stu-id="65801-110">You need to change the resource parameter to https://graph.windows.net while creating the credentials instance</span></span>

```python
from azure.graphrbac import GraphRbacManagementClient
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
            'user@domain.com',      # Your user
            'my_password',          # Your password
            resource="https://graph.windows.net"
    )

tenant_id = "myad.onmicrosoft.com"

graphrbac_client = GraphRbacManagementClient(
    credentials,
    tenant_id
)
```
<span data-ttu-id="65801-111">Le code suivant crée un utilisateur. Récupérez-le en filtrant la liste et supprimez-le.</span><span class="sxs-lookup"><span data-stu-id="65801-111">The following code creates a user, get it directly and by list filtering, and then delete it.</span></span>
```python
from azure.graphrbac.models import UserCreateParameters, PasswordProfile

user = graphrbac_client.users.create(
    UserCreateParameters(
        user_principal_name="testbuddy@{}".format(MY_AD_DOMAIN),
        account_enabled=False,
        display_name='Test Buddy',
        mail_nickname='testbuddy',
        password_profile=PasswordProfile(
            password='MyStr0ngP4ssword',
            force_change_password_next_login=True
        )
    )
)
# user is a User instance
self.assertEqual(user.display_name, 'Test Buddy')

user = graphrbac_client.users.get(user.object_id)
self.assertEqual(user.display_name, 'Test Buddy')

for user in graphrbac_client.users.list(filter="displayName eq 'Test Buddy'"):
    self.assertEqual(user.display_name, 'Test Buddy')

graphrbac_client.users.delete(user.object_id)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="65801-112">Explorer les API clientes</span><span class="sxs-lookup"><span data-stu-id="65801-112">Explore the Client APIs</span></span>](/python/api/overview/azure/activedirectory/clientlibrary?)

<span data-ttu-id="65801-113">Explorez davantage d’[exemples de code Python pour Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python) à utiliser avec vos applications.</span><span class="sxs-lookup"><span data-stu-id="65801-113">Explore more [sample Python code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python) you can use in your apps.</span></span>