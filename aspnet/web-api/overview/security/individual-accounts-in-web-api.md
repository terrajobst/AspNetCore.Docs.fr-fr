---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Sécuriser une API Web avec des comptes individuels et de la connexion locale dans ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Cette rubrique montre comment sécuriser une API web à l’aide d’OAuth2 pour s’authentifier auprès d’une base de données d’appartenance. Versions des logiciels utilisées dans le didacticiel 201 de Studio Visual...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 01d117260ef458453bee79285a37a8977221998c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825531"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Sécuriser une API Web avec des comptes individuels et de la connexion locale dans ASP.NET Web API 2.2
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger l’exemple d’application](https://github.com/MikeWasson/LocalAccountsApp)

> Cette rubrique montre comment sécuriser une API web à l’aide d’OAuth2 pour s’authentifier auprès d’une base de données d’appartenance.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


Dans Visual Studio 2013, le modèle de projet API Web propose trois options pour l’authentification :

- **Comptes individuels.** L’application utilise une base de données d’appartenance.
- **Comptes professionnels.** Les utilisateurs se connectent avec leurs Azure Active Directory, Office 365 ou informations d’identification de Active Directory sur site.
- **Authentification Windows.** Cette option est destinée aux applications de l’Intranet et utilise le module IIS de l’authentification Windows.

Pour plus d’informations sur ces options, consultez [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Les comptes individuels proposent deux méthodes pour un utilisateur de se connecter :

- **Connexion locale**. L’utilisateur s’inscrit sur le site, entrer un nom d’utilisateur et un mot de passe. L’application stocke le hachage de mot de passe dans la base de données d’appartenance. Lorsque l’utilisateur se connecte, le système d’identité ASP.NET vérifie le mot de passe.
- **Connexion de réseau social**. L’utilisateur se connecte avec un service externe, tel que Facebook, Microsoft ou Google. L’application crée une entrée pour l’utilisateur dans la base de données d’appartenance toujours, mais ne stocke pas les informations d’identification. L’utilisateur s’authentifie en vous connectant à un service externe.

Cet article examine le scénario de connexion locale. Pour la connexion à la fois locale et sociaux, les API Web utilise OAuth2 pour authentifier les demandes. Toutefois, les flux d’informations d’identification sont différents pour la connexion locale et sociale.

Dans cet article, je vais expliquer une application simple qui permet à l’utilisateur de se connecter et envoyer des appels AJAX authentifiés à une API web. Vous pouvez télécharger l’exemple de code [ici](https://github.com/MikeWasson/LocalAccountsApp). Le fichier Lisez-moi explique comment créer l’exemple à partir de zéro dans Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

L’exemple d’application utilise Knockout.js pour la liaison de données et jQuery pour envoyer des demandes d’AJAX. Je vais me concentrer sur les appels AJAX, vous n’avez pas besoin de connaître Knockout.js pour cet article.

Tout au long du processus, je vais décrire :

- Ce que fait l’application côté client.
- Ce qui se passe sur le serveur.
- Le trafic HTTP au milieu.

Tout d’abord, nous devons définir certains termes OAuth2.

- *Ressources*. Certains éléments de données qui peuvent être protégées.
- *Serveur de ressources*. Le serveur qui héberge la ressource.
- *Propriétaire de la ressource*. L’entité qui peut accorder l’autorisation d’accéder à une ressource. (En général, l’utilisateur.)
- *Client*: l’application qui souhaite accéder à la ressource. Dans cet article, le client est un navigateur web.
- *Jeton d’accès*. Un jeton qui accorde l’accès à une ressource.
- *Jeton du porteur*. Un type particulier de jeton d’accès, avec la propriété que tout le monde peut utiliser le jeton. En d’autres termes, un client n’a pas besoin une clé de chiffrement ou un autre secret pour utiliser un jeton du porteur. Pour cette raison, les jetons du porteur doivent uniquement être utilisées sur un HTTPS et doivent avoir des délais d’expiration relativement courte.
- *Serveur d’autorisation*. Un serveur qui fournit des jetons d’accès.

Une application peut agir comme serveur d’autorisation et le serveur de ressources. Le modèle de projet API Web suit ce modèle.

## <a name="local-login-credential-flow"></a>Flux des informations d’identification de connexion locale

Pour la connexion locale, API Web utilise le [flux de mot de passe de propriétaire de ressource](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) défini dans OAuth2.

1. L’utilisateur entre un nom et un mot de passe dans le client.
2. Le client envoie ces informations d’identification du serveur d’autorisation.
3. Le serveur d’autorisation authentifie les informations d’identification et retourne un jeton d’accès.
4. Pour accéder à une ressource protégée, le client inclut le jeton d’accès dans l’en-tête d’autorisation de la requête HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Lorsque vous sélectionnez **comptes individuels** dans le modèle de projet API Web, le projet inclut un serveur d’autorisation qui valide les informations d’identification utilisateur et émet des jetons. Le diagramme suivant montre le même flux d’informations d’identification en termes de composants de l’API Web.

![](individual-accounts-in-web-api/_static/image4.png)

Dans ce scénario, les contrôleurs d’API Web agissent comme serveurs de ressources. Un filtre d’authentification valide les jetons d’accès et le **[Authorize]** attribut est utilisé pour protéger une ressource. Lorsqu’un contrôleur ou une action a le **[Authorize]** attribut, toutes les demandes à ce contrôleur ou d’action doit être authentifiée. Sinon, l’autorisation est refusée et API Web renvoie une erreur 401 (non autorisé).

Le serveur d’autorisation et le filtre d’authentification à l’appel dans un [intergiciel (middleware) OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) composant qui gère les détails d’OAuth2. Je vais décrire la conception plus en détail plus loin dans ce didacticiel.

## <a name="sending-an-unauthorized-request"></a>Envoyer une demande non autorisée

Pour commencer, exécutez l’application et cliquez sur le **appeler l’API** bouton. Lorsque la demande est terminée, vous devez voir un message d’erreur dans le **résultat** boîte. C’est parce que la demande ne contient pas un jeton d’accès, donc la demande n’est pas autorisée.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Le **appeler l’API** bouton envoie une requête AJAX à ~/api/valeurs, qui appelle une action de contrôleur d’API Web. Voici la section de code JavaScript qui envoie la requête AJAX. Dans l’exemple d’application, tout le code d’application JavaScript se trouve dans le fichier Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Jusqu'à ce que l’utilisateur se connecte, il existe aucun jeton du porteur et par conséquent aucun en-tête d’autorisation dans la demande. Cela entraîne la demande retourner une erreur 401.

Voici la requête HTTP. (J’ai utilisé [Fiddler](http://www.telerik.com/fiddler) pour capturer le trafic HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Notez que la réponse inclut un en-tête Www-Authenticate au défi de la valeur du porteur. Qui indique que le serveur attend un jeton du porteur.

## <a name="register-a-user"></a>Inscrire un utilisateur

Dans le **inscrire** section de l’application, entrez un e-mail et le mot de passe, puis cliquez sur le **inscrire** bouton.

Vous n’avez pas besoin d’utiliser une adresse e-mail valide pour cet exemple, mais une application réelle confirmer l’adresse. (Consultez [créer une application web de ASP.NET MVC 5 sécurisée avec connexion, réinitialisation de confirmation et le mot de passe de messagerie](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Le mot de passe, utilisez quelque chose comme « Password1 ! », avec une lettre majuscule, une lettre minuscule, un nombre et un caractère non alphanumérique. Pour que l’application reste simple, j’ai omis validation côté client, s’il existe un problème avec le format de mot de passe, vous obtiendrez une erreur 400 (demande incorrecte).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Le **inscrire** bouton envoie une demande POST à ~/api/Account/Register /. Le corps de la demande est un objet JSON qui contient le nom et le mot de passe. Voici le code JavaScript qui envoie la demande :

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Requête HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Cette requête est gérée par le `AccountController` classe. En interne, `AccountController` utilise ASP.NET Identity pour gérer la base de données d’appartenance.

Si vous exécutez l’application localement à partir de Visual Studio, les comptes d’utilisateur sont stockés dans la base de données locale, dans la table AspNetUsers. Pour afficher les tableaux dans Visual Studio, cliquez sur le **vue** menu, sélectionnez **Explorateur de serveurs**, puis développez **des connexions de données**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Obtenir un jeton accès

Jusqu'à présent, nous n’avons pas n’importe quel OAuth, mais nous allons maintenant voir le serveur d’autorisation OAuth en action, lors de la demande un jeton d’accès. Dans le **Log In** zone de l’exemple d’application, entrez l’e-mail et le mot de passe et cliquez sur **Log In**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Le **Log In** bouton envoie une demande au point de terminaison de jeton. Le corps de la requête contient les données de forme codée en url suivantes :

- accorder\_type : « password »
- nom d’utilisateur : &lt;messagerie de l’utilisateur&gt;
- mot de passe : &lt;mot de passe&gt;

Voici le code JavaScript qui envoie la requête AJAX :

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Si la demande aboutit, le serveur d’autorisation renvoie un jeton d’accès dans le corps de réponse. Notez que nous conservons pas le jeton dans le stockage de session, pour une utilisation ultérieure lors de l’envoi de demandes à l’API. Contrairement à certaines formes d’authentification (par exemple, l’authentification basée sur les cookies), le navigateur automatiquement inclura pas le jeton d’accès dans les demandes suivantes. L’application doit le faire explicitement. C’est une bonne chose, car il limite [des vulnérabilités CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Requête HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Vous pouvez voir que la demande contient des informations d’identification de l’utilisateur. Vous *doit* utiliser HTTPS pour assurer la sécurité de couche de transport.

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Pour une meilleure lisibilité, j’ai mis en retrait le code JSON et tronqué le jeton d’accès, qui est assez long.

Le `access_token`, `token_type`, et `expires_in` propriétés sont définies par la spécification OAuth2. Les autres propriétés (`userName`, `.issued`, et `.expires`) sont uniquement à titre d’information. Vous trouverez le code qui ajoute ces propriétés supplémentaires dans le `TokenEndpoint` (méthode), dans le fichier /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Envoyer une demande authentifiée

Maintenant que nous disposons d’un jeton du porteur, nous pouvons faire une demande authentifiée à l’API. Pour cela, en définissant l’en-tête d’autorisation dans la demande. Cliquez sur le **appeler l’API** bouton à nouveau pour voir cela.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Requête HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Se déconnecter

Étant donné que le navigateur ne met pas en cache les informations d’identification ou le jeton d’accès, la déconnexion est simplement de « oubli » le jeton, en le supprimant de stockage de session :

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Comprendre le modèle de projet de comptes individuels

Lorsque vous sélectionnez **comptes individuels** dans le modèle de projet d’Application Web ASP.NET, le projet inclut :

- Un serveur d’autorisation OAuth2.
- Un point de terminaison d’API Web pour la gestion des comptes d’utilisateur
- Un modèle EF pour le stockage des comptes d’utilisateur.

Voici les classes de l’application principale qui implémentent ces fonctionnalités :

- `AccountController`. Fournit un point de terminaison d’API Web pour gérer les comptes d’utilisateur. Le `Register` action est le seul qui nous avons utilisé dans ce didacticiel. Autres méthodes sur la classe prend en charge la réinitialisation de mot de passe, les connexions réseaux sociaux et autres fonctionnalités.
- `ApplicationUser`, défini dans /Models/IdentityModels.cs. Cette classe est le modèle EF pour les comptes d’utilisateur dans la base de données d’appartenance.
- `ApplicationUserManager`, défini dans /App\_Start/IdentityConfig.cs cette classe dérive [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) et effectue des opérations sur les comptes d’utilisateur, telles que la création d’un nouvel utilisateur, vérification des mots de passe et ainsi de suite et applique automatiquement modifications apportées à la base de données.
- `ApplicationOAuthProvider`. Cet objet s’intègre à l’intergiciel (middleware) OWIN et traite les événements déclenchés par l’intergiciel (middleware). Il dérive [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configuration du serveur d’autorisation

Dans StartupAuth.cs, le code suivant configure le serveur d’autorisation OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Le `TokenEndpointPath` propriété est le chemin d’URL pour le point de terminaison de serveur d’autorisation. Qui est l’URL de cette application utilise pour obtenir les jetons du porteur.

Le `Provider` propriété spécifie un fournisseur qui s’intègre à l’intergiciel (middleware) OWIN et traite les événements déclenchés par l’intergiciel (middleware).

Voici le flux de base lors de l’application souhaite obtenir un jeton :

1. Pour obtenir un jeton d’accès, l’application envoie une demande à ~ / Token.
2. Les appels d’intergiciel (middleware) OAuth `GrantResourceOwnerCredentials` sur le fournisseur.
3. Le fournisseur appelle le `ApplicationUserManager` pour valider les informations d’identification et de créer une identité de revendications.
4. Si cette tentative réussit, le fournisseur crée un ticket d’authentification, qui est utilisé pour générer le jeton.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Le middleware OAuth ne sait rien sur les comptes d’utilisateur. Le fournisseur communique avec l’intergiciel (middleware) et l’identité de ASP.NET. Pour plus d’informations sur l’implémentation du serveur d’autorisation, consultez [OWIN OAuth 2.0 d’autorisation serveur](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configuration d’API Web à utiliser des jetons du porteur

Dans le `WebApiConfig.Register` (méthode), le code suivant configure l’authentification pour le pipeline API Web :

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Le **HostAuthenticationFilter** classe permet l’authentification à l’aide de jetons du porteur.

Le **SuppressDefaultHostAuthentication** méthode indique à l’API Web pour ignorer toute authentification se produit avant que la demande atteint le pipeline API Web, IIS ou en intergiciel (middleware) OWIN. De cette façon, nous pouvons limiter l’API Web pour s’authentifier à l’aide uniquement des jetons du porteur.

> [!NOTE]
> En particulier, la partie MVC de votre application peut utiliser l’authentification par formulaire, qui stocke les informations d’identification dans un cookie. Authentification basée sur les cookies nécessite l’utilisation de jetons anti-contrefaçon, pour empêcher les attaques CSRF. C’est un problème pour les API, web, car il n’existe aucun moyen pratique de l’API web pour envoyer le jeton anti-contrefaçon au client. (Pour plus d’informations sur ce problème, consultez [empêcher les attaques CSRF dans l’API Web](preventing-cross-site-request-forgery-csrf-attacks.md).) Appel **SuppressDefaultHostAuthentication** garantit que l’API Web n’est pas vulnérable aux attaques de falsification de requête Intersites à partir des informations d’identification stockées dans des cookies.


Lorsque le client demande une ressource protégée, voici ce qui se passe dans le pipeline de l’API Web :

1. Le **HostAuthentication** filtre appelle l’intergiciel de OAuth pour valider le jeton.
2. L’intergiciel (middleware) convertit le jeton en une identité de revendications.
3. À ce stade, la demande est *authentifié* mais pas *autorisé*.
4. Le filtre d’autorisation examine l’identité des revendications. Si les revendications autorisent l’utilisateur pour cette ressource, la demande est autorisée. Par défaut, le **[Authorize]** attribut autorise toute demande qui est authentifié. Toutefois, vous pouvez autoriser par rôle ou par d’autres revendications. Pour plus d’informations, consultez [authentification et autorisation dans Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Si les étapes précédentes sont réussies, le contrôleur retourne la ressource protégée. Sinon, le client reçoit une erreur 401 (non autorisé).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.NET Identity](../../../identity/index.md)
- [Présentation des fonctionnalités de sécurité dans le modèle SPA pour VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Billet de blog MSDN par Hongye Sun.
- [La personne d’API Web de dissection comptes modèle – partie 2 : comptes locaux](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Billet de blog de Dominick Baier.
- [Héberger l’authentification et les API Web avec OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Une bonne explication de `SuppressDefaultHostAuthentication` et `HostAuthenticationFilter` par Brock Allen.
- [Personnalisation des informations de profil dans ASP.NET Identity dans les modèles de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Billet de blog MSDN Pranav Rastogi.
- [Par la gestion des demandes de durée de vie pour la classe UserManager dans ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Billet de blog MSDN par Suhas Joshi, avec une bonne explication de la `UserManager` classe.
