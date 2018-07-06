---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Serveur de d’autorisation OAuth 2.0 OWIN | Microsoft Docs
author: hongyes
description: Ce didacticiel vous guide sur la façon d’implémenter un serveur d’autorisation OAuth 2.0 à l’aide d’intergiciel (middleware) OWIN OAuth. Ceci est un didacticiel avancé que seule Group...
ms.author: aspnetcontent
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e3b5b37b4f22f3c59d3c1f4043e9b52e46a8926b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828417"
---
<a name="owin-oauth-20-authorization-server"></a>Serveur de d’autorisation OAuth 2.0 OWIN
====================
par [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous guide sur la façon d’implémenter un serveur d’autorisation OAuth 2.0 à l’aide d’intergiciel (middleware) OWIN OAuth. Il s’agit d’un didacticiel avancé qui ne présente les étapes pour créer un serveur d’autorisation de OWIN OAuth 2.0. Cela n’est pas un didacticiel étape par étape. [Télécharger l’exemple de code](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Ce plan ne doit pas être destiné à utiliser pour la création d’une application de production sécurisé. Ce didacticiel vise à fournir uniquement un plan d’implémentation d’un serveur d’autorisation OAuth 2.0 à l’aide d’intergiciel (middleware) OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Versions des logiciels
> 
> | **Indiqué dans le didacticiel** | **Fonctionne également avec** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express pour Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 avec la dernière mise à jour devrait fonctionner, mais le didacticiel n’a pas été testé avec lui et des sélections de menu et les boîtes de dialogue sont différents. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à [projet Katana sur GitHub](https://github.com/aspnet/AspNetKatana/). Pour les questions et commentaires concernant ce didacticiel, consultez la section des commentaires en bas de la page.


Le [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) permet à une application tierce obtenir un accès limité à un service HTTP. Au lieu d’utiliser les informations d’identification du propriétaire des ressources pour accéder à une ressource protégée, le client obtient un jeton d’accès (qui est une chaîne qui dénote une étendue spécifique, de durée de vie et d’autres attributs d’accès). Jetons d’accès sont émis pour les clients tiers par un serveur d’autorisation avec l’approbation du propriétaire de la ressource.

Ce didacticiel couvre :

- Comment créer un serveur d’autorisation pour prendre en charge d’autorisation quatre types d’octroi et jetons d’actualisation :
    - Octroi de code d’autorisation
    - Octroi implicite
    - Octroi des informations d’identification mot de passe de propriétaire de la ressource
    - Octroi des informations d’identification client
- Création d’un serveur de ressource qui est protégé par un jeton d’accès.
- Création de clients de OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prérequis

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) ou la version gratuite [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), comme indiqué dans **Versions logicielles** en haut de la page.
- Vous êtes familiarisé avec OWIN. Consultez [mise en route avec le projet Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) et [Nouveautés OWIN et Katana](index.md).
- Vous êtes familiarisé avec [OAuth](http://tools.ietf.org/html/rfc6749) la terminologie, y compris [rôles](http://tools.ietf.org/html/rfc6749#section-1.1), [flux du protocole](http://tools.ietf.org/html/rfc6749#section-1.2), et [octroi d’autorisation](http://tools.ietf.org/html/rfc6749#section-1.3). [Présentation d’OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) constitue une bonne introduction.

## <a name="create-an-authorization-server"></a>Créer un serveur d’autorisation

Dans ce didacticiel, nous sera à peu près esquissez comment utiliser [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) et ASP.NET MVC pour créer un serveur d’autorisation. Nous espérons que bientôt proposer un téléchargement pour l’exemple terminé, que ce didacticiel n’inclut pas chaque étape. Commencez par créer une application web vide nommée *AuthorizationServer* et installez les packages suivants :

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (ou toute autre connexion de réseau social de package telles que Microsoft.owin.Security.Facebook contient)

Ajouter un [classe de démarrage OWIN](owin-startup-class-detection.md) sous le dossier racine du projet nommé *démarrage*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Créer un *application\_Démarrer* dossier. Sélectionnez le *application\_Démarrer* dossier et utilisez Maj + Alt + A pour ajouter la version téléchargée de le *AuthorizationServer\App\_Start\Startup.Auth.cs* fichier.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Le code ci-dessus autorise les cookies et l’authentification de Google, qui sont utilisés par le serveur d’autorisation lui-même pour gérer les comptes de connexion/externe de l’application.

Le `UseOAuthAuthorizationServer` méthode d’extension consiste à configurer le serveur d’autorisation. Les options d’installation sont :

- `AuthorizeEndpointPath`: Le chemin d’accès de requête où les applications clientes redirigeront l’agent utilisateur afin d’obtenir les utilisateurs de consentement pour émettre un jeton ou du code. Il doit commencer par une barre oblique, par exemple, «`/Authorize`».
- `TokenEndpointPath`: Les applications de client de chemin d’accès de requête communiquent directement pour obtenir le jeton d’accès. Il doit commencer par une barre oblique, par exemple « /Token ». Si le client reçoit un [client\_secret](http://tools.ietf.org/html/rfc6749#appendix-A.2), elle doit être fournie à ce point de terminaison.
- `ApplicationCanDisplayErrors`: Défini sur `true` si l’application web veut générer une page d’erreur personnalisée pour les erreurs de validation client sur `/Authorize` point de terminaison. Ce paramètre est uniquement nécessaire pour les cas où le navigateur n’est pas redirigé sauvegarder à l’application cliente, par exemple, lorsque le `client_id` ou `redirect_uri` sont incorrectes. Le `/Authorize` point de terminaison doit s’attendre à voir le « oauth. Erreur «, « oauth. ErrorDescription » et « oauth. Propriétés de ErrorUri » sont ajoutées à l’environnement OWIN. 

    > [!NOTE]
    > Si ce n’est pas le cas, la valeur est true, le serveur d’autorisation renverra une page d’erreur par défaut avec les détails de l’erreur.
- `AllowInsecureHttp`: True pour autoriser l’autoriser et demandes de jetons pour arriver sur des adresses URI HTTP et pour autoriser le trafic entrant `redirect_uri` autoriser les paramètres de la demande d’avoir des adresses URI HTTP. 

    > [!WARNING]
    > Sécurité - Il s’agit uniquement pour le développement.
- `Provider`: L’objet fourni par l’application pour traiter les événements déclenchés par l’intergiciel (middleware) serveur d’autorisation. L’application peut implémenter totalement l’interface, ou il peut créer une instance de `OAuthAuthorizationServerProvider` et attribuer des délégués nécessaire pour les flux OAuth prend en charge par ce serveur.
- `AuthorizationCodeProvider`: Génère un code d’autorisation à usage unique pour revenir à l’application cliente. Sécurisation de l’application pour le serveur OAuth soit **doit** fournir une instance de `AuthorizationCodeProvider` où le jeton produit par le `OnCreate/OnCreateAsync` événement est considéré comme valide pour un seul appel à `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Génère un jeton d’actualisation qui peut-être être utilisé pour produire un nouveau jeton d’accès si nécessaire. Si ne pas fourni le serveur d’autorisation ne retourne pas les jetons d’actualisation à partir de la `/Token` point de terminaison.

## <a name="account-management"></a>Gestion des comptes

OAuth ne soucie pas où et comment gérer les informations de votre compte d’utilisateur. Il a [ASP.NET Identity](../../../identity/index.md) qui en est responsable. Dans ce didacticiel, nous simplifier le code de gestion de compte et vérifiez simplement que cet utilisateur peut ouvrir une session à l’aide de l’intergiciel OWIN cookie. Voici le code exemple simplifié pour le `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` est utilisé pour valider le client avec son URL de redirection inscrits. `ValidateClientAuthentication` vérifie l’en-tête du schéma de base et le corps de formulaire pour obtenir des informations d’identification du client.

La page de connexion est indiquée ci-dessous :

![](owin-oauth-20-authorization-server/_static/image1.png)

Passez en revue OAuth 2 l’IETF [octroi de Code d’autorisation](http://tools.ietf.org/html/rfc6749#section-4.1) section maintenant. 

**Fournisseur** (dans le tableau ci-dessous) est [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Fournisseur, qui est de type `OAuthAuthorizationServerProvider`, qui contient tous les événements de serveur OAuth. 

| Étapes du flux à partir de la section d’octroi de Code d’autorisation | Téléchargement de l’exemple effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) le client lance le flux en dirigeant l’agent utilisateur du propriétaire des ressources au point de terminaison d’autorisation. Le client inclut son identificateur client, étendue demandée, état local et un URI de redirection auquel le serveur d’autorisation envoie l’agent utilisateur une fois l’accès est accordé (ou refusé). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) le serveur d’autorisation authentifie le propriétaire de la ressource (par le biais de l’agent utilisateur) et détermine si le propriétaire de la ressource accorde ou refuse la demande d’accès du client. | **&lt;Si l’utilisateur accorde l’accès&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) en supposant que le propriétaire de la ressource accorde l’accès, le serveur d’autorisation redirige l’agent utilisateur vers le client à l’aide de l’URI de redirection fourni précédemment (dans la demande ou lors de l’inscription du client). ... |  |
|  |  |
| (D) le client demande un jeton d’accès à partir du point de terminaison de jeton du serveur d’autorisation en incluant le code d’autorisation reçu à l’étape précédente. Lors de sa demande, le client s’authentifie auprès du serveur d’autorisation. Le client inclut l’URI de redirection utilisé pour obtenir le code d’autorisation pour la vérification. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Un exemple d’implémentation pour `AuthorizationCodeProvider.CreateAsync` et `ReceiveAsync` pour contrôler la création et la validation de code d’autorisation est indiqué ci-dessous.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Le code ci-dessus utilise un dictionnaire simultané en mémoire pour stocker le ticket d’identité et de code et de restaurer l’identité après avoir reçu le code. Dans une application réelle, il serait remplacé par un magasin de données persistantes. Le point de terminaison d’autorisation est propriétaire de la ressource accorder l’accès au client. En règle générale, il a besoin d’une interface utilisateur pour autoriser l’utilisateur à cliquer sur un bouton et à confirmer l’octroi. Intergiciel (middleware) OWIN OAuth permet au code d’application gérer le point de terminaison d’autorisation. Dans notre exemple d’application, nous utilisons un contrôleur MVC appelé `OAuthController` afin de le gérer. Voici l’exemple d’implémentation :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

Le `Authorize` action vérifie d’abord si l’utilisateur s’est connecté au serveur d’autorisation. Si ce n’est pas le cas, le middleware d’authentification défis en matière de l’appelant de s’authentifier en utilisant le cookie de « Application » et redirige vers la page de connexion. (Voir le code en surbrillance ci-dessus). Si l’utilisateur s’est connecté, il restitue la vue Authorize, comme indiqué ci-dessous :

![](owin-oauth-20-authorization-server/_static/image2.png)

Si le **Grant** bouton est sélectionné, le `Authorize` action crée une nouvelle identité de « Porteur » et connectez-vous avec elle. Il déclenche le serveur d’autorisation pour générer un jeton du porteur et l’envoyer au client avec une charge utile JSON. 

### <a name="implicit-grant"></a>Octroi implicite

Reportez-vous à OAuth 2 l’IETF [octroi implicite](http://tools.ietf.org/html/rfc6749#section-4.2) section maintenant.

 Le [octroi implicite](http://tools.ietf.org/html/rfc6749#section-4.2) flux illustré Figure 4 est le flux et le OAuth OWIN de mappage qui la suit intergiciel (middleware).  

| Étapes du flux à partir de la section d’octroi implicite | Téléchargement de l’exemple effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) le client lance le flux en dirigeant l’agent utilisateur du propriétaire des ressources au point de terminaison d’autorisation. Le client inclut son identificateur client, étendue demandée, état local et un URI de redirection auquel le serveur d’autorisation envoie l’agent utilisateur une fois l’accès est accordé (ou refusé). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) le serveur d’autorisation authentifie le propriétaire de la ressource (par le biais de l’agent utilisateur) et détermine si le propriétaire de la ressource accorde ou refuse la demande d’accès du client. | **&lt;Si l’utilisateur accorde l’accès&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) en supposant que le propriétaire de la ressource accorde l’accès, le serveur d’autorisation redirige l’agent utilisateur vers le client à l’aide de l’URI de redirection fourni précédemment (dans la demande ou lors de l’inscription du client). ... |  |
|  |  |
| (D) le client demande un jeton d’accès à partir du point de terminaison de jeton du serveur d’autorisation en incluant le code d’autorisation reçu à l’étape précédente. Lors de sa demande, le client s’authentifie auprès du serveur d’autorisation. Le client inclut l’URI de redirection utilisé pour obtenir le code d’autorisation pour la vérification. |  |

Étant donné que nous avons déjà implémenté le point de terminaison d’autorisation (`OAuthController.Authorize` action) pour l’octroi de code d’autorisation, il active automatiquement également le flux implicite. Remarque : `Provider.ValidateClientRedirectUri` est utilisé pour valider l’ID de client avec son URL de redirection, qui protège le flux d’octroi implicite d’envoyer des jetons aux clients l’accès ([attaque de Man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Octroi des informations d’identification mot de passe de propriétaire de la ressource

Reportez-vous à OAuth 2 l’IETF [octroi des informations de mot de passe propriétaire de la ressource](http://tools.ietf.org/html/rfc6749#section-4.3) section maintenant.

 Le [octroi des informations de mot de passe propriétaire de la ressource](http://tools.ietf.org/html/rfc6749#section-4.3) flux illustré Figure 5 est le flux et le OAuth OWIN de mappage qui la suit intergiciel (middleware).  

| Étapes du flux à partir de la section d’octroi des informations de mot de passe propriétaire de la ressource | Téléchargement de l’exemple effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) le propriétaire de la ressource fournit au client avec son nom d’utilisateur et le mot de passe. |  |
|  |  |
| (B) le client demande un jeton d’accès à partir du point de terminaison de jeton du serveur d’autorisation en incluant les informations d’identification reçues du propriétaire de ressources. Lors de sa demande, le client s’authentifie auprès du serveur d’autorisation. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) le serveur d’autorisation authentifie le client et valide les informations d’identification du propriétaire de ressource et s’il est valide, émet un jeton d’accès. |  |

Voici l’exemple d’implémentation pour `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Le code ci-dessus a vocation d’expliquer cette section du didacticiel et ne doit pas être utilisé dans sécurisée ou les applications de production. Il ne vérifie pas les informations d’identification des propriétaires de ressources. Il part du principe que chaque information d’identification n’est valide et crée une nouvelle identité pour celle-ci. La nouvelle identité sera utilisée pour générer le jeton d’accès et le jeton d’actualisation. Remplacez le code avec votre propre code de gestion de compte sécurisé.


### <a name="client-credentials-grant"></a>Octroi des informations d’identification client

Reportez-vous à OAuth 2 l’IETF [octroi des informations d’identification du Client](http://tools.ietf.org/html/rfc6749#section-4.4) section maintenant.

 Le [octroi des informations d’identification du Client](http://tools.ietf.org/html/rfc6749#section-4.4) flux indiqué dans la Figure 6 est le flux et le OAuth OWIN de mappage qui la suit intergiciel (middleware).  

| Étapes du flux à partir de la section d’octroi des informations d’identification du Client | Téléchargement de l’exemple effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) le client s’authentifie auprès du serveur d’autorisation et demande un jeton d’accès du point de terminaison de jeton. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) le serveur d’autorisation authentifie le client et s’il est valide, émet un jeton d’accès. |  |

Voici l’exemple d’implémentation pour `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Le code ci-dessus a vocation d’expliquer cette section du didacticiel et ne doit pas être utilisé dans sécurisée ou les applications de production. Remplacez le code avec votre propre code de gestion de client sécurisé.


### <a name="refresh-token"></a>Jeton d’actualisation

Reportez-vous à OAuth 2 l’IETF [jeton d’actualisation](http://tools.ietf.org/html/rfc6749#section-1.5) section maintenant.

 Le [jeton d’actualisation](http://tools.ietf.org/html/rfc6749#section-1.5) flux indiqué dans la Figure 2 est le flux et le OAuth OWIN de mappage qui la suit intergiciel (middleware).  

| Étapes du flux à partir de la section d’octroi des informations d’identification du Client | Téléchargement de l’exemple effectue ces étapes avec : |
| --- | --- |
|  |  |
| (G), le client demande un nouveau jeton d’accès en authentifie auprès du serveur d’autorisation et en fournissant le jeton d’actualisation. Les exigences d’authentification client sont basées sur le type de client et sur les stratégies de serveur d’autorisation. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) le serveur d’autorisation authentifie le client et valide le jeton d’actualisation et s’il est valide, émet un nouveau jeton d’accès (et, éventuellement, un nouveau jeton d’actualisation). |  |

Voici l’exemple d’implémentation pour `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Créer un serveur de ressources qui est protégé par le jeton d’accès

Créez un projet d’application web vide et installez après les packages dans le projet :

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Créez une classe de démarrage et configurer l’authentification et l’API Web. Consultez *AuthorizationServer\ResourceServer\Startup.cs* dans le téléchargement d’exemple.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Consultez *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* dans le téléchargement d’exemple.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Consultez *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* dans le téléchargement d’exemple.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` méthode permet de CORS pour tous les domaines.
- `UseOAuthBearerAuthentication` méthode permet d’intergiciel d’authentification du jeton de support OAuth qui recevra et valider le jeton de porteur dans l’en-tête d’autorisation dans la demande.
- `Config.SuppressDefaultHostAuthenticaiton` Supprime la valeur par défaut hôte principal authentifié à partir de l’application, par conséquent, toutes les demandes seront anonymes après cet appel.
- `HostAuthenticationFilter` Active l’authentification uniquement pour le type d’authentification spécifié. Dans ce cas, il est de type d’authentification du porteur.

Pour illustrer l’identité authentifiée, nous créons une classe ApiController pour générer des revendications de l’utilisateur actuel.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Si le serveur d’autorisation et le serveur de ressources ne sont pas sur le même ordinateur, le middleware OAuth utilise les clés de l’autre ordinateur pour chiffrer et déchiffrer le jeton d’accès du porteur. Afin de partager la même clé privée entre les deux projets, nous ajoutons le même `machinekey` définition dans les deux *web.config* fichiers.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Créer des Clients OAuth 2.0

 Nous utilisons le [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) package NuGet pour simplifier le code client.

### <a name="authorization-code-grant-client"></a>Octroi de Code d’autorisation Client

 Ce client est une application MVC. Il déclenche un flux d’octroi de code d’autorisation pour obtenir l’accès au jeton du serveur principal. Il possède une seule page comme indiqué ci-dessous :

![](owin-oauth-20-authorization-server/_static/image3.png)

- Le **Authorize** bouton redirigera le navigateur vers le serveur d’autorisation pour notifier le propriétaire de la ressource à accorder l’accès à ce client.
- Le **Actualiser** bouton obtiendra un nouveau jeton d’accès et le jeton d’actualisation à l’aide de l’actualisation en cours jeton.
- Le **accès protégé ressources API** bouton appellera le serveur de ressources pour obtenir des données de revendications de l’utilisateur actuel et les afficher sur la page.

Voici l’exemple de code de la `HomeController` du client.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` nécessite SSL par défaut. Étant donné que notre démonstration à l’aide de HTTP, vous devez ajouter suivant paramètre dans le fichier de configuration :

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Sécurité - jamais désactiver SSL dans une application de production. Vos informations d’identification de connexion sont désormais envoyées en texte clair sur le réseau. Le code ci-dessus est uniquement pour le débogage local exemple et l’exploration.


### <a name="implicit-grant-client"></a>Client d’octroi implicite

Ce client utilise JavaScript pour :

1. Ouvrez une nouvelle fenêtre et rediriger vers le point de terminaison authorize du serveur d’autorisation.
2. Obtenir le jeton d’accès à partir de fragments d’URL quand il redirige le retour.

L’illustration suivante montre ce processus :

![](owin-oauth-20-authorization-server/_static/image4.png)

Le client doit avoir deux pages : une pour la page d’accueil et l’autre pour le rappel. Voici l’exemple de code JavaScript de code se trouve dans le *Index.cshtml* fichier :

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Voici le code dans de gestion de rappel *SignIn.cshtml* fichier :

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Une bonne pratique consiste à déplacer le code JavaScript dans un fichier externe et pas l’incorporer avec le balisage Razor. Pour simplifier cet exemple, ils ont été combinés.


### <a name="resource-owner-password-credentials-grant-client"></a>Mot de passe de propriétaire de la ressource Client de l’octroi des informations d’identification

Nous utilise une application de console pour ce client de démonstration. Voici le code :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Client de l’octroi des informations d’identification client

Comme pour l’octroi d’informations d’identification de mot de passe ressource propriétaire, voici code d’application de console :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
