---
title: Razor Pages conventions d’autorisation dans ASP.NET Core
author: rick-anderson
description: Découvrez comment contrôler l’accès aux pages à l’aide de conventions qui autorisent les utilisateurs et autorisent les utilisateurs anonymes à accéder aux pages ou aux dossiers de pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 00fc487c6ac802f213bcf83994ecc2b1a1468589
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662056"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="fe6a9-103">Razor Pages conventions d’autorisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe6a9-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fe6a9-104">L’une des façons de contrôler l’accès dans votre application Razor Pages consiste à utiliser des conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="fe6a9-105">Ces conventions vous permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes à accéder à des pages ou des dossiers de pages individuels.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="fe6a9-106">Les conventions décrites dans cette rubrique appliquent automatiquement des [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="fe6a9-107">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe6a9-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fe6a9-108">L’exemple d’application utilise [l’authentification par cookie sans ASP.net Core identité](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="fe6a9-109">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core identité.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="fe6a9-110">Pour utiliser ASP.NET Core identité, suivez les instructions de <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="fe6a9-111">Demander l’autorisation d’accéder à une page</span><span class="sxs-lookup"><span data-stu-id="fe6a9-111">Require authorization to access a page</span></span>

<span data-ttu-id="fe6a9-112">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="fe6a9-113">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="fe6a9-114">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="fe6a9-115">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> peut être appliqué à une classe de modèle de page avec l’attribut de filtre `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="fe6a9-116">Pour plus d’informations, consultez [Authorize Filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="fe6a9-117">Demander l’autorisation d’accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="fe6a9-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="fe6a9-118">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les pages d’un dossier à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="fe6a9-119">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="fe6a9-120">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="fe6a9-121">Exiger une autorisation pour accéder à une page de zone</span><span class="sxs-lookup"><span data-stu-id="fe6a9-121">Require authorization to access an area page</span></span>

<span data-ttu-id="fe6a9-122">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page de zone à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="fe6a9-123">Le nom de la page est le chemin d’accès du fichier sans extension par rapport au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="fe6a9-124">Par exemple, le nom de la page zones de fichier */identité/pages/manage/Accounts. cshtml* est */Manage/Accounts*.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="fe6a9-125">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="fe6a9-126">Demander l’autorisation d’accéder à un dossier de zones</span><span class="sxs-lookup"><span data-stu-id="fe6a9-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="fe6a9-127">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les zones d’un dossier au chemin d’accès spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="fe6a9-128">Le chemin d’accès au dossier est le chemin d’accès du dossier relatif au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="fe6a9-129">Par exemple, le chemin d’accès au dossier pour les fichiers sous *zones/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="fe6a9-130">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="fe6a9-131">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="fe6a9-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="fe6a9-132">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à une page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="fe6a9-133">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="fe6a9-134">Autoriser l’accès anonyme à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="fe6a9-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="fe6a9-135">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à toutes les pages d’un dossier à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="fe6a9-136">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="fe6a9-137">Remarque sur la combinaison des accès autorisés et anonymes</span><span class="sxs-lookup"><span data-stu-id="fe6a9-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="fe6a9-138">Il est possible de spécifier qu’un dossier de pages requiert une autorisation, puis de spécifier qu’une page de ce dossier autorise un accès anonyme :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="fe6a9-139">L’inverse, toutefois, n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="fe6a9-140">Vous ne pouvez pas déclarer un dossier de pages pour un accès anonyme, puis spécifier une page dans ce dossier qui nécessite une autorisation :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="fe6a9-141">La demande d’autorisation sur la page privée échoue.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="fe6a9-142">Lorsque les <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> et <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sont appliqués à la page, l' <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> est prioritaire et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe6a9-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fe6a9-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fe6a9-144">L’une des façons de contrôler l’accès dans votre application Razor Pages consiste à utiliser des conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="fe6a9-145">Ces conventions vous permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes à accéder à des pages ou des dossiers de pages individuels.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="fe6a9-146">Les conventions décrites dans cette rubrique appliquent automatiquement des [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="fe6a9-147">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe6a9-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fe6a9-148">L’exemple d’application utilise [l’authentification par cookie sans ASP.net Core identité](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="fe6a9-149">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core identité.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="fe6a9-150">Pour utiliser ASP.NET Core identité, suivez les instructions de <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="fe6a9-151">Demander l’autorisation d’accéder à une page</span><span class="sxs-lookup"><span data-stu-id="fe6a9-151">Require authorization to access a page</span></span>

<span data-ttu-id="fe6a9-152">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="fe6a9-153">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="fe6a9-154">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="fe6a9-155">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> peut être appliqué à une classe de modèle de page avec l’attribut de filtre `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="fe6a9-156">Pour plus d’informations, consultez [Authorize Filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="fe6a9-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="fe6a9-157">Demander l’autorisation d’accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="fe6a9-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="fe6a9-158">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les pages d’un dossier à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="fe6a9-159">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="fe6a9-160">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="fe6a9-161">Exiger une autorisation pour accéder à une page de zone</span><span class="sxs-lookup"><span data-stu-id="fe6a9-161">Require authorization to access an area page</span></span>

<span data-ttu-id="fe6a9-162">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à la page de zone à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="fe6a9-163">Le nom de la page est le chemin d’accès du fichier sans extension par rapport au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="fe6a9-164">Par exemple, le nom de la page zones de fichier */identité/pages/manage/Accounts. cshtml* est */Manage/Accounts*.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="fe6a9-165">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="fe6a9-166">Demander l’autorisation d’accéder à un dossier de zones</span><span class="sxs-lookup"><span data-stu-id="fe6a9-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="fe6a9-167">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> à toutes les zones d’un dossier au chemin d’accès spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="fe6a9-168">Le chemin d’accès au dossier est le chemin d’accès du dossier relatif au répertoire racine des pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="fe6a9-169">Par exemple, le chemin d’accès au dossier pour les fichiers sous *zones/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="fe6a9-170">Pour spécifier une [stratégie d’autorisation](xref:security/authorization/policies), utilisez une [surcharge AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="fe6a9-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="fe6a9-171">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="fe6a9-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="fe6a9-172">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à une page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="fe6a9-173">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages sans extension et qui ne contient que des barres obliques.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="fe6a9-174">Autoriser l’accès anonyme à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="fe6a9-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="fe6a9-175">Utilisez la Convention de <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour ajouter un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> à toutes les pages d’un dossier à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="fe6a9-176">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif à la racine Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="fe6a9-177">Remarque sur la combinaison des accès autorisés et anonymes</span><span class="sxs-lookup"><span data-stu-id="fe6a9-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="fe6a9-178">Il est possible de spécifier qu’un dossier de pages nécessitant une autorisation et de spécifier qu’une page de ce dossier autorise un accès anonyme :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="fe6a9-179">L’inverse, toutefois, n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="fe6a9-180">Vous ne pouvez pas déclarer un dossier de pages pour un accès anonyme, puis spécifier une page dans ce dossier qui nécessite une autorisation :</span><span class="sxs-lookup"><span data-stu-id="fe6a9-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="fe6a9-181">La demande d’autorisation sur la page privée échoue.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="fe6a9-182">Lorsque les <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> et <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sont appliqués à la page, l' <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> est prioritaire et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="fe6a9-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe6a9-183">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fe6a9-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
