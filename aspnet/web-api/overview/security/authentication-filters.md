---
uid: web-api/overview/security/authentication-filters
title: Filtres d’authentification dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Un filtre d’authentification est un composant qui s’authentifie une requête HTTP. L’API Web 2 et MVC 5 prennent en charge les filtres d’authentification, mais elles diffèrent légèrement...
ms.author: aspnetcontent
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 6cad52e0454d685c6e96746524fbbad21e1c274d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839815"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtres d’authentification dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

> Un filtre d’authentification est un composant qui s’authentifie une requête HTTP. L’API Web 2 et MVC 5 prennent en charge les filtres d’authentification, mais elles diffèrent légèrement, principalement dans les conventions d’affectation de noms pour l’interface de filtre. Cette rubrique décrit les filtres d’authentification Web API.


Filtres d’authentification vous permettent de définir un schéma d’authentification pour des contrôleurs ou des actions. De cette façon, votre application peut prendre en charge différents mécanismes d’authentification pour les différentes ressources HTTP.

Dans cet article, je vais montrer le code à partir de la [l’authentification de base](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) exemple sur [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). L’exemple montre un filtre d’authentification qui implémente le schéma d’authentification de l’accès de base HTTP (RFC 2617). Le filtre est implémenté dans une classe nommée `IdentityBasicAuthenticationAttribute`. Je ne sont pas tout le code de l’exemple, afficher uniquement les parties qui montrent comment écrire un filtre d’authentification.

## <a name="setting-an-authentication-filter"></a>Définition d’un filtre d’authentification

Comme d’autres filtres, filtres d’authentification peuvent être appliqué par contrôleur, par action ou globalement à tous les contrôleurs d’API Web.

Pour appliquer un filtre d’authentification à un contrôleur, décorez la classe de contrôleur avec l’attribut de filtre. Le code suivant définit la `[IdentityBasicAuthentication]` filtre sur une classe de contrôleur, ce qui permet l’authentification de base pour toutes les actions du contrôleur.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Pour appliquer le filtre à une seule action, décorer l’action avec le filtre. Le code suivant définit la `[IdentityBasicAuthentication]` filtre sur le contrôleur `Post` (méthode).

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Pour appliquer le filtre à tous les contrôleurs d’API Web, ajoutez-le à **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implémentation d’un filtre de l’authentification d’API Web

