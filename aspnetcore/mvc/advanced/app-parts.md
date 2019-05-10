---
title: Composants d’application dans ASP.NET Core
author: ardalis
description: Apprenez à utiliser les composants d’application, c’est-à-dire les abstractions des ressources d’une application, pour découvrir ou éviter de charger les fonctionnalités d’un assembly.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 5d5e79d3afe0eee6d0b126d4642ccd5f61bff409
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888134"
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="ffe10-103">Composants d’application dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe10-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="ffe10-104">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ffe10-104">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ffe10-105">Un *composant d’application* est une abstraction des ressources d’une application, qui permet de découvrir des fonctionnalités MVC telles que les contrôleurs, les composants de vue ou les Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="ffe10-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="ffe10-106">AssemblyPart est un exemple de composant d’application qui encapsule une référence d’assembly, et expose les types et les références de compilation.</span><span class="sxs-lookup"><span data-stu-id="ffe10-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="ffe10-107">Les *fournisseurs de fonctionnalités* utilisent les composants d’application pour remplir les fonctionnalités d’une application ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ffe10-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="ffe10-108">Le cas d’usage principal des composants d’application est de vous permettre de configurer votre application pour découvrir (ou éviter de charger) les fonctionnalités MVC d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="ffe10-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="ffe10-109">Présentation des composants d’application</span><span class="sxs-lookup"><span data-stu-id="ffe10-109">Introducing Application Parts</span></span>

