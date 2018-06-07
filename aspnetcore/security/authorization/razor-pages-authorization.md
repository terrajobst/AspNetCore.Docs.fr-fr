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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="0a80e-103">Conventions d’autorisation de Pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a80e-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="0a80e-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a80e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0a80e-105">Une façon de contrôler l’accès de votre application de Pages Razor consiste à utiliser des conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="0a80e-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="0a80e-106">Ces conventions permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages.</span><span class="sxs-lookup"><span data-stu-id="0a80e-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="0a80e-107">Les conventions décrites dans cette rubrique automatiquement s’appliquent [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="0a80e-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="0a80e-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a80e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0a80e-109">L’application exemple utilise [l’authentification de Cookie sans ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="0a80e-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="0a80e-110">Le compte d’utilisateur pour l’utilisateur hypothétique, Maria Rodriguez, est codé en dur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="0a80e-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="0a80e-111">Utilisez le nom d’utilisateur de messagerie «maria.rodriguez@contoso.com» et n’importe quel mot de passe de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0a80e-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="0a80e-112">L’utilisateur est authentifié dans le `AuthenticateUser` méthode dans le *Pages/Account/Login.cshtml.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="0a80e-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="0a80e-113">L’utilisateur est un exemple concret, être authentifié par rapport à une base de données.</span><span class="sxs-lookup"><span data-stu-id="0a80e-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="0a80e-114">Pour utiliser ASP.NET Core Identity, suivez les instructions de la [Introduction à l’identité sur ASP.NET Core](xref:security/authentication/identity) rubrique.</span><span class="sxs-lookup"><span data-stu-id="0a80e-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="0a80e-115">Les concepts et les exemples présentés dans cette rubrique s’appliquent également aux applications qui utilisent l’identité de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a80e-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="0a80e-116">Nécessite une autorisation pour accéder à une page</span><span class="sxs-lookup"><span data-stu-id="0a80e-116">Require authorization to access a page</span></span>

<span data-ttu-id="0a80e-117">Utilisez le [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="0a80e-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="0a80e-118">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de la racine Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="0a80e-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="0a80e-119">Un [AuthorizePage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) n’est disponible que si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="0a80e-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="0a80e-120">Un `AuthorizeFilter` peut être appliqué à une classe de modèle de page avec le `[Authorize]` attribut de filtre.</span><span class="sxs-lookup"><span data-stu-id="0a80e-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="0a80e-121">Pour plus d’informations, consultez [attribut de filtre Authorize](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="0a80e-121">For more information, see [Authorize filter attribute](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="0a80e-122">Nécessite une autorisation pour accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="0a80e-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="0a80e-123">Utilisez le [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages dans un dossier dans le chemin d’accès spécifié :</span><span class="sxs-lookup"><span data-stu-id="0a80e-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="0a80e-124">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif racine Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="0a80e-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="0a80e-125">Un [AuthorizeFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) n’est disponible que si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="0a80e-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="0a80e-126">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="0a80e-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="0a80e-127">Utilisez le [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="0a80e-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="0a80e-128">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de la racine Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="0a80e-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="0a80e-129">Autoriser l’accès anonyme dans un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="0a80e-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="0a80e-130">Utilisez le [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages dans un dossier dans le chemin d’accès spécifié :</span><span class="sxs-lookup"><span data-stu-id="0a80e-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="0a80e-131">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif racine Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="0a80e-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="0a80e-132">Remarque sur la combinaison autorisé et l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="0a80e-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="0a80e-133">Il est parfaitement possible de spécifier un dossier de pages de requièrent une autorisation et de spécifier qu’une page de ce dossier autorise l’accès anonyme :</span><span class="sxs-lookup"><span data-stu-id="0a80e-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="0a80e-134">L’inverse, toutefois, n’est pas vrai.</span><span class="sxs-lookup"><span data-stu-id="0a80e-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="0a80e-135">Vous ne peut pas déclarer un dossier de pages pour l’accès anonyme et spécifier une page au sein d’autorisation :</span><span class="sxs-lookup"><span data-stu-id="0a80e-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="0a80e-136">Nécessitant une autorisation sur la page privée ne fonctionne pas, car lorsqu’à la fois le `AllowAnonymousFilter` et `AuthorizeFilter` filtres sont appliqués à la page, le `AllowAnonymousFilter` wins et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="0a80e-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a80e-137">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0a80e-137">Additional resources</span></span>

* [<span data-ttu-id="0a80e-138">Itinéraire personnalisé des pages Razor et fournisseurs de modèle de page</span><span class="sxs-lookup"><span data-stu-id="0a80e-138">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="0a80e-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="0a80e-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
