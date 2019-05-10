---
title: Partager des cookies entre applications avec ASP.NET et ASP.NET Core
author: rick-anderson
description: Découvrez comment partager des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: security/cookie-sharing
ms.openlocfilehash: 94fafc91012b5a7e0888a6ebf37f517c129af2ac
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892796"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Partager des cookies entre applications avec ASP.NET et ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Luke Latham](https://github.com/guardrex)

Sites Web sont souvent constitués de collaboration des applications web individuelles. Pour fournir une expérience d’authentification unique (SSO), les applications web au sein d’un site doivent partager des cookies d’authentification. Pour prendre en charge ce scénario, la pile de protection des données permet le partage de Katana l’authentification des cookies et tickets d’authentification par cookie ASP.NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple illustre le partage de cookies entre trois applications qui utilisent l’authentification de cookie :

* ASP.NET Core 2.0 Razor Pages application sans utiliser [ASP.NET Core Identity](xref:security/authentication/identity)
* Application MVC ASP.NET Core 2.0 avec ASP.NET Core Identity
* Application MVC ASP.NET Framework 4.6.1 avec ASP.NET Identity

Dans les exemples qui suivent :

* Le nom du cookie d’authentification est défini sur une valeur commune de `.AspNet.SharedCookie`.
* Le `AuthenticationType` a la valeur `Identity.Application` explicitement ou par défaut.
* Un nom d’application commun est utilisé pour permettre au système de protection des données à partager les clés de protection de données (`SharedCookieApp`).
* `Identity.Application` est utilisé en tant que le schéma d’authentification. Tout schéma est utilisé, il doit être utilisé de manière cohérente *au sein et entre* les applications de cookie partagé en tant que le schéma par défaut ou en le définissant explicitement. Le schéma est utilisé pour le chiffrement et déchiffrement de cookies, donc un schéma cohérent doit être utilisé dans les applications.
* Un commun [clé de protection des données](xref:security/data-protection/implementation/key-management) emplacement de stockage est utilisé. L’exemple d’application utilise un dossier nommé *porte-clés* à la racine de la solution pour stocker les clés de protection des données.
* Dans les applications ASP.NET Core, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) est utilisé pour définir l’emplacement de stockage de clés. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) est utilisé pour configurer un nom d’application partagé commun.
* Dans l’application .NET Framework, le middleware cookie d’authentification utilise une implémentation de [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` Fournit des services de protection des données pour le chiffrement et le déchiffrement des données de charge utile de cookie d’authentification. Le `DataProtectionProvider` instance est isolée à partir du système de protection des données utilisé par les autres parties de l’application.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, Action\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepte un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pour spécifier l’emplacement de stockage de clés de protection de données. L’exemple d’application fournit le chemin d’accès de la *porte-clés* dossier `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) définit le nom d’application courants.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) nécessite le [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) package NuGet. Pour obtenir ce package pour ASP.NET Core 2.1 et des applications plus loin, vous devez référencer le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app). Lorsque vous ciblez le .NET Framework, ajoutez une référence de package à `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Partager des cookies d’authentification entre les applications ASP.NET Core

Lorsque vous utilisez ASP.NET Core Identity :

::: moniker range=">= aspnetcore-2.0"

Dans le `ConfigureServices` (méthode), utilisez le [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) méthode d’extension pour configurer le service de protection des données pour les cookies.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Clés de protection des données et le nom de l’application doivent être partagés entre les applications. Dans les exemples d’applications, `GetKeyRingDirInfo` retourne l’emplacement de stockage de clés communes à la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (méthode). Utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) pour configurer un nom d’application partagé commun (`SharedCookieApp` dans l’exemple). Pour plus d’informations, consultez [Protection des données de configuration](xref:security/data-protection/configuration/overview).

Lorsque vous hébergez des applications qui partagent des cookies entre les sous-domaines, spécifiez un domaine commun dans le [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propriété. Pour partager des cookies entre applications à `contoso.com`, tel que `first_subdomain.contoso.com` et `second_subdomain.contoso.com`, spécifiez la `Cookie.Domain` comme `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Consultez le *CookieAuthWithIdentity.Core* de projet dans le [exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dans le `Configure` (méthode), utilisez le [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) à configurer :

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

::: moniker-end

Lorsque vous utilisez directement les cookies :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Clés de protection des données et le nom de l’application doivent être partagés entre les applications. Dans les exemples d’applications, `GetKeyRingDirInfo` retourne l’emplacement de stockage de clés communes à la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (méthode). Utilisez [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) pour configurer un nom d’application partagé commun (`SharedCookieApp` dans l’exemple). Pour plus d’informations, consultez [Protection des données de configuration](xref:security/data-protection/configuration/overview).

Lorsque vous hébergez des applications qui partagent des cookies entre les sous-domaines, spécifiez un domaine commun dans le [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) propriété. Pour partager des cookies entre applications à `contoso.com`, tel que `first_subdomain.contoso.com` et `second_subdomain.contoso.com`, spécifiez la `Cookie.Domain` comme `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Consultez le *CookieAuth.Core* de projet dans le [exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:index#how-to-download-a-sample)).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

::: moniker-end

## <a name="encrypting-data-protection-keys-at-rest"></a>Chiffrement des clés de protection des données au repos

Pour les déploiements de production, configurez le `DataProtectionProvider` pour chiffrer les clés au repos avec DPAPI ou un X509Certificate. Consultez [clé de chiffrement au repos](xref:security/data-protection/implementation/key-encryption-at-rest) pour plus d’informations.

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Partage des cookies d’authentification entre ASP.NET 4.x et les applications ASP.NET Core

Les applications ASP.NET 4.x qui utilisent l’intergiciel (middleware) de Katana cookie d’authentification peuvent être configurées pour générer des cookies d’authentification qui sont compatibles avec l’intergiciel d’authentification de cookie ASP.NET Core. Cela permet la mise à niveau des applications individuelles d’un site grand fragmentaire tout en garantissant une expérience sans heurts de l’authentification unique sur le site.

Lorsqu’une application utilise l’intergiciel (middleware) de Katana cookie d’authentification, il appelle `UseCookieAuthentication` dans le projet *Startup.Auth.cs* fichier. Projets d’application web ASP.NET 4.x créées avec Visual Studio 2013 et l’utilisent ultérieurement l’intergiciel d’authentification de cookie Katana par défaut. Bien que `UseCookieAuthentication` est obsolète et non pris en charge pour les applications ASP.NET Core, appelant `UseCookieAuthentication` dans une application ASP.NET 4.x utilise Katana intergiciel d’authentification de cookie est valide.

Une application de 4.x ASP.NET doit cibler .NET Framework 4.5.1 ou version ultérieure. Sinon, les packages NuGet nécessaires échouent à installer.

Pour partager des cookies d’authentification entre d’une application ASP.NET 4.x et une application ASP.NET Core, configurez l’application ASP.NET Core comme indiqué ci-dessus, puis configurer l’application de 4.x ASP.NET en suivant ces étapes :

1. Installez le package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) à chaque application que ASP.NET 4.x.

2. Dans *Startup.Auth.cs*, recherchez l’appel à `UseCookieAuthentication` et modifiez-le comme suit. Modifiez le nom de cookie doit correspondre au nom utilisé par l’intergiciel d’authentification de cookie ASP.NET Core. Fournir une instance d’un `DataProtectionProvider` initialisé à l’emplacement de stockage de clés de protection données courantes. Assurez-vous que le nom de l’application est défini sur le nom d’application commun utilisé par toutes les applications qui partagent des cookies, `SharedCookieApp` dans l’exemple d’application.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Consultez le *CookieAuthWithIdentity.NETFramework* de projet dans le [exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([comment télécharger](xref:index#how-to-download-a-sample)).

Lors de la génération d’une identité d’utilisateur, le type d’authentification doit correspondre au type défini dans `AuthenticationType` avec `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Utiliser une base de données commune de l’utilisateur

Vérifiez que le système d’identité pour chaque application est pointé vers la même base de données utilisateur. Sinon, le système d’identité génère des échecs lors de l’exécution lorsqu’il tente de faire correspondre les informations contenues dans le cookie d’authentification sur les informations contenues dans sa base de données.

## <a name="additional-resources"></a>Ressources supplémentaires

<xref:host-and-deploy/web-farm>
