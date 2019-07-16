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
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="9d29a-103">Partager des cookies d’authentification entre les applications ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9d29a-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="9d29a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9d29a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9d29a-105">Sites Web sont souvent constitués de collaboration des applications web individuelles.</span><span class="sxs-lookup"><span data-stu-id="9d29a-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="9d29a-106">Pour fournir une expérience d’authentification unique (SSO), les applications web au sein d’un site doivent partager des cookies d’authentification.</span><span class="sxs-lookup"><span data-stu-id="9d29a-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="9d29a-107">Pour prendre en charge ce scénario, la pile de protection des données permet le partage de Katana l’authentification des cookies et tickets d’authentification par cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d29a-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="9d29a-108">Dans les exemples qui suivent :</span><span class="sxs-lookup"><span data-stu-id="9d29a-108">In the examples that follow:</span></span>

* <span data-ttu-id="9d29a-109">Le nom du cookie d’authentification est défini sur une valeur commune de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="9d29a-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="9d29a-110">Le `AuthenticationType` a la valeur `Identity.Application` explicitement ou par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d29a-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="9d29a-111">Un nom d’application commun est utilisé pour permettre au système de protection des données à partager les clés de protection de données (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="9d29a-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="9d29a-112">`Identity.Application` est utilisé en tant que le schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="9d29a-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="9d29a-113">Tout schéma est utilisé, il doit être utilisé de manière cohérente *au sein et entre* les applications de cookie partagé en tant que le schéma par défaut ou en le définissant explicitement.</span><span class="sxs-lookup"><span data-stu-id="9d29a-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="9d29a-114">Le schéma est utilisé pour le chiffrement et déchiffrement de cookies, donc un schéma cohérent doit être utilisé dans les applications.</span><span class="sxs-lookup"><span data-stu-id="9d29a-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="9d29a-115">Un commun [clé de protection des données](xref:security/data-protection/implementation/key-management) emplacement de stockage est utilisé.</span><span class="sxs-lookup"><span data-stu-id="9d29a-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="9d29a-116">Dans les applications ASP.NET Core, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> est utilisé pour définir l’emplacement de stockage de clés.</span><span class="sxs-lookup"><span data-stu-id="9d29a-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="9d29a-117">Dans les applications .NET Framework, le Middleware de Cookie d’authentification utilise une implémentation de <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span><span class="sxs-lookup"><span data-stu-id="9d29a-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="9d29a-118">`DataProtectionProvider` Fournit des services de protection des données pour le chiffrement et le déchiffrement des données de charge utile de cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="9d29a-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="9d29a-119">Le `DataProtectionProvider` instance est isolée à partir du système de protection des données utilisé par les autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="9d29a-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="9d29a-120">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Action\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepte un <xref:System.IO.DirectoryInfo> pour spécifier l’emplacement de stockage de clés de protection de données.</span><span class="sxs-lookup"><span data-stu-id="9d29a-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="9d29a-121">`DataProtectionProvider` nécessite le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package NuGet :</span><span class="sxs-lookup"><span data-stu-id="9d29a-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="9d29a-122">Dans les applications ASP.NET Core 2.x, référencez le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9d29a-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="9d29a-123">Dans les applications .NET Framework, ajoutez une référence de package à [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="9d29a-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="9d29a-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> définit le nom d’application courants.</span><span class="sxs-lookup"><span data-stu-id="9d29a-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="9d29a-125">Partager des cookies d’authentification entre les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d29a-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="9d29a-126">Lorsque vous utilisez ASP.NET Core Identity :</span><span class="sxs-lookup"><span data-stu-id="9d29a-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="9d29a-127">Clés de protection des données et le nom de l’application doivent être partagés entre les applications.</span><span class="sxs-lookup"><span data-stu-id="9d29a-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="9d29a-128">Un emplacement de stockage de clés commun est fourni pour le <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> méthode dans les exemples suivants.</span><span class="sxs-lookup"><span data-stu-id="9d29a-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="9d29a-129">Utilisez <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> pour configurer un nom d’application partagé commun (`SharedCookieApp` dans les exemples suivants).</span><span class="sxs-lookup"><span data-stu-id="9d29a-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="9d29a-130">Pour plus d'informations, consultez <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="9d29a-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="9d29a-131">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> méthode d’extension pour configurer le service de protection des données pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="9d29a-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="9d29a-132">Dans l’exemple suivant, le type d’authentification est défini `Identity.Application` par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d29a-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="9d29a-133">Dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9d29a-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="9d29a-134">Lors de l’utilisation de cookies directement sans ASP.NET Core Identity, configurer la protection des données et l’authentification dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9d29a-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9d29a-135">Dans l’exemple suivant, le type d’authentification est défini `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="9d29a-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="9d29a-136">Lorsque vous hébergez des applications qui partagent des cookies entre les sous-domaines, spécifiez un domaine commun dans le [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) propriété.</span><span class="sxs-lookup"><span data-stu-id="9d29a-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="9d29a-137">Pour partager des cookies entre applications à `contoso.com`, tel que `first_subdomain.contoso.com` et `second_subdomain.contoso.com`, spécifiez la `Cookie.Domain` comme `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="9d29a-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="9d29a-138">Chiffrer les clés de protection des données au repos</span><span class="sxs-lookup"><span data-stu-id="9d29a-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="9d29a-139">Pour les déploiements de production, configurez le `DataProtectionProvider` pour chiffrer les clés au repos avec DPAPI ou un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="9d29a-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="9d29a-140">Pour plus d'informations, consultez <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="9d29a-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="9d29a-141">Dans l’exemple suivant, un empreinte numérique du certificat est fourni à <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="9d29a-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="9d29a-142">Partager des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d29a-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="9d29a-143">Les applications ASP.NET 4.x qui utilisent l’intergiciel (middleware) de Katana Cookie d’authentification peuvent être configurées pour générer des cookies d’authentification qui sont compatibles avec l’intergiciel d’authentification ASP.NET Core Cookie.</span><span class="sxs-lookup"><span data-stu-id="9d29a-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="9d29a-144">Cela permet la mise à niveau des applications individuelles d’un site grande taille en plusieurs étapes tout en garantissant une expérience sans heurts de l’authentification unique sur le site.</span><span class="sxs-lookup"><span data-stu-id="9d29a-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="9d29a-145">Lorsqu’une application utilise l’intergiciel (middleware) de Katana Cookie d’authentification, il appelle `UseCookieAuthentication` dans le projet *Startup.Auth.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="9d29a-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="9d29a-146">Projets d’application web ASP.NET 4.x créées avec Visual Studio 2013 et l’utilisent ultérieurement l’intergiciel d’authentification de Cookie Katana par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d29a-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="9d29a-147">Bien que `UseCookieAuthentication` est obsolète et non pris en charge pour les applications ASP.NET Core, appelant `UseCookieAuthentication` dans ASP.NET 4.x application qui utilise l’intergiciel (middleware) de Katana Cookie d’authentification est valide.</span><span class="sxs-lookup"><span data-stu-id="9d29a-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="9d29a-148">Une application de 4.x ASP.NET doit cibler .NET Framework 4.5.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9d29a-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="9d29a-149">Sinon, les packages NuGet nécessaires échouent à installer.</span><span class="sxs-lookup"><span data-stu-id="9d29a-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="9d29a-150">Pour partager des cookies d’authentification entre d’une application ASP.NET 4.x et une application ASP.NET Core, configurez l’application ASP.NET Core, comme indiqué dans le [partager des cookies d’authentification entre les applications ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) section, puis configurez l’application de 4.x ASP.NET en tant que suit.</span><span class="sxs-lookup"><span data-stu-id="9d29a-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="9d29a-151">Vérifiez que les packages de l’application sont mis à jour pour les dernières versions.</span><span class="sxs-lookup"><span data-stu-id="9d29a-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="9d29a-152">Installer le [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package à chaque application que ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="9d29a-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="9d29a-153">Recherchez et modifiez l’appel à `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="9d29a-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="9d29a-154">Modifier le nom du cookie doit correspondre au nom utilisé par l’intergiciel d’authentification ASP.NET Core Cookie (`.AspNet.SharedCookie` dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="9d29a-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="9d29a-155">Dans l’exemple suivant, le type d’authentification est défini `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="9d29a-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="9d29a-156">Fournir une instance d’un `DataProtectionProvider` initialisé à l’emplacement de stockage de clés de protection données courantes.</span><span class="sxs-lookup"><span data-stu-id="9d29a-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="9d29a-157">Vérifiez que le nom de l’application est défini sur le nom d’application commun utilisé par toutes les applications qui partagent des cookies d’authentification (`SharedCookieApp` dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="9d29a-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="9d29a-158">Si vous ne définissez ne pas `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` et `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, affectez la valeur <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> pour une revendication qui distingue les utilisateurs uniques.</span><span class="sxs-lookup"><span data-stu-id="9d29a-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="9d29a-159">*App_Start/Startup.auth.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d29a-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="9d29a-160">Lors de la génération d’une identité d’utilisateur, le type d’authentification (`Identity.Application`) doit correspondre au type défini dans `AuthenticationType` avec `UseCookieAuthentication` dans *App_Start/Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="9d29a-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="9d29a-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d29a-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="9d29a-162">Utiliser une base de données commune de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="9d29a-162">Use a common user database</span></span>

<span data-ttu-id="9d29a-163">Lorsque les applications utilisent la même identité que le schéma (même version de l’identité), vérifiez que le système d’identité pour chaque application est pointé vers la même base de données utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9d29a-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="9d29a-164">Sinon, le système d’identité génère des échecs lors de l’exécution lorsqu’il tente de faire correspondre les informations contenues dans le cookie d’authentification sur les informations contenues dans sa base de données.</span><span class="sxs-lookup"><span data-stu-id="9d29a-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="9d29a-165">Lorsque le schéma de l’identité est différent entre les applications, généralement parce que les applications sont à l’aide de différentes versions de l’identité, partage d’une base de données commune basée sur la dernière version de l’identité n’est pas possible sans le remappage et ajout de colonnes dans les schémas d’identité de l’autre application.</span><span class="sxs-lookup"><span data-stu-id="9d29a-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="9d29a-166">Il est souvent plus efficace de mettre à niveau les autres applications pour utiliser la dernière version de l’identité afin qu’une base de données courants peut être partagée par les applications.</span><span class="sxs-lookup"><span data-stu-id="9d29a-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d29a-167">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9d29a-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
