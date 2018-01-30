---
title: Partage des cookies entre applications
author: rick-anderson
description: "Découvrez comment partager des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e87caa5ba78c6b4c365facc0dea07d747e7c9589
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="1d90f-103">Partage des cookies entre applications</span><span class="sxs-lookup"><span data-stu-id="1d90f-103">Sharing cookies among apps</span></span>

<span data-ttu-id="1d90f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1d90f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1d90f-105">Sites Web sont souvent composent d’applications web individuelles fonctionnent ensemble.</span><span class="sxs-lookup"><span data-stu-id="1d90f-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="1d90f-106">Pour fournir une expérience d’authentification unique (SSO), les applications web au sein d’un site doivent partager les cookies d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1d90f-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="1d90f-107">Pour prendre en charge ce scénario, la pile de protection des données permet de partager une authentification de cookie Katana et tickets d’authentification par cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d90f-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="1d90f-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d90f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1d90f-109">L’exemple illustre le partage de cookie entre trois applications qui utilisent l’authentification de cookie :</span><span class="sxs-lookup"><span data-stu-id="1d90f-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="1d90f-110">Principales 2.0 Razor Pages d’application ASP.NET sans utiliser [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="1d90f-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="1d90f-111">Application MVC de base d’ASP.NET 2.0 avec l’identité de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d90f-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="1d90f-112">Application avec l’identité d’ASP.NET MVC est ASP.NET Framework 4.6.1</span><span class="sxs-lookup"><span data-stu-id="1d90f-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="1d90f-113">Dans les exemples qui suivent :</span><span class="sxs-lookup"><span data-stu-id="1d90f-113">In the examples that follow:</span></span>

* <span data-ttu-id="1d90f-114">Le nom du cookie d’authentification est défini sur une valeur de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="1d90f-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="1d90f-115">Le `AuthenticationType` a la valeur `Identity.Application` explicitement ou par défaut.</span><span class="sxs-lookup"><span data-stu-id="1d90f-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="1d90f-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) est utilisé en tant que le schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1d90f-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="1d90f-117">La constante est résolue en une valeur de `Cookies`.</span><span class="sxs-lookup"><span data-stu-id="1d90f-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="1d90f-118">Le middleware du cookie d’authentification utilise une implémentation de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="1d90f-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="1d90f-119">`DataProtectionProvider`Fournit des services de protection des données pour le chiffrement et déchiffrement de données de charge utile de cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1d90f-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="1d90f-120">Le `DataProtectionProvider` instance est isolée à partir du système de protection des données utilisé par d’autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="1d90f-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="1d90f-121">Commune [clé de protection des données](xref:security/data-protection/implementation/key-management) emplacement de stockage est utilisé.</span><span class="sxs-lookup"><span data-stu-id="1d90f-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="1d90f-122">L’exemple d’application utilise un dossier nommé *porte-clé* à la racine de la solution pour stocker les clés de protection des données.</span><span class="sxs-lookup"><span data-stu-id="1d90f-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="1d90f-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepte un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pour une utilisation avec les cookies d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1d90f-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="1d90f-124">L’exemple d’application fournit le chemin d’accès de la *porte-clé* dossier `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="1d90f-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="1d90f-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiert le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d90f-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="1d90f-126">Pour obtenir ce package pour ASP.NET 2.0 de base et des applications plus loin, référencer la [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="1d90f-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="1d90f-127">Lorsque vous ciblez .NET Framework, ajoutez une référence de package à `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="1d90f-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="1d90f-128">Partager les cookies d’authentification entre les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d90f-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="1d90f-129">Lorsque vous utilisez ASP.NET Core identité :</span><span class="sxs-lookup"><span data-stu-id="1d90f-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d90f-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d90f-131">Dans le `ConfigureServices` méthode, utilisez la [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) méthode d’extension pour configurer le service de protection des données pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="1d90f-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="1d90f-132">Consultez le *CookieAuthWithIdentity.Core* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1d90f-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d90f-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1d90f-134">Dans le `Configure` méthode, utilisez la [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) à configurer :</span><span class="sxs-lookup"><span data-stu-id="1d90f-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="1d90f-135">Le service de protection des données pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="1d90f-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="1d90f-136">Le `AuthenticationScheme` pour correspondre à ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="1d90f-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

<span data-ttu-id="1d90f-137">Lors de l’utilisation de cookies directement :</span><span class="sxs-lookup"><span data-stu-id="1d90f-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d90f-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="1d90f-139">Consultez le *CookieAuth.Core* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1d90f-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d90f-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="1d90f-141">Chiffrement des clés de protection des données au repos</span><span class="sxs-lookup"><span data-stu-id="1d90f-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="1d90f-142">Pour les déploiements de production, configurez le `DataProtectionProvider` pour chiffrer les clés au repos avec DPAPI ou un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="1d90f-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="1d90f-143">Consultez [clé de chiffrement à l’arrêt](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="1d90f-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d90f-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d90f-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="1d90f-146">Partage des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d90f-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="1d90f-147">Les applications ASP.NET 4.x qui utilisent l’intergiciel (middleware) d’authentification de cookie Katana peuvent être configurées pour générer des cookies d’authentification qui sont compatibles avec le middleware du cookie d’authentification ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d90f-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="1d90f-148">Cela permet la mise à niveau les applications individuelles d’un site volumineux fragmentaire tout en offrant une meilleure expérience d’authentification unique sur le site.</span><span class="sxs-lookup"><span data-stu-id="1d90f-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="1d90f-149">Lorsqu’une application utilise un intergiciel (middleware) d’authentification de cookie Katana, il appelle `UseCookieAuthentication` dans le fichier *Startup.Auth.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="1d90f-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="1d90f-150">Projets d’application web ASP.NET 4.x créées avec Visual Studio 2013 et versions ultérieures utilisent l’intergiciel (middleware) d’authentification de cookie Katana par défaut.</span><span class="sxs-lookup"><span data-stu-id="1d90f-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="1d90f-151">D’une application ASP.NET 4.x doit cibler le .NET Framework 4.5.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="1d90f-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="1d90f-152">Dans le cas contraire, les packages NuGet Impossible d’installer.</span><span class="sxs-lookup"><span data-stu-id="1d90f-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="1d90f-153">Pour partager des cookies d’authentification entre les applications ASP.NET 4.x et les applications ASP.NET Core, configurer l’application ASP.NET Core comme indiqué ci-dessus, puis configurez les applications ASP.NET 4.x en suivant les étapes ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1d90f-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="1d90f-154">Installez le package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) dans chaque application de 4.x ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1d90f-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="1d90f-155">Dans *Startup.Auth.cs*, localisez l’appel à `UseCookieAuthentication` et modifiez-la comme suit.</span><span class="sxs-lookup"><span data-stu-id="1d90f-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="1d90f-156">Modifiez le nom de cookie pour correspondre au nom utilisé par l’intergiciel (middleware) d’authentification de cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d90f-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="1d90f-157">Fournir une instance d’un `DataProtectionProvider` initialisée à l’emplacement de stockage de clés de protection données commun.</span><span class="sxs-lookup"><span data-stu-id="1d90f-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d90f-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="1d90f-159">Consultez le *CookieAuthWithIdentity.NETFramework* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1d90f-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="1d90f-160">Lorsque vous générez une identité d’utilisateur, le type d’authentification doit correspondre au type défini dans `AuthenticationType` avec `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="1d90f-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="1d90f-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d90f-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d90f-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d90f-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1d90f-163">Définir le `CookieManager` à interop `ChunkingCookieManager` le format de segmentation est compatible.</span><span class="sxs-lookup"><span data-stu-id="1d90f-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="1d90f-164">Utiliser une base de données commune de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="1d90f-164">Use a common user database</span></span>

<span data-ttu-id="1d90f-165">Vérifiez que le système d’identité pour chaque application pointe vers la même base de données utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1d90f-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="1d90f-166">Sinon, le système d’identité génère des erreurs lors de l’exécution lorsqu’il tente de faire correspondre les informations contenues dans le cookie d’authentification contre les informations contenues dans sa base de données.</span><span class="sxs-lookup"><span data-stu-id="1d90f-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
