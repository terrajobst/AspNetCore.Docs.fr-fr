---
title: Composants d’application dans ASP.NET Core
author: rick-anderson
description: Partager des contrôleurs, afficher, Razor Pages et plus avec des composants d’application dans ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ad0372f25377115e6fc7c8ea42db75de56b3e6d2
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187016"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="b521f-103">Partager des contrôleurs, des affichages, des Razor Pages et plus avec des composants d’application dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b521f-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>
=======

<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

<span data-ttu-id="b521f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b521f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b521f-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b521f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b521f-106">Une *partie d’application* est une abstraction sur les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="b521f-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="b521f-107">Les composants de l’application permettent à ASP.NET Core de découvrir des contrôleurs, des composants de vue, des tag helpers, des Razor Pages, des sources de compilation Razor et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="b521f-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="b521f-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) est une partie de l’application.</span><span class="sxs-lookup"><span data-stu-id="b521f-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="b521f-109">`AssemblyPart`encapsule une référence d’assembly et expose des types et des références de compilation.</span><span class="sxs-lookup"><span data-stu-id="b521f-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="b521f-110">Les *fournisseurs de fonctionnalités* utilisent des composants d’application pour remplir les fonctionnalités d’une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="b521f-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="b521f-111">Le principal cas d’utilisation des composants d’application consiste à configurer une application pour découvrir (ou éviter le chargement) ASP.NET Core fonctionnalités d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="b521f-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="b521f-112">Par exemple, vous pouvez souhaiter partager des fonctionnalités communes entre plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="b521f-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="b521f-113">À l’aide de composants d’application, vous pouvez partager un assembly (DLL) contenant des contrôleurs, des vues, des Razor Pages, des sources de compilation Razor, des balises d’aide et bien plus encore avec plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="b521f-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="b521f-114">Le partage d’un assembly est préférable à la duplication du code dans plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="b521f-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="b521f-115">Les applications ASP.NET Core chargent <xref:System.Web.WebPages.ApplicationPart>les fonctionnalités à partir de.</span><span class="sxs-lookup"><span data-stu-id="b521f-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="b521f-116">La <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> classe représente une partie d’application qui est sauvegardée par un assembly.</span><span class="sxs-lookup"><span data-stu-id="b521f-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="b521f-117">Charger les fonctionnalités de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b521f-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="b521f-118">Utilisez les `ApplicationPart` classes `AssemblyPart` et pour découvrir et charger des fonctionnalités de ASP.net Core (contrôleurs, composants de vue, etc.).</span><span class="sxs-lookup"><span data-stu-id="b521f-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="b521f-119">Le <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> effectue le suivi des composants d’application et des fournisseurs de fonctionnalités disponibles.</span><span class="sxs-lookup"><span data-stu-id="b521f-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="b521f-120">`ApplicationPartManager`est configuré dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b521f-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="b521f-121">Le code suivant présente une approche alternative à la configuration `ApplicationPartManager` de `AssemblyPart`à l’aide de :</span><span class="sxs-lookup"><span data-stu-id="b521f-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="b521f-122">Les deux exemples `SharedController` de code précédents chargent à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="b521f-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="b521f-123">Le `SharedController` n’est pas dans le projet d’application.</span><span class="sxs-lookup"><span data-stu-id="b521f-123">The `SharedController` is not in the applications project.</span></span> <span data-ttu-id="b521f-124">Consultez le téléchargement de l’exemple de [solution WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="b521f-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="b521f-125">Inclure les vues</span><span class="sxs-lookup"><span data-stu-id="b521f-125">Include views</span></span>

<span data-ttu-id="b521f-126">Pour inclure des vues dans l’assembly :</span><span class="sxs-lookup"><span data-stu-id="b521f-126">To include views in the assembly:</span></span>

* <span data-ttu-id="b521f-127">Ajoutez le balisage suivant au fichier projet partagé :</span><span class="sxs-lookup"><span data-stu-id="b521f-127">Add the following markup to the shared project file:</span></span>

  ```csproj
    <ItemGroup>
      <EmbeddedResource Include = "Views\**\*.cshtml" />
    </ ItemGroup >
  ```

* <span data-ttu-id="b521f-128">Ajoutez le <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>au :</span><span class="sxs-lookup"><span data-stu-id="b521f-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="b521f-129">Empêcher le chargement des ressources</span><span class="sxs-lookup"><span data-stu-id="b521f-129">Prevent loading resources</span></span>

<span data-ttu-id="b521f-130">Les composants d’application peuvent être utilisés pour *éviter* de charger des ressources dans un assembly ou un emplacement particulier.</span><span class="sxs-lookup"><span data-stu-id="b521f-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="b521f-131">Ajoutez ou supprimez des membres <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> du regroupement pour masquer ou rendre disponibles les ressources.</span><span class="sxs-lookup"><span data-stu-id="b521f-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="b521f-132">L’ordre des entrées dans la collection `ApplicationParts` n’est pas important.</span><span class="sxs-lookup"><span data-stu-id="b521f-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="b521f-133">Configurez l `ApplicationPartManager` 'avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="b521f-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="b521f-134">Par exemple, configurez le `ApplicationPartManager` avant d’appeler. `AddControllersAsServices`</span><span class="sxs-lookup"><span data-stu-id="b521f-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="b521f-135">Appelez `Remove` sur la `ApplicationParts` collection pour supprimer une ressource.</span><span class="sxs-lookup"><span data-stu-id="b521f-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="b521f-136">Le code suivant utilise <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour supprimer `MyDependentLibrary` de l’application :[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="b521f-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="b521f-137">Le `ApplicationPartManager` comprend des parties pour :</span><span class="sxs-lookup"><span data-stu-id="b521f-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="b521f-138">Assembly d’applications et assemblys dépendants.</span><span class="sxs-lookup"><span data-stu-id="b521f-138">The apps assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.TagHelpers`
* <span data-ttu-id="b521f-139">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="b521f-139">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="b521f-140">Fournisseurs de fonctionnalités d’application</span><span class="sxs-lookup"><span data-stu-id="b521f-140">Application feature providers</span></span>

<span data-ttu-id="b521f-141">Les fournisseurs de fonctionnalités d’application examinent les parties de l’application et fournissent des fonctionnalités pour ces parties.</span><span class="sxs-lookup"><span data-stu-id="b521f-141">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="b521f-142">Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités de ASP.NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="b521f-142">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="b521f-143">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="b521f-143">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="b521f-144">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="b521f-144">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="b521f-145">Les composants de vues</span><span class="sxs-lookup"><span data-stu-id="b521f-145">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="b521f-146">Les fournisseurs de fonctionnalités héritent de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, où `T` correspond au type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="b521f-146">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="b521f-147">Les fournisseurs de fonctionnalités peuvent être implémentés pour l’un des types de fonctionnalités précédemment listés.</span><span class="sxs-lookup"><span data-stu-id="b521f-147">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="b521f-148">L’ordre des fournisseurs de fonctionnalités dans `ApplicationPartManager.FeatureProviders` le peut avoir un impact sur le comportement de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="b521f-148">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="b521f-149">Les fournisseurs ajoutés ultérieurement peuvent réagir aux actions effectuées par les fournisseurs précédemment ajoutés.</span><span class="sxs-lookup"><span data-stu-id="b521f-149">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="b521f-150">Fonctionnalité de contrôleur générique</span><span class="sxs-lookup"><span data-stu-id="b521f-150">Generic controller feature</span></span>

<span data-ttu-id="b521f-151">ASP.NET Core ignore les [contrôleurs génériques](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="b521f-151">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="b521f-152">Un contrôleur générique a un paramètre de type (par exemple `MyController<T>`,).</span><span class="sxs-lookup"><span data-stu-id="b521f-152">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="b521f-153">L’exemple suivant ajoute des instances de contrôleur génériques pour une liste de types spécifiée.</span><span class="sxs-lookup"><span data-stu-id="b521f-153">The following sample adds generic controller instances for a specified list of types.</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="b521f-154">Les types sont définis dans `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="b521f-154">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="b521f-155">Le fournisseur de fonctionnalités est ajouté dans `Startup` :</span><span class="sxs-lookup"><span data-stu-id="b521f-155">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="b521f-156">Les noms de contrôleurs génériques utilisés pour le routage se présentent sous la forme *GenericController' 1 [widget]* au lieu du *widget*.</span><span class="sxs-lookup"><span data-stu-id="b521f-156">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="b521f-157">L’attribut suivant modifie le nom pour qu’il corresponde au type générique utilisé par le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="b521f-157">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="b521f-158">La classe `GenericController` :</span><span class="sxs-lookup"><span data-stu-id="b521f-158">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

### <a name="display-available-features"></a><span data-ttu-id="b521f-159">Afficher les fonctionnalités disponibles</span><span class="sxs-lookup"><span data-stu-id="b521f-159">Display available features</span></span>

<span data-ttu-id="b521f-160">Les fonctionnalités disponibles pour une application peuvent être énumérées par en demandant un `ApplicationPartManager` par [injection de dépendances](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="b521f-160">The features available to an app can be enumerated by by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="b521f-161">L' [exemple Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) utilise le code précédent pour afficher les fonctionnalités de l’application.</span><span class="sxs-lookup"><span data-stu-id="b521f-161">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features.</span></span>