---
title: Filtres dans ASP.NET Core
author: ardalis
description: Découvrez comment les filtres fonctionnent et comment les utiliser dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 50b199744f32ad19335080da406db69665ec1ae9
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856160"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="8cc8a-103">Filtres dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8cc8a-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="8cc8a-104">Par [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8cc8a-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8cc8a-105">Les *filtres* dans ASP.NET Core permettent d’exécuter du code avant ou après des étapes spécifiques dans le pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="8cc8a-106">Les filtres intégrés gèrent notamment les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="8cc8a-107">Autorisation (empêcher un utilisateur non autorisé d’accéder à des ressources).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="8cc8a-108">Mise en cache des réponses (court-circuitage du pipeline de requêtes pour retourner une réponse mise en cache).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="8cc8a-109">Il est possible de créer des filtres personnalisés pour gérer les problèmes transversaux.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="8cc8a-110">Les exemples de problèmes transversaux incluent la gestion des erreurs, la mise en cache, la configuration, l’autorisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="8cc8a-111">Les filtres évitent la duplication de code.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="8cc8a-112">Par exemple, un filtre d’exceptions de gestion des erreurs peut servir à consolider la gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="8cc8a-113">Ce document s’applique aux Razor Pages, aux contrôleurs d’API et aux contrôleurs avec affichages.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="8cc8a-114">[Afficher ou télécharger l’échantillon](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([comment télécharger](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="8cc8a-115">Fonctionnement des filtres</span><span class="sxs-lookup"><span data-stu-id="8cc8a-115">How filters work</span></span>

<span data-ttu-id="8cc8a-116">Les filtres s’exécutent dans le *pipeline des appels d’action ASP.NET Core*, parfois appelé *pipeline de filtres*.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="8cc8a-117">Le pipeline de filtres s’exécute après la sélection par ASP.NET Core de l’action à exécuter.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La requête est traitée via un autre intergiciel, un intergiciel de routage, une sélection d’action et le pipeline d’appels d’action ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="8cc8a-120">Types de filtres</span><span class="sxs-lookup"><span data-stu-id="8cc8a-120">Filter types</span></span>

<span data-ttu-id="8cc8a-121">Chaque type de filtre est exécuté dans une étape différente du pipeline de filtres :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="8cc8a-122">Les [filtres d’autorisations](#authorization-filters) s’exécutent en premier et sont utilisés pour déterminer si l’utilisateur est autorisé pour la requête.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="8cc8a-123">Les filtres d’autorisations peuvent court-circuiter le pipeline si la requête n’est pas autorisée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="8cc8a-124">[Filtres de ressources](#resource-filters) :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="8cc8a-125">Exécuter après l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-125">Run after authorization.</span></span>  
  * <span data-ttu-id="8cc8a-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> peut exécuter du code avant le reste du pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="8cc8a-127">Par exemple, `OnResourceExecuting` peut exécuter du code avant la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="8cc8a-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> peut exécuter du code une fois que le reste du pipeline terminé.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="8cc8a-129">Les [filtres d’actions](#action-filters) peuvent exécuter du code juste avant et juste après l’appel d’une méthode d’action individuelle.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="8cc8a-130">Ils peuvent être utilisés pour manipuler les arguments passés à une action et le résultat retourné depuis l’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="8cc8a-131">Les filtres d’actions ne sont **pas** pris en charge dans les Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="8cc8a-132">Les [filtres d’exceptions](#exception-filters) sont utilisés pour appliquer des stratégies globales à des exceptions non gérées qui se produisent avant que des éléments aient été écrits dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="8cc8a-133">Les [filtres de résultats](#result-filters) peuvent exécuter du code juste avant et juste après l’exécution des résultats d’une action individuelle.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="8cc8a-134">Ils s’exécutent uniquement au terme de l’exécution de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="8cc8a-135">Ils sont utiles pour la logique qui doit entourer l’exécution de la vue ou du formateur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="8cc8a-136">Le diagramme suivant montre comment ces types de filtres interagissent dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La requête est traitée à travers les filtres d’autorisations, les filtres de ressources, la liaison de modèle, les filtres d’actions, l’exécution d’actions et la conversion des résultats d’actions, les filtres d’exceptions, les filtres de résultats et l’exécution de résultats.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="8cc8a-139">Implémentation</span><span class="sxs-lookup"><span data-stu-id="8cc8a-139">Implementation</span></span>

<span data-ttu-id="8cc8a-140">Les filtres prennent en charge les implémentations synchrones et asynchrones via différentes définitions d’interface.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="8cc8a-141">Les filtres synchrones peuvent exécuter du code avant (`On-Stage-Executing`) et après (`On-Stage-Executed`) leur étape de pipeline.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="8cc8a-142">Par exemple, `OnActionExecuting` est appelé avant l’appel de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="8cc8a-143">`OnActionExecuted` est appelé après le retour de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="8cc8a-144">Les filtres asynchrones définissent une méthode `On-Stage-ExecutionAsync` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="8cc8a-145">Dans le code précédent, le `SampleAsyncActionFilter` a un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) qui exécute la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="8cc8a-146">Chacune des méthodes `On-Stage-ExecutionAsync` prennent un `FilterType-ExecutionDelegate` qui exécute l’étape de pipeline du filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="8cc8a-147">Plusieurs étapes de filtres</span><span class="sxs-lookup"><span data-stu-id="8cc8a-147">Multiple filter stages</span></span>

<span data-ttu-id="8cc8a-148">Vous pouvez implémenter des interfaces pour plusieurs étapes de filtres dans une même classe.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="8cc8a-149">Par exemple, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implémente `IActionFilter`, `IResultFilter` et leurs équivalents asynchrones.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="8cc8a-150">Implémentez la version synchrone **ou bien** la version asynchrone d’une interface de filtre, mais **pas** les deux.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="8cc8a-151">Le runtime vérifie d’abord si le filtre implémente l’interface asynchrone et, le cas échéant, il appelle cette interface.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="8cc8a-152">Dans le cas contraire, il appelle la ou les méthodes de l’interface synchrone.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="8cc8a-153">Si des interfaces synchrones et asynchrones sont implémentées dans une classe, seule la méthode asynchrone est appelée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="8cc8a-154">Quand vous utilisez des classes abstraites comme <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, vous devez remplacer seulement les méthodes synchrones ou la méthode asynchrone pour chaque type de filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="8cc8a-155">Attributs de filtre intégrés</span><span class="sxs-lookup"><span data-stu-id="8cc8a-155">Built-in filter attributes</span></span>

<span data-ttu-id="8cc8a-156">ASP.NET Core inclut les filtres intégrés basés sur des attributs que vous pouvez définir dans une sous-classe et personnaliser.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="8cc8a-157">Par exemple, le filtre de résultats suivant ajoute un en-tête à la réponse :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="8cc8a-158">Les attributs autorisent les filtres à accepter des arguments, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="8cc8a-159">Appliquez le `AddHeaderAttribute` à un contrôleur ou une méthode d’action et spécifiez le nom et la valeur de l’en-tête HTTP :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="8cc8a-160">Plusieurs des interfaces de filtre ont des attributs correspondants qui peuvent être utilisés comme classes de base pour des implémentations personnalisées.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="8cc8a-161">Les attributs de filtre :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="8cc8a-162">Étendues de filtre et ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="8cc8a-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="8cc8a-163">Un filtre peut être ajouté au pipeline à une des trois *étendues* :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="8cc8a-164">À l’aide d’un attribut sur une action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="8cc8a-165">À l’aide d’un attribut sur un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="8cc8a-166">Dans le monde entier pour tous les contrôleurs et actions, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="8cc8a-167">Le code précédent ajoute trois filtres globalement à l’aide de la collection [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="8cc8a-168">Ordre d’exécution par défaut</span><span class="sxs-lookup"><span data-stu-id="8cc8a-168">Default order of execution</span></span>

<span data-ttu-id="8cc8a-169">Quand il existe plusieurs filtres pour une étape particulière du pipeline, l’étendue détermine l’ordre par défaut de l’exécution du filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="8cc8a-170">Les filtres globaux entourent les filtres de classe, qui à leur tour entourent les filtres de méthode.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="8cc8a-171">En raison de l’imbrication de filtres, le code *après* des filtres s’exécute dans l’ordre inverse du code *avant*.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="8cc8a-172">La séquence de filtre :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-172">The filter sequence:</span></span>

* <span data-ttu-id="8cc8a-173">Le code *avant* des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="8cc8a-174">Le code *avant* des filtres du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="8cc8a-175">Le code *avant* des filtres de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="8cc8a-176">Le code *après* des filtres de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="8cc8a-177">Le code *après* des filtres du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="8cc8a-178">Le code *après* des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="8cc8a-179">Voici un exemple qui illustre l’ordre dans lequel les méthodes de filtre sont appelées pour les filtres d’actions synchrones.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="8cc8a-180">Séquence</span><span class="sxs-lookup"><span data-stu-id="8cc8a-180">Sequence</span></span> | <span data-ttu-id="8cc8a-181">Étendue de filtre</span><span class="sxs-lookup"><span data-stu-id="8cc8a-181">Filter scope</span></span> | <span data-ttu-id="8cc8a-182">Méthode de filtre</span><span class="sxs-lookup"><span data-stu-id="8cc8a-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="8cc8a-183">1</span><span class="sxs-lookup"><span data-stu-id="8cc8a-183">1</span></span> | <span data-ttu-id="8cc8a-184">Global</span><span class="sxs-lookup"><span data-stu-id="8cc8a-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="8cc8a-185">2</span><span class="sxs-lookup"><span data-stu-id="8cc8a-185">2</span></span> | <span data-ttu-id="8cc8a-186">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="8cc8a-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="8cc8a-187">3</span><span class="sxs-lookup"><span data-stu-id="8cc8a-187">3</span></span> | <span data-ttu-id="8cc8a-188">Méthode</span><span class="sxs-lookup"><span data-stu-id="8cc8a-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="8cc8a-189">4</span><span class="sxs-lookup"><span data-stu-id="8cc8a-189">4</span></span> | <span data-ttu-id="8cc8a-190">Méthode</span><span class="sxs-lookup"><span data-stu-id="8cc8a-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="8cc8a-191">5</span><span class="sxs-lookup"><span data-stu-id="8cc8a-191">5</span></span> | <span data-ttu-id="8cc8a-192">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="8cc8a-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="8cc8a-193">6</span><span class="sxs-lookup"><span data-stu-id="8cc8a-193">6</span></span> | <span data-ttu-id="8cc8a-194">Global</span><span class="sxs-lookup"><span data-stu-id="8cc8a-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="8cc8a-195">Cette séquence montre que :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-195">This sequence shows:</span></span>

* <span data-ttu-id="8cc8a-196">Le filtre de méthode est imbriqué dans le filtre de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="8cc8a-197">Le filtre de contrôleur est imbriqué dans le filtre global.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="8cc8a-198">Filtres au niveau de contrôleur et de Razor Page</span><span class="sxs-lookup"><span data-stu-id="8cc8a-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="8cc8a-199">Chaque contrôleur qui hérite de la classe de base <xref:Microsoft.AspNetCore.Mvc.Controller> inclut les méthodes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) et [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="8cc8a-200">Ces méthodes :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-200">These methods:</span></span>

* <span data-ttu-id="8cc8a-201">Incluent dans un wrapper les filtres qui s’exécutent pour une action donnée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="8cc8a-202">`OnActionExecuting` est appelé avant tous les filtres de l’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="8cc8a-203">`OnActionExecuted` est appelé après tous les filtres d’actions.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="8cc8a-204">`OnActionExecutionAsync` est appelé avant tous les filtres de l’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="8cc8a-205">Le code dans le filtre après `next` s’exécute après la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="8cc8a-206">Par exemple, dans l’échantillon à télécharger, `MySampleActionFilter` est appliqué globalement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="8cc8a-207">Voici le `TestController` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-207">The `TestController`:</span></span>

* <span data-ttu-id="8cc8a-208">Applique le `SampleActionFilterAttribute` (`[SampleActionFilter]`) pour l’action `FilterTest2` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="8cc8a-209">Remplace `OnActionExecuting` et `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="8cc8a-210">La navigation vers `https://localhost:5001/Test/FilterTest2` exécute le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="8cc8a-211">Pour Razor Pages, consultez [Implémenter des filtres Razor Page en remplaçant les méthodes de filtre](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="8cc8a-212">Remplacement de l’ordre par défaut</span><span class="sxs-lookup"><span data-stu-id="8cc8a-212">Overriding the default order</span></span>

<span data-ttu-id="8cc8a-213">Vous pouvez remplacer la séquence d’exécution par défaut en implémentant <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="8cc8a-214">`IOrderedFilter` expose la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> qui est prioritaire sur l’étendue afin de déterminer l’ordre d’exécution.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="8cc8a-215">Un filtre avec une valeur `Order` inférieure :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="8cc8a-216">Exécute le code *avant* avant celui d’un filtre avec une valeur supérieure à `Order`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="8cc8a-217">Exécute le code *après* après celui d’un filtre avec une valeur `Order` supérieure.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="8cc8a-218">La propriété `Order` peut être définie avec un paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="8cc8a-219">Prenez en compte les mêmes 3 filtres d’actions indiqués dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="8cc8a-220">Si la propriété `Order` du contrôleur et les filtres globaux sont définis sur 1 et 2 respectivement, l’ordre d’exécution est inversé.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="8cc8a-221">Séquence</span><span class="sxs-lookup"><span data-stu-id="8cc8a-221">Sequence</span></span> | <span data-ttu-id="8cc8a-222">Étendue de filtre</span><span class="sxs-lookup"><span data-stu-id="8cc8a-222">Filter scope</span></span> | <span data-ttu-id="8cc8a-223">Propriété`Order`</span><span class="sxs-lookup"><span data-stu-id="8cc8a-223">`Order` property</span></span> | <span data-ttu-id="8cc8a-224">Méthode de filtre</span><span class="sxs-lookup"><span data-stu-id="8cc8a-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="8cc8a-225">1</span><span class="sxs-lookup"><span data-stu-id="8cc8a-225">1</span></span> | <span data-ttu-id="8cc8a-226">Méthode</span><span class="sxs-lookup"><span data-stu-id="8cc8a-226">Method</span></span> | <span data-ttu-id="8cc8a-227">0</span><span class="sxs-lookup"><span data-stu-id="8cc8a-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="8cc8a-228">2</span><span class="sxs-lookup"><span data-stu-id="8cc8a-228">2</span></span> | <span data-ttu-id="8cc8a-229">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="8cc8a-229">Controller</span></span> | <span data-ttu-id="8cc8a-230">1</span><span class="sxs-lookup"><span data-stu-id="8cc8a-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="8cc8a-231">3</span><span class="sxs-lookup"><span data-stu-id="8cc8a-231">3</span></span> | <span data-ttu-id="8cc8a-232">Global</span><span class="sxs-lookup"><span data-stu-id="8cc8a-232">Global</span></span> | <span data-ttu-id="8cc8a-233">2</span><span class="sxs-lookup"><span data-stu-id="8cc8a-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="8cc8a-234">4</span><span class="sxs-lookup"><span data-stu-id="8cc8a-234">4</span></span> | <span data-ttu-id="8cc8a-235">Global</span><span class="sxs-lookup"><span data-stu-id="8cc8a-235">Global</span></span> | <span data-ttu-id="8cc8a-236">2</span><span class="sxs-lookup"><span data-stu-id="8cc8a-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="8cc8a-237">5</span><span class="sxs-lookup"><span data-stu-id="8cc8a-237">5</span></span> | <span data-ttu-id="8cc8a-238">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="8cc8a-238">Controller</span></span> | <span data-ttu-id="8cc8a-239">1</span><span class="sxs-lookup"><span data-stu-id="8cc8a-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="8cc8a-240">6</span><span class="sxs-lookup"><span data-stu-id="8cc8a-240">6</span></span> | <span data-ttu-id="8cc8a-241">Méthode</span><span class="sxs-lookup"><span data-stu-id="8cc8a-241">Method</span></span> | <span data-ttu-id="8cc8a-242">0</span><span class="sxs-lookup"><span data-stu-id="8cc8a-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="8cc8a-243">La propriété `Order` remplace l’étendue lors de la détermination de l’ordre dans lequel les filtres s’exécutent.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="8cc8a-244">Les filtres sont d’abord classés par ordre, puis l’étendue est utilisée pour couper les liens.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="8cc8a-245">Tous les filtres intégrés implémentent `IOrderedFilter` et affectent 0 à la valeur `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="8cc8a-246">Pour les filtres intégrés, l’étendue détermine l’ordre, sauf si `Order` est défini sur une valeur différente de zéro.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="8cc8a-247">Annulation et court-circuit</span><span class="sxs-lookup"><span data-stu-id="8cc8a-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="8cc8a-248">Vous pouvez court-circuiter le pipeline de filtres en définissant la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sur le paramètre <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fourni à la méthode de filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="8cc8a-249">Par exemple, le filtre de ressources suivant empêche l’exécution du reste du pipeline :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="8cc8a-250">Dans le code suivant, les filtres `ShortCircuitingResourceFilter` et `AddHeader` ciblent tous deux la méthode d’action `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="8cc8a-251">Voici le `ShortCircuitingResourceFilter` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="8cc8a-252">S’exécute en premier (puisqu’il s’agit d’un filtre de ressources et que `AddHeader` est un filtre d’action).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="8cc8a-253">Court-circuite le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="8cc8a-254">Le filtre `AddHeader` ne s’exécute donc jamais pour l’action `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="8cc8a-255">Ce comportement est le même si les deux filtres sont appliqués au niveau de la méthode d’action, à condition que `ShortCircuitingResourceFilter` soit exécuté en premier.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="8cc8a-256">`ShortCircuitingResourceFilter` s’exécute en premier en raison de son type de filtre ou de l’utilisation explicite de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="8cc8a-257">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="8cc8a-257">Dependency injection</span></span>

<span data-ttu-id="8cc8a-258">Les filtres peuvent être ajoutés par type ou par instance.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="8cc8a-259">Si vous ajoutez une instance, celle-ci sera utilisée pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="8cc8a-260">Si un type est ajouté, il est activé par type.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="8cc8a-261">Un filtre activé par type signifie :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-261">A type-activated filter means:</span></span>

* <span data-ttu-id="8cc8a-262">Une instance est créée pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-262">An instance is created for each request.</span></span>
* <span data-ttu-id="8cc8a-263">Les dépendances de constructeur sont remplies par [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="8cc8a-264">Les filtres qui sont implémentés en tant qu’attributs et ajoutés directement à des classes de contrôleur ou à de méthodes d’action ne peut pas avoir de dépendances de constructeur fournies par [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="8cc8a-265">Les dépendances de constructeur ne peuvent pas être fournies par l’injection de dépendance, car :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="8cc8a-266">Les paramètres du constructeur des attributs doivent être fournis là où ils sont appliqués.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="8cc8a-267">Il s’agit d’une limitation de la façon dont les attributs fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="8cc8a-268">Les filtres suivants prennent en charge les dépendances de constructeur fournies à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="8cc8a-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implémenté sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="8cc8a-270">Les filtres précédents peuvent être appliqués à une méthode de contrôleur ou d’action :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="8cc8a-271">Des enregistreurs d’événements sont disponibles à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-271">Loggers are available from DI.</span></span> <span data-ttu-id="8cc8a-272">Toutefois, évitez la création et l’utilisation de filtres à des fins purement d’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="8cc8a-273">L’[enregistrement d’infrastructure intégrée](xref:fundamentals/logging/index) fournit généralement ce qui est nécessaire pour l’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="8cc8a-274">Enregistrement ajouté aux filtres :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-274">Logging added to filters:</span></span>

* <span data-ttu-id="8cc8a-275">Doit se concentrer sur les problèmes ou le comportement du domaine métier spécifique au filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="8cc8a-276">Ne doit **pas** enregistrer des actions ou d’autres événements d’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="8cc8a-277">Les filtres intégrés enregistrent des actions et des événements d’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="8cc8a-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="8cc8a-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="8cc8a-279">Les types d’implémentation du filtre de services sont enregistrés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="8cc8a-280">Un <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> récupère une instance du filtre à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="8cc8a-281">Le code suivant affiche le `AddHeaderResultServiceFilter` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="8cc8a-282">Dans le code suivant, `AddHeaderResultServiceFilter` est ajouté au conteneur d’injection de dépendance :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="8cc8a-283">Dans le code suivant, l’attribut `ServiceFilter` récupère une instance du filtre `AddHeaderResultServiceFilter` à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="8cc8a-284">Lors de l’utilisation de `ServiceFilterAttribute`, définir [ServiceFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable) :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="8cc8a-285">Conseille que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle elle a été créée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="8cc8a-286">Le runtime ASP.NET Core ne garantit pas :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="8cc8a-287">Qu’une seule instance du filtre sera créée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="8cc8a-288">Le filtre ne sera pas demandé à nouveau à partir du conteneur d’injection de dépendance à un stade ultérieur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="8cc8a-289">Évitez de l’utiliser avec un filtre qui dépend de services avec une durée de vie autre que singleton.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="8cc8a-290">L'objet <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="8cc8a-291">`IFilterFactory` expose la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> pour la création d’une instance <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="8cc8a-292">`CreateInstance` charge le type spécifié à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="8cc8a-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="8cc8a-293">TypeFilterAttribute</span></span>

<span data-ttu-id="8cc8a-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> est similaire à <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, mais son type n’est pas résolu directement à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="8cc8a-295">Il instancie le type en utilisant <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="8cc8a-296">Étant donné que les types `TypeFilterAttribute` ne sont pas résolus directement à partir du conteneur d’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="8cc8a-297">Les types qui sont référencés avec `TypeFilterAttribute` ne doivent pas d’abord être inscrits auprès du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="8cc8a-298">Leurs dépendances ne sont pas remplies par le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="8cc8a-299">`TypeFilterAttribute` peut éventuellement accepter des arguments de constructeur pour le type.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="8cc8a-300">Lors de l’utilisation de `TypeFilterAttribute`, définir [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable) :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="8cc8a-301">Indique que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle elle a été créée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="8cc8a-302">Le runtime ASP.NET Core ne fournit aucune garantie qu’une seule instance du filtre sera créée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="8cc8a-303">Évitez de l’utiliser avec un filtre qui dépend de services avec une durée de vie autre que singleton.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="8cc8a-304">L’exemple suivant indique comment passer des arguments vers un type en utilisant `TypeFilterAttribute` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="8cc8a-305">Filtres d’autorisations</span><span class="sxs-lookup"><span data-stu-id="8cc8a-305">Authorization filters</span></span>

<span data-ttu-id="8cc8a-306">Filtres d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-306">Authorization filters:</span></span>

* <span data-ttu-id="8cc8a-307">Sont les premiers filtres à être exécutés dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="8cc8a-308">Contrôlent l’accès aux méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-308">Control access to action methods.</span></span>
* <span data-ttu-id="8cc8a-309">Ont une méthode avant, mais pas de méthode après.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="8cc8a-310">Les filtres d’autorisations personnalisés nécessitent une infrastructure d’autorisation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="8cc8a-311">Préférez la configuration des stratégies d’autorisation ou l’écriture d’une stratégie d’autorisation personnalisée à l’écriture d’un filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="8cc8a-312">Le filtre d'autorisations intégré :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="8cc8a-313">Appelle le système d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-313">Calls the authorization system.</span></span>
* <span data-ttu-id="8cc8a-314">N’autoriser les requêtes.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-314">Does not authorize requests.</span></span>

<span data-ttu-id="8cc8a-315">Ne levez **pas** d’exceptions dans les filtres d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="8cc8a-316">L’exception ne sera pas gérée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-316">The exception will not be handled.</span></span>
* <span data-ttu-id="8cc8a-317">Les filtres d’exceptions ne gèreront pas l’exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="8cc8a-318">Songez à émettre une stimulation quand un filtre d’autorisations se produit.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="8cc8a-319">Découvrez plus d’informations sur [l’autorisation](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="8cc8a-320">Filtres de ressources</span><span class="sxs-lookup"><span data-stu-id="8cc8a-320">Resource filters</span></span>

<span data-ttu-id="8cc8a-321">Filtres de ressources :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-321">Resource filters:</span></span>

* <span data-ttu-id="8cc8a-322">Implémentez l’interface <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="8cc8a-323">L’exécution inclut dans un wrapper la majeure partie du pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="8cc8a-324">Seuls les [filtres d’autorisations](#authorization-filters) s’exécutent avant les filtres de ressources.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="8cc8a-325">Les filtres de ressources sont utiles pour court-circuiter la majeure partie du pipeline.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="8cc8a-326">Par exemple, un filtre de mise en cache peut éviter le reste du pipeline dans une correspondance dans le cache.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="8cc8a-327">Exemples de filtre de ressources :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-327">Resource filter examples:</span></span>

* <span data-ttu-id="8cc8a-328">[Le filtre de ressources de court-circuit](#short-circuiting-resource-filter) illustré précédemment.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="8cc8a-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="8cc8a-330">Il empêche la liaison de données d’accéder aux données de formulaire.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="8cc8a-331">Il est utilisé pour les chargements de fichiers volumineux et pour empêcher que le formulaire de données ne soit lu en mémoire.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="8cc8a-332">Filtres d’actions</span><span class="sxs-lookup"><span data-stu-id="8cc8a-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8cc8a-333">Les filtres d’action ne s’appliquent **pas** aux Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="8cc8a-334">Razor Pages prennent en charge <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span><span class="sxs-lookup"><span data-stu-id="8cc8a-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="8cc8a-335">Pour plus d’informations, consultez [Méthodes de filtre pour les pages Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="8cc8a-336">Filtres d'actions :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-336">Action filters:</span></span>

* <span data-ttu-id="8cc8a-337">Implémentez l’interface <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="8cc8a-338">Leur exécution entoure l’exécution de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="8cc8a-339">Le code suivant montre un exemple de filtre d’actions :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="8cc8a-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> fournit les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="8cc8a-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> : permet de lire les entrées vers une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="8cc8a-342"><xref:Microsoft.AspNetCore.Mvc.Controller> : permet de manipuler l’instance de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="8cc8a-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> : la définition de `Result` court-circuite l’exécution de la méthode d’action et les filtres d’actions suivants.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="8cc8a-344">Levée d’une exception dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="8cc8a-345">Empêche l’exécution des filtres suivants.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="8cc8a-346">Contrairement au paramètre `Result`, est traité comme un échec plutôt que comme un résultat positif.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="8cc8a-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> fournit `Controller` et `Result`, en plus des propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="8cc8a-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> : sa valeur est true si l’exécution de l’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="8cc8a-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> : sa valeur est non Null si l’action ou un filtre d’actions précédemment exécuté a lancé une exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="8cc8a-350">Paramètre de cette propriété sur null :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-350">Setting this property to null:</span></span>

  * <span data-ttu-id="8cc8a-351">Gère effectivement une exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="8cc8a-352">`Result` est exécuté comme s’il avait été retourné à partir de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="8cc8a-353">Pour un `IAsyncActionFilter`, un appel à <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="8cc8a-354">Exécute tous les filtres d’actions suivants et la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="8cc8a-355">Retourne `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="8cc8a-356">Pour court-circuiter, attribuez <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> à une instance de résultat et n’appelez pas le `next` (le `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="8cc8a-357">L’infrastructure fournit un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> abstrait que vous pouvez placer dans une sous-classe.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="8cc8a-358">Le filtre d’actions `OnActionExecuting` peut être utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="8cc8a-359">Valider l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-359">Validate model state.</span></span>
* <span data-ttu-id="8cc8a-360">Renvoyer une erreur si l’état n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="8cc8a-361">La méthode `OnActionExecuted` s’exécute après la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="8cc8a-362">Et peut voir et manipuler les résultats de l’action via la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="8cc8a-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> est défini sur true si l’exécution de l’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="8cc8a-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> est défini sur une valeur non Null si l’action ou un filtre d’actions suivant a levé une exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="8cc8a-365">Le fait de définir `Exception` avec la valeur null :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="8cc8a-366">Gère effectivement une exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="8cc8a-367">`ActionExecutedContext.Result` est exécuté comme s’il avait été retourné normalement à partir de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="8cc8a-368">Filtres d’exceptions</span><span class="sxs-lookup"><span data-stu-id="8cc8a-368">Exception filters</span></span>

<span data-ttu-id="8cc8a-369">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-369">Exception filters:</span></span>

* <span data-ttu-id="8cc8a-370">Implémentent <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="8cc8a-371">Ils peuvent être utilisés pour implémenter des stratégies de gestion des erreurs communes.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="8cc8a-372">L’échantillon de filtre d’exceptions suivant utilise un affichage personnalisé des erreurs pour afficher des détails sur les exceptions qui se produisent pendant que l’application est en développement :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="8cc8a-373">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-373">Exception filters:</span></span>

* <span data-ttu-id="8cc8a-374">N’ont pas d’événements avant et après.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-374">Don't have before and after events.</span></span>
* <span data-ttu-id="8cc8a-375">Implémentent <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="8cc8a-376">Gèrent des exceptions non prises en charge qui se produisent dans Razor Page ou dans la création des contrôleurs, la [liaison de données](xref:mvc/models/model-binding), les filtres d’actions ou les méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="8cc8a-377">N’interceptent **pas** les exceptions qui se produisent dans l’exécution des filtres de ressources, des filtres de résultats ou des résultats MVC.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="8cc8a-378">Pour gérer une exception, définissez la propriété <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> sur `true` ou écrivez une réponse.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="8cc8a-379">Ceci arrête la propagation de l’exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-379">This stops propagation of the exception.</span></span> <span data-ttu-id="8cc8a-380">Un filtre d’exceptions ne peut pas changer une exception en « réussite ».</span><span class="sxs-lookup"><span data-stu-id="8cc8a-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="8cc8a-381">Seul un filtre d’actions peut le faire.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-381">Only an action filter can do that.</span></span>

<span data-ttu-id="8cc8a-382">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-382">Exception filters:</span></span>

* <span data-ttu-id="8cc8a-383">Conviennent pour intercepter des exceptions qui se produisent dans des actions.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="8cc8a-384">N’offrent pas la même souplesse que le middleware (intergiciel) de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="8cc8a-385">Privilégiez le middleware pour la gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="8cc8a-386">Utilisez les filtres d’exceptions uniquement lorsque la gestion des erreurs *diffère* selon la méthode d’action appelée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="8cc8a-387">Par exemple, votre application peut avoir des méthodes d’action à la fois pour des points de terminaison d’API et pour des affichages/HTML.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="8cc8a-388">Les points de terminaison d’API peuvent retourner des informations d’erreur au format JSON, tandis que les actions basées sur une vue peuvent retourner une page d’erreur au format HTML.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="8cc8a-389">Filtres de résultats</span><span class="sxs-lookup"><span data-stu-id="8cc8a-389">Result filters</span></span>

<span data-ttu-id="8cc8a-390">Filtres de résultats :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-390">Result filters:</span></span>

* <span data-ttu-id="8cc8a-391">Implémenter une interface :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-391">Implement an interface:</span></span>
  * <span data-ttu-id="8cc8a-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="8cc8a-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="8cc8a-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="8cc8a-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="8cc8a-394">Leur exécution entoure l’exécution de résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="8cc8a-395">IResultFilter et IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="8cc8a-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="8cc8a-396">Le code suivant indique un filtre de résultats qui ajoute un en-tête HTTP :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="8cc8a-397">Le type de résultat à exécuter dépend de l’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="8cc8a-398">Une action retournant un affichage inclut tous les traitements Razor dans le cadre de la <xref:Microsoft.AspNetCore.Mvc.ViewResult> à exécuter.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="8cc8a-399">Une méthode d’API peut effectuer une sérialisation dans le cadre de l’exécution du résultat.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="8cc8a-400">Découvrez plus d’informations sur les [résultats d’actions](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-400">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="8cc8a-401">Les filtres de résultats sont exécutés seulement pour les résultats qui sont des réussites, quand l’action ou les filtres d’actions produisent un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-401">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="8cc8a-402">Les filtres de résultats ne sont pas exécutés quand les filtres d’exceptions gèrent une exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-402">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="8cc8a-403">La méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> peut court-circuiter l’exécution du résultat d’action et les filtres de résultats suivants en définissant <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-403">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="8cc8a-404">Écrivez dans l’objet de réponse quand vous court-circuitez pour éviter de générer une réponse vide.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-404">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="8cc8a-405">La levée d’une exception dans `IResultFilter.OnResultExecuting` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-405">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="8cc8a-406">Empêche l’exécution du résultat d’action et des filtres suivants.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-406">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="8cc8a-407">Est traitée comme une erreur et non comme une réussite.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-407">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="8cc8a-408">Lorsque la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> s’exécute :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-408">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="8cc8a-409">La réponse a probablement déjà été envoyée au client et ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-409">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="8cc8a-410">Si une exception a été levée, le corps de la réponse n’est pas envoyé.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-410">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="8cc8a-411">`ResultExecutedContext.Canceled` est défini sur `true` si l’exécution du résultat d’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-411">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="8cc8a-412">`ResultExecutedContext.Exception` est défini sur une valeur non Null si le résultat d’action ou un filtre de résultats suivant a levé une exception.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-412">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="8cc8a-413">Le paramètre de `Exception` sur Null gère effectivement une exception et évite à l’exception d’être à nouveau levée par ASP.NET Core plus loin dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-413">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="8cc8a-414">Il n’existe aucun moyen fiable pour écrire des données dans une réponse lors de la gestion d’une exception dans un filtre de résultats.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-414">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="8cc8a-415">Si les en-têtes ont été vidés pour le client lorsqu’un résultat d’action lance une exception, il n’existe aucun mécanisme fiable pour envoyer un code d’échec.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-415">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="8cc8a-416">Pour un <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, un appel à `await next` sur le <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> exécute tous les filtres de résultats suivants et le résultat de l’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-416">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="8cc8a-417">Pour court-circuiter, définissez [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) sur `true` et n’appelez pas `ResultExecutionDelegate` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-417">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="8cc8a-418">L’infrastructure fournit un `ResultFilterAttribute` abstrait que vous pouvez placer dans une sous-classe.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-418">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="8cc8a-419">La classe [AddHeaderAttribute](#add-header-attribute) ci-dessus est un exemple d’un attribut de filtre de résultats.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-419">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="8cc8a-420">IAlwaysRunResultFilter et IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="8cc8a-420">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="8cc8a-421">Les interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> déclarent une implémentation <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> qui s’exécute pour tous les résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-421">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="8cc8a-422">Le filtre est appliqué à tous les résultats d’action, sauf si :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-422">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="8cc8a-423">Un <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> s’applique et court-circuite la réponse.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-423">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="8cc8a-424">Un filtre d’exception gère une exception en générant un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-424">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="8cc8a-425">Les filtres autres que `IExceptionFilter` et `IAuthorizationFilter` ne court-circuitent pas `IAlwaysRunResultFilter` et `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-425">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="8cc8a-426">Par exemple, le filtre suivant exécute et définit toujours un résultat d’action (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) avec un code d’état *422 Entité non traitée* en cas d’échec de la négociation de contenu :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="8cc8a-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="8cc8a-427">IFilterFactory</span></span>

<span data-ttu-id="8cc8a-428">L'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="8cc8a-429">Par conséquent, une instance de `IFilterFactory` peut être utilisée comme instance de `IFilterMetadata` n’importe où dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="8cc8a-430">Quan se prépare à appeler le filtre, il tente de le caster en `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="8cc8a-431">Si ce cast réussit, la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> est appelée pour créer l’instance `IFilterMetadata` qui sera appelée.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="8cc8a-432">La conception est flexible, car il n’est pas nécessaire de définir explicitement le pipeline de filtres exact quand l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="8cc8a-433">Une autre approche pour la création de filtres est d’implémenter `IFilterFactory` à l’aide des implémentations d’attribut personnalisé :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="8cc8a-434">Le code précédent peut être testé en exécutant l’[échantillon de téléchargement](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="8cc8a-435">Appeler les outils de développement F12.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="8cc8a-436">Accédez à `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="8cc8a-437">Les outils de développement F12 affichent les en-têtes de réponse suivants ajoutés par l’exemple de code :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="8cc8a-438">**auteur :** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="8cc8a-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="8cc8a-439">**globaladdheader :** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="8cc8a-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="8cc8a-440">**interne :** `My header`</span><span class="sxs-lookup"><span data-stu-id="8cc8a-440">**internal:** `My header`</span></span>

<span data-ttu-id="8cc8a-441">Le code précédent crée l’en-tête de réponse **interne :** `My header`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="8cc8a-442">IFilterFactory implémenté sur un attribut</span><span class="sxs-lookup"><span data-stu-id="8cc8a-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="8cc8a-443">Les filtres qui implémentent `IFilterFactory` sont utiles pour les filtres qui :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="8cc8a-444">Ne nécessitent pas le passage de paramètres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="8cc8a-445">Disposent de dépendances de constructeur qui doivent être remplies par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="8cc8a-446">L'objet <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="8cc8a-447">`IFilterFactory` expose la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> pour la création d’une instance <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="8cc8a-448">`CreateInstance` charge le type spécifié à partir du conteneur de services (injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="8cc8a-449">Le code suivant illustre trois approches pour appliquer `[SampleActionFilter]` :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="8cc8a-450">Dans le code précédent, la décoration de la méthode avec `[SampleActionFilter]` est la meilleure approche pour appliquer `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="8cc8a-451">Utilisation d’un intergiciel dans le pipeline de filtres</span><span class="sxs-lookup"><span data-stu-id="8cc8a-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="8cc8a-452">Les filtres de ressources fonctionnent comme un [intergiciel](xref:fundamentals/middleware/index) dans la mesure où ils entourent l’exécution de tout ce qui vient plus tard dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="8cc8a-453">Les filtres diffèrent cependant d’un intergiciel dans la mesure où ils font partie du runtime ASP.NET Core, ce qui signifie qu’ils ont accès au contexte et aux constructions ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="8cc8a-454">Pour utiliser un intergiciel comme filtre, créez un type avec une méthode `Configure` qui spécifie l’intergiciel que vous voulez injecter dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="8cc8a-455">Voici un exemple qui utilise l’intergiciel de localisation pour établir la culture actuelle d’une requête :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="8cc8a-456">Utilisez le <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> pour exécuter l’intergiciel :</span><span class="sxs-lookup"><span data-stu-id="8cc8a-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="8cc8a-457">Les filtres d’intergiciels s’exécutent à la même étape du pipeline de filtres en tant que filtres de ressources, avant la liaison de modèle et après le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="8cc8a-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="8cc8a-458">Actions suivantes</span><span class="sxs-lookup"><span data-stu-id="8cc8a-458">Next actions</span></span>

* <span data-ttu-id="8cc8a-459">Consultez [Méthodes de filtre pour les Razor Pages](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="8cc8a-459">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="8cc8a-460">Pour expérimenter les filtres, [téléchargez, testez et modifiez l’échantillon Github](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="8cc8a-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
