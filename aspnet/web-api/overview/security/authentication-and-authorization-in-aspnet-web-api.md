---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Authentification et autorisation dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Donne une vue d’ensemble de l’authentification et autorisation dans l’API Web ASP.NET.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a78606a74b2149e68e3b01f4fe204f4a13edf4b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824495"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentification et autorisation dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Vous avez créé une API web, mais maintenant que vous souhaitez contrôler l’accès. Dans cette série d’articles, nous allons examiner certaines options pour sécuriser une API web des utilisateurs non autorisés. Cette série couvre l’authentification et autorisation.

- *Authentification* est de connaître l’identité de l’utilisateur. Par exemple, Alice se connecte avec son nom d’utilisateur et le mot de passe, et le serveur utilise le mot de passe pour authentifier d’Alice.
- *Autorisation* consiste à décider si un utilisateur est autorisé à effectuer une action. Par exemple, Alice a l’autorisation pour obtenir une ressource, mais pas de créer une ressource.

Le premier article de la série donne une vue d’ensemble de l’authentification et autorisation dans l’API Web ASP.NET. Autres rubriques décrivent les scénarios d’authentification courants pour l’API Web.

> [!NOTE]
> Grâce à des personnes consulté cette série et fournissaient des commentaires précieux : Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Authentification

API Web suppose que l’authentification se produit dans l’hôte. Pour l’hébergement web, l’hôte est IIS, qui utilise des modules HTTP pour l’authentification. Vous pouvez configurer votre projet pour utiliser un des modules d’authentification intégrées à IIS ou ASP.NET, ou écrire votre propre module HTTP pour effectuer une authentification personnalisée.

