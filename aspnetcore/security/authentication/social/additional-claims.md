---
title: Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core
author: guardrex
description: Découvrez comment établir des revendications supplémentaires et des jetons provenant de fournisseurs externes.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 37c7a51217576669bcaed79d4a212e6412aa8945
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897666"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Conserver des revendications supplémentaires et les jetons provenant de fournisseurs externes dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Une application ASP.NET Core peut établir des revendications supplémentaires et des jetons provenant de fournisseurs d’authentification externes, tels que Facebook, Google, Microsoft et Twitter. Chaque fournisseur révèle des informations différentes sur les utilisateurs sur sa plateforme, mais le modèle de réception et de transformation des données de l’utilisateur vers des revendications supplémentaires est le même.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

Décidez quels fournisseurs d’authentification externes pour prendre en charge dans l’application. Pour chaque fournisseur, inscrire l’application et obtenir un ID client et la clé secrète client. Pour plus d'informations, consultez <xref:security/authentication/social/index>. Le [exemple d’application](#sample-app-instructions) utilise le [fournisseur d’authentification Google](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>Définir l’ID client et la clé secrète client

Le fournisseur d’authentification OAuth établit une relation d’approbation avec une application à l’aide d’un ID client et la clé secrète client. ID client et les valeurs de clé secrète du client sont créés pour l’application par le fournisseur d’authentification externe lorsque l’application est inscrite auprès du fournisseur. Chaque fournisseur externe qui utilise l’application doit être configuré indépendamment avec l’ID client et la clé secrète client du fournisseur. Pour plus d’informations, consultez les rubriques de fournisseur d’authentification externe qui s’appliquent à votre scénario :

* [Authentification Facebook](xref:security/authentication/facebook-logins)
* [Authentification Google](xref:security/authentication/google-logins)
* [Authentification Microsoft](xref:security/authentication/microsoft-logins)
* [Authentification Twitter](xref:security/authentication/twitter-logins)
* [Autres fournisseurs d’authentification](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

L’exemple d’application configure le fournisseur d’authentification Google avec un ID client et la clé secrète client fournie par Google :

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a>Établir l’étendue de l’authentification

Spécifier la liste des autorisations nécessaires pour récupérer à partir du fournisseur en spécifiant le <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Étendues de l’authentification pour les fournisseurs externes courantes apparaissent dans le tableau suivant.

| Fournisseur  | Portée                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

L’exemple d’application ajoute Google `plus.login` étendue pour demander des autorisations de connexion Google + :

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a>Mapper des clés de données utilisateur et de créer des revendications

Dans les options du fournisseur, vous devez spécifier un <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> pour chaque clé dans les données d’utilisateur JSON du fournisseur externe pour l’identité de l’application à lire sur la connexion. Pour plus d’informations sur les types de revendications, consultez <xref:System.Security.Claims.ClaimTypes>.

L’exemple d’application crée un <xref:System.Security.Claims.ClaimTypes.Gender> revendication à partir de la `gender` clés dans les données utilisateur de Google :

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

Dans <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, un <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) est connecté à l’application avec <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Lors de la connexion des processus, le <xref:Microsoft.AspNetCore.Identity.UserManager%601> peut stocker un `ApplicationUser` de revendication pour les données utilisateur disponibles à partir de la <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

Dans l’exemple d’application, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) établit une <xref:System.Security.Claims.ClaimTypes.Gender> de revendication pour l’élément signé dans `ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a>Enregistrer le jeton d’accès

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> définit si les jetons d’accès et d’actualisation doivent être stockées dans le <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> après une autorisation réussie. `SaveTokens` a la valeur `false` par défaut pour réduire la taille du cookie d’authentification finale.

L’exemple d’application définit la valeur de `SaveTokens` à `true` dans <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

Lorsque `OnPostConfirmationAsync` s’exécute, stocker le jeton d’accès ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) à partir du fournisseur externe dans le `ApplicationUser`de `AuthenticationProperties`.

L’exemple d’application enregistre dans le jeton d’accès :

* `OnPostConfirmationAsync` &ndash; S’exécute pour la nouvelle inscription de l’utilisateur.
* `OnGetCallbackAsync` &ndash; S’exécute lorsqu’un utilisateur inscrit précédemment se connecte à l’application.

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a>Comment ajouter des jetons personnalisés supplémentaires

Pour montrer comment ajouter un jeton personnalisé, qui est stocké dans le cadre de `SaveTokens`, l’exemple d’application ajoute un <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> avec actuel <xref:System.DateTime> pour un [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) de `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>Instructions de l’application exemple

L’exemple d’application montre comment :

* Obtenir le sexe de l’utilisateur à partir de Google et enregistre une revendication de sexe avec la valeur.
* Store le jeton d’accès Google de l’utilisateur `AuthenticationProperties`.

Pour utiliser l’exemple d’application :

1. Inscrire l’application et obtenir un ID client valide et une clé secrète client pour l’authentification Google. Pour plus d'informations, consultez <xref:security/authentication/google-logins>.
1. Fournir l’ID client et le secret du client à l’application dans le <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> de `Startup.ConfigureServices`.
1. Exécutez l’application et demandez la page Mes revendications. Lorsque l’utilisateur n’est pas connecté, l’application redirige vers Google. Se connecter avec Google. Google redirige l’utilisateur vers l’application (`/Home/MyClaims`). L’utilisateur est authentifié, et la page Mes revendications est chargée. La revendication de sexe est présente sous **revendications utilisateur** avec la valeur obtenue à partir de Google. Le jeton d’accès s’affiche dans le **propriétés d’authentification**.

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
