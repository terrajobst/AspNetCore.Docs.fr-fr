---
title: Razor Pages conventions d’autorisation dans ASP.NET Core
author: guardrex
description: Découvrez comment contrôler l’accès aux pages à l’aide de conventions qui autorisent les utilisateurs et autorisent les utilisateurs anonymes à accéder aux pages ou aux dossiers de pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994034"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="94c7a-103">Razor Pages conventions d’autorisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94c7a-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="94c7a-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="94c7a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="94c7a-105">L’une des façons de contrôler l’accès dans votre application Razor Pages consiste à utiliser des conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="94c7a-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="94c7a-106">Ces conventions vous permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes à accéder à des pages ou des dossiers de pages individuels.</span><span class="sxs-lookup"><span data-stu-id="94c7a-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="94c7a-107">Les conventions décrites dans cette rubrique appliquent automatiquement des [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="94c7a-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="94c7a-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="94c7a-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="94c7a-109">L’exemple d’application utilise [l’authentification par cookie sans ASP.net Core identité](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="94c7a-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="94c7a-110">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core identité.</span><span class="sxs-lookup"><span data-stu-id="94c7a-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="94c7a-111">Pour utiliser ASP.NET Core identité, suivez les instructions de <xref:security/authentication/identity>la.</span><span class="sxs-lookup"><span data-stu-id="94c7a-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="94c7a-112">Demander l’autorisation d’accéder à une page</span><span class="sxs-lookup"><span data-stu-id="94c7a-112">Require authorization to access a page</span></span>

<span data-ttu-id="94c7a-113">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="94c7a-114">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="94c7a-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="94c7a-115">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="94c7a-116">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> peut être appliqué à une classe de modèle de page `[Authorize]` avec l’attribut de filtre.</span><span class="sxs-lookup"><span data-stu-id="94c7a-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="94c7a-117">Pour plus d’informations, consultez [Authorize Filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="94c7a-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="94c7a-118">Demander l’autorisation d’accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="94c7a-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="94c7a-119">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les pages d’un dossier à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="94c7a-120">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="94c7a-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="94c7a-121">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="94c7a-122">Exiger une autorisation pour accéder à une page de zone</span><span class="sxs-lookup"><span data-stu-id="94c7a-122">Require authorization to access an area page</span></span>

<span data-ttu-id="94c7a-123">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page de zone à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="94c7a-124">Le nom de la page est le chemin d’accès du fichier sans extension par rapport au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="94c7a-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="94c7a-125">Par exemple, le nom de la page zones de fichier */identité/pages/manage/Accounts. cshtml* est */Manage/Accounts*.</span><span class="sxs-lookup"><span data-stu-id="94c7a-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="94c7a-126">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="94c7a-127">Demander l’autorisation d’accéder à un dossier de zones</span><span class="sxs-lookup"><span data-stu-id="94c7a-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="94c7a-128">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les zones d’un dossier au chemin d’accès spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="94c7a-129">Le chemin d’accès au dossier est le chemin d’accès du dossier relatif au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="94c7a-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="94c7a-130">Par exemple, le chemin d’accès au dossier pour les fichiers sous *zones/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="94c7a-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="94c7a-131">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="94c7a-132">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="94c7a-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="94c7a-133">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à une page à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="94c7a-134">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="94c7a-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="94c7a-135">Autoriser l’accès anonyme à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="94c7a-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="94c7a-136">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à toutes les pages d’un dossier à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="94c7a-137">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="94c7a-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="94c7a-138">Remarque sur la combinaison des accès autorisés et anonymes</span><span class="sxs-lookup"><span data-stu-id="94c7a-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="94c7a-139">Il est possible de spécifier qu’un dossier de pages nécessitant une autorisation et de spécifier qu’une page de ce dossier autorise un accès anonyme:</span><span class="sxs-lookup"><span data-stu-id="94c7a-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="94c7a-140">L’inverse, toutefois, n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="94c7a-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="94c7a-141">Vous ne pouvez pas déclarer un dossier de pages pour un accès anonyme, puis spécifier une page dans ce dossier qui nécessite une autorisation:</span><span class="sxs-lookup"><span data-stu-id="94c7a-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="94c7a-142">La demande d’autorisation sur la page privée échoue.</span><span class="sxs-lookup"><span data-stu-id="94c7a-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="94c7a-143"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Lorsque et <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sont appliqués à la page, le <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> est prioritaire et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="94c7a-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94c7a-144">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="94c7a-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="94c7a-145">L’une des façons de contrôler l’accès dans votre application Razor Pages consiste à utiliser des conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="94c7a-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="94c7a-146">Ces conventions vous permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes à accéder à des pages ou des dossiers de pages individuels.</span><span class="sxs-lookup"><span data-stu-id="94c7a-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="94c7a-147">Les conventions décrites dans cette rubrique appliquent automatiquement des [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="94c7a-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="94c7a-148">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="94c7a-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="94c7a-149">L’exemple d’application utilise [l’authentification par cookie sans ASP.net Core identité](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="94c7a-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="94c7a-150">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core identité.</span><span class="sxs-lookup"><span data-stu-id="94c7a-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="94c7a-151">Pour utiliser ASP.NET Core identité, suivez les instructions de <xref:security/authentication/identity>la.</span><span class="sxs-lookup"><span data-stu-id="94c7a-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="94c7a-152">Demander l’autorisation d’accéder à une page</span><span class="sxs-lookup"><span data-stu-id="94c7a-152">Require authorization to access a page</span></span>

<span data-ttu-id="94c7a-153">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="94c7a-154">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="94c7a-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="94c7a-155">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="94c7a-156">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> peut être appliqué à une classe de modèle de page `[Authorize]` avec l’attribut de filtre.</span><span class="sxs-lookup"><span data-stu-id="94c7a-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="94c7a-157">Pour plus d’informations, consultez [Authorize Filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="94c7a-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="94c7a-158">Demander l’autorisation d’accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="94c7a-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="94c7a-159">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les pages d’un dossier à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="94c7a-160">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="94c7a-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="94c7a-161">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="94c7a-162">Exiger une autorisation pour accéder à une page de zone</span><span class="sxs-lookup"><span data-stu-id="94c7a-162">Require authorization to access an area page</span></span>

<span data-ttu-id="94c7a-163">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page de zone à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="94c7a-164">Le nom de la page est le chemin d’accès du fichier sans extension par rapport au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="94c7a-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="94c7a-165">Par exemple, le nom de la page zones de fichier */identité/pages/manage/Accounts. cshtml* est */Manage/Accounts*.</span><span class="sxs-lookup"><span data-stu-id="94c7a-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="94c7a-166">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="94c7a-167">Demander l’autorisation d’accéder à un dossier de zones</span><span class="sxs-lookup"><span data-stu-id="94c7a-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="94c7a-168">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les zones d’un dossier au chemin d’accès spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="94c7a-169">Le chemin d’accès au dossier est le chemin d’accès du dossier relatif au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="94c7a-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="94c7a-170">Par exemple, le chemin d’accès au dossier pour les fichiers sous *zones/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="94c7a-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="94c7a-171">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="94c7a-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="94c7a-172">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="94c7a-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="94c7a-173">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à une page à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="94c7a-174">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="94c7a-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="94c7a-175">Autoriser l’accès anonyme à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="94c7a-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="94c7a-176">Utilisez la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à toutes les pages d’un dossier à l’emplacement spécifié:</span><span class="sxs-lookup"><span data-stu-id="94c7a-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="94c7a-177">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="94c7a-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="94c7a-178">Remarque sur la combinaison des accès autorisés et anonymes</span><span class="sxs-lookup"><span data-stu-id="94c7a-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="94c7a-179">Il est possible de spécifier qu’un dossier de pages nécessitant une autorisation et de spécifier qu’une page de ce dossier autorise un accès anonyme:</span><span class="sxs-lookup"><span data-stu-id="94c7a-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="94c7a-180">L’inverse, toutefois, n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="94c7a-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="94c7a-181">Vous ne pouvez pas déclarer un dossier de pages pour un accès anonyme, puis spécifier une page dans ce dossier qui nécessite une autorisation:</span><span class="sxs-lookup"><span data-stu-id="94c7a-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="94c7a-182">La demande d’autorisation sur la page privée échoue.</span><span class="sxs-lookup"><span data-stu-id="94c7a-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="94c7a-183"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Lorsque et <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sont appliqués à la page, le <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> est prioritaire et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="94c7a-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="94c7a-184">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="94c7a-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
