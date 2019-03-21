---
title: Utiliser l’authentification par cookie sans ASP.NET Core Identity
author: rick-anderson
description: Une explication de l’authentification par cookie sans ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: c6cba588abb003ee677d8f1753b73a1ced1414b0
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209373"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Utiliser l’authentification par cookie sans ASP.NET Core Identity

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)

Comme vous l’avez vu dans les rubriques précédentes de l’authentification, [ASP.NET Core Identity](xref:security/authentication/identity) est un fournisseur d’authentification complète et complet pour créer et maintenir des connexions. Toutefois, vous souhaiterez utiliser votre propre logique d’authentification personnalisée avec l’authentification basée sur les cookies dans certains cas. Vous pouvez utiliser l’authentification par cookie comme un fournisseur d’authentification autonome sans ASP.NET Core Identity.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Fins de démonstration dans l’exemple d’application, le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application. Utilisez le nom d’utilisateur de messagerie «maria.rodriguez@contoso.com» et n’importe quel mot de passe pour vous connecter l’utilisateur. L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier. Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données.

Pour plus d’informations sur l’authentification basée sur les cookies de migration d’ASP.NET Core 1.x vers 2.0, consultez [migrer l’authentification et l’identité pour ASP.NET Core 2.0 rubrique (authentification basée sur les cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

Pour utiliser ASP.NET Core Identity, consultez le [présentation d’Identity](xref:security/authentication/identity) rubrique.

## <a name="configuration"></a>Configuration

::: moniker range=">= aspnetcore-2.0"

Si l’application n’utilise pas le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), créez une référence de package dans le fichier de projet pour le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 ou plus tard).

