---
title: Composants d’application dans ASP.NET Core
author: rick-anderson
description: Partager des contrôleurs, afficher, Razor Pages et plus avec des composants d’application dans ASP.NET Core
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905762"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="7098f-103">Partager des contrôleurs, des affichages, des Razor Pages et plus avec des composants d’application dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7098f-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7098f-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7098f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7098f-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7098f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7098f-106">Une *partie d’application* est une abstraction sur les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="7098f-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="7098f-107">Les composants de l’application permettent à ASP.NET Core de découvrir des contrôleurs, des composants de vue, des tag helpers, des Razor Pages, des sources de compilation Razor et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="7098f-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="7098f-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) est une partie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7098f-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="7098f-109">`AssemblyPart` encapsule une référence d’assembly et expose des types et des références de compilation.</span><span class="sxs-lookup"><span data-stu-id="7098f-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="7098f-110">Les *fournisseurs de fonctionnalités* utilisent des composants d’application pour remplir les fonctionnalités d’une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="7098f-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="7098f-111">Le principal cas d’utilisation des composants d’application consiste à configurer une application pour découvrir (ou éviter le chargement) ASP.NET Core fonctionnalités d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="7098f-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="7098f-112">Par exemple, vous pouvez souhaiter partager des fonctionnalités communes entre plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="7098f-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="7098f-113">À l’aide de composants d’application, vous pouvez partager un assembly (DLL) contenant des contrôleurs, des vues, des Razor Pages, des sources de compilation Razor, des balises d’aide et bien plus encore avec plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="7098f-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="7098f-114">Le partage d’un assembly est préférable à la duplication du code dans plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="7098f-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="7098f-115">Les applications ASP.NET Core chargent les fonctionnalités à partir de <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="7098f-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="7098f-116">La classe <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> représente une partie d’application qui est sauvegardée par un assembly.</span><span class="sxs-lookup"><span data-stu-id="7098f-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="7098f-117">Charger les fonctionnalités de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7098f-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="7098f-118">Utilisez les classes `ApplicationPart` et `AssemblyPart` pour découvrir et charger des fonctionnalités ASP.NET Core (contrôleurs, composants de vue, etc.).</span><span class="sxs-lookup"><span data-stu-id="7098f-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="7098f-119">Le <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> effectue le suivi des composants d’application et des fournisseurs de fonctionnalités disponibles.</span><span class="sxs-lookup"><span data-stu-id="7098f-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="7098f-120">`ApplicationPartManager` est configurée dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7098f-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="7098f-121">Le code suivant présente une approche alternative à la configuration de `ApplicationPartManager` à l’aide de `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="7098f-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="7098f-122">Les deux exemples de code précédents chargent les `SharedController` à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="7098f-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="7098f-123">Le `SharedController` n’est pas dans le projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="7098f-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="7098f-124">Consultez le téléchargement de l’exemple de [solution WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="7098f-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="7098f-125">Inclure les vues</span><span class="sxs-lookup"><span data-stu-id="7098f-125">Include views</span></span>

<span data-ttu-id="7098f-126">Pour inclure des vues dans l’assembly :</span><span class="sxs-lookup"><span data-stu-id="7098f-126">To include views in the assembly:</span></span>

* <span data-ttu-id="7098f-127">Ajoutez le balisage suivant au fichier projet partagé :</span><span class="sxs-lookup"><span data-stu-id="7098f-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="7098f-128">Ajoutez le <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> à la <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="7098f-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="7098f-129">Empêcher le chargement des ressources</span><span class="sxs-lookup"><span data-stu-id="7098f-129">Prevent loading resources</span></span>

<span data-ttu-id="7098f-130">Les composants d’application peuvent être utilisés pour *éviter* de charger des ressources dans un assembly ou un emplacement particulier.</span><span class="sxs-lookup"><span data-stu-id="7098f-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="7098f-131">Ajoutez ou supprimez des membres de la collection <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour masquer ou rendre disponibles les ressources.</span><span class="sxs-lookup"><span data-stu-id="7098f-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="7098f-132">L’ordre des entrées dans la collection `ApplicationParts` n’est pas important.</span><span class="sxs-lookup"><span data-stu-id="7098f-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="7098f-133">Configurez l' `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="7098f-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="7098f-134">Par exemple, configurez la `ApplicationPartManager` avant d’appeler `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="7098f-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="7098f-135">Appelez `Remove` sur la collection `ApplicationParts` pour supprimer une ressource.</span><span class="sxs-lookup"><span data-stu-id="7098f-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="7098f-136">Le code suivant utilise <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour supprimer des `MyDependentLibrary` de l’application : [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="7098f-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="7098f-137">Le `ApplicationPartManager` comprend des parties pour :</span><span class="sxs-lookup"><span data-stu-id="7098f-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="7098f-138">Assembly de l’application et assemblys dépendants.</span><span class="sxs-lookup"><span data-stu-id="7098f-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="7098f-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.,</span><span class="sxs-lookup"><span data-stu-id="7098f-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="7098f-140">`Microsoft.AspNetCore.Mvc.Razor`.,</span><span class="sxs-lookup"><span data-stu-id="7098f-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="7098f-141">Fournisseurs de fonctionnalités d’application</span><span class="sxs-lookup"><span data-stu-id="7098f-141">Application feature providers</span></span>

<span data-ttu-id="7098f-142">Les fournisseurs de fonctionnalités d’application examinent les parties de l’application et fournissent des fonctionnalités pour ces parties.</span><span class="sxs-lookup"><span data-stu-id="7098f-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="7098f-143">Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités de ASP.NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="7098f-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="7098f-144">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="7098f-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="7098f-145">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="7098f-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="7098f-146">Les composants de vues</span><span class="sxs-lookup"><span data-stu-id="7098f-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="7098f-147">Les fournisseurs de fonctionnalités héritent de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, où `T` correspond au type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="7098f-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="7098f-148">Les fournisseurs de fonctionnalités peuvent être implémentés pour l’un des types de fonctionnalités précédemment listés.</span><span class="sxs-lookup"><span data-stu-id="7098f-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="7098f-149">L’ordre des fournisseurs de fonctionnalités dans le `ApplicationPartManager.FeatureProviders` peut avoir un impact sur le comportement de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="7098f-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="7098f-150">Les fournisseurs ajoutés ultérieurement peuvent réagir aux actions effectuées par les fournisseurs précédemment ajoutés.</span><span class="sxs-lookup"><span data-stu-id="7098f-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="7098f-151">Fonctionnalité de contrôleur générique</span><span class="sxs-lookup"><span data-stu-id="7098f-151">Generic controller feature</span></span>

<span data-ttu-id="7098f-152">ASP.NET Core ignore les [contrôleurs génériques](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="7098f-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="7098f-153">Un contrôleur générique a un paramètre de type (par exemple, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="7098f-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="7098f-154">L’exemple suivant ajoute des instances de contrôleur génériques pour une liste de types spécifiée :</span><span class="sxs-lookup"><span data-stu-id="7098f-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="7098f-155">Les types sont définis dans `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="7098f-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="7098f-156">Le fournisseur de fonctionnalités est ajouté dans `Startup` :</span><span class="sxs-lookup"><span data-stu-id="7098f-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="7098f-157">Les noms de contrôleurs génériques utilisés pour le routage se présentent sous la forme *GenericController' 1 [widget]* au lieu du *widget*.</span><span class="sxs-lookup"><span data-stu-id="7098f-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="7098f-158">L’attribut suivant modifie le nom pour qu’il corresponde au type générique utilisé par le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="7098f-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="7098f-159">Classe `GenericController` :</span><span class="sxs-lookup"><span data-stu-id="7098f-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="7098f-160">Par exemple, la demande d’une URL de `https://localhost:5001/Sprocket` entraîne la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="7098f-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="7098f-161">Afficher les fonctionnalités disponibles</span><span class="sxs-lookup"><span data-stu-id="7098f-161">Display available features</span></span>

<span data-ttu-id="7098f-162">Les fonctionnalités disponibles pour une application peuvent être énumérées en demandant une `ApplicationPartManager` via l' [injection de dépendances](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="7098f-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="7098f-163">L' [exemple Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) utilise le code précédent pour afficher les fonctionnalités de l’application :</span><span class="sxs-lookup"><span data-stu-id="7098f-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7098f-164">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7098f-164">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7098f-165">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7098f-165">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7098f-166">Une *partie d’application* est une abstraction sur les ressources d’une application.</span><span class="sxs-lookup"><span data-stu-id="7098f-166">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="7098f-167">Les composants de l’application permettent à ASP.NET Core de découvrir des contrôleurs, des composants de vue, des tag helpers, des Razor Pages, des sources de compilation Razor et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="7098f-167">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="7098f-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) est une partie de l’application.</span><span class="sxs-lookup"><span data-stu-id="7098f-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="7098f-169">`AssemblyPart` encapsule une référence d’assembly et expose des types et des références de compilation.</span><span class="sxs-lookup"><span data-stu-id="7098f-169">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="7098f-170">Les *fournisseurs de fonctionnalités* utilisent des composants d’application pour remplir les fonctionnalités d’une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="7098f-170">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="7098f-171">Le principal cas d’utilisation des composants d’application consiste à configurer une application pour découvrir (ou éviter le chargement) ASP.NET Core fonctionnalités d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="7098f-171">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="7098f-172">Par exemple, vous pouvez souhaiter partager des fonctionnalités communes entre plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="7098f-172">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="7098f-173">À l’aide de composants d’application, vous pouvez partager un assembly (DLL) contenant des contrôleurs, des vues, des Razor Pages, des sources de compilation Razor, des balises d’aide et bien plus encore avec plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="7098f-173">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="7098f-174">Le partage d’un assembly est préférable à la duplication du code dans plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="7098f-174">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="7098f-175">Les applications ASP.NET Core chargent les fonctionnalités à partir de <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="7098f-175">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="7098f-176">La classe <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> représente une partie d’application qui est sauvegardée par un assembly.</span><span class="sxs-lookup"><span data-stu-id="7098f-176">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="7098f-177">Charger les fonctionnalités de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7098f-177">Load ASP.NET Core features</span></span>

<span data-ttu-id="7098f-178">Utilisez les classes `ApplicationPart` et `AssemblyPart` pour découvrir et charger des fonctionnalités ASP.NET Core (contrôleurs, composants de vue, etc.).</span><span class="sxs-lookup"><span data-stu-id="7098f-178">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="7098f-179">Le <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> effectue le suivi des composants d’application et des fournisseurs de fonctionnalités disponibles.</span><span class="sxs-lookup"><span data-stu-id="7098f-179">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="7098f-180">`ApplicationPartManager` est configurée dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7098f-180">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="7098f-181">Le code suivant présente une approche alternative à la configuration de `ApplicationPartManager` à l’aide de `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="7098f-181">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="7098f-182">Les deux exemples de code précédents chargent les `SharedController` à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="7098f-182">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="7098f-183">Le `SharedController` n’est pas dans le projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="7098f-183">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="7098f-184">Consultez le téléchargement de l’exemple de [solution WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) .</span><span class="sxs-lookup"><span data-stu-id="7098f-184">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="7098f-185">Inclure les vues</span><span class="sxs-lookup"><span data-stu-id="7098f-185">Include views</span></span>

<span data-ttu-id="7098f-186">Pour inclure des vues dans l’assembly :</span><span class="sxs-lookup"><span data-stu-id="7098f-186">To include views in the assembly:</span></span>

* <span data-ttu-id="7098f-187">Ajoutez le balisage suivant au fichier projet partagé :</span><span class="sxs-lookup"><span data-stu-id="7098f-187">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="7098f-188">Ajoutez le <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> à la <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="7098f-188">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="7098f-189">Empêcher le chargement des ressources</span><span class="sxs-lookup"><span data-stu-id="7098f-189">Prevent loading resources</span></span>

<span data-ttu-id="7098f-190">Les composants d’application peuvent être utilisés pour *éviter* de charger des ressources dans un assembly ou un emplacement particulier.</span><span class="sxs-lookup"><span data-stu-id="7098f-190">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="7098f-191">Ajoutez ou supprimez des membres de la collection <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour masquer ou rendre disponibles les ressources.</span><span class="sxs-lookup"><span data-stu-id="7098f-191">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="7098f-192">L’ordre des entrées dans la collection `ApplicationParts` n’est pas important.</span><span class="sxs-lookup"><span data-stu-id="7098f-192">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="7098f-193">Configurez l' `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="7098f-193">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="7098f-194">Par exemple, configurez la `ApplicationPartManager` avant d’appeler `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="7098f-194">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="7098f-195">Appelez `Remove` sur la collection `ApplicationParts` pour supprimer une ressource.</span><span class="sxs-lookup"><span data-stu-id="7098f-195">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="7098f-196">Le code suivant utilise <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> pour supprimer des `MyDependentLibrary` de l’application : [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="7098f-196">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="7098f-197">Le `ApplicationPartManager` comprend des parties pour :</span><span class="sxs-lookup"><span data-stu-id="7098f-197">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="7098f-198">Assembly de l’application et assemblys dépendants.</span><span class="sxs-lookup"><span data-stu-id="7098f-198">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="7098f-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.,</span><span class="sxs-lookup"><span data-stu-id="7098f-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="7098f-200">`Microsoft.AspNetCore.Mvc.Razor`.,</span><span class="sxs-lookup"><span data-stu-id="7098f-200">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="7098f-201">Fournisseurs de fonctionnalités d’application</span><span class="sxs-lookup"><span data-stu-id="7098f-201">Application feature providers</span></span>

<span data-ttu-id="7098f-202">Les fournisseurs de fonctionnalités d’application examinent les parties de l’application et fournissent des fonctionnalités pour ces parties.</span><span class="sxs-lookup"><span data-stu-id="7098f-202">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="7098f-203">Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités de ASP.NET Core suivantes :</span><span class="sxs-lookup"><span data-stu-id="7098f-203">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="7098f-204">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="7098f-204">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="7098f-205">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="7098f-205">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="7098f-206">Les composants de vues</span><span class="sxs-lookup"><span data-stu-id="7098f-206">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="7098f-207">Les fournisseurs de fonctionnalités héritent de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, où `T` correspond au type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="7098f-207">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="7098f-208">Les fournisseurs de fonctionnalités peuvent être implémentés pour l’un des types de fonctionnalités précédemment listés.</span><span class="sxs-lookup"><span data-stu-id="7098f-208">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="7098f-209">L’ordre des fournisseurs de fonctionnalités dans le `ApplicationPartManager.FeatureProviders` peut avoir un impact sur le comportement de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="7098f-209">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="7098f-210">Les fournisseurs ajoutés ultérieurement peuvent réagir aux actions effectuées par les fournisseurs précédemment ajoutés.</span><span class="sxs-lookup"><span data-stu-id="7098f-210">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="7098f-211">Fonctionnalité de contrôleur générique</span><span class="sxs-lookup"><span data-stu-id="7098f-211">Generic controller feature</span></span>

<span data-ttu-id="7098f-212">ASP.NET Core ignore les [contrôleurs génériques](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="7098f-212">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="7098f-213">Un contrôleur générique a un paramètre de type (par exemple, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="7098f-213">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="7098f-214">L’exemple suivant ajoute des instances de contrôleur génériques pour une liste de types spécifiée :</span><span class="sxs-lookup"><span data-stu-id="7098f-214">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="7098f-215">Les types sont définis dans `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="7098f-215">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="7098f-216">Le fournisseur de fonctionnalités est ajouté dans `Startup` :</span><span class="sxs-lookup"><span data-stu-id="7098f-216">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="7098f-217">Les noms de contrôleurs génériques utilisés pour le routage se présentent sous la forme *GenericController' 1 [widget]* au lieu du *widget*.</span><span class="sxs-lookup"><span data-stu-id="7098f-217">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="7098f-218">L’attribut suivant modifie le nom pour qu’il corresponde au type générique utilisé par le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="7098f-218">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="7098f-219">Classe `GenericController` :</span><span class="sxs-lookup"><span data-stu-id="7098f-219">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="7098f-220">Par exemple, la demande d’une URL de `https://localhost:5001/Sprocket` entraîne la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="7098f-220">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="7098f-221">Afficher les fonctionnalités disponibles</span><span class="sxs-lookup"><span data-stu-id="7098f-221">Display available features</span></span>

<span data-ttu-id="7098f-222">Les fonctionnalités disponibles pour une application peuvent être énumérées en demandant une `ApplicationPartManager` via l' [injection de dépendances](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="7098f-222">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="7098f-223">L' [exemple Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) utilise le code précédent pour afficher les fonctionnalités de l’application :</span><span class="sxs-lookup"><span data-stu-id="7098f-223">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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

::: moniker-end
