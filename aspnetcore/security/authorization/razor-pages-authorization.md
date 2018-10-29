---
title: Conventions d’autorisation des Pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment contrôler l’accès aux pages avec les conventions qui autorisent les utilisateurs et permettent aux utilisateurs anonymes d’accéder aux pages ou des dossiers de pages.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207262"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="b361a-103">Conventions d’autorisation des Pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b361a-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="b361a-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b361a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b361a-105">Une façon de contrôler l’accès dans votre application Pages Razor consiste à utiliser les conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="b361a-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="b361a-106">Ces conventions permettent d’autoriser les utilisateurs et permettre aux utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages.</span><span class="sxs-lookup"><span data-stu-id="b361a-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="b361a-107">Appliquent les conventions décrites dans cette rubrique automatiquement [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="b361a-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="b361a-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b361a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b361a-109">L’exemple d’application utilise [authentification par Cookie sans ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="b361a-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="b361a-110">Le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="b361a-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="b361a-111">Utilisez le nom d’utilisateur de messagerie «maria.rodriguez@contoso.com» et n’importe quel mot de passe pour vous connecter l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b361a-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="b361a-112">L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="b361a-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="b361a-113">Dans un exemple réel, l’utilisateur serait être authentifié par rapport à une base de données.</span><span class="sxs-lookup"><span data-stu-id="b361a-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="b361a-114">Pour utiliser ASP.NET Core Identity, suivez les instructions de la [présentation d’Identity dans ASP.NET Core](xref:security/authentication/identity) rubrique.</span><span class="sxs-lookup"><span data-stu-id="b361a-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="b361a-115">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="b361a-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="b361a-116">Requièrent une autorisation pour accéder à une page</span><span class="sxs-lookup"><span data-stu-id="b361a-116">Require authorization to access a page</span></span>

<span data-ttu-id="b361a-117">Utilisez le [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="b361a-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="b361a-118">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="b361a-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="b361a-119">Un [AuthorizePage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="b361a-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="b361a-120">Un `AuthorizeFilter` peut être appliqué à une classe de modèle de page avec le `[Authorize]` attribut de filtre.</span><span class="sxs-lookup"><span data-stu-id="b361a-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="b361a-121">Pour plus d’informations, consultez [attribut de filtre Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="b361a-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="b361a-122">Requièrent une autorisation pour accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="b361a-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="b361a-123">Utilisez le [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages dans un dossier dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="b361a-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="b361a-124">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="b361a-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="b361a-125">Un [AuthorizeFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="b361a-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="b361a-126">Requièrent une autorisation pour accéder à une page de zone</span><span class="sxs-lookup"><span data-stu-id="b361a-126">Require authorization to access an area page</span></span>

<span data-ttu-id="b361a-127">Utilisez le [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page de zone dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="b361a-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="b361a-128">Le nom de la page est le chemin d’accès du fichier sans extension relatif au répertoire racine de pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="b361a-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="b361a-129">Par exemple, le nom de page pour le fichier *Areas/Identity/Pages/Manage/Accounts.cshtml* est */gérer/comptes*.</span><span class="sxs-lookup"><span data-stu-id="b361a-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="b361a-130">Un [AuthorizeAreaPage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="b361a-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="b361a-131">Requièrent une autorisation pour accéder à un dossier de zones</span><span class="sxs-lookup"><span data-stu-id="b361a-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="b361a-132">Utilisez le [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à tous les domaines dans un dossier dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="b361a-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="b361a-133">Le chemin du dossier est le chemin d’accès du dossier relatif au répertoire racine de pages pour la zone spécifiée.</span><span class="sxs-lookup"><span data-stu-id="b361a-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="b361a-134">Par exemple, le chemin du dossier pour les fichiers sous *zones/Identity/Pages/gérer/* est */gérer*.</span><span class="sxs-lookup"><span data-stu-id="b361a-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="b361a-135">Un [AuthorizeAreaFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) est disponible si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="b361a-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="b361a-136">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="b361a-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="b361a-137">Utilisez le [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="b361a-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="b361a-138">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="b361a-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="b361a-139">Autoriser l’accès anonyme dans un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="b361a-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="b361a-140">Utilisez le [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages dans un dossier dans le chemin spécifié :</span><span class="sxs-lookup"><span data-stu-id="b361a-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="b361a-141">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de racine de Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="b361a-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="b361a-142">Remarque sur la combinaison autorisé et l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="b361a-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="b361a-143">Il est parfaitement possible de spécifier un dossier de pages de requièrent une autorisation et de spécifier qu’une page dans ce dossier autorise l’accès anonyme :</span><span class="sxs-lookup"><span data-stu-id="b361a-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="b361a-144">L’inverse, toutefois, n’est pas vrai.</span><span class="sxs-lookup"><span data-stu-id="b361a-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="b361a-145">Vous ne pouvez pas déclarer un dossier de pages pour l’accès anonyme et spécifier une page dans pour l’autorisation :</span><span class="sxs-lookup"><span data-stu-id="b361a-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="b361a-146">Nécessitant une autorisation sur la page privée ne fonctionnera pas, car lorsqu’à la fois le `AllowAnonymousFilter` et `AuthorizeFilter` filtres sont appliqués à la page, le `AllowAnonymousFilter` wins et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="b361a-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b361a-147">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b361a-147">Additional resources</span></span>

* [<span data-ttu-id="b361a-148">Itinéraire personnalisé des pages Razor et fournisseurs de modèle de page</span><span class="sxs-lookup"><span data-stu-id="b361a-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="b361a-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="b361a-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
