---
title: Conventions d’autorisation des Pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment contrôler l’accès aux pages avec les conventions qui autorisent les utilisateurs et permettent aux utilisateurs anonymes d’accéder aux pages ou des dossiers de pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345499"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Conventions d’autorisation des Pages Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Une façon de contrôler l’accès dans votre application Pages Razor consiste à utiliser les conventions d’autorisation au démarrage. Ces conventions permettent d’autoriser les utilisateurs et permettre aux utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages. Appliquent les conventions décrites dans cette rubrique automatiquement [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

L’exemple d’application utilise [authentification par cookie sans ASP.NET Core Identity](xref:security/authentication/cookie). Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core Identity. Pour utiliser ASP.NET Core Identity, suivez les instructions de <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Requièrent une autorisation pour accéder à une page

Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.

Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizePage surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> peut être appliqué à une classe de modèle de page avec le `[Authorize]` attribut de filtre. Pour plus d’informations, consultez [attribut de filtre Authorize](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requièrent une autorisation pour accéder à un dossier de pages

Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> pour toutes les pages dans un dossier dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.

Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizeFolder surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Requièrent une autorisation pour accéder à une page de zone

Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page de zone dans le chemin spécifié :

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Le nom de la page est le chemin d’accès du fichier sans extension relatif au répertoire racine de pages pour la zone spécifiée. Par exemple, le nom de page pour le fichier *Areas/Identity/Pages/Manage/Accounts.cshtml* est */gérer/comptes*.

Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizeAreaPage surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Requièrent une autorisation pour accéder à un dossier de zones

Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à tous les domaines dans un dossier dans le chemin spécifié :

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Le chemin du dossier est le chemin d’accès du dossier relatif au répertoire racine de pages pour la zone spécifiée. Par exemple, le chemin du dossier pour les fichiers sous *zones/Identity/Pages/gérer/* est */gérer*.

Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizeAreaFolder surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Autoriser l’accès anonyme à une page

Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à une page dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Autoriser l’accès anonyme dans un dossier de pages

Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> pour toutes les pages dans un dossier dans le chemin spécifié :

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Remarque sur la combinaison autorisé et l’accès anonyme

Il est valide pour spécifier qu’un dossier de pages qui requièrent une autorisation et que de spécifier qu’une page dans ce dossier autorise l’accès anonyme :

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

L’inverse, toutefois, n’est pas valide. Vous ne pouvez pas déclarer un dossier de pages pour l’accès anonyme, puis spécifiez une page dans ce dossier qui nécessite une autorisation :

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Nécessité d’une autorisation sur la page privée échoue. Lorsqu’à la fois le <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> et <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sont appliqués à la page, le <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> est prioritaire et contrôle l’accès.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