Dans le `ConfigureServices` (méthode), créez le service de l’intergiciel d’authentification avec le `AddAuthentication` et `AddCookie` méthodes :

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` passé à `AddAuthentication` définit le schéma d’authentification par défaut pour l’application. `AuthenticationScheme` est utile quand il existe plusieurs instances de l’authentification des cookies et que vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme). Définition de la `AuthenticationScheme` à `CookieAuthenticationDefaults.AuthenticationScheme` fournit une valeur de « Cookies » pour le schéma. Vous pouvez fournir n’importe quelle valeur de chaîne qui distingue le schéma.

Schéma d’authentification de l’application est différent de schéma d’authentification de cookie de l’application. Lorsqu’un schéma d’authentification de cookie n’est pas fourni pour <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, elle utilise [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) (« Cookies »).

Le cookie d’authentification <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> propriété a la valeur `true` par défaut. Les cookies d’authentification sont autorisées lorsqu’un visiteur du site n’a pas donné son consentement pour la collecte de données. Pour plus d'informations, consultez <xref:security/gdpr#essential-cookies>.

Dans le `Configure` (méthode), utilisez le `UseAuthentication` méthode à appeler l’intergiciel d’authentification qui définit le `HttpContext.User` propriété. Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**AddCookie Options**

Le [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe est utilisée pour configurer les options de fournisseur d’authentification.

| Option | Description |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Fournit le chemin d’accès pour fournir un 302 trouvé (redirection d’URL) lorsque déclenchée par `HttpContext.ForbidAsync`. La valeur par défaut est `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | L’émetteur à utiliser pour le [émetteur](/dotnet/api/system.security.claims.claim.issuer) propriété sur toutes les revendications créés par le service d’authentification de cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Le nom de domaine dans lequel le cookie est pris en charge. Par défaut, cela est le nom d’hôte de la demande. Le navigateur envoie uniquement le cookie dans les demandes à un nom d’hôte correspondant. Vous pouvez souhaiter réglez cette option pour que les cookies disponibles pour n’importe quel hôte dans votre domaine. Par exemple, si le domaine du cookie à `.contoso.com` met à disposition `contoso.com`, `www.contoso.com`, et `staging.www.contoso.com`. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Un indicateur qui spécifie si le cookie doit être accessible uniquement aux serveurs. Modification de cette valeur à `false` autorise les scripts côté client pour accéder au cookie et peut s’ouvrir à votre application au vol de cookies doit disposer de votre application un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnérabilité. La valeur par défaut est `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Définit le nom du cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Utilisé pour isoler les applications en cours d’exécution sur le même nom d’hôte. Si vous avez une application en cours d’exécution à `/app1` et souhaitez limitent les cookies à cette application, définissez la `CookiePath` propriété `/app1`. En procédant ainsi, le cookie est uniquement disponible sur les demandes à `/app1` et n’importe quelle application situé en dessous. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Indique si le navigateur doit autoriser le cookie à attacher aux demandes du même site uniquement (`SameSiteMode.Strict`) ou de requêtes inter-site à l’aide des méthodes HTTP sécurisés et requêtes same-site (`SameSiteMode.Lax`). Lorsque la valeur `SameSiteMode.None`, la valeur d’en-tête de cookie n’est pas définie. Notez que [intergiciel (middleware) de Cookie stratégie](#cookie-policy-middleware) peut remplacer la valeur que vous fournissez. Pour prendre en charge l’authentification OAuth, la valeur par défaut est `SameSiteMode.Lax`. Pour plus d’informations, consultez [l’authentification OAuth divisée en raison de la stratégie de cookies SameSite](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Un indicateur qui spécifie si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`). La valeur par défaut est `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Définit le `DataProtectionProvider` qui est utilisé pour créer la valeur par défaut `TicketDataFormat`. Si le `TicketDataFormat` propriété est définie, la `DataProtectionProvider` option n’est pas utilisée. Si ne pas fourni, le fournisseur de protection de données de l’application par défaut est utilisé. |
| [Événements](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Le gestionnaire appelle des méthodes sur le fournisseur qui donnent le contrôle de l’application à certains points de traitement. Si `Events` ne sont pas fournies, une instance par défaut est fournie qui ne fait rien lorsque les méthodes sont appelées. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Utilisé en tant que le type de service pour obtenir le `Events` instance au lieu de la propriété. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | Le `TimeSpan` issue de laquelle le ticket d’authentification stocké dans le cookie expire. `ExpireTimeSpan` est ajouté à l’heure actuelle pour créer le délai d’expiration du ticket. Le `ExpiredTimeSpan` valeur prendra toujours chiffrée AuthTicket vérifié par le serveur. Il peut également aller dans le [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) en-tête, mais uniquement si `IsPersistent` est défini. Pour définir `IsPersistent` à `true`, configurer le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passé à `SignInAsync`. La valeur par défaut de `ExpireTimeSpan` est de 14 jours. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Fournit le chemin d’accès pour fournir un 302 trouvé (redirection d’URL) lorsque déclenchée par `HttpContext.ChallengeAsync`. L’URL actuelle qui a généré l’erreur 401 est ajoutée à la `LoginPath` comme paramètre de chaîne de requête nommé par le `ReturnUrlParameter`. Une fois une demande pour le `LoginPath` accorde une nouvelle identité de connexion, le `ReturnUrlParameter` valeur est utilisée pour rediriger le navigateur vers l’URL qui a provoqué le code d’état non autorisé d’origine. La valeur par défaut est `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Si le `LogoutPath` est fourni pour le gestionnaire, puis redirige une demande à ce chemin d’accès basé sur la valeur de la `ReturnUrlParameter`. La valeur par défaut est `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Détermine le nom du paramètre de chaîne de requête qui est ajouté par le gestionnaire pour une réponse 302 trouvé (redirection d’URL). `ReturnUrlParameter` est utilisé lorsqu’une requête arrive sur le `LoginPath` ou `LogoutPath` pour retourner le navigateur vers l’URL d’origine après l’exécution de l’action de connexion ou déconnexion. La valeur par défaut est `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Un conteneur facultatif utilisé pour stocker l’identité entre les requêtes. Lorsqu’il est utilisé, uniquement un identificateur de session est envoyé au client. `SessionStore` peut être utilisé pour atténuer les problèmes potentiels liés aux identités de grande taille. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Un indicateur qui spécifie si un nouveau cookie avec un délai d’expiration de mise à jour doit être émis de manière dynamique. Cela peut se produire sur toute requête dont la période d’expiration du cookie actuelle est supérieure à 50 % expiré. La nouvelle date d’expiration est déplacée vers l’avant qu’ils doivent être la date actuelle plus la `ExpireTimespan`. Un [délai d’expiration de cookie absolu](xref:security/authentication/cookie#absolute-cookie-expiration) peut être définie à l’aide de la `AuthenticationProperties` lors de l’appel de la classe `SignInAsync`. Un délai d’expiration absolue peut améliorer la sécurité de votre application en limitant la durée pendant laquelle le cookie d’authentification est valide. La valeur par défaut est `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | Le `TicketDataFormat` est utilisé pour protéger et déprotéger l’identité et autres propriétés qui sont stockées dans la valeur du cookie. Si n’est fourni, un `TicketDataFormat` est créé à l’aide de la [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Validate](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Méthode qui vérifie que les options sont valides. |

Définissez `CookieAuthenticationOptions` dans la configuration du service pour l’authentification dans le `ConfigureServices` méthode :

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x utilise cookie [intergiciel (middleware)](xref:fundamentals/middleware/index) qui sérialise un principal d’utilisateur dans un cookie chiffré. Pour les requêtes ultérieures, le cookie est validé, et le principal est recréé et affecté à la `HttpContext.User` propriété.

Installer le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package NuGet dans votre projet. Ce package contient le middleware de cookie.

Utilisez le `UseCookieAuthentication` méthode dans le `Configure` méthode dans votre *Startup.cs* fichier avant `UseMvc` ou `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**Options de CookieAuthenticationOptions**

Le [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe est utilisée pour configurer les options de fournisseur d’authentification.

| Option | Description |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Définit le schéma d’authentification. `AuthenticationScheme` est utile quand il existe plusieurs instances de l’authentification et que vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme). Définition de la `AuthenticationScheme` à `CookieAuthenticationDefaults.AuthenticationScheme` fournit une valeur de « Cookies » pour le schéma. Vous pouvez fournir n’importe quelle valeur de chaîne qui distingue le schéma. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Définit une valeur pour indiquer que l’authentification de cookie doit s’exécuter sur chaque demande et tentez de valider et de reconstruire n’importe quel principal sérialisé qu'il créé. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Si la valeur est true, le middleware d’authentification gère les défis automatique. Si false, le middleware d’authentification modifie uniquement les réponses lorsque indiqué explicitement par le `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | L’émetteur à utiliser pour le [émetteur](/dotnet/api/system.security.claims.claim.issuer) propriété sur toutes les revendications créés par l’intergiciel d’authentification de cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Le nom de domaine dans lequel le cookie est pris en charge. Par défaut, cela est le nom d’hôte de la demande. Le navigateur sert uniquement le cookie à un nom d’hôte correspondant. Vous pouvez souhaiter réglez cette option pour que les cookies disponibles pour n’importe quel hôte dans votre domaine. Par exemple, si le domaine du cookie à `.contoso.com` met à disposition `contoso.com`, `www.contoso.com`, et `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Un indicateur qui spécifie si le cookie doit être accessible uniquement aux serveurs. Modification de cette valeur à `false` autorise les scripts côté client pour accéder au cookie et peut s’ouvrir à votre application au vol de cookies doit disposer de votre application un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnérabilité. La valeur par défaut est `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Utilisé pour isoler les applications en cours d’exécution sur le même nom d’hôte. Si vous avez une application en cours d’exécution à `/app1` et souhaitez limitent les cookies à cette application, définissez la `CookiePath` propriété `/app1`. En procédant ainsi, le cookie est uniquement disponible sur les demandes à `/app1` et n’importe quelle application situé en dessous. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Un indicateur qui spécifie si le cookie créé doit être limité à HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou le même protocole que la demande (`CookieSecurePolicy.SameAsRequest`). La valeur par défaut est `CookieSecurePolicy.SameAsRequest`. |
| [Description](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Informations supplémentaires sur le type d’authentification qui est à votre disposition à l’application. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | Le `TimeSpan` issue de laquelle le ticket d’authentification expire. Il est ajouté à l’heure actuelle pour créer le délai d’expiration du ticket. Pour utiliser `ExpireTimeSpan`, vous devez définir `IsPersistent` à `true` dans le `AuthenticationProperties` passé à `SignInAsync`. La valeur par défaut est de 14 jours. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Un indicateur qui spécifie si la date d’expiration du cookie réinitialise lorsque plusieurs la moitié de la `ExpireTimeSpan` est écoulé. La nouvelle heure d’expiration est déplacée vers l’avant qu’ils doivent être la date actuelle plus la `ExpireTimespan`. Un [délai d’expiration de cookie absolu](xref:security/authentication/cookie#absolute-cookie-expiration) peut être définie à l’aide de la `AuthenticationProperties` lors de l’appel de la classe `SignInAsync`. Un délai d’expiration absolue peut améliorer la sécurité de votre application en limitant la durée pendant laquelle le cookie d’authentification est valide. La valeur par défaut est `true`. |

Définissez `CookieAuthenticationOptions` du Middleware de Cookie d’authentification dans le `Configure` méthode :

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>Intergiciel (middleware) de cookie stratégie

[Intergiciel (middleware) de cookie stratégie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) offre des fonctionnalités de stratégie de cookie dans une application. Ajout de l’intergiciel (middleware) au pipeline de traitement d’application est l’ordre de la casse ; Il affecte uniquement les composants enregistrés après lui dans le pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 Le [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fourni au Middleware Cookie stratégie vous permettent de contrôler les caractéristiques globales du traitement du cookie et le crochet dans les gestionnaires de traitement du cookie lorsque les cookies sont ajoutées ou supprimées.

| Propriété | Description |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Affecte si les cookies doivent être HttpOnly, qui est un indicateur indiquant si le cookie doit être accessible uniquement aux serveurs. La valeur par défaut est `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Affecte l’attribut du même site du cookie (voir ci-dessous). La valeur par défaut est `SameSiteMode.Lax`. Cette option est disponible pour ASP.NET Core 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Appelé lorsqu’un cookie est ajouté. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Appelée lorsqu’un cookie est supprimé. |
| [Sécuriser](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Détermine si les cookies doivent être sécurisé. La valeur par défaut est `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + uniquement)

La valeur par défaut `MinimumSameSitePolicy` valeur est `SameSiteMode.Lax` pour autoriser l’authentification OAuth2. À strictement appliquer une stratégie de même site de `SameSiteMode.Strict`, définissez le `MinimumSameSitePolicy`. Bien que ce paramètre s’arrête OAuth2 et autres schémas d’authentification de cross-origin, elle élève le niveau de sécurité du cookie pour les autres types d’applications qui ne reposent pas sur le traitement de la demande cross-origin.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Le paramètre d’intergiciel (middleware) stratégie de Cookie pour `MinimumSameSitePolicy` peuvent affecter votre valeur de `Cookie.SameSite` dans `CookieAuthenticationOptions` paramètres en fonction de la matrice ci-dessous.

| MinimumSameSitePolicy | Cookie.SameSite | Paramètre Cookie.SameSite résultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Créer un cookie d’authentification

Pour créer un cookie contenant les informations de l’utilisateur, vous devez construire un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal). Les informations de l’utilisateur sont sérialisées et stockées dans le cookie. 

::: moniker range=">= aspnetcore-2.0"

Créer un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) avec n’importe quel requis [revendication](/dotnet/api/system.security.claims.claim)s et appelez [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) pour connecter l’utilisateur :

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Appelez [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) pour connecter l’utilisateur :

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` Crée un cookie chiffré et l’ajoute à la réponse actuelle. Si vous ne spécifiez pas un `AuthenticationScheme`, le schéma par défaut est utilisé.

En coulisses, du chiffrement utilisé est d’ASP.NET Core [Protection des données](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) système. Si vous hébergez une application sur plusieurs ordinateurs, l’équilibrage de charge entre les applications ou à l’aide d’une batterie de serveurs web, vous devez [configurer la protection des données](xref:security/data-protection/configuration/overview) à utiliser le même porte-clés et l’identificateur de l’application.

## <a name="sign-out"></a>Déconnexion

::: moniker range=">= aspnetcore-2.0"

Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

Si vous n’utilisez pas `CookieAuthenticationDefaults.AuthenticationScheme` (ou « Cookies ») en tant que le schéma (par exemple, « ContosoCookie »), fournir le schéma que vous avez utilisé lors de la configuration du fournisseur d’authentification. Sinon, le schéma par défaut est utilisé.

## <a name="react-to-back-end-changes"></a>Réagir aux modifications du serveur principal

Une fois qu’un cookie est créé, il devient la seule source d’identité. Même si vous désactivez un utilisateur dans vos systèmes back-end, le système d’authentification de cookie n’a aucune connaissance de ce et un utilisateur reste connecté tant que leur cookie est valide.

Le [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) événements dans ASP.NET Core 2.x ou [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) méthode dans ASP.NET Core 1.x peut être utilisé pour intercepter et de remplacer la validation de l’identité de cookie. Cette approche réduit le risque d’utilisateurs révoqués l’accès à l’application.

Une approche à la validation de cookie est basée sur le suivi des lorsque la base de données utilisateur a été modifiée. Si la base de données n’a pas été modifié depuis que le cookie d’utilisateur a été émis, il n’est pas nécessaire de s’authentifier de nouveau l’utilisateur si le cookie est toujours valide. Pour implémenter ce scénario, la base de données, qui est implémenté dans `IUserRepository` pour cet exemple, stocke un `LastChanged` valeur. Mise à jour de n’importe quel utilisateur dans la base de données, le `LastChanged` a la valeur à l’heure actuelle.

Afin d’invalider un cookie lorsque les modifications de base de données basée sur le `LastChanged` valeur, la création du cookie avec un `LastChanged` revendication contenant actuel `LastChanged` valeur à partir de la base de données :

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker range=">= aspnetcore-2.0"

Pour implémenter une substitution pour le `ValidatePrincipal` événement, écriture, une méthode avec la signature suivante dans une classe que vous dérivez de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Un exemple se présente comme suit :

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Enregistrer l’instance d’événements lors de l’inscription du service cookie dans le `ConfigureServices` (méthode). Fournir une inscription de service délimité pour votre `CustomCookieAuthenticationEvents` classe :

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pour implémenter une substitution pour la `ValidateAsync` événement, écriture, une méthode avec la signature suivante :

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity implémente cette vérification dans le cadre de son [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Un exemple se présente comme suit :

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Inscrire l’événement lors de la configuration de l’authentification de cookie dans le `Configure` méthode :

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

Considérez une situation dans laquelle le nom d’utilisateur est mise à jour &mdash; une décision qui n’affecte pas la sécurité en aucune façon. Si vous souhaitez mettre à jour non-destructive de l’utilisateur principal, appelez `context.ReplacePrincipal` et définir le `context.ShouldRenew` propriété `true`.

> [!WARNING]
> L’approche décrite ici est déclenchée à chaque demande. Cela peut entraîner une altération des performances pour l’application.

## <a name="persistent-cookies"></a>Cookies persistants

Vous souhaiterez peut-être le cookie à conserver entre les sessions de navigateur. Cette persistance doit uniquement être activé avec le consentement explicite de l’utilisateur avec une case à cocher « Mémoriser mes informations » sur la connexion ou un mécanisme similaire. 

L’extrait de code suivant crée une identité et le cookie correspondant qui persiste lors par le biais des fermetures de navigateur. Tous les paramètres d’expiration décalée précédemment configurés sont respectées. Si le cookie expire pendant que le navigateur est fermé, le navigateur supprime le cookie s’est arrêté.

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) classe réside dans le `Microsoft.AspNetCore.Authentication` espace de noms.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Le [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) classe réside dans le `Microsoft.AspNetCore.Http.Authentication` espace de noms.

::: moniker-end

## <a name="absolute-cookie-expiration"></a>Expiration du cookie absolu

Vous pouvez définir une heure d’expiration absolue avec `ExpiresUtc`. Pour créer un cookie persistant, vous devez également définir `IsPersistent`; sinon, le cookie est créé avec une durée de vie de session et peut expirer avant ou après l’authentification de ticket qui il détient. Lorsque `ExpiresUtc` est définie sur `SignInAsync`, il remplace la valeur de la `ExpireTimeSpan` option de `CookieAuthenticationOptions`, si définie.

L’extrait de code suivant crée une identité et le cookie correspondant qui dure 20 minutes. Il ignore tous les paramètres d’expiration décalée précédemment configurés.

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [Modifications de AUTH 2.0 / annonce de la Migration](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Vérifications de rôle de stratégie](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
