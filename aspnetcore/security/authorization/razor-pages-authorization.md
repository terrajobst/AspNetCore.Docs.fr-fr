---
title: Conventions d’autorisation des Pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment contrôler l’accès aux pages avec les conventions qui autorisent les utilisateurs et permettent aux utilisateurs anonymes d’accéder aux pages ou des dossiers de pages.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829558"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Conventions d’autorisation des Pages Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Une façon de contrôler l’accès dans votre application Pages Razor consiste à utiliser les conventions d’autorisation au démarrage. Ces conventions permettent d’autoriser les utilisateurs et permettre aux utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages. Appliquent les conventions décrites dans cette rubrique automatiquement [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’exemple d’application utilise [authentification par Cookie sans ASP.NET Core Identity](xref:security/authentication/cookie). Le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application. Utilisez le nom d’utilisateur de messagerie «maria.rodriguez@contoso.com» et n’importe quel mot de passe pour vous connecter l’utilisateur. L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier. Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données. Pour utiliser ASP.NET Core Identity, suivez les instructions de la [présentation d’Identity dans ASP.NET Core](xref:security/authentication/identity) rubrique. Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core Identity.

## <a name="require-authorization-to-access-a-page"></a>Requièrent une autorisation pour accéder à une page

Utilisez le [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.

Un [AuthorizePage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Un `AuthorizeFilter` peut être appliqué à une classe de modèle de page avec le `[Authorize]` attribut de filtre. Pour plus d’informations, consultez [attribut de filtre Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requièrent une autorisation pour accéder à un dossier de pages

Utilisez le [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages dans un dossier dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.

Un [AuthorizeFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Requièrent une autorisation pour accéder à une page de zone

Utilisez le [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page de zone dans le chemin spécifié :

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Le nom de la page est le chemin d’accès du fichier sans extension relatif au répertoire racine de pages pour la zone spécifiée. Par exemple, le nom de page pour le fichier *Areas/Identity/Pages/Manage/Accounts.cshtml* est */gérer/comptes*.

Un [AuthorizeAreaPage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Requièrent une autorisation pour accéder à un dossier de zones

Utilisez le [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à tous les domaines dans un dossier dans le chemin spécifié :

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Le chemin du dossier est le chemin d’accès du dossier relatif au répertoire racine de pages pour la zone spécifiée. Par exemple, le chemin du dossier pour les fichiers sous *zones/Identity/Pages/gérer/* est */gérer*.

Un [AuthorizeAreaFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Autoriser l’accès anonyme à une page

Utilisez le [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Autoriser l’accès anonyme dans un dossier de pages

Utilisez le [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages dans un dossier dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Remarque sur la combinaison autorisé et l’accès anonyme

Il est parfaitement possible de spécifier un dossier de pages de requièrent une autorisation et de spécifier qu’une page dans ce dossier autorise l’accès anonyme :

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

L’inverse, toutefois, n’est pas vrai. Vous ne pouvez pas déclarer un dossier de pages pour l’accès anonyme et spécifier une page dans pour l’autorisation :

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Nécessitant une autorisation sur la page privée ne fonctionnera pas, car lorsqu’à la fois le `AllowAnonymousFilter` et `AuthorizeFilter` filtres sont appliqués à la page, le `AllowAnonymousFilter` wins et contrôle l’accès.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Itinéraire personnalisé des pages Razor et fournisseurs de modèle de page](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
