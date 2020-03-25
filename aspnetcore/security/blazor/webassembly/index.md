---
title: ASP.NET Core sécurisé Blazor webassembly
author: guardrex
description: Apprenez à sécuriser Blazor applications WebAssemlby en tant qu’applications à page unique (SPAs).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: 652d4c61110f786396d9d5af4f131b817c40e333
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219244"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>ASP.NET Core sécurisé Blazor webassembly

Par [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor applications webassembly sont sécurisées de la même façon que les applications à page unique (SPAs). Il existe plusieurs approches pour authentifier les utilisateurs dans la base de code, mais l’approche la plus courante et la plus complète consiste à utiliser une implémentation basée sur le [protocole oAuth 2,0](https://oauth.net/), par exemple [Open ID Connect (OIDC)](https://openid.net/connect/).

## <a name="authentication-library"></a>Bibliothèque d’authentification

Blazor webassembly prend en charge l’authentification et l’autorisation d’applications à l’aide de OIDC via la bibliothèque `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. La bibliothèque fournit un ensemble de primitives pour l’authentification en toute transparence par rapport aux principaux de ASP.NET Core. La bibliothèque intègre ASP.NET Core identité avec la prise en charge des autorisations d’API basée sur le [serveur d’identité](https://identityserver.io/). La bibliothèque peut s’authentifier auprès d’un fournisseur d’identité (IP) tiers qui prend en charge OIDC, qui sont appelés fournisseurs OpenID.

La prise en charge de l’authentification dans Blazor webassembly est basée sur la bibliothèque *OIDC-client. js* , qui est utilisée pour gérer les détails du protocole d’authentification sous-jacent.

D’autres options d’authentification de la fonction de l’interauthentification existent, telles que l’utilisation de cookies SameSite. Toutefois, la conception d’ingénierie de Blazor webassembly est réglée sur oAuth et OIDC comme meilleure option pour l’authentification dans Blazor applications webassembly. [L’authentification basée sur les jetons](xref:security/anti-request-forgery#token-based-authentication) basée sur des [jetons Web JSON (jetons JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a été choisie via [l’authentification basée](xref:security/anti-request-forgery#cookie-based-authentication) sur les cookies pour des raisons fonctionnelles et de sécurité :

* L’utilisation d’un protocole basé sur les jetons offre une surface d’attaque plus réduite, car les jetons ne sont pas envoyés dans toutes les demandes.
* Les points de terminaison de serveur ne nécessitent pas de protection contre la [falsification de requête intersites (CSRF)](xref:security/anti-request-forgery) , car les jetons sont envoyés explicitement. Cela vous permet d’héberger des applications Blazor webassembly avec des applications MVC ou de pages Razor.
* Les jetons ont des autorisations plus étroites que les cookies. Par exemple, les jetons ne peuvent pas être utilisés pour gérer le compte d’utilisateur ou modifier le mot de passe d’un utilisateur, sauf si une telle fonctionnalité est implémentée de manière explicite.
* Les jetons ont une durée de vie brève, par défaut d’une heure, qui limite la fenêtre d’attaque. Les jetons peuvent également être révoqués à tout moment.
* Les jetons JWT autonomes offrent des garanties au client et au serveur sur le processus d’authentification. Par exemple, un client a la possibilité de détecter et de valider que les jetons qu’il reçoit sont légitimes et ont été émis dans le cadre d’un processus d’authentification donné. Si un tiers tente de basculer un jeton au milieu du processus d’authentification, le client peut détecter le jeton commuté et éviter de l’utiliser.
* Les jetons avec oAuth et OIDC ne reposent pas sur l’agent utilisateur qui se comporte correctement pour garantir la sécurité de l’application.
* Les protocoles basés sur les jetons, tels que oAuth et OIDC, permettent d’authentifier et d’autoriser des applications hébergées et autonomes avec le même ensemble de caractéristiques de sécurité.

## <a name="authentication-process-with-oidc"></a>Processus d’authentification avec OIDC

La bibliothèque `Microsoft.AspNetCore.Components.WebAssembly.Authentication` offre plusieurs primitives pour implémenter l’authentification et l’autorisation à l’aide de OIDC. En général, l’authentification fonctionne comme suit :

* Lorsqu’un utilisateur anonyme sélectionne le bouton de connexion ou demande une page avec l’attribut [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) appliqué, l’utilisateur est redirigé vers la page de connexion de l’application (`/authentication/login`).
* Dans la page de connexion, la bibliothèque d’authentification prépare une redirection vers le point de terminaison d’autorisation. Le point de terminaison d’autorisation est en dehors de l’application Blazor webassembly et peut être hébergé à une origine distincte. Le point de terminaison est chargé de déterminer si l’utilisateur est authentifié et d’émettre un ou plusieurs jetons en réponse. La bibliothèque d’authentification fournit un rappel de connexion pour recevoir la réponse d’authentification.
  * Si l’utilisateur n’est pas authentifié, l’utilisateur est redirigé vers le système d’authentification sous-jacent, qui est généralement ASP.NET Core identité.
  * Si l’utilisateur a déjà été authentifié, le point de terminaison d’autorisation génère les jetons appropriés et redirige le navigateur vers le point de terminaison de rappel de connexion (`/authentication/login-callback`).
* Lorsque l' Blazor application webassembly charge le point de terminaison de rappel de connexion (`/authentication/login-callback`), la réponse d’authentification est traitée.
  * Si le processus d’authentification se termine correctement, l’utilisateur est authentifié et éventuellement renvoyé à l’URL protégée d’origine que l’utilisateur a demandée.
  * Si le processus d’authentification échoue pour une raison quelconque, l’utilisateur est envoyé à la page Échec de la connexion (`/authentication/login-failed`) et une erreur s’affiche.
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a>Options pour les applications hébergées et les fournisseurs de connexion tiers

Lors de l’authentification et de l’autorisation d’une application Blazor webassembly hébergée auprès d’un fournisseur tiers, plusieurs options sont disponibles pour l’authentification de l’utilisateur. Celui que vous choisissez dépend de votre scénario.

Pour plus d’informations, consultez <xref:security/authentication/social/additional-claims>.

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a>Authentifier les utilisateurs pour appeler uniquement des API tierces protégées

Authentifiez l’utilisateur avec un fluide oAuth côté client par rapport au fournisseur d’API tiers :

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 Dans ce scénario :

* Le serveur hébergeant l’application ne joue aucun rôle.
* Les API sur le serveur ne peuvent pas être protégées.
* L’application ne peut appeler que des API tierces protégées.

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a>Authentifier les utilisateurs auprès d’un fournisseur tiers et appeler des API protégées sur le serveur hôte et le tiers

Configurez l’identité avec un fournisseur de connexion tiers. Obtenez les jetons requis pour l’accès de l’API tierce et stockez-les.

Lorsqu’un utilisateur se connecte, l’identité collecte les jetons d’accès et d’actualisation dans le cadre du processus d’authentification. À ce stade, il existe deux approches disponibles pour effectuer des appels d’API à des API tierces.

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a>Utiliser un jeton d’accès au serveur pour récupérer le jeton d’accès tiers

Utilisez le jeton d’accès généré sur le serveur pour récupérer le jeton d’accès tiers à partir d’un point de terminaison d’API serveur. À partir de là, utilisez le jeton d’accès tiers pour appeler des ressources d’API tierces directement à partir de l’identité sur le client.

Nous ne recommandons pas cette approche. Cette approche nécessite le traitement du jeton d’accès tiers comme s’il avait été généré pour un client public. Dans les termes oAuth, l’application publique n’a pas de clé secrète client, car elle ne peut pas être approuvée pour stocker des secrets en toute sécurité, et le jeton d’accès est généré pour un client confidentiel. Un client confidentiel est un client qui a une clé secrète client et est supposé être en mesure de stocker des secrets en toute sécurité.

* Le jeton d’accès tiers peut recevoir des étendues supplémentaires pour effectuer des opérations sensibles en fonction du fait que le tiers a émis le jeton pour un client plus fiable.
* De même, les jetons d’actualisation ne doivent pas être émis pour un client qui n’est pas approuvé, car cela donne au client un accès illimité, sauf si d’autres restrictions sont mises en place.

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a>Effectuer des appels d’API à partir du client vers l’API du serveur afin d’appeler des API tierces

Effectuez un appel d’API à partir du client vers l’API serveur. À partir du serveur, récupérez le jeton d’accès pour la ressource d’API tierce et émettez l’appel nécessaire.

Bien que cette approche nécessite un saut de réseau supplémentaire par le biais du serveur pour appeler une API tierce, elle a finalement pour résultat une expérience plus sûre :

* Le serveur peut stocker des jetons d’actualisation et s’assurer que l’application ne perd pas l’accès aux ressources tierces.
* L’application ne peut pas perdre les jetons d’accès du serveur qui peuvent contenir des autorisations plus sensibles.
