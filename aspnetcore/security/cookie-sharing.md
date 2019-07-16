---
title: Partager des cookies d’authentification entre les applications ASP.NET
author: rick-anderson
description: Découvrez comment partager des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223924"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Partager des cookies d’authentification entre les applications ASP.NET

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)

Sites Web sont souvent constitués de collaboration des applications web individuelles. Pour fournir une expérience d’authentification unique (SSO), les applications web au sein d’un site doivent partager des cookies d’authentification. Pour prendre en charge ce scénario, la pile de protection des données permet le partage de Katana l’authentification des cookies et tickets d’authentification par cookie ASP.NET Core.

Dans les exemples qui suivent :

* Le nom du cookie d’authentification est défini sur une valeur commune de `.AspNet.SharedCookie`.
* Le `AuthenticationType` a la valeur `Identity.Application` explicitement ou par défaut.
* Un nom d’application commun est utilisé pour permettre au système de protection des données à partager les clés de protection de données (`SharedCookieApp`).
* `Identity.Application` est utilisé en tant que le schéma d’authentification. Tout schéma est utilisé, il doit être utilisé de manière cohérente *au sein et entre* les applications de cookie partagé en tant que le schéma par défaut ou en le définissant explicitement. Le schéma est utilisé pour le chiffrement et déchiffrement de cookies, donc un schéma cohérent doit être utilisé dans les applications.
* Un commun [clé de protection des données](xref:security/data-protection/implementation/key-management) emplacement de stockage est utilisé.
  * Dans les applications ASP.NET Core, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> est utilisé pour définir l’emplacement de stockage de clés.
  * Dans les applications .NET Framework, le Middleware de Cookie d’authentification utilise une implémentation de <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` Fournit des services de protection des données pour le chiffrement et le déchiffrement des données de charge utile de cookie d’authentification. Le `DataProtectionProvider` instance est isolée à partir du système de protection des données utilisé par les autres parties de l’application. [DataProtectionProvider.Create (System.IO.DirectoryInfo, Action\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepte un <xref:System.IO.DirectoryInfo> pour spécifier l’emplacement de stockage de clés de protection de données.
* `DataProtectionProvider` nécessite le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package NuGet :
  * Dans les applications ASP.NET Core 2.x, référencez le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).
  * Dans les applications .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> définit le nom d’application courants.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Partager des cookies d’authentification entre les applications ASP.NET Core

Lorsque vous utilisez ASP.NET Core Identity :

* Clés de protection des données et le nom de l’application doivent être partagés entre les applications. Un emplacement de stockage de clés commun est fourni pour le <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> méthode dans les exemples suivants. Utilisez <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> pour configurer un nom d’application partagé commun (`SharedCookieApp` dans les exemples suivants). Pour plus d'informations, consultez <xref:security/data-protection/configuration/overview>.
* Utilisez le <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> méthode d’extension pour configurer le service de protection des données pour les cookies.
* Dans l’exemple suivant, le type d’authentification est défini `Identity.Application` par défaut.

Dans `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

Lors de l’utilisation de cookies directement sans ASP.NET Core Identity, configurer la protection des données et l’authentification dans `Startup.ConfigureServices`. Dans l’exemple suivant, le type d’authentification est défini `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

Lorsque vous hébergez des applications qui partagent des cookies entre les sous-domaines, spécifiez un domaine commun dans le [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) propriété. Pour partager des cookies entre applications à `contoso.com`, tel que `first_subdomain.contoso.com` et `second_subdomain.contoso.com`, spécifiez la `Cookie.Domain` comme `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Chiffrer les clés de protection des données au repos

Pour les déploiements de production, configurez le `DataProtectionProvider` pour chiffrer les clés au repos avec DPAPI ou un X509Certificate. Pour plus d'informations, consultez <xref:security/data-protection/implementation/key-encryption-at-rest>. Dans l’exemple suivant, un empreinte numérique du certificat est fourni à <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Partager des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core

Les applications ASP.NET 4.x qui utilisent l’intergiciel (middleware) de Katana Cookie d’authentification peuvent être configurées pour générer des cookies d’authentification qui sont compatibles avec l’intergiciel d’authentification ASP.NET Core Cookie. Cela permet la mise à niveau des applications individuelles d’un site grande taille en plusieurs étapes tout en garantissant une expérience sans heurts de l’authentification unique sur le site.

Lorsqu’une application utilise l’intergiciel (middleware) de Katana Cookie d’authentification, il appelle `UseCookieAuthentication` dans le projet *Startup.Auth.cs* fichier. Projets d’application web ASP.NET 4.x créées avec Visual Studio 2013 et l’utilisent ultérieurement l’intergiciel d’authentification de Cookie Katana par défaut. Bien que `UseCookieAuthentication` est obsolète et non pris en charge pour les applications ASP.NET Core, appelant `UseCookieAuthentication` dans ASP.NET 4.x application qui utilise l’intergiciel (middleware) de Katana Cookie d’authentification est valide.

Une application de 4.x ASP.NET doit cibler .NET Framework 4.5.1 ou version ultérieure. Sinon, les packages NuGet nécessaires échouent à installer.

Pour partager des cookies d’authentification entre d’une application ASP.NET 4.x et une application ASP.NET Core, configurez l’application ASP.NET Core, comme indiqué dans le [partager des cookies d’authentification entre les applications ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) section, puis configurez l’application de 4.x ASP.NET en tant que suit.

Vérifiez que les packages de l’application sont mis à jour pour les dernières versions. Installer le [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package à chaque application que ASP.NET 4.x.

Recherchez et modifiez l’appel à `UseCookieAuthentication`:

* Modifier le nom du cookie doit correspondre au nom utilisé par l’intergiciel d’authentification ASP.NET Core Cookie (`.AspNet.SharedCookie` dans l’exemple).
* Dans l’exemple suivant, le type d’authentification est défini `Identity.Application`.
* Fournir une instance d’un `DataProtectionProvider` initialisé à l’emplacement de stockage de clés de protection données courantes.
* Vérifiez que le nom de l’application est défini sur le nom d’application commun utilisé par toutes les applications qui partagent des cookies d’authentification (`SharedCookieApp` dans l’exemple).

Si vous ne définissez ne pas `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` et `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, affectez la valeur <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> pour une revendication qui distingue les utilisateurs uniques.

*App_Start/Startup.auth.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

Lors de la génération d’une identité d’utilisateur, le type d’authentification (`Identity.Application`) doit correspondre au type défini dans `AuthenticationType` avec `UseCookieAuthentication` dans *App_Start/Startup.Auth.cs*.

*Models/IdentityModels.cs*:

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a>Utiliser une base de données commune de l’utilisateur

Lorsque les applications utilisent la même identité que le schéma (même version de l’identité), vérifiez que le système d’identité pour chaque application est pointé vers la même base de données utilisateur. Sinon, le système d’identité génère des échecs lors de l’exécution lorsqu’il tente de faire correspondre les informations contenues dans le cookie d’authentification sur les informations contenues dans sa base de données.

Lorsque le schéma de l’identité est différent entre les applications, généralement parce que les applications sont à l’aide de différentes versions de l’identité, partage d’une base de données commune basée sur la dernière version de l’identité n’est pas possible sans le remappage et ajout de colonnes dans les schémas d’identité de l’autre application. Il est souvent plus efficace de mettre à niveau les autres applications pour utiliser la dernière version de l’identité afin qu’une base de données courants peut être partagée par les applications.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/web-farm>
