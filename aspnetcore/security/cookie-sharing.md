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
ms.openlocfilehash: f6d62d5f6e446e3e2001ed6bde72a6c409aa2833
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734677"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Partager les cookies entre les applications avec ASP.NET et ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)

Sites Web sont souvent composent d’applications web individuelles fonctionnent ensemble. Pour fournir une expérience d’authentification unique (SSO), les applications web au sein d’un site doivent partager les cookies d’authentification. Pour prendre en charge ce scénario, la pile de protection des données permet de partager une authentification de cookie Katana et tickets d’authentification par cookie ASP.NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple illustre le partage de cookie entre trois applications qui utilisent l’authentification de cookie :

* Principales 2.0 Razor Pages d’application ASP.NET sans utiliser [ASP.NET Core Identity](xref:security/authentication/identity)
* Application MVC de base d’ASP.NET 2.0 avec l’identité de ASP.NET Core
* Application avec l’identité d’ASP.NET MVC est ASP.NET Framework 4.6.1

Dans les exemples qui suivent :

* Le nom du cookie d’authentification est défini sur une valeur de `.AspNet.SharedCookie`.
* Le `AuthenticationType` a la valeur `Identity.Application` explicitement ou par défaut.
* Un nom commun de l’application est utilisé pour activer le système de protection des données partager les clés de protection de données (`SharedCookieApp`).
* `Identity.Application` est utilisé en tant que le schéma d’authentification. Tout schéma est utilisé, il doit être utilisé de manière cohérente *dans et entre les* les applications de cookie partagé en tant que le schéma par défaut ou en lui affectant explicitement. Le schéma est utilisé lors du chiffrement et déchiffrement de cookies, donc un schéma cohérent doit être utilisé dans les applications.
* Commune [clé de protection des données](xref:security/data-protection/implementation/key-management) emplacement de stockage est utilisé. L’exemple d’application utilise un dossier nommé *porte-clé* à la racine de la solution pour stocker les clés de protection des données.
* Dans les applications ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) est utilisé pour définir l’emplacement de stockage de clés. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) est utilisé pour configurer un nom d’application partagé commun.
* Dans l’application .NET Framework, le middleware du cookie d’authentification utilise une implémentation de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` Fournit des services de protection des données pour le chiffrement et déchiffrement de données de charge utile de cookie d’authentification. Le `DataProtectionProvider` instance est isolée à partir du système de protection des données utilisé par d’autres parties de l’application.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, Action\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepte un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pour spécifier l’emplacement de stockage de clés de protection de données. L’exemple d’application fournit le chemin d’accès de la *porte-clé* dossier `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) définit le nom commun de l’application.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requiert le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package NuGet. Pour obtenir ce package pour ASP.NET Core 2.1 et des applications plus loin, référencer la [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Lorsque vous ciblez le .NET Framework, ajoutez une référence de package à `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Partager les cookies d’authentification entre les applications ASP.NET Core

Lorsque vous utilisez ASP.NET Core identité :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Dans le `ConfigureServices` méthode, utilisez la [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) méthode d’extension pour configurer le service de protection des données pour les cookies.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Les clés de protection de données et le nom de l’application doivent être partagés entre les applications. Dans les exemples d’applications, `GetKeyRingDirInfo` renvoie l’emplacement de stockage de clés communes à la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (méthode). Utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) pour configurer un nom d’application partagé commun (`SharedCookieApp` dans l’exemple). Pour plus d’informations, consultez [Protection des données de configuration](xref:security/data-protection/configuration/overview).

Consultez le *CookieAuthWithIdentity.Core* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Dans le `Configure` méthode, utilisez la [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) à configurer :

* Le service de protection des données pour les cookies.
* Le `AuthenticationScheme` pour correspondre à ASP.NET 4.x.

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

Lors de l’utilisation de cookies directement :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Les clés de protection de données et le nom de l’application doivent être partagés entre les applications. Dans les exemples d’applications, `GetKeyRingDirInfo` renvoie l’emplacement de stockage de clés communes à la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (méthode). Utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) pour configurer un nom d’application partagé commun (`SharedCookieApp` dans l’exemple). Pour plus d’informations, consultez [Protection des données de configuration](xref:security/data-protection/configuration/overview). 

Consultez le *CookieAuth.Core* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Chiffrement des clés de protection des données au repos

Pour les déploiements de production, configurez le `DataProtectionProvider` pour chiffrer les clés au repos avec DPAPI ou un X509Certificate. Consultez [clé de chiffrement à l’arrêt](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’informations.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Partage des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core

Les applications ASP.NET 4.x qui utilisent l’intergiciel (middleware) d’authentification de cookie Katana peuvent être configurées pour générer des cookies d’authentification qui sont compatibles avec le middleware du cookie d’authentification ASP.NET Core. Cela permet la mise à niveau les applications individuelles d’un site volumineux fragmentaire tout en offrant une meilleure expérience d’authentification unique sur le site.

> [!TIP]
> Lorsqu’une application utilise un intergiciel (middleware) d’authentification de cookie Katana, il appelle `UseCookieAuthentication` dans le fichier *Startup.Auth.cs* fichier. Projets d’application web ASP.NET 4.x créées avec Visual Studio 2013 et versions ultérieures utilisent l’intergiciel (middleware) d’authentification de cookie Katana par défaut.

> [!NOTE]
> D’une application ASP.NET 4.x doit cibler le .NET Framework 4.5.1 ou version ultérieure. Dans le cas contraire, les packages NuGet Impossible d’installer.

Pour partager des cookies d’authentification entre les applications ASP.NET 4.x et les applications ASP.NET Core, configurer l’application ASP.NET Core comme indiqué ci-dessus, puis configurez les applications ASP.NET 4.x en suivant les étapes ci-dessous.

1. Installez le package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) dans chaque application de 4.x ASP.NET.

2. Dans *Startup.Auth.cs*, localisez l’appel à `UseCookieAuthentication` et modifiez-la comme suit. Modifiez le nom de cookie pour correspondre au nom utilisé par l’intergiciel (middleware) d’authentification de cookie ASP.NET Core. Fournir une instance d’un `DataProtectionProvider` initialisée à l’emplacement de stockage de clés de protection données commun. Assurez-vous que le nom de l’application est défini pour le nom d’application courants utilisé par toutes les applications qui partagent des cookies, `SharedCookieApp` dans l’exemple d’application.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Consultez le *CookieAuthWithIdentity.NETFramework* de projet dans le [exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).

Lorsque vous générez une identité d’utilisateur, le type d’authentification doit correspondre au type défini dans `AuthenticationType` avec `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Définir le `CookieManager` à interop `ChunkingCookieManager` le format de segmentation est compatible.

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

## <a name="use-a-common-user-database"></a>Utiliser une base de données commune de l’utilisateur

Vérifiez que le système d’identité pour chaque application pointe vers la même base de données utilisateur. Sinon, le système d’identité génère des erreurs lors de l’exécution lorsqu’il tente de faire correspondre les informations contenues dans le cookie d’authentification contre les informations contenues dans sa base de données.
