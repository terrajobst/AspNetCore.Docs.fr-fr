---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prévention des attaques par falsification de Cross-Site Request dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Décrit comment implémenter des mesures anti-CSRF dans ASP.NET Web API et l’attaque de falsification de requête intersites.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: cd7d978190d28a028285746781a380d9bb5f91d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832200"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Prévention des attaques par falsification de Cross-Site Request dans l’API Web ASP.NET
====================
par [Mike Wasson](https://github.com/MikeWasson)

Falsification de demande intersites (CSRF) est une attaque dans laquelle un site malveillant envoie une demande à un site vulnérable auquel l’utilisateur est actuellement connecté dans

Voici un exemple d’une attaque CSRF :

1. Un utilisateur se connecte à `www.example.com` à l’aide de l’authentification par formulaire.
2. Le serveur authentifie l’utilisateur. La réponse du serveur inclut un cookie d’authentification.
3. Sans la déconnexion, l’utilisateur visite un site web malveillant. Ce site malveillant contient le formulaire HTML suivant : 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Notez que l’action de formulaire valide vers le site vulnérable, pas pour le site malveillant. Il s’agit de la partie « cross-site » de falsification de requête Intersites.
4. L’utilisateur clique sur le bouton Envoyer. Le navigateur inclut le cookie d’authentification avec la demande.
5. La demande s’exécute sur le serveur avec le contexte de l’utilisateur d’authentification et peut faire tout ce qu’un utilisateur authentifié est autorisé à faire.

Bien que cet exemple nécessite l’utilisateur clique sur le bouton du formulaire, la page malveillante pourrait aussi facilement exécuter un script qui envoie le formulaire automatiquement. En outre, à l’aide de SSL n’empêche pas une attaque CSRF, car le site malveillant peut envoyer une demande de « https:// ».

En règle générale, les attaques CSRF sont possibles contre les sites web qui utilisent des cookies pour l’authentification, étant donné que les navigateurs envoient tous les cookies utiles pour le site de destination. Toutefois, les attaques CSRF ne sont pas limités à exploiter les cookies. Par exemple, l’authentification de base et Digest sont également vulnérables. Après un utilisateur se connecte avec l’authentification de base ou Digest. le navigateur envoie automatiquement les informations d’identification jusqu'à ce que la session se termine.

## <a name="anti-forgery-tokens"></a>Les jetons anti-contrefaçon

Pour aider à empêcher les attaques CSRF, ASP.NET MVC utilise des jetons anti-contrefaçon, également appelés *demander des jetons de vérification*.

1. Le client demande une page HTML qui contienne un formulaire.
2. Le serveur inclut deux jetons dans la réponse. Un jeton est envoyé en tant que cookie. L’autre est placée dans un champ de formulaire masqué. Les jetons sont générés au hasard, pour un adversaire ne peuvent pas deviner les valeurs.
3. Lorsque le client envoie le formulaire, il doit envoyer les deux jetons sur le serveur. Le client envoie le jeton de cookie en tant que cookie, et il envoie le jeton de formulaire dans les données du formulaire. (Un client de navigateur fait automatiquement lorsque l’utilisateur envoie le formulaire.)
4. Si une demande n’inclut pas les deux jetons, le serveur n’autorise pas la demande.

Voici un exemple d’un formulaire HTML avec un jeton de formulaire masqué :

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Les jetons anti-contrefaçon fonctionnent, car la page malveillante ne peut pas lire des jetons de l’utilisateur, en raison des stratégies de même origine. ([Des stratégies de même origine](http://www.w3.org/Security/wiki/Same_Origin_Policy) empêcher les documents hébergés sur deux sites différents d’accéder au contenu de l’autre. Par conséquent, dans l’exemple précédent, la page malveillante peut envoyer des demandes à exemple.com, mais il ne peut pas lire la réponse.)

Pour empêcher les attaques CSRF, utilisez des jetons anti-contrefaçon avec n’importe quel protocole d’authentification où le navigateur en mode silencieux envoie des informations d’identification une fois que l’utilisateur se connecte. Cela inclut les protocoles d’authentification basée sur les cookies, telles que l’authentification par formulaire, mais aussi protocoles, tels que l’authentification de base et Digest.

Vous devez exiger des jetons anti-contrefaçon pour toutes les méthodes nonsafe (POST, PUT, DELETE). En outre, assurez-vous que les méthodes sans échec (GET, HEAD) n’ont pas de tous les effets. En outre, si vous activez la prise en charge des domaines, tels que CORS ou JSONP, même sans échec méthodes telles que GET sont potentiellement vulnérables aux attaques CSRF, l’attaquant de lire des données potentiellement sensibles.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Les jetons anti-contrefaçon dans ASP.NET MVC

Pour ajouter les jetons anti-contrefaçon à une page Razor, utilisez le **HtmlHelper.AntiForgeryToken** méthode d’assistance :

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Cette méthode ajoute le champ de formulaire masqué et définit également le jeton de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF et AJAX

Le jeton de formulaire peut être un problème pour les demandes AJAX, car une requête AJAX peut envoyer des données JSON, pas les données de formulaire HTML. Une solution consiste à envoyer les jetons dans un en-tête HTTP personnalisé. Le code suivant utilise la syntaxe Razor pour générer les jetons, puis ajoute les jetons à une demande AJAX. Les jetons sont générés au niveau du serveur en appelant **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Lorsque vous traitez la demande, extrayez les jetons à partir de l’en-tête de demande. Appelez ensuite la **AntiForgery.Validate** méthode pour valider les jetons. Le **Validate** méthode lève une exception si les jetons ne sont pas valides.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
