---
title: Partager les cookies entre les applications avec ASP.NET et ASP.NET Core
author: rick-anderson
description: Découvrez comment partager des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 5f77377f168993d48686217adac54a75313766ec
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="1a6f7-103">Partager les cookies entre les applications avec ASP.NET et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a6f7-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="1a6f7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1a6f7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1a6f7-105">Sites Web sont souvent composent d’applications web individuelles fonctionnent ensemble.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="1a6f7-106">Pour fournir une expérience d’authentification unique (SSO), les applications web au sein d’un site doivent partager les cookies d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="1a6f7-107">Pour prendre en charge ce scénario, la pile de protection des données permet de partager une authentification de cookie Katana et tickets d’authentification par cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="1a6f7-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1a6f7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1a6f7-109">L’exemple illustre le partage de cookie entre trois applications qui utilisent l’authentification de cookie :</span><span class="sxs-lookup"><span data-stu-id="1a6f7-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="1a6f7-110">Principales 2.0 Razor Pages d’application ASP.NET sans utiliser [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="1a6f7-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="1a6f7-111">Application MVC de base d’ASP.NET 2.0 avec l’identité de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a6f7-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="1a6f7-112">Application avec l’identité d’ASP.NET MVC est ASP.NET Framework 4.6.1</span><span class="sxs-lookup"><span data-stu-id="1a6f7-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="1a6f7-113">Dans les exemples qui suivent :</span><span class="sxs-lookup"><span data-stu-id="1a6f7-113">In the examples that follow:</span></span>

