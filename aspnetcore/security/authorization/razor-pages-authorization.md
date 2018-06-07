---
title: Conventions d’autorisation de Pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment contrôler l’accès aux pages avec conventions d’autorisent des utilisateurs, et permettent aux utilisateurs anonymes d’accéder aux pages ou aux dossiers de pages.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 35a21156c001d8703e09e604129c4c2c500fe25f
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734651"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Conventions d’autorisation de Pages Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Une façon de contrôler l’accès de votre application de Pages Razor consiste à utiliser des conventions d’autorisation au démarrage. Ces conventions permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages. Les conventions décrites dans cette rubrique automatiquement s’appliquent [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

L’application exemple utilise [l’authentification de Cookie sans ASP.NET Core Identity](xref:security/authentication/cookie). Le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application. Utilisez le nom d’utilisateur de messagerie «maria.rodriguez@contoso.com» et n’importe quel mot de passe de l’utilisateur. L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier. L’utilisateur est un exemple concret, être authentifié par rapport à une base de données. Pour utiliser ASP.NET Core Identity, suivez les instructions de la [Introduction à l’identité sur ASP.NET Core](xref:security/authentication/identity) rubrique. Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent l’identité de ASP.NET Core.

## <a name="require-authorization-to-access-a-page"></a>Nécessite une autorisation pour accéder à une page

Utilisez le [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page à l’emplacement spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de la racine Pages Razor sans une extension et contenant des barres obliques uniquement.

Un [AuthorizePage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) n’est disponible que si vous devez spécifier une stratégie d’autorisation.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Un `AuthorizeFilter` peut être appliqué à une classe de modèle de page avec le `[Authorize]` attribut de filtre. Pour plus d’informations, consultez [attribut de filtre Authorize](xref:mvc/razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Nécessite une autorisation pour accéder à un dossier de pages

Utilisez le [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages dans un dossier dans le chemin d’accès spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif racine Pages Razor.

Un [AuthorizeFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) n’est disponible que si vous devez spécifier une stratégie d’autorisation.

## <a name="allow-anonymous-access-to-a-page"></a>Autoriser l’accès anonyme à une page

Utilisez le [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page à l’emplacement spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de la racine Pages Razor sans une extension et contenant des barres obliques uniquement.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Autoriser l’accès anonyme dans un dossier de pages

Utilisez le [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages dans un dossier dans le chemin d’accès spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif racine Pages Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Remarque sur la combinaison autorisé et l’accès anonyme

Il est parfaitement possible de spécifier un dossier de pages de requièrent une autorisation et de spécifier qu’une page de ce dossier autorise l’accès anonyme :

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

L’inverse, toutefois, n’est pas vrai. Vous ne peut pas déclarer un dossier de pages pour l’accès anonyme et spécifier une page au sein d’autorisation :

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Nécessitant une autorisation sur la page privée ne fonctionne pas, car lorsqu’à la fois le `AllowAnonymousFilter` et `AuthorizeFilter` filtres sont appliqués à la page, le `AllowAnonymousFilter` wins et contrôle l’accès.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Itinéraire personnalisé des pages Razor et fournisseurs de modèle de page](xref:mvc/razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