<span data-ttu-id="ffe10-110">Les applications MVC chargent leurs fonctionnalités à partir des [composants d’application](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="ffe10-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="ffe10-111">En l’occurrence, la classe [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) représente un composant d’application qui s’appuie sur un assembly.</span><span class="sxs-lookup"><span data-stu-id="ffe10-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="ffe10-112">Vous pouvez utiliser ces classes pour découvrir et charger des fonctionnalités MVC, par exemple les contrôleurs, les composants de vues, les Tag Helpers et les sources de compilation Razor.</span><span class="sxs-lookup"><span data-stu-id="ffe10-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="ffe10-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) est responsable du suivi des composants d’application et des fournisseurs de fonctionnalités accessibles à l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="ffe10-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="ffe10-114">Vous pouvez interagir avec `ApplicationPartManager` dans `Startup` quand vous configurez MVC :</span><span class="sxs-lookup"><span data-stu-id="ffe10-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="ffe10-115">Par défaut, MVC recherche les contrôleurs dans l’arborescence des dépendances (même dans d’autres assemblys).</span><span class="sxs-lookup"><span data-stu-id="ffe10-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="ffe10-116">Pour charger un assembly arbitraire (par exemple, à partir d’un plug-in qui n’est pas référencé au moment de la compilation), vous pouvez utiliser un composant d’application.</span><span class="sxs-lookup"><span data-stu-id="ffe10-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="ffe10-117">Vous pouvez utiliser des composants d’application pour *éviter* de rechercher des contrôleurs dans un assembly ou un emplacement particulier.</span><span class="sxs-lookup"><span data-stu-id="ffe10-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="ffe10-118">Vous pouvez contrôler les composants (ou assemblys) accessibles à l’application en modifiant la collection `ApplicationParts` de `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="ffe10-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="ffe10-119">L’ordre des entrées dans la collection `ApplicationParts` n’est pas important.</span><span class="sxs-lookup"><span data-stu-id="ffe10-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="ffe10-120">Il est important de configurer complètement `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="ffe10-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="ffe10-121">Par exemple, vous devez configurer entièrement `ApplicationPartManager` avant d’appeler `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="ffe10-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="ffe10-122">Si vous ne le faites pas, les contrôleurs des composants d’application ajoutés après cet appel de méthode ne seront pas affectés (ne seront pas inscrits en tant que services), ce qui risque d’entraîner un comportement incorrect de votre application.</span><span class="sxs-lookup"><span data-stu-id="ffe10-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="ffe10-123">Si vous avez un assembly qui contient des contrôleurs que vous ne souhaitez pas utiliser, supprimez-le de `ApplicationPartManager` :</span><span class="sxs-lookup"><span data-stu-id="ffe10-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="ffe10-124">En plus de l’assembly de votre projet et de ses assemblys dépendants, `ApplicationPartManager` inclut des composants pour `Microsoft.AspNetCore.Mvc.TagHelpers` et `Microsoft.AspNetCore.Mvc.Razor` par défaut.</span><span class="sxs-lookup"><span data-stu-id="ffe10-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="ffe10-125">Fournisseurs de fonctionnalités d’application</span><span class="sxs-lookup"><span data-stu-id="ffe10-125">Application Feature Providers</span></span>

<span data-ttu-id="ffe10-126">Les fournisseurs de fonctionnalités d’application examinent les composants d’application et fournissent des fonctionnalités pour ces composants.</span><span class="sxs-lookup"><span data-stu-id="ffe10-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="ffe10-127">Il existe des fournisseurs de fonctionnalités intégrés pour les fonctionnalités MVC suivantes :</span><span class="sxs-lookup"><span data-stu-id="ffe10-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="ffe10-128">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="ffe10-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="ffe10-129">Référence de métadonnées</span><span class="sxs-lookup"><span data-stu-id="ffe10-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="ffe10-130">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="ffe10-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="ffe10-131">Les composants de vues</span><span class="sxs-lookup"><span data-stu-id="ffe10-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="ffe10-132">Les fournisseurs de fonctionnalités héritent de `IApplicationFeatureProvider<T>`, où `T` correspond au type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="ffe10-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="ffe10-133">Vous pouvez implémenter vos propres fournisseurs de fonctionnalités pour tous les types de fonctionnalité de MVC listés ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="ffe10-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="ffe10-134">L’ordre des fournisseurs de fonctionnalités dans la collection `ApplicationPartManager.FeatureProviders` peut avoir son importance, car les fournisseurs suivants peuvent réagir à des actions entreprises par les fournisseurs précédents.</span><span class="sxs-lookup"><span data-stu-id="ffe10-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="ffe10-135">Aperçu : Fonctionnalité de contrôleur générique</span><span class="sxs-lookup"><span data-stu-id="ffe10-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="ffe10-136">Par défaut, ASP.NET Core MVC ignore les contrôleurs génériques (par exemple `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="ffe10-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="ffe10-137">Cet exemple utilise un fournisseur de fonctionnalités de contrôleur qui s’exécute après le fournisseur par défaut, et qui ajoute des instances de contrôleurs génériques pour une liste spécifique de types (définis dans `EntityTypes.Types`) :</span><span class="sxs-lookup"><span data-stu-id="ffe10-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="ffe10-138">Types d’entité :</span><span class="sxs-lookup"><span data-stu-id="ffe10-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="ffe10-139">Le fournisseur de fonctionnalités est ajouté dans `Startup` :</span><span class="sxs-lookup"><span data-stu-id="ffe10-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="ffe10-140">Par défaut, les noms de contrôleurs génériques utilisés pour le routage se présentent sous la forme *GenericController\`1[Widget]* au lieu de *Widget*.</span><span class="sxs-lookup"><span data-stu-id="ffe10-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="ffe10-141">L’attribut suivant est utilisé pour modifier le nom afin qu’il corresponde au type générique utilisé par le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="ffe10-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="ffe10-142">Classe `GenericController` :</span><span class="sxs-lookup"><span data-stu-id="ffe10-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="ffe10-143">Voici le résultat, quand un routage correspondant est demandé :</span><span class="sxs-lookup"><span data-stu-id="ffe10-143">The result, when a matching route is requested:</span></span>

![L’exemple de sortie de l’exemple d’application est : « Hello from a generic Sprocket controller. »](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="ffe10-145">Aperçu : Afficher les fonctionnalités disponibles</span><span class="sxs-lookup"><span data-stu-id="ffe10-145">Sample: Display available features</span></span>

<span data-ttu-id="ffe10-146">Vous pouvez effectuer une itération parmi les fonctionnalités renseignées accessibles à votre application en demandant un `ApplicationPartManager` via une [injection de dépendances](../../fundamentals/dependency-injection.md) et en l’utilisant pour remplir les instances des fonctionnalités appropriées :</span><span class="sxs-lookup"><span data-stu-id="ffe10-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="ffe10-147">Exemple de sortie :</span><span class="sxs-lookup"><span data-stu-id="ffe10-147">Example output:</span></span>

![Exemple de sortie de l’exemple d’application](app-parts/_static/available-features.png)
