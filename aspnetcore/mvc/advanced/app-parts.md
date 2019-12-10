---
title: Partager des contrôleurs, des affichages, des Razor Pages et plus avec des composants d’application dans ASP.NET Core
author: rick-anderson
description: Partager des contrôleurs, afficher, Razor Pages et plus avec des composants d’application dans ASP.NET Core
ms.author: riande
ms.date: 11/11/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a102511478c40ae64aada919fee7072c3027ddcd
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74958993"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts"></a><span data-ttu-id="f0ba1-103">Partager des contrôleurs, des vues, des Razor Pages et plus avec des composants d’application</span><span class="sxs-lookup"><span data-stu-id="f0ba1-103">Share controllers, views, Razor Pages and more with Application Parts</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0ba1-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f0ba1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f0ba1-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0ba1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f0ba1-106">Une *partie d’application* est une abstraction sur les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="f0ba1-107">Les composants de l’application permettent à ASP.NET Core de découvrir des contrôleurs, des composants de vue, des tag helpers, des Razor Pages, des sources de compilation Razor et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="f0ba1-108"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> est une partie de l’application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-108"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> is an Application part.</span></span> <span data-ttu-id="f0ba1-109">`AssemblyPart` encapsule une référence d’assembly et expose des types et des références de compilation.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="f0ba1-110">Les [fournisseurs de fonctionnalités](#fp) utilisent des composants d’application pour remplir les fonctionnalités d’une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-110">[Feature providers](#fp) work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="f0ba1-111">Le principal cas d’utilisation des composants d’application consiste à configurer une application pour découvrir (ou éviter le chargement) ASP.NET Core fonctionnalités d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="f0ba1-112">Par exemple, vous pouvez souhaiter partager des fonctionnalités communes entre plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="f0ba1-113">À l’aide de composants d’application, vous pouvez partager un assembly (DLL) contenant des contrôleurs, des vues, des Razor Pages, des sources de compilation Razor, des balises d’aide et bien plus encore avec plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="f0ba1-114">Le partage d’un assembly est préférable à la duplication du code dans plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="f0ba1-115">Les applications ASP.NET Core chargent les fonctionnalités à partir de <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="f0ba1-116">La classe <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> représente une partie d’application qui est sauvegardée par un assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="f0ba1-117">Charger les fonctionnalités de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0ba1-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="f0ba1-118">Utilisez les classes <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> et <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> pour découvrir et charger des fonctionnalités ASP.NET Core (contrôleurs, composants de vue, etc.).</span><span class="sxs-lookup"><span data-stu-id="f0ba1-118">Use the <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> and <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="f0ba1-119">Le <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> effectue le suivi des composants d’application et des fournisseurs de fonctionnalités disponibles.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="f0ba1-120">`ApplicationPartManager` est configurée dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f0ba1-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="f0ba1-121">Le code suivant présente une approche alternative à la configuration de `ApplicationPartManager` à l’aide de `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="f0ba1-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="f0ba1-122">Les deux exemples de code précédents chargent les `SharedController` à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="f0ba1-123">Le `SharedController` n’est pas dans le projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-123">The `SharedController` is not in the app's project.</span></span> <span data-ttu-id="f0ba1-124">Consultez le téléchargement de l’exemple de [solution WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="f0ba1-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="f0ba1-125">Inclure les vues</span><span class="sxs-lookup"><span data-stu-id="f0ba1-125">Include views</span></span>

<span data-ttu-id="f0ba1-126">Utilisez une [bibliothèque de classes Razor](xref:razor-pages/ui-class) pour inclure des vues dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-126">Use a [Razor class library](xref:razor-pages/ui-class) to include views in the assembly.</span></span>

### <a name="prevent-loading-resources"></a><span data-ttu-id="f0ba1-127">Empêcher le chargement des ressources</span><span class="sxs-lookup"><span data-stu-id="f0ba1-127">Prevent loading resources</span></span>

<span data-ttu-id="f0ba1-128">Les composants d’application peuvent être utilisés pour *éviter* de charger des ressources dans un assembly ou un emplacement particulier.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-128">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="f0ba1-129">Ajoutez ou supprimez des membres de la collection <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour masquer ou rendre disponibles les ressources.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-129">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="f0ba1-130">L’ordre des entrées dans la collection `ApplicationParts` n’est pas important.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-130">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="f0ba1-131">Configurez l' `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-131">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="f0ba1-132">Par exemple, configurez la `ApplicationPartManager` avant d’appeler `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-132">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="f0ba1-133">Appelez `Remove` sur la collection `ApplicationParts` pour supprimer une ressource.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-133">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="f0ba1-134">Le `ApplicationPartManager` comprend des parties pour :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-134">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="f0ba1-135">Assembly de l’application et assemblys dépendants.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-135">The app's assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.ApplicationParts.CompiledRazorAssemblyPart`
* `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`
* <span data-ttu-id="f0ba1-136">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-136">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="f0ba1-137">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-137">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

<a name="fp"></a>

## <a name="feature-providers"></a><span data-ttu-id="f0ba1-138">Fournisseurs de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="f0ba1-138">Feature providers</span></span>

<span data-ttu-id="f0ba1-139">Les fournisseurs de fonctionnalités d’application examinent les parties de l’application et fournissent des fonctionnalités pour ces parties.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-139">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="f0ba1-140">Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités de ASP.NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-140">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Controllers.ControllerFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.MetadataReferenceFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.ViewsFeatureProvider>
* <span data-ttu-id="f0ba1-141">`internal class` [RazorCompiledItemFeatureProvider](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)</span><span class="sxs-lookup"><span data-stu-id="f0ba1-141">`internal class` [RazorCompiledItemFeatureProvider](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)</span></span>

<span data-ttu-id="f0ba1-142">Les fournisseurs de fonctionnalités héritent de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, où `T` correspond au type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-142">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="f0ba1-143">Les fournisseurs de fonctionnalités peuvent être implémentés pour l’un des types de fonctionnalités précédemment listés.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-143">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="f0ba1-144">L’ordre des fournisseurs de fonctionnalités dans le `ApplicationPartManager.FeatureProviders` peut avoir un impact sur le comportement de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-144">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="f0ba1-145">Les fournisseurs ajoutés ultérieurement peuvent réagir aux actions effectuées par les fournisseurs précédemment ajoutés.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-145">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="display-available-features"></a><span data-ttu-id="f0ba1-146">Afficher les fonctionnalités disponibles</span><span class="sxs-lookup"><span data-stu-id="f0ba1-146">Display available features</span></span>

<span data-ttu-id="f0ba1-147">Les fonctionnalités disponibles pour une application peuvent être énumérées en demandant une `ApplicationPartManager` via l' [injection de dépendances](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="f0ba1-147">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="f0ba1-148">L' [exemple Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) utilise le code précédent pour afficher les fonctionnalités de l’application :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-148">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a><span data-ttu-id="f0ba1-149">Détection dans les composants de l’application</span><span class="sxs-lookup"><span data-stu-id="f0ba1-149">Discovery in application parts</span></span>

<span data-ttu-id="f0ba1-150">Les erreurs HTTP 404 ne sont pas rares lors du développement avec des composants d’application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-150">HTTP 404 errors are not uncommon when developing with application parts.</span></span> <span data-ttu-id="f0ba1-151">Ces erreurs sont généralement dues à l’absence d’une exigence essentielle pour la façon dont les composants des applications sont détectés.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-151">These errors are typically caused by missing an essential requirement for how applications parts are discovered.</span></span> <span data-ttu-id="f0ba1-152">Si votre application renvoie une erreur HTTP 404, vérifiez que les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-152">If your app returns an HTTP 404 error, verify the following requirements have been met:</span></span>

* <span data-ttu-id="f0ba1-153">Le paramètre de `applicationName` doit être défini sur l’assembly racine utilisé pour la découverte.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-153">The `applicationName` setting needs to be set to the root assembly used for discovery.</span></span> <span data-ttu-id="f0ba1-154">L’assembly racine utilisé pour la découverte est normalement l’assembly de point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-154">The root assembly used for discovery is normally the entry point assembly.</span></span>
* <span data-ttu-id="f0ba1-155">L’assembly racine doit avoir une référence aux parties utilisées pour la découverte.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-155">The root assembly needs to have a reference to the parts used for discovery.</span></span> <span data-ttu-id="f0ba1-156">La référence peut être directe ou transitive.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-156">The reference can be direct or transitive.</span></span>
* <span data-ttu-id="f0ba1-157">L’assembly racine doit référencer le kit de développement logiciel (SDK) Web.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-157">The root assembly needs to reference the Web SDK.</span></span> <span data-ttu-id="f0ba1-158">L’infrastructure a une logique qui marque les attributs dans l’assembly racine utilisé pour la découverte.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-158">The framework has logic that stamps attributes into the root assembly that are used for discovery.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0ba1-159">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f0ba1-159">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f0ba1-160">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0ba1-160">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f0ba1-161">Une *partie d’application* est une abstraction sur les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-161">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="f0ba1-162">Les composants de l’application permettent à ASP.NET Core de découvrir des contrôleurs, des composants de vue, des tag helpers, des Razor Pages, des sources de compilation Razor et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-162">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="f0ba1-163">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) est une partie de l’application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-163">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="f0ba1-164">`AssemblyPart` encapsule une référence d’assembly et expose des types et des références de compilation.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-164">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="f0ba1-165">Les *fournisseurs de fonctionnalités* utilisent des composants d’application pour remplir les fonctionnalités d’une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-165">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="f0ba1-166">Le principal cas d’utilisation des composants d’application consiste à configurer une application pour découvrir (ou éviter le chargement) ASP.NET Core fonctionnalités d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-166">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="f0ba1-167">Par exemple, vous pouvez souhaiter partager des fonctionnalités communes entre plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-167">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="f0ba1-168">À l’aide de composants d’application, vous pouvez partager un assembly (DLL) contenant des contrôleurs, des vues, des Razor Pages, des sources de compilation Razor, des balises d’aide et bien plus encore avec plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-168">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="f0ba1-169">Le partage d’un assembly est préférable à la duplication du code dans plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-169">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="f0ba1-170">Les applications ASP.NET Core chargent les fonctionnalités à partir de <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-170">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="f0ba1-171">La classe <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> représente une partie d’application qui est sauvegardée par un assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-171">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="f0ba1-172">Charger les fonctionnalités de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0ba1-172">Load ASP.NET Core features</span></span>

<span data-ttu-id="f0ba1-173">Utilisez les classes `ApplicationPart` et `AssemblyPart` pour découvrir et charger des fonctionnalités ASP.NET Core (contrôleurs, composants de vue, etc.).</span><span class="sxs-lookup"><span data-stu-id="f0ba1-173">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="f0ba1-174">Le <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> effectue le suivi des composants d’application et des fournisseurs de fonctionnalités disponibles.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-174">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="f0ba1-175">`ApplicationPartManager` est configurée dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f0ba1-175">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="f0ba1-176">Le code suivant présente une approche alternative à la configuration de `ApplicationPartManager` à l’aide de `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="f0ba1-176">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="f0ba1-177">Les deux exemples de code précédents chargent les `SharedController` à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-177">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="f0ba1-178">Le `SharedController` n’est pas dans le projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-178">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="f0ba1-179">Consultez le téléchargement de l’exemple de [solution WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="f0ba1-179">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="f0ba1-180">Inclure les vues</span><span class="sxs-lookup"><span data-stu-id="f0ba1-180">Include views</span></span>

<span data-ttu-id="f0ba1-181">Utilisez une [bibliothèque de classes Razor](xref:razor-pages/ui-class) pour inclure des vues dans l’assembly.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-181">Use a [Razor class library](xref:razor-pages/ui-class) to include views in the assembly.</span></span>

### <a name="prevent-loading-resources"></a><span data-ttu-id="f0ba1-182">Empêcher le chargement des ressources</span><span class="sxs-lookup"><span data-stu-id="f0ba1-182">Prevent loading resources</span></span>

<span data-ttu-id="f0ba1-183">Les composants d’application peuvent être utilisés pour *éviter* de charger des ressources dans un assembly ou un emplacement particulier.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-183">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="f0ba1-184">Ajoutez ou supprimez des membres de la collection <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour masquer ou rendre disponibles les ressources.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-184">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="f0ba1-185">L’ordre des entrées dans la collection `ApplicationParts` n’est pas important.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-185">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="f0ba1-186">Configurez l' `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-186">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="f0ba1-187">Par exemple, configurez la `ApplicationPartManager` avant d’appeler `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-187">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="f0ba1-188">Appelez `Remove` sur la collection `ApplicationParts` pour supprimer une ressource.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-188">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="f0ba1-189">Le code suivant utilise <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour supprimer des `MyDependentLibrary` de l’application : [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="f0ba1-189">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="f0ba1-190">Le `ApplicationPartManager` comprend des parties pour :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-190">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="f0ba1-191">Assembly de l’application et assemblys dépendants.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-191">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="f0ba1-192">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-192">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="f0ba1-193">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-193">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="f0ba1-194">Fournisseurs de fonctionnalités d’application</span><span class="sxs-lookup"><span data-stu-id="f0ba1-194">Application feature providers</span></span>

<span data-ttu-id="f0ba1-195">Les fournisseurs de fonctionnalités d’application examinent les parties de l’application et fournissent des fonctionnalités pour ces parties.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-195">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="f0ba1-196">Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités de ASP.NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-196">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="f0ba1-197">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="f0ba1-197">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="f0ba1-198">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="f0ba1-198">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="f0ba1-199">Les composants de vues</span><span class="sxs-lookup"><span data-stu-id="f0ba1-199">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="f0ba1-200">Les fournisseurs de fonctionnalités héritent de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, où `T` correspond au type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-200">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="f0ba1-201">Les fournisseurs de fonctionnalités peuvent être implémentés pour l’un des types de fonctionnalités précédemment listés.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-201">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="f0ba1-202">L’ordre des fournisseurs de fonctionnalités dans le `ApplicationPartManager.FeatureProviders` peut avoir un impact sur le comportement de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-202">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="f0ba1-203">Les fournisseurs ajoutés ultérieurement peuvent réagir aux actions effectuées par les fournisseurs précédemment ajoutés.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-203">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="display-available-features"></a><span data-ttu-id="f0ba1-204">Afficher les fonctionnalités disponibles</span><span class="sxs-lookup"><span data-stu-id="f0ba1-204">Display available features</span></span>

<span data-ttu-id="f0ba1-205">Les fonctionnalités disponibles pour une application peuvent être énumérées en demandant une `ApplicationPartManager` via l' [injection de dépendances](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="f0ba1-205">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="f0ba1-206">L' [exemple Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) utilise le code précédent pour afficher les fonctionnalités de l’application :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-206">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a><span data-ttu-id="f0ba1-207">Détection dans les composants de l’application</span><span class="sxs-lookup"><span data-stu-id="f0ba1-207">Discovery in application parts</span></span>

<span data-ttu-id="f0ba1-208">Les erreurs HTTP 404 ne sont pas rares lors du développement avec des composants d’application.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-208">HTTP 404 errors are not uncommon when developing with application parts.</span></span> <span data-ttu-id="f0ba1-209">Ces erreurs sont généralement dues à l’absence d’une exigence essentielle pour la façon dont les composants des applications sont détectés.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-209">These errors are typically caused by missing an essential requirement for how applications parts are discovered.</span></span> <span data-ttu-id="f0ba1-210">Si votre application renvoie une erreur HTTP 404, vérifiez que les conditions suivantes sont remplies :</span><span class="sxs-lookup"><span data-stu-id="f0ba1-210">If your app returns an HTTP 404 error, verify the following requirements have been met:</span></span>

* <span data-ttu-id="f0ba1-211">Le paramètre de `applicationName` doit être défini sur l’assembly racine utilisé pour la découverte.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-211">The `applicationName` setting needs to be set to the root assembly used for discovery.</span></span> <span data-ttu-id="f0ba1-212">L’assembly racine utilisé pour la découverte est normalement l’assembly de point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-212">The root assembly used for discovery is normally the entry point assembly.</span></span>
* <span data-ttu-id="f0ba1-213">L’assembly racine doit avoir une référence aux parties utilisées pour la découverte.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-213">The root assembly needs to have a reference to the parts used for discovery.</span></span> <span data-ttu-id="f0ba1-214">La référence peut être directe ou transitive.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-214">The reference can be direct or transitive.</span></span>
* <span data-ttu-id="f0ba1-215">L’assembly racine doit référencer le kit de développement logiciel (SDK) Web.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-215">The root assembly needs to reference the Web SDK.</span></span>
  * <span data-ttu-id="f0ba1-216">L’infrastructure de ASP.NET Core a une logique de génération personnalisée qui marque les attributs dans l’assembly racine utilisé pour la découverte.</span><span class="sxs-lookup"><span data-stu-id="f0ba1-216">The ASP.NET Core framework has custom build logic that stamps attributes into the root assembly that are used for discovery.</span></span>

::: moniker-end
