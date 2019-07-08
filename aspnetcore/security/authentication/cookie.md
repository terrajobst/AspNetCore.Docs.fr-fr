---
title: Utiliser l’authentification par cookie sans ASP.NET Core Identity
author: rick-anderson
description: Découvrez comment utiliser l’authentification par cookie sans ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622744"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Utiliser l’authentification par cookie sans ASP.NET Core Identity

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)

ASP.NET Core Identity est un fournisseur d’authentification complète et complet pour créer et maintenir des connexions. Toutefois, un fournisseur d’authentification de l’authentification par cookie sans ASP.NET Core Identity peut être utilisé. Pour plus d'informations, consultez <xref:security/authentication/identity>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Fins de démonstration dans l’exemple d’application, le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application. Utilisez le **E-mail** nom d’utilisateur `maria.rodriguez@contoso.com` et n’importe quel mot de passe pour vous connecter l’utilisateur. L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier. Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données.

## <a name="configuration"></a>Configuration

Si l’application n’utilise pas le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), créez une référence de package dans le fichier de projet pour le [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.

Dans le `Startup.ConfigureServices` (méthode), créez le service de l’intergiciel d’authentification avec le <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> et <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> méthodes :

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passé à `AddAuthentication` définit le schéma d’authentification par défaut pour l’application. `AuthenticationScheme` est utile quand il existe plusieurs instances de l’authentification des cookies et que vous souhaitez [autoriser avec un schéma spécifique](xref:security/authorization/limitingidentitybyscheme). Définition de la `AuthenticationScheme` à [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) fournit une valeur de « Cookies » pour le schéma. Vous pouvez fournir n’importe quelle valeur de chaîne qui distingue le schéma.

Schéma d’authentification de l’application est différent de schéma d’authentification de cookie de l’application. Lorsqu’un schéma d’authentification de cookie n’est pas fourni pour <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, elle utilise `CookieAuthenticationDefaults.AuthenticationScheme` (« Cookies »).

Le cookie d’authentification <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> propriété a la valeur `true` par défaut. Les cookies d’authentification sont autorisées lorsqu’un visiteur du site n’a pas donné son consentement pour la collecte de données. Pour plus d'informations, consultez <xref:security/gdpr#essential-cookies>.

Dans le `Startup.Configure` méthode, appelez le `UseAuthentication` méthode à appeler l’intergiciel d’authentification qui définit le `HttpContext.User` propriété. Appelez le `UseAuthentication` méthode avant d’appeler `UseMvcWithDefaultRoute` ou `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

Le <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> classe est utilisée pour configurer les options de fournisseur d’authentification.

Définissez `CookieAuthenticationOptions` dans la configuration du service pour l’authentification dans le `Startup.ConfigureServices` méthode :

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Intergiciel (middleware) de cookie stratégie

[Intergiciel (middleware) de cookie stratégie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) offre des fonctionnalités de stratégie de cookie. Ajout de l’intergiciel (middleware) au pipeline de traitement d’application est l’ordre sensible&mdash;il affecte uniquement les composants en aval enregistrés dans le pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Utilisez <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fourni à l’intergiciel de stratégie de Cookie pour contrôler des caractéristiques globales du traitement du cookie et de raccordement en gestionnaires de traitement du cookie lorsque les cookies sont ajoutées ou supprimées.

La valeur par défaut <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> valeur est `SameSiteMode.Lax` pour autoriser l’authentification OAuth2. À strictement appliquer une stratégie de même site de `SameSiteMode.Strict`, définissez le `MinimumSameSitePolicy`. Bien que ce paramètre s’arrête OAuth2 et autres schémas d’authentification de cross-origin, elle élève le niveau de sécurité du cookie pour les autres types d’applications qui ne reposent pas sur le traitement de la demande cross-origin.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Le paramètre d’intergiciel (middleware) stratégie de Cookie pour `MinimumSameSitePolicy` peuvent affecter la valeur de `Cookie.SameSite` dans `CookieAuthenticationOptions` paramètres en fonction de la matrice ci-dessous.

| MinimumSameSitePolicy | Cookie.SameSite | Paramètre Cookie.SameSite résultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Créer un cookie d’authentification

Pour créer un cookie contenant les informations de l’utilisateur, construisez un <xref:System.Security.Claims.ClaimsPrincipal>. Les informations de l’utilisateur sont sérialisées et stockées dans le cookie. 

Créer un <xref:System.Security.Claims.ClaimsIdentity> avec n’importe quel requis <xref:System.Security.Claims.Claim>s et appelez <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> pour connecter l’utilisateur :

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` Crée un cookie chiffré et l’ajoute à la réponse actuelle. Si `AuthenticationScheme` n’est pas spécifié, le schéma par défaut est utilisé.

D’ASP.NET Core [Protection des données](xref:security/data-protection/using-data-protection) système est utilisé pour le chiffrement. Pour une application hébergée sur plusieurs ordinateurs, charge équilibrage entre applications, ou à l’aide d’une batterie de serveurs web, [configurer la protection des données](xref:security/data-protection/configuration/overview) à utiliser le même porte-clés et l’identificateur de l’application.

## <a name="sign-out"></a>Déconnexion

Pour déconnecter l’utilisateur actuel et supprimer leur cookie, appelez <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Si `CookieAuthenticationDefaults.AuthenticationScheme` (ou « Cookies ») n’est pas utilisé comme le schéma (par exemple, « ContosoCookie »), fournir le schéma utilisé lors de la configuration du fournisseur d’authentification. Sinon, le schéma par défaut est utilisé.

## <a name="react-to-back-end-changes"></a>Réagir aux modifications du serveur principal

Une fois qu’un cookie est créé, le cookie est la seule source d’identité. Si un compte d’utilisateur est désactivé dans les systèmes back-end :

* Système d’authentification de cookie de l’application continue à traiter les demandes basées sur le cookie d’authentification.
* L’utilisateur reste connecté à l’application tant que le cookie d’authentification est valid.

Le <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> événement peut être utilisé pour intercepter et de remplacer la validation de l’identité de cookie. Valider le cookie à chaque demande d’atténue le risque des utilisateurs révoqués l’accès à l’application.

Une approche à la validation de cookie est basée sur le suivi des lorsque la base de données utilisateur est modifié. Si la base de données n’a pas été modifié depuis que le cookie d’utilisateur a été émis, il n’est pas nécessaire de s’authentifier de nouveau l’utilisateur si le cookie est toujours valide. Dans l’exemple d’application, la base de données est implémenté dans `IUserRepository` et stocke un `LastChanged` valeur. Lorsqu’un utilisateur est mis à jour dans la base de données, le `LastChanged` a la valeur à l’heure actuelle.

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

Pour implémenter une substitution pour le `ValidatePrincipal` événement, écriture, une méthode avec la signature suivante dans une classe qui dérive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Voici un exemple d’implémentation de `CookieAuthenticationEvents`:

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

Enregistrer l’instance d’événements lors de l’inscription du service cookie dans le `Startup.ConfigureServices` (méthode). Fournir un [étendue de l’inscription du service](xref:fundamentals/dependency-injection#service-lifetimes) pour votre `CustomCookieAuthenticationEvents` classe :

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Considérez une situation dans laquelle le nom d’utilisateur est mise à jour&mdash;une décision qui n’affecte pas la sécurité en aucune façon. Si vous souhaitez mettre à jour non-destructive de l’utilisateur principal, appelez `context.ReplacePrincipal` et définir le `context.ShouldRenew` propriété `true`.

> [!WARNING]
> L’approche décrite ici est déclenchée à chaque demande. Validation des cookies d’authentification pour tous les utilisateurs à chaque demande peut entraîner une altération des performances pour l’application.

## <a name="persistent-cookies"></a>Cookies persistants

Vous souhaiterez peut-être le cookie à conserver entre les sessions de navigateur. Cette persistance doit uniquement être activé avec le consentement explicite de l’utilisateur avec une case à cocher « Mémoriser mes informations » sur la connexion ou un mécanisme similaire. 

L’extrait de code suivant crée une identité et le cookie correspondant qui persiste lors par le biais des fermetures de navigateur. Tous les paramètres d’expiration décalée précédemment configurés sont respectées. Si le cookie expire pendant que le navigateur est fermé, le navigateur supprime le cookie s’est arrêté.

Définissez <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> à `true` dans <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Expiration du cookie absolu

Un délai d’expiration absolue peut être défini avec <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Pour créer un cookie persistant, `IsPersistent` doit également être défini. Sinon, le cookie est créé avec une durée de vie de session et peut expirer avant ou après le ticket d’authentification qu’il détient. Lorsque `ExpiresUtc` est défini, il remplace la valeur de la <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si définie.

L’extrait de code suivant crée une identité et le cookie correspondant qui dure 20 minutes. Il ignore tous les paramètres d’expiration décalée précédemment configurés.

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Vérifications de rôle de stratégie](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
