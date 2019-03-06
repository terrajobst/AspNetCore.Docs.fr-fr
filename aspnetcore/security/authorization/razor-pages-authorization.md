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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="cacc6-103">Conventions d’autorisation des Pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cacc6-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="cacc6-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cacc6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cacc6-105">Une façon de contrôler l’accès dans votre application Pages Razor consiste à utiliser les conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="cacc6-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="cacc6-106">Ces conventions permettent d’autoriser les utilisateurs et permettre aux utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages.</span><span class="sxs-lookup"><span data-stu-id="cacc6-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="cacc6-107">Appliquent les conventions décrites dans cette rubrique automatiquement [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="cacc6-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="cacc6-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cacc6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cacc6-109">L’exemple d’application utilise [authentification par cookie sans ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="cacc6-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="cacc6-110">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="cacc6-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="cacc6-111">Pour utiliser ASP.NET Core Identity, suivez les instructions de <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="cacc6-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="cacc6-112">Requièrent une autorisation pour accéder à une page</span><span class="sxs-lookup"><span data-stu-id="cacc6-112">Require authorization to access a page</span></span>

<span data-ttu-id="cacc6-113">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="cacc6-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="cacc6-114">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="cacc6-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="cacc6-115">Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizePage surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="cacc6-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="cacc6-116">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> peut être appliqué à une classe de modèle de page avec le `[Authorize]` attribut de filtre.</span><span class="sxs-lookup"><span data-stu-id="cacc6-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="cacc6-117">Pour plus d’informations, consultez [attribut de filtre Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="cacc6-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="cacc6-118">Requièrent une autorisation pour accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="cacc6-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="cacc6-119">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> pour toutes les pages dans un dossier dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="cacc6-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="cacc6-120">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="cacc6-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="cacc6-121">Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizeFolder surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="cacc6-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="cacc6-122">Requièrent une autorisation pour accéder à une page de zone</span><span class="sxs-lookup"><span data-stu-id="cacc6-122">Require authorization to access an area page</span></span>

<span data-ttu-id="cacc6-123">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page de zone dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="cacc6-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="cacc6-124">Le nom de la page est le chemin d’accès du fichier sans extension relatif au répertoire racine de pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cacc6-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="cacc6-125">Par exemple, le nom de page pour le fichier *Areas/Identity/Pages/Manage/Accounts.cshtml* est */gérer/comptes*.</span><span class="sxs-lookup"><span data-stu-id="cacc6-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="cacc6-126">Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizeAreaPage surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="cacc6-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="cacc6-127">Requièrent une autorisation pour accéder à un dossier de zones</span><span class="sxs-lookup"><span data-stu-id="cacc6-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="cacc6-128">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à tous les domaines dans un dossier dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="cacc6-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="cacc6-129">Le chemin du dossier est le chemin d’accès du dossier relatif au répertoire racine de pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="cacc6-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="cacc6-130">Par exemple, le chemin du dossier pour les fichiers sous *zones/Identity/Pages/gérer/* est */gérer*.</span><span class="sxs-lookup"><span data-stu-id="cacc6-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="cacc6-131">Pour spécifier un [stratégie d’autorisation](xref:security/authorization/policies), utilisez un [AuthorizeAreaFolder surcharge](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="cacc6-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="cacc6-132">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="cacc6-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="cacc6-133">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à une page dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="cacc6-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="cacc6-134">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="cacc6-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="cacc6-135">Autoriser l’accès anonyme dans un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="cacc6-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="cacc6-136">Utilisez le <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> pour toutes les pages dans un dossier dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="cacc6-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="cacc6-137">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="cacc6-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="cacc6-138">Remarque sur la combinaison autorisé et l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="cacc6-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="cacc6-139">Il est valide pour spécifier qu’un dossier de pages qui requièrent une autorisation et que de spécifier qu’une page dans ce dossier autorise l’accès anonyme :</span><span class="sxs-lookup"><span data-stu-id="cacc6-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="cacc6-140">L’inverse, toutefois, n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="cacc6-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="cacc6-141">Vous ne pouvez pas déclarer un dossier de pages pour l’accès anonyme, puis spécifiez une page dans ce dossier qui nécessite une autorisation :</span><span class="sxs-lookup"><span data-stu-id="cacc6-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="cacc6-142">Nécessité d’une autorisation sur la page privée échoue.</span><span class="sxs-lookup"><span data-stu-id="cacc6-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="cacc6-143">Lorsqu’à la fois le <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> et <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sont appliqués à la page, le <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> est prioritaire et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="cacc6-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cacc6-144">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cacc6-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