Dans l’API Web, les filtres d’authentification implémentent le [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interface. Ils doivent également hériter **System.Attribute**, afin d’être appliquées en tant qu’attributs.

Le **IAuthenticationFilter** interface possède deux méthodes :

- **AuthenticateAsync** authentifie la demande en validant les informations d’identification dans la demande, le cas échéant.
- **ChallengeAsync** ajoute une demande d’authentification à la réponse HTTP, si nécessaire.

Ces méthodes correspondent aux flux d’authentification défini dans [RFC 2612](http://tools.ietf.org/html/rfc2616) et [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Le client envoie des informations d’identification dans l’en-tête d’autorisation. Cela se produit généralement une fois que le client reçoit une réponse 401 (non autorisé) à partir du serveur. Toutefois, un client peut envoyer des informations d’identification avec les demandes, pas juste après l’obtention d’une erreur 401.
2. Si le serveur n’accepte pas les informations d’identification, elle retourne une réponse 401 (non autorisé). La réponse inclut un en-tête Www-Authenticate qui contient un ou plusieurs défis. Chaque demande spécifie un schéma d’authentification reconnu par le serveur.

Le serveur peut également retourner 401 à partir d’une demande anonyme. En fait, c’est en général, comment le processus d’authentification est lancé :

1. Le client envoie une demande anonyme.
2. Le serveur renvoie 401.
3. Les clients renvoie la demande avec les informations d’identification.

Ce flux comprend à la fois *authentification* et *autorisation* étapes.

- Authentification prouve l’identité du client.
- L’autorisation détermine si le client peut accéder à une ressource particulière.

Dans l’API Web, les filtres d’authentification gérer l’authentification, mais pas l’autorisation. L’autorisation doit être effectuée par un filtre d’autorisation ou à l’intérieur de l’action du contrôleur.

Voici le flux dans le pipeline de l’API Web 2 :

1. Avant d’appeler une action, API Web crée une liste des filtres d’authentification pour cette action. Cela inclut les filtres avec la portée de l’action, la portée de contrôleur et portée globale.
2. Appels d’API Web **AuthenticateAsync** sur chaque filtre dans la liste. Chaque filtre peut valider les informations d’identification dans la demande. Si un filtre valide avec succès les informations d’identification, le filtre crée un **IPrincipal** et l’attache à la demande. Un filtre peut également déclencher une erreur à ce stade. Dans ce cas, le reste du pipeline ne s’exécute pas.
3. En supposant qu’aucune erreur, la demande transite via le reste du pipeline.
4. Enfin, les API Web appelle chaque filtre d’authentification **ChallengeAsync** (méthode). Filtres utilisent cette méthode pour ajouter un défi à la réponse, si nécessaire. Généralement (mais pas toujours) qui serait se produisent en réponse à une erreur 401.

Les diagrammes suivants montrent les deux cas. Dans le premier, le filtre d’authentification s’authentifie correctement la demande, un filtre d’autorisation autorise la demande et l’action du contrôleur retourne 200 (OK).

![](authentication-filters/_static/image1.png)

Dans le deuxième exemple, le filtre d’authentification authentifie la demande, mais le filtre d’autorisation retourne 401 (non autorisé). Dans ce cas, l’action du contrôleur n’est pas appelée. Le filtre d’authentification ajoute un en-tête Www-Authenticate dans la réponse.

![](authentication-filters/_static/image2.png)

Autres combinaisons sont possibles&mdash;par exemple, si l’action du contrôleur autorise les demandes anonymes, vous pouvez avoir un filtre d’authentification, mais aucune autorisation.

## <a name="implementing-the-authenticateasync-method"></a>Implémentation de la méthode AuthenticateAsync

Le **AuthenticateAsync** méthode tente d’authentifier la demande. Voici la signature de méthode :

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Le **AuthenticateAsync** méthode doit effectuer l’une des opérations suivantes :

1. Nothing (nulle).
2. Créer un **IPrincipal** et définissez-le sur la demande.
3. Définir un résultat d’erreur.

Option (1) signifie que la demande n’a pas d’informations d’identification qui comprend le filtre. Option (2) signifie que le filtre a été authentifié la demande. Option (3) signifie que la demande avait des informations d’identification non valides (par exemple, le mot de passe incorrect), ce qui déclenche une réponse d’erreur.

Voici un aperçu général d’implémentation **AuthenticateAsync**.

1. Recherchez des informations d’identification dans la demande.
2. S’il n’y a aucune information d’identification, ne rien faire et retourner (nulle).
3. Si les informations d’identification, mais le filtre ne reconnaît pas le schéma d’authentification, ne rien faire et retourner (nulle). Un autre filtre dans le pipeline peut comprendre le schéma.
4. S’il existe des informations d’identification qui comprend le filtre, essayez de les authentifier.
5. Si les informations d’identification sont incorrectes, retourner 401 en définissant `context.ErrorResult`.
6. Si les informations d’identification sont valides, créer un **IPrincipal** et définissez `context.Principal`.

Le code de suivi montre le **AuthenticateAsync** méthode à partir de la [l’authentification de base](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) exemple. Les commentaires indiquent chaque étape. Le code illustre plusieurs types d’erreur : l’en-tête d’autorisation un sans informations d’identification, les informations d’identification incorrecte, et nom d’utilisateur/mot de passe incorrect.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Définition d’un résultat d’erreur

Si les informations d’identification ne sont pas valides, le filtre doit définir `context.ErrorResult` à un **IHttpActionResult** qui crée une réponse d’erreur. Pour plus d’informations sur **IHttpActionResult**, consultez [résultats d’Action dans Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

L’exemple de l’authentification de base inclut un `AuthenticationFailureResult` classe qui convient à cet effet.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implémentation ChallengeAsync

L’objectif de la **ChallengeAsync** méthode consiste à ajouter des demandes d’authentification à la réponse, si nécessaire. Voici la signature de méthode :

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

La méthode est appelée sur chaque filtre d’authentification dans le pipeline de requête.

Il est important de comprendre que **ChallengeAsync** est appelée *avant* la réponse HTTP est créé et peut-être même avant l’exécution d’action de contrôleur. Lorsque **ChallengeAsync** est appelée, `context.Result` contient un **IHttpActionResult**, qui est utilisé ultérieurement pour créer la réponse HTTP. Par conséquent, lorsque **ChallengeAsync** est appelée, vous ne connaissez rien sur la réponse HTTP encore. Le **ChallengeAsync** méthode doit remplacer la valeur d’origine `context.Result` avec un nouveau **IHttpActionResult**. Cela **IHttpActionResult** devez encapsuler l’original `context.Result`.

![](authentication-filters/_static/image3.png)

Je vais l’appeler la version d’origine **IHttpActionResult** le *résultats interne*et le nouveau **IHttpActionResult** le *résultat externe*. Le résultat externe doit les opérations suivantes :

1. Appeler le résultat interne pour créer la réponse HTTP.
2. Examiner la réponse.
3. Ajouter une demande d’authentification à la réponse, si nécessaire.

L’exemple suivant provient de l’exemple de l’authentification de base. Il définit un **IHttpActionResult** pour le résultat externe.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Le `InnerResult` propriété contient l’exception interne **IHttpActionResult**. Le `Challenge` propriété représente un en-tête Www-Authenticate. Notez que **ExecuteAsync** appelle d’abord `InnerResult.ExecuteAsync` pour créer la réponse HTTP, puis ajoute le défi si nécessaire.

Vérifiez le code de réponse avant d’ajouter le défi. La plupart des schémas d’authentification uniquement ajouter un défi si la réponse est 401, comme illustré ici. Toutefois, certains schémas d’authentification ajoutez un défi à une réponse de réussite. Par exemple, consultez [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Étant donné la `AddChallengeOnUnauthorizedResult` classe, le code réel de **ChallengeAsync** est simple. Vous venez de créer le résultat et l’attachez `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Remarque : L’authentification de base extraits cette logique un peu, en le plaçant dans une méthode d’extension.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinaison de filtres d’authentification avec authentification au niveau de l’hôte

« Authentification au niveau hôte » est l’authentification réalisée par l’hôte (comme IIS), avant du framework atteint l’API Web demande.

Souvent, vous souhaitez activer l’authentification au niveau de l’hôte pour le reste de votre application, mais la désactiver pour vos contrôleurs d’API Web. Par exemple, un scénario classique consiste à activer l’authentification par formulaire au niveau de l’hôte, mais utiliser l’authentification basée sur le jeton pour l’API Web.

Pour désactiver l’authentification au niveau de l’hôte à l’intérieur du pipeline API Web, appelez `config.SuppressHostPrincipal()` dans votre configuration. Cela entraîne des API Web supprimer le **IPrincipal** à partir de toute demande qui entre dans le pipeline API Web. En effet, il &quot;Annuler-authentifie&quot; la demande.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Ressources supplémentaires

[Filtres de sécurité d’API Web ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