* <span data-ttu-id="1a6f7-114">Le nom du cookie d’authentification est défini sur une valeur de `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="1a6f7-115">Le `AuthenticationType` a la valeur `Identity.Application` explicitement ou par défaut.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="1a6f7-116">Un nom commun de l’application est utilisé pour activer le système de protection des données partager les clés de protection de données (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="1a6f7-117">`Identity.Application` est utilisé en tant que le schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="1a6f7-118">Tout schéma est utilisé, il doit être utilisé de manière cohérente *dans et entre les* les applications de cookie partagé en tant que le schéma par défaut ou en lui affectant explicitement.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="1a6f7-119">Le schéma est utilisé lors du chiffrement et déchiffrement de cookies, donc un schéma cohérent doit être utilisé dans les applications.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="1a6f7-120">Commune [clé de protection des données](xref:security/data-protection/implementation/key-management) emplacement de stockage est utilisé.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="1a6f7-121">L’exemple d’application utilise un dossier nommé *porte-clé* à la racine de la solution pour stocker les clés de protection des données.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="1a6f7-122">Dans les applications ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) est utilisé pour définir l’emplacement de stockage de clés.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="1a6f7-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) est utilisé pour configurer un nom d’application partagé commun.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="1a6f7-124">Dans l’application .NET Framework, le middleware du cookie d’authentification utilise une implémentation de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="1a6f7-125">`DataProtectionProvider` Fournit des services de protection des données pour le chiffrement et déchiffrement de données de charge utile de cookie d’authentification.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="1a6f7-126">Le `DataProtectionProvider` instance est isolée à partir du système de protection des données utilisé par d’autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="1a6f7-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Action\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepte un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pour spécifier l’emplacement de stockage de clés de protection de données.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="1a6f7-128">L’exemple d’application fournit le chemin d’accès de la *porte-clé* dossier `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="1a6f7-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) définit le nom commun de l’application.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="1a6f7-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiert le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="1a6f7-131">Pour obtenir ce package pour ASP.NET 2.0 de base et des applications plus loin, référencer la [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-131">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="1a6f7-132">Lorsque vous ciblez le .NET Framework, ajoutez une référence de package à `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="1a6f7-133">Partager les cookies d’authentification entre les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a6f7-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="1a6f7-134">Lorsque vous utilisez ASP.NET Core identité :</span><span class="sxs-lookup"><span data-stu-id="1a6f7-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1a6f7-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="1a6f7-136">Dans le `ConfigureServices` méthode, utilisez la [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) méthode d’extension pour configurer le service de protection des données pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="1a6f7-137">Les clés de protection de données et le nom de l’application doivent être partagés entre les applications.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="1a6f7-138">Dans les exemples d’applications, `GetKeyRingDirInfo` renvoie l’emplacement de stockage de clés communes à la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (méthode).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="1a6f7-139">Utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) pour configurer un nom d’application partagé commun (`SharedCookieApp` dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="1a6f7-140">Pour plus d’informations, consultez [Protection des données de configuration](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="1a6f7-141">Consultez le *CookieAuthWithIdentity.Core* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1a6f7-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1a6f7-143">Dans le `Configure` méthode, utilisez la [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) à configurer :</span><span class="sxs-lookup"><span data-stu-id="1a6f7-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="1a6f7-144">Le service de protection des données pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="1a6f7-145">Le `AuthenticationScheme` pour correspondre à ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="1a6f7-146">Lors de l’utilisation de cookies directement :</span><span class="sxs-lookup"><span data-stu-id="1a6f7-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1a6f7-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="1a6f7-148">Les clés de protection de données et le nom de l’application doivent être partagés entre les applications.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="1a6f7-149">Dans les exemples d’applications, `GetKeyRingDirInfo` renvoie l’emplacement de stockage de clés communes à la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (méthode).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="1a6f7-150">Utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) pour configurer un nom d’application partagé commun (`SharedCookieApp` dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="1a6f7-151">Pour plus d’informations, consultez [Protection des données de configuration](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="1a6f7-152">Consultez le *CookieAuth.Core* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1a6f7-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="1a6f7-154">Chiffrement des clés de protection des données au repos</span><span class="sxs-lookup"><span data-stu-id="1a6f7-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="1a6f7-155">Pour les déploiements de production, configurez le `DataProtectionProvider` pour chiffrer les clés au repos avec DPAPI ou un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="1a6f7-156">Consultez [clé de chiffrement à l’arrêt](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1a6f7-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1a6f7-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="1a6f7-159">Partage des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a6f7-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="1a6f7-160">Les applications ASP.NET 4.x qui utilisent l’intergiciel (middleware) d’authentification de cookie Katana peuvent être configurées pour générer des cookies d’authentification qui sont compatibles avec le middleware du cookie d’authentification ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="1a6f7-161">Cela permet la mise à niveau les applications individuelles d’un site volumineux fragmentaire tout en offrant une meilleure expérience d’authentification unique sur le site.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="1a6f7-162">Lorsqu’une application utilise un intergiciel (middleware) d’authentification de cookie Katana, il appelle `UseCookieAuthentication` dans le fichier *Startup.Auth.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="1a6f7-163">Projets d’application web ASP.NET 4.x créées avec Visual Studio 2013 et versions ultérieures utilisent l’intergiciel (middleware) d’authentification de cookie Katana par défaut.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="1a6f7-164">D’une application ASP.NET 4.x doit cibler le .NET Framework 4.5.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="1a6f7-165">Dans le cas contraire, les packages NuGet Impossible d’installer.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="1a6f7-166">Pour partager des cookies d’authentification entre les applications ASP.NET 4.x et les applications ASP.NET Core, configurer l’application ASP.NET Core comme indiqué ci-dessus, puis configurez les applications ASP.NET 4.x en suivant les étapes ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="1a6f7-167">Installez le package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) dans chaque application de 4.x ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="1a6f7-168">Dans *Startup.Auth.cs*, localisez l’appel à `UseCookieAuthentication` et modifiez-la comme suit.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="1a6f7-169">Modifiez le nom de cookie pour correspondre au nom utilisé par l’intergiciel (middleware) d’authentification de cookie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="1a6f7-170">Fournir une instance d’un `DataProtectionProvider` initialisée à l’emplacement de stockage de clés de protection données commun.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="1a6f7-171">Assurez-vous que le nom de l’application est défini pour le nom d’application courants utilisé par toutes les applications qui partagent des cookies, `SharedCookieApp` dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1a6f7-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="1a6f7-173">Consultez le *CookieAuthWithIdentity.NETFramework* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1a6f7-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="1a6f7-174">Lorsque vous générez une identité d’utilisateur, le type d’authentification doit correspondre au type défini dans `AuthenticationType` avec `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="1a6f7-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a6f7-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1a6f7-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1a6f7-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="1a6f7-177">Définir le `CookieManager` à interop `ChunkingCookieManager` le format de segmentation est compatible.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="1a6f7-178">Utiliser une base de données commune de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="1a6f7-178">Use a common user database</span></span>

<span data-ttu-id="1a6f7-179">Vérifiez que le système d’identité pour chaque application pointe vers la même base de données utilisateur.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="1a6f7-180">Sinon, le système d’identité génère des erreurs lors de l’exécution lorsqu’il tente de faire correspondre les informations contenues dans le cookie d’authentification contre les informations contenues dans sa base de données.</span><span class="sxs-lookup"><span data-stu-id="1a6f7-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
