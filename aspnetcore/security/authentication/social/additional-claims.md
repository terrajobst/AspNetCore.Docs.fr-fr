---
title: Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core
author: guardrex
description: Découvrez comment établir des revendications supplémentaires et des jetons provenant de fournisseurs externes.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874844"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Une application ASP.NET Core peut établir des revendications supplémentaires et des jetons provenant de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter. Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données de l’utilisateur vers des revendications supplémentaires est le même.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

Décidez quels fournisseurs d’authentification externes pour prendre en charge dans l’application. Pour chaque fournisseur, inscrire l’application et obtenir un ID client et la clé secrète client. Pour plus d'informations, consultez <xref:security/authentication/social/index>. L’exemple d’application utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Définir l’ID client et la clé secrète client

Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et la clé secrète client. ID client et les valeurs de clé secrète du client sont créés pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur. Chaque fournisseur externe qui utilise l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur. Pour plus d’informations, consultez les rubriques de fournisseur d’authentification externe qui s’appliquent à votre scénario :

* [Authentification Facebook](xref:security/authentication/facebook-logins)
* [Authentification Google](xref:security/authentication/google-logins)
* [Authentification Microsoft](xref:security/authentication/microsoft-logins)
* [Authentification Twitter](xref:security/authentication/twitter-logins)
* [Autres fournisseurs d’authentification](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et la clé secrète client fournie par Google :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Établir l’étendue de l’authentification

Spécifier la liste des autorisations nécessaires pour récupérer à partir du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Étendues de l’authentification pour les fournisseurs externes courantes apparaissent dans le tableau suivant.

| Fournisseur  | Portée                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Dans de l’exemple d’application, Google `userinfo.profile` étendue est ajoutée automatiquement par le framework lorsque <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> est appelée sur le <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Si l’application requiert des étendues supplémentaires, ajoutez-les aux options. Dans l’exemple suivant, Google `https://www.googleapis.com/auth/user.birthday.read` étendue est ajoutée afin de récupérer la date d’anniversaire d’un utilisateur :

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Mapper des clés de données utilisateur et de créer des revendications

Dans les options du fournisseur, vous devez spécifier un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> ou <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> pour chaque sous-clé/de clé dans les données d’utilisateur JSON du fournisseur externe pour l’identité de l’application à lire sur la connexion. Pour plus d’informations sur les types de revendications, consultez <xref:System.Security.Claims.ClaimTypes>.

L’exemple d’application crée des paramètres régionaux (`urn:google:locale`) et image (`urn:google:picture`) des revendications à partir du `locale` et `picture` clés dans les données utilisateur de Google :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

Dans <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Lors de la connexion des processus, le <xref:Microsoft.AspNetCore.Identity.UserManager%601> peut stocker un `ApplicationUser` revendications pour les données utilisateur disponibles à partir de la <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) établit les paramètres régionaux (`urn:google:locale`) et image (`urn:google:picture`) revendications pour le signé dans `ApplicationUser`, y compris une revendication pour <xref:System.Security.Claims.ClaimTypes.GivenName> :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

Par défaut, les revendications d’un utilisateur sont stockées dans le cookie d’authentification. Si le cookie d’authentification est trop volumineux, il peut entraîner l’application en échec, car :

* Le navigateur détecte que l’en-tête de cookie est trop long.
* La taille globale de la demande est trop grande.

Si une grande quantité de données utilisateur est nécessaire pour traiter les demandes d’utilisateur :

* Limiter le nombre et la taille de revendications d’utilisateur pour le traitement de requête à ce que l’application, il suffit.
* Utiliser un personnalisé <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> du Middleware de Cookie d’authentification <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> pour stocker l’identité entre les requêtes. Conserver les grandes quantités d’informations d’identité sur le serveur lors de l’envoi uniquement une clé d’identificateur de session small au client.

## <a name="save-the-access-token"></a>Enregistrer le jeton d’accès

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockées dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie. `SaveTokens` a la valeur `false` par défaut pour réduire la taille du cookie d’authentification finale.

L’exemple d’application définit la valeur de `SaveTokens` à `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

Lorsque `OnPostConfirmationAsync` s’exécute, stocker le jeton d’accès ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `ApplicationUser`de `AuthenticationProperties`.

L’exemple d’application enregistre le jeton d’accès dans `OnPostConfirmationAsync` (nouvelle inscription d’utilisateur) et `OnGetCallbackAsync` (utilisateur inscrit précédemment) dans *Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Comment ajouter des jetons personnalisés supplémentaires

Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec actuel <xref:System.DateTime> pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>Création et ajout de revendications

Le framework fournit des actions courantes et les méthodes d’extension pour la création et ajout de revendications à la collection. Pour plus d’informations, consultez <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> et <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Les utilisateurs peuvent définir des actions personnalisées en dérivant de <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> et l’implémentation abstraite <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> (méthode).

Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Suppression des actions de revendication et revendications

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) supprime toutes les revendications des actions pour la donnée <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> à partir de la collection. [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) supprime une revendication de la donnée <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> à partir de l’identité. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> est principalement utilisé avec [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) pour supprimer les revendications généré par le protocole.

## <a name="sample-app-output"></a>Résultat de l’application exemple

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [ASPNET/AspNetCore ingénierie SocialSample application](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; l’application exemple lié se trouve sur le [du référentiel de GitHub aspnet/AspNetCore](https://github.com/aspnet/AspNetCore) `master` branche ingénierie. Le `master` branche contient du code en cours de développement pour la prochaine version d’ASP.NET Core. Pour afficher une version de l’exemple d’application pour une version finale d’ASP.NET Core, utilisez le **branche** liste déroulante liste pour sélectionner une branche de version (par exemple `release/2.2`).
