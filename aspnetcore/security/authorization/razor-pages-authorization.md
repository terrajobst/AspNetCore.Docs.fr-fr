---
title: "Conventions d’autorisation de Pages Razor dans ASP.NET Core"
author: guardrex
description: "Découvrez comment contrôler l’accès aux pages avec conventions au démarrage d’autorisent des utilisateurs et d’autoriser les utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages."
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 2bad6e1cc654b972206af03f99160628f81e026f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="5b9f6-103">Conventions d’autorisation de Pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b9f6-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="5b9f6-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b9f6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b9f6-105">Une façon de contrôler l’accès de votre application de Pages Razor consiste à utiliser des conventions d’autorisation au démarrage.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="5b9f6-106">Ces conventions permettent d’autoriser les utilisateurs et d’autoriser les utilisateurs anonymes d’accéder à des pages individuelles ou des dossiers de pages.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="5b9f6-107">Les conventions décrites dans cette rubrique automatiquement s’appliquent [filtres d’autorisation](xref:mvc/controllers/filters#authorization-filters) pour contrôler l’accès.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="5b9f6-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b9f6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="5b9f6-109">Nécessite une autorisation pour accéder à une page</span><span class="sxs-lookup"><span data-stu-id="5b9f6-109">Require authorization to access a page</span></span>

<span data-ttu-id="5b9f6-110">Utilisez le [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à la page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="5b9f6-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="5b9f6-111">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de la racine Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="5b9f6-112">Un [AuthorizePage surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) n’est disponible que si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="5b9f6-113">Nécessite une autorisation pour accéder à un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="5b9f6-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="5b9f6-114">Utilisez le [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) à toutes les pages dans un dossier dans le chemin d’accès spécifié :</span><span class="sxs-lookup"><span data-stu-id="5b9f6-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="5b9f6-115">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif racine Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="5b9f6-116">Un [AuthorizeFolder surcharge](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) n’est disponible que si vous devez spécifier une stratégie d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="5b9f6-117">Autoriser l’accès anonyme à une page</span><span class="sxs-lookup"><span data-stu-id="5b9f6-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="5b9f6-118">Utilisez le [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à une page à l’emplacement spécifié :</span><span class="sxs-lookup"><span data-stu-id="5b9f6-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="5b9f6-119">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin relatif de la racine Pages Razor sans une extension et contenant des barres obliques uniquement.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="5b9f6-120">Autoriser l’accès anonyme dans un dossier de pages</span><span class="sxs-lookup"><span data-stu-id="5b9f6-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="5b9f6-121">Utilisez le [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) pour ajouter un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) à toutes les pages dans un dossier dans le chemin d’accès spécifié :</span><span class="sxs-lookup"><span data-stu-id="5b9f6-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="5b9f6-122">Le chemin d’accès spécifié est le chemin d’accès du moteur d’affichage, qui est le chemin d’accès relatif racine Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="5b9f6-123">Remarque sur la combinaison autorisé et l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="5b9f6-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="5b9f6-124">Il est parfaitement possible de spécifier un dossier de pages de requièrent une autorisation et de spécifier qu’une page de ce dossier autorise l’accès anonyme :</span><span class="sxs-lookup"><span data-stu-id="5b9f6-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="5b9f6-125">L’inverse, toutefois, n’est pas vrai.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="5b9f6-126">Vous ne peut pas déclarer un dossier de pages pour l’accès anonyme et spécifier une page au sein d’autorisation :</span><span class="sxs-lookup"><span data-stu-id="5b9f6-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="5b9f6-127">Nécessitant une autorisation sur la page privée ne fonctionne pas, car lorsqu’à la fois le `AllowAnonymousFilter` et `AuthorizeFilter` filtres sont appliqués à la page, le `AllowAnonymousFilter` wins et contrôle l’accès.</span><span class="sxs-lookup"><span data-stu-id="5b9f6-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="5b9f6-128">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="5b9f6-128">See also</span></span>

* [<span data-ttu-id="5b9f6-129">Itinéraire personnalisé des pages Razor et fournisseurs de modèle de page</span><span class="sxs-lookup"><span data-stu-id="5b9f6-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="5b9f6-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span><span class="sxs-lookup"><span data-stu-id="5b9f6-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