Lorsque l’hôte authentifie l’utilisateur, il crée un *principal*, qui est un [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) objet qui représente le contexte de sécurité sous lequel le code est en cours d’exécution. L’hôte attache l’entité de sécurité pour le thread actuel en définissant **Thread.CurrentPrincipal**. Le principal contient associé à un **identité** objet qui contient des informations sur l’utilisateur. Si l’utilisateur est authentifié, le **Identity.IsAuthenticated** retourne de la propriété **true**. Pour les demandes anonymes, **IsAuthenticated** retourne **false**. Pour plus d’informations sur les principaux, consultez [en fonction du rôle de sécurité](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Gestionnaires de messages HTTP pour l’authentification

Au lieu d’utiliser l’hôte pour l’authentification, vous pouvez placer la logique d’authentification dans un [Gestionnaire de messages HTTP](../advanced/http-message-handlers.md). Dans ce cas, le Gestionnaire de messages examine la requête HTTP et définit le principal.

Quand devez-vous utiliser des gestionnaires de messages pour l’authentification ? Voici quelques inconvénients :

- Un module HTTP voit toutes les demandes qui passent par le pipeline ASP.NET. Un gestionnaire de messages ne voit que les demandes sont acheminées vers l’API Web.
- Vous pouvez définir des gestionnaires de messages par route, qui vous permet d’appliquer un schéma d’authentification à un itinéraire spécifique.
- Les modules HTTP sont spécifiques à IIS. Gestionnaires de messages sont indépendantes de l’hôte afin de pouvoir être utilisés avec l’hébergement de sites web et d’auto-hébergement.
- Modules HTTP participent journalisation IIS, l’audit et ainsi de suite.
- Les modules HTTP exécutent précédemment dans le pipeline. Si vous gérez l’authentification dans un gestionnaire de messages, le principal ne pas obtenir défini jusqu'à ce que le gestionnaire s’exécute. En outre, le principal rétablit le principal précédent lorsque la réponse quitte le Gestionnaire de messages.

En règle générale, si vous n’avez pas besoin de prendre en charge l’auto-hébergement, un module HTTP est une meilleure option. Si vous avez besoin prendre en charge l’auto-hébergement, envisagez d’un gestionnaire de messages.

### <a name="setting-the-principal"></a>Définition de l’entité de sécurité

Si votre application effectue une logique d’authentification personnalisée, vous devez définir le principal sur deux emplacements :

- **Thread.CurrentPrincipal**. Cette propriété est la méthode standard pour définir le principal du thread dans .NET.
- **HttpContext.Current.User**. Cette propriété est spécifique à ASP.NET.

Le code suivant montre comment définir le principal :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Pour l’hébergement web, vous devez définir l’entité de sécurité dans les deux emplacements ; Sinon, le contexte de sécurité peut-être devenir incohérent. Pour l’auto-hébergement, toutefois, **HttpContext.Current** a la valeur null. Pour vérifier votre code est indépendant de l’hôte, par conséquent, vérifiez les valeurs null avant d’affecter à **HttpContext.Current**, comme indiqué.

## <a name="authorization"></a>Autorisation

L’autorisation se produit plus loin dans le pipeline, plus proche au contrôleur. Qui vous permet de prendre des décisions plus granulaires lorsque vous vous accorder l’accès aux ressources.

- *Filtres d’autorisation* exécuter avant l’action du contrôleur. Si la demande n’est pas autorisée, le filtre retourne une réponse d’erreur et l’action n’est pas appelée.
- Au sein d’une action de contrôleur, vous pouvez obtenir l’objet principal actuel à partir de la **ApiController.User** propriété. Par exemple, vous pouvez filtrer une liste de ressources en fonction du nom d’utilisateur, en retournant uniquement les ressources qui appartiennent à cet utilisateur.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>À l’aide de la [autoriser] attribut

API Web fournit un filtre d’autorisation intégré, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Ce filtre vérifie si l’utilisateur est authentifié. Si ce n’est pas le cas, elle retourne le code d’état HTTP 401 (non autorisé), sans appeler l’action.

Vous pouvez appliquer le filtre dans le monde entier, au niveau du contrôleur ou au niveau des actions individuelles.

**Dans le monde entier**: pour limiter l’accès pour chaque contrôleur d’API Web, ajoutez le **AuthorizeAttribute** filtre à la liste de filtres globaux :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Contrôleur**: pour restreindre l’accès pour un contrôleur spécifique, ajoutez le filtre en tant qu’attribut au contrôleur :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Action**: pour restreindre l’accès pour des actions spécifiques, ajoutez l’attribut à la méthode d’action :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Ou bien, vous pouvez restreindre le contrôleur et ensuite d’autoriser l’accès anonyme à des actions spécifiques, à l’aide de la `[AllowAnonymous]` attribut. Dans l’exemple suivant, le `Post` méthode est limitée, mais le `Get` méthode autorise l’accès anonyme.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Dans les exemples précédents, le filtre permet à tout utilisateur authentifié à accéder aux méthodes restreintes ; Seuls les utilisateurs anonymes sont conservés. Vous pouvez également limiter l’accès à des utilisateurs spécifiques ou pour les utilisateurs dans des rôles spécifiques :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Le **AuthorizeAttribute** filtre pour les contrôleurs d’API Web se trouve dans le **System.Web.Http** espace de noms. Il existe un filtre similaire pour les contrôleurs MVC dans le **System.Web.Mvc** espace de noms, qui n’est pas compatible avec les contrôleurs d’API Web.


### <a name="custom-authorization-filters"></a>Filtres d’autorisation personnalisée

Pour écrire un filtre d’autorisation personnalisé, dérivé d’un de ces types :

- **AuthorizeAttribute**. Étendez cette classe pour exécuter la logique d’autorisation en fonction de l’utilisateur actuel et les rôles de l’utilisateur.
- **AuthorizationFilterAttribute**. Étendez cette classe pour exécuter la logique d’autorisation synchrone qui n’est pas nécessairement basée sur l’utilisateur actuel ou un rôle.
- **IAuthorizationFilter**. Implémentez cette interface pour exécuter une logique d’autorisation asynchrone ; par exemple, si votre logique d’autorisation effectue des appels réseau ou d’e/s asynchrones. (Si votre logique d’autorisation est dépendant du processeur, il est plus simple de dériver de **AuthorizationFilterAttribute**, car vous n’avez pas besoin d’écrire une méthode asynchrone.)

Le diagramme suivant illustre la hiérarchie de classes pour la **AuthorizeAttribute** classe.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorisation à l’intérieur d’une Action de contrôleur

Dans certains cas, vous pouvez autoriser une demande à poursuivre, mais modifier le comportement en fonction de l’entité de sécurité. Par exemple, les informations que vous retournez peuvent changer en fonction du rôle de l’utilisateur. Au sein d’une méthode de contrôleur, vous pouvez obtenir l’objet principal actuel à partir de la **ApiController.User** propriété.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
