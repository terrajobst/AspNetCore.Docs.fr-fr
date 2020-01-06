---
title: Filtres dans ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les filtres fonctionnent et comment les utiliser dans ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 2300b14a6a89191d3d8c673311880fc144183da9
ms.sourcegitcommit: e7d4fe6727d423f905faaeaa312f6c25ef844047
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75608121"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="e4b42-103">Filtres dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4b42-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e4b42-104">Par [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e4b42-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e4b42-105">Les *filtres* dans ASP.NET Core permettent d’exécuter du code avant ou après des étapes spécifiques dans le pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="e4b42-106">Les filtres intégrés gèrent notamment les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="e4b42-107">Autorisation (empêcher un utilisateur non autorisé d’accéder à des ressources).</span><span class="sxs-lookup"><span data-stu-id="e4b42-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="e4b42-108">Mise en cache des réponses (court-circuitage du pipeline de requêtes pour retourner une réponse mise en cache).</span><span class="sxs-lookup"><span data-stu-id="e4b42-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="e4b42-109">Il est possible de créer des filtres personnalisés pour gérer les problèmes transversaux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="e4b42-110">Les exemples de problèmes transversaux incluent la gestion des erreurs, la mise en cache, la configuration, l’autorisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="e4b42-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="e4b42-111">Les filtres évitent la duplication de code.</span><span class="sxs-lookup"><span data-stu-id="e4b42-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="e4b42-112">Par exemple, un filtre d’exceptions de gestion des erreurs peut servir à consolider la gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="e4b42-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="e4b42-113">Ce document s’applique aux Razor Pages, aux contrôleurs d’API et aux contrôleurs avec affichages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="e4b42-114">[Afficher ou télécharger l’échantillon](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([comment télécharger](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e4b42-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="e4b42-115">Fonctionnement des filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-115">How filters work</span></span>

<span data-ttu-id="e4b42-116">Les filtres s’exécutent dans le *pipeline des appels d’action ASP.NET Core*, parfois appelé *pipeline de filtres*.</span><span class="sxs-lookup"><span data-stu-id="e4b42-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="e4b42-117">Le pipeline de filtres s’exécute après la sélection par ASP.NET Core de l’action à exécuter.</span><span class="sxs-lookup"><span data-stu-id="e4b42-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La demande est traitée par un autre intergiciel, un intergiciel (middleware) de routage, une sélection d’action et le pipeline d’appel d’action.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="e4b42-120">Types de filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-120">Filter types</span></span>

<span data-ttu-id="e4b42-121">Chaque type de filtre est exécuté dans une étape différente du pipeline de filtres :</span><span class="sxs-lookup"><span data-stu-id="e4b42-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="e4b42-122">Les [filtres d’autorisations](#authorization-filters) s’exécutent en premier et sont utilisés pour déterminer si l’utilisateur est autorisé pour la requête.</span><span class="sxs-lookup"><span data-stu-id="e4b42-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="e4b42-123">Filtres d’autorisation court-circuiter le pipeline si la demande n’est pas autorisée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="e4b42-124">[Filtres de ressources](#resource-filters) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="e4b42-125">Exécuter après l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e4b42-125">Run after authorization.</span></span>  
  * <span data-ttu-id="e4b42-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> exécute le code avant le reste du pipeline de filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="e4b42-127">Par exemple, `OnResourceExecuting` exécute le code avant la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="e4b42-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="e4b42-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> exécute le code une fois que le reste du pipeline est terminé.</span><span class="sxs-lookup"><span data-stu-id="e4b42-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="e4b42-129">[Filtres d’actions](#action-filters) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="e4b42-130">Exécutez le code juste avant et après l’appel d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="e4b42-131">Peut modifier les arguments passés dans une action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="e4b42-132">Peut modifier le résultat retourné par l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="e4b42-133">Ne sont **pas** pris en charge dans Razor pages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="e4b42-134">Les [filtres d’exception](#exception-filters) appliquent des stratégies globales aux exceptions non gérées qui se produisent avant l’écriture du corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4b42-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="e4b42-135">Les [filtres de résultats](#result-filters) exécutent le code immédiatement avant et après l’exécution des résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="e4b42-136">Ils s’exécutent uniquement au terme de l’exécution de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="e4b42-137">Ils sont utiles pour la logique qui doit entourer l’exécution de la vue ou du formateur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="e4b42-138">Le diagramme suivant montre comment ces types de filtres interagissent dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La requête est traitée à travers les filtres d’autorisations, les filtres de ressources, la liaison de modèle, les filtres d’actions, l’exécution d’actions et la conversion des résultats d’actions, les filtres d’exceptions, les filtres de résultats et l’exécution de résultats.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="e4b42-141">Implémentation</span><span class="sxs-lookup"><span data-stu-id="e4b42-141">Implementation</span></span>

<span data-ttu-id="e4b42-142">Les filtres prennent en charge les implémentations synchrones et asynchrones via différentes définitions d’interface.</span><span class="sxs-lookup"><span data-stu-id="e4b42-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="e4b42-143">Les filtres synchrones exécutent le code avant et après leur étape de pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="e4b42-144">Par exemple, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> est appelé avant l’appel de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="e4b42-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> est appelé après le retour de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="e4b42-146">Les filtres asynchrones définissent une méthode `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="e4b42-147">Par exemple, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="e4b42-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="e4b42-148">Dans le code précédent, le `SampleAsyncActionFilter` a un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) qui exécute la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="e4b42-149">Plusieurs étapes de filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-149">Multiple filter stages</span></span>

<span data-ttu-id="e4b42-150">Vous pouvez implémenter des interfaces pour plusieurs étapes de filtres dans une même classe.</span><span class="sxs-lookup"><span data-stu-id="e4b42-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="e4b42-151">Par exemple, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implémente :</span><span class="sxs-lookup"><span data-stu-id="e4b42-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="e4b42-152">Synchrone : <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="e4b42-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="e4b42-153">Asynchrone : <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="e4b42-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="e4b42-154">Implémentez la version synchrone **ou bien** la version asynchrone d’une interface de filtre, mais **pas** les deux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="e4b42-155">Le runtime vérifie d’abord si le filtre implémente l’interface asynchrone et, le cas échéant, il appelle cette interface.</span><span class="sxs-lookup"><span data-stu-id="e4b42-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="e4b42-156">Dans le cas contraire, il appelle la ou les méthodes de l’interface synchrone.</span><span class="sxs-lookup"><span data-stu-id="e4b42-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="e4b42-157">Si des interfaces synchrones et asynchrones sont implémentées dans une classe, seule la méthode asynchrone est appelée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="e4b42-158">Lorsque vous utilisez des classes abstraites comme <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, substituez uniquement les méthodes synchrones ou la méthode asynchrone pour chaque type de filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="e4b42-159">Attributs de filtre intégrés</span><span class="sxs-lookup"><span data-stu-id="e4b42-159">Built-in filter attributes</span></span>

<span data-ttu-id="e4b42-160">ASP.NET Core inclut les filtres intégrés basés sur des attributs que vous pouvez définir dans une sous-classe et personnaliser.</span><span class="sxs-lookup"><span data-stu-id="e4b42-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="e4b42-161">Par exemple, le filtre de résultats suivant ajoute un en-tête à la réponse :</span><span class="sxs-lookup"><span data-stu-id="e4b42-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-162">Les attributs autorisent les filtres à accepter des arguments, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="e4b42-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="e4b42-163">Appliquez le `AddHeaderAttribute` à un contrôleur ou une méthode d’action et spécifiez le nom et la valeur de l’en-tête HTTP :</span><span class="sxs-lookup"><span data-stu-id="e4b42-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="e4b42-164">Utilisez un outil tel que les [outils de développement du navigateur](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) pour examiner les en-têtes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="e4b42-165">Sous **en-têtes de réponse**, `author: Rick Anderson` s’affiche.</span><span class="sxs-lookup"><span data-stu-id="e4b42-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="e4b42-166">Le code suivant implémente une `ActionFilterAttribute` qui :</span><span class="sxs-lookup"><span data-stu-id="e4b42-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="e4b42-167">Lit le titre et le nom à partir du système de configuration.</span><span class="sxs-lookup"><span data-stu-id="e4b42-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="e4b42-168">Contrairement à l’exemple précédent, le code suivant ne nécessite pas l’ajout de paramètres de filtre au code.</span><span class="sxs-lookup"><span data-stu-id="e4b42-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="e4b42-169">Ajoute le titre et le nom à l’en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="e4b42-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-170">Les options de configuration sont fournies par le [système de configuration](xref:fundamentals/configuration/index) à l’aide du modèle d' [options](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="e4b42-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="e4b42-171">Par exemple, à partir du fichier *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="e4b42-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="e4b42-172">Dans la `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e4b42-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="e4b42-173">La classe `PositionOptions` est ajoutée au conteneur de services avec la zone de configuration `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="e4b42-174">Le `MyActionFilterAttribute` est ajouté au conteneur de services.</span><span class="sxs-lookup"><span data-stu-id="e4b42-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="e4b42-175">Le code suivant illustre la classe `PositionOptions` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="e4b42-176">Le code suivant applique l' `MyActionFilterAttribute` à la méthode `Index2` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="e4b42-177">Sous **en-têtes de réponse**, `author: Rick Anderson`et `Editor: Joe Smith` s’affichent lorsque le point de terminaison `Sample/Index2` est appelé.</span><span class="sxs-lookup"><span data-stu-id="e4b42-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="e4b42-178">Le code suivant applique la `MyActionFilterAttribute` et la `AddHeaderAttribute` à la page Razor :</span><span class="sxs-lookup"><span data-stu-id="e4b42-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="e4b42-179">Les filtres ne peuvent pas être appliqués aux méthodes de gestionnaire de page Razor.</span><span class="sxs-lookup"><span data-stu-id="e4b42-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="e4b42-180">Ils peuvent être appliqués au modèle de page Razor ou globalement.</span><span class="sxs-lookup"><span data-stu-id="e4b42-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="e4b42-181">Plusieurs des interfaces de filtre ont des attributs correspondants qui peuvent être utilisés comme classes de base pour des implémentations personnalisées.</span><span class="sxs-lookup"><span data-stu-id="e4b42-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="e4b42-182">Les attributs de filtre :</span><span class="sxs-lookup"><span data-stu-id="e4b42-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="e4b42-183">Étendues de filtre et ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="e4b42-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="e4b42-184">Un filtre peut être ajouté au pipeline à une des trois *étendues* :</span><span class="sxs-lookup"><span data-stu-id="e4b42-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="e4b42-185">Utilisation d’un attribut sur une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="e4b42-186">Les attributs de filtre ne peuvent pas être appliqués aux méthodes de gestionnaire Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="e4b42-187">Utilisation d’un attribut sur un contrôleur ou une page Razor.</span><span class="sxs-lookup"><span data-stu-id="e4b42-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="e4b42-188">Globalement pour tous les contrôleurs, actions et Razor Pages comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="e4b42-189">Ordre d’exécution par défaut</span><span class="sxs-lookup"><span data-stu-id="e4b42-189">Default order of execution</span></span>

<span data-ttu-id="e4b42-190">Quand il existe plusieurs filtres pour une étape particulière du pipeline, l’étendue détermine l’ordre par défaut de l’exécution du filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="e4b42-191">Les filtres globaux entourent les filtres de classe, qui à leur tour entourent les filtres de méthode.</span><span class="sxs-lookup"><span data-stu-id="e4b42-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="e4b42-192">En raison de l’imbrication de filtres, le code *après* des filtres s’exécute dans l’ordre inverse du code *avant*.</span><span class="sxs-lookup"><span data-stu-id="e4b42-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="e4b42-193">La séquence de filtre :</span><span class="sxs-lookup"><span data-stu-id="e4b42-193">The filter sequence:</span></span>

* <span data-ttu-id="e4b42-194">Le code *avant* des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="e4b42-195">Le code *avant* des filtres de page Razor et de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="e4b42-196">Le code *avant* des filtres de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="e4b42-197">Le code *après* des filtres de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="e4b42-198">Le code *après* les filtres de page de contrôleur et de page Razor.</span><span class="sxs-lookup"><span data-stu-id="e4b42-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="e4b42-199">Le code *après* des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="e4b42-200">Voici un exemple qui illustre l’ordre dans lequel les méthodes de filtre sont appelées pour les filtres d’actions synchrones.</span><span class="sxs-lookup"><span data-stu-id="e4b42-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="e4b42-201">Séquence</span><span class="sxs-lookup"><span data-stu-id="e4b42-201">Sequence</span></span> | <span data-ttu-id="e4b42-202">Étendue de filtre</span><span class="sxs-lookup"><span data-stu-id="e4b42-202">Filter scope</span></span> | <span data-ttu-id="e4b42-203">Filter, méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="e4b42-204">1</span><span class="sxs-lookup"><span data-stu-id="e4b42-204">1</span></span> | <span data-ttu-id="e4b42-205">Global</span><span class="sxs-lookup"><span data-stu-id="e4b42-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="e4b42-206">2</span><span class="sxs-lookup"><span data-stu-id="e4b42-206">2</span></span> | <span data-ttu-id="e4b42-207">Page de contrôleur ou Razor</span><span class="sxs-lookup"><span data-stu-id="e4b42-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="e4b42-208">3</span><span class="sxs-lookup"><span data-stu-id="e4b42-208">3</span></span> | <span data-ttu-id="e4b42-209">Méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="e4b42-210">4</span><span class="sxs-lookup"><span data-stu-id="e4b42-210">4</span></span> | <span data-ttu-id="e4b42-211">Méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="e4b42-212">5</span><span class="sxs-lookup"><span data-stu-id="e4b42-212">5</span></span> | <span data-ttu-id="e4b42-213">Page de contrôleur ou Razor</span><span class="sxs-lookup"><span data-stu-id="e4b42-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="e4b42-214">6</span><span class="sxs-lookup"><span data-stu-id="e4b42-214">6</span></span> | <span data-ttu-id="e4b42-215">Global</span><span class="sxs-lookup"><span data-stu-id="e4b42-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="e4b42-216">Filtres au niveau du contrôleur</span><span class="sxs-lookup"><span data-stu-id="e4b42-216">Controller level filters</span></span>

<span data-ttu-id="e4b42-217">Chaque contrôleur qui hérite de la classe de base <xref:Microsoft.AspNetCore.Mvc.Controller> inclut les méthodes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) et [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="e4b42-218">Ces méthodes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-218">These methods:</span></span>

* <span data-ttu-id="e4b42-219">Incluent dans un wrapper les filtres qui s’exécutent pour une action donnée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="e4b42-220">`OnActionExecuting` est appelé avant tous les filtres de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="e4b42-221">`OnActionExecuted` est appelé après tous les filtres d’actions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="e4b42-222">`OnActionExecutionAsync` est appelé avant tous les filtres de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="e4b42-223">Le code dans le filtre après `next` s’exécute après la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="e4b42-224">Par exemple, dans l’échantillon à télécharger, `MySampleActionFilter` est appliqué globalement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="e4b42-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="e4b42-225">Voici le `TestController` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-225">The `TestController`:</span></span>

* <span data-ttu-id="e4b42-226">Applique la `SampleActionFilterAttribute` (`[SampleActionFilter]`) à l’action `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="e4b42-227">Remplace `OnActionExecuting` et `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="e4b42-228">La navigation vers `https://localhost:5001/Test2/FilterTest2` exécute le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="e4b42-229">Les filtres au niveau du contrôleur définissent la propriété [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) sur `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-229">Controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="e4b42-230">Les filtres au niveau du contrôleur **ne peuvent pas** être configurés pour s’exécuter après les filtres appliqués aux méthodes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="e4b42-231">L’ordre est expliqué dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e4b42-231">Order is explained in the next section.</span></span>

<span data-ttu-id="e4b42-232">Pour Razor Pages, consultez [Implémenter des filtres Razor Page en remplaçant les méthodes de filtre](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="e4b42-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="e4b42-233">Remplacement de l’ordre par défaut</span><span class="sxs-lookup"><span data-stu-id="e4b42-233">Overriding the default order</span></span>

<span data-ttu-id="e4b42-234">Vous pouvez remplacer la séquence d’exécution par défaut en implémentant <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="e4b42-235">`IOrderedFilter` expose la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> qui est prioritaire sur l’étendue afin de déterminer l’ordre d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e4b42-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="e4b42-236">Un filtre avec une valeur `Order` inférieure :</span><span class="sxs-lookup"><span data-stu-id="e4b42-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="e4b42-237">Exécute le code *avant* avant celui d’un filtre avec une valeur supérieure à `Order`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="e4b42-238">Exécute le code *après* après celui d’un filtre avec une valeur `Order` supérieure.</span><span class="sxs-lookup"><span data-stu-id="e4b42-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="e4b42-239">La propriété `Order` est définie à l’aide d’un paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="e4b42-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="e4b42-240">Examinez les deux filtres d’action dans le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="e4b42-241">Un filtre global est ajouté dans `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e4b42-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="e4b42-242">Les 3 filtres s’exécutent dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="e4b42-243">La propriété `Order` remplace l’étendue lors de la détermination de l’ordre dans lequel les filtres s’exécutent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="e4b42-244">Les filtres sont d’abord classés par ordre, puis l’étendue est utilisée pour couper les liens.</span><span class="sxs-lookup"><span data-stu-id="e4b42-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="e4b42-245">Tous les filtres intégrés implémentent `IOrderedFilter` et affectent 0 à la valeur `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="e4b42-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="e4b42-246">Comme mentionné précédemment, les filtres au niveau du contrôleur définissent la propriété [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) sur `int.MinValue` pour les filtres intégrés, Scope détermine l’ordre, sauf si `Order` est défini sur une valeur différente de zéro.</span><span class="sxs-lookup"><span data-stu-id="e4b42-246">As mentioned previously, controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="e4b42-247">Dans le code précédent, `MySampleActionFilter` a une portée globale pour qu’elle s’exécute avant `MyAction2FilterAttribute`, qui a une portée de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="e4b42-248">Pour que `MyAction2FilterAttribute` exécuter en premier, définissez l’ordre sur `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="e4b42-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="e4b42-249">Pour que le filtre global `MySampleActionFilter` s’exécuter en premier, définissez `Order` sur `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="e4b42-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="e4b42-250">Annulation et court-circuit</span><span class="sxs-lookup"><span data-stu-id="e4b42-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="e4b42-251">Vous pouvez court-circuiter le pipeline de filtres en définissant la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sur le paramètre <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fourni à la méthode de filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="e4b42-252">Par exemple, le filtre de ressources suivant empêche l’exécution du reste du pipeline :</span><span class="sxs-lookup"><span data-stu-id="e4b42-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-253">Dans le code suivant, les filtres `ShortCircuitingResourceFilter` et `AddHeader` ciblent tous deux la méthode d’action `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="e4b42-254">Voici le `ShortCircuitingResourceFilter` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="e4b42-255">S’exécute en premier (puisqu’il s’agit d’un filtre de ressources et que `AddHeader` est un filtre d’action).</span><span class="sxs-lookup"><span data-stu-id="e4b42-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="e4b42-256">Court-circuite le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="e4b42-257">Le filtre `AddHeader` ne s’exécute donc jamais pour l’action `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="e4b42-258">Ce comportement est le même si les deux filtres sont appliqués au niveau de la méthode d’action, à condition que `ShortCircuitingResourceFilter` soit exécuté en premier.</span><span class="sxs-lookup"><span data-stu-id="e4b42-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="e4b42-259">`ShortCircuitingResourceFilter` s’exécute en premier en raison de son type de filtre ou de l’utilisation explicite de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="e4b42-260">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e4b42-260">Dependency injection</span></span>

<span data-ttu-id="e4b42-261">Les filtres peuvent être ajoutés par type ou par instance.</span><span class="sxs-lookup"><span data-stu-id="e4b42-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="e4b42-262">Si vous ajoutez une instance, celle-ci sera utilisée pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="e4b42-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="e4b42-263">Si un type est ajouté, il est activé par type.</span><span class="sxs-lookup"><span data-stu-id="e4b42-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="e4b42-264">Un filtre activé par type signifie :</span><span class="sxs-lookup"><span data-stu-id="e4b42-264">A type-activated filter means:</span></span>

* <span data-ttu-id="e4b42-265">Une instance est créée pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="e4b42-265">An instance is created for each request.</span></span>
* <span data-ttu-id="e4b42-266">Les dépendances de constructeur sont remplies par [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="e4b42-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="e4b42-267">Les filtres qui sont implémentés en tant qu’attributs et ajoutés directement à des classes de contrôleur ou à de méthodes d’action ne peut pas avoir de dépendances de constructeur fournies par [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4b42-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="e4b42-268">Les dépendances de constructeur ne peuvent pas être fournies par l’injection de dépendance, car :</span><span class="sxs-lookup"><span data-stu-id="e4b42-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="e4b42-269">Les paramètres du constructeur des attributs doivent être fournis là où ils sont appliqués.</span><span class="sxs-lookup"><span data-stu-id="e4b42-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="e4b42-270">Il s’agit d’une limitation de la façon dont les attributs fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="e4b42-271">Les filtres suivants prennent en charge les dépendances de constructeur fournies à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="e4b42-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="e4b42-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implémenté sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="e4b42-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="e4b42-273">Les filtres précédents peuvent être appliqués à une méthode de contrôleur ou d’action :</span><span class="sxs-lookup"><span data-stu-id="e4b42-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="e4b42-274">Des enregistreurs d’événements sont disponibles à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-274">Loggers are available from DI.</span></span> <span data-ttu-id="e4b42-275">Toutefois, évitez la création et l’utilisation de filtres à des fins purement d’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e4b42-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="e4b42-276">L’[enregistrement d’infrastructure intégrée](xref:fundamentals/logging/index) fournit généralement ce qui est nécessaire pour l’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e4b42-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="e4b42-277">Enregistrement ajouté aux filtres :</span><span class="sxs-lookup"><span data-stu-id="e4b42-277">Logging added to filters:</span></span>

* <span data-ttu-id="e4b42-278">Doit se concentrer sur les problèmes ou le comportement du domaine métier spécifique au filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="e4b42-279">Ne doit **pas** enregistrer des actions ou d’autres événements d’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="e4b42-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="e4b42-280">Les filtres intégrés consignent les actions et les événements du Framework.</span><span class="sxs-lookup"><span data-stu-id="e4b42-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="e4b42-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="e4b42-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="e4b42-282">Les types d’implémentation du filtre de services sont enregistrés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="e4b42-283">Un <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> récupère une instance du filtre à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="e4b42-284">Le code suivant affiche le `AddHeaderResultServiceFilter` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="e4b42-285">Dans le code suivant, `AddHeaderResultServiceFilter` est ajouté au conteneur d’injection de dépendance :</span><span class="sxs-lookup"><span data-stu-id="e4b42-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="e4b42-286">Dans le code suivant, l’attribut `ServiceFilter` récupère une instance du filtre `AddHeaderResultServiceFilter` à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="e4b42-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="e4b42-287">Lors de l’utilisation de `ServiceFilterAttribute`, définir [ServiceFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="e4b42-288">Conseille que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle elle a été créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="e4b42-289">Le runtime ASP.NET Core ne garantit pas :</span><span class="sxs-lookup"><span data-stu-id="e4b42-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="e4b42-290">Qu’une seule instance du filtre sera créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="e4b42-291">Le filtre ne sera pas demandé à nouveau à partir du conteneur d’injection de dépendance à un stade ultérieur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="e4b42-292">Évitez de l’utiliser avec un filtre qui dépend de services avec une durée de vie autre que singleton.</span><span class="sxs-lookup"><span data-stu-id="e4b42-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="e4b42-293">L'objet <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="e4b42-294">`IFilterFactory` expose la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> pour la création d’une instance <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="e4b42-295">`CreateInstance` charge le type spécifié à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="e4b42-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="e4b42-296">TypeFilterAttribute</span></span>

<span data-ttu-id="e4b42-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> est similaire à <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, mais son type n’est pas résolu directement à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="e4b42-298">Il instancie le type en utilisant <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="e4b42-299">Étant donné que les types `TypeFilterAttribute` ne sont pas résolus directement à partir du conteneur d’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="e4b42-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="e4b42-300">Les types qui sont référencés avec `TypeFilterAttribute` ne doivent pas d’abord être inscrits auprès du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="e4b42-301">Leurs dépendances ne sont pas remplies par le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="e4b42-302">`TypeFilterAttribute` peut éventuellement accepter des arguments de constructeur pour le type.</span><span class="sxs-lookup"><span data-stu-id="e4b42-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="e4b42-303">Lors de l’utilisation de `TypeFilterAttribute`, définir [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="e4b42-304">Indique que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle elle a été créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="e4b42-305">Le runtime ASP.NET Core ne fournit aucune garantie qu’une seule instance du filtre sera créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="e4b42-306">Évitez de l’utiliser avec un filtre qui dépend de services avec une durée de vie autre que singleton.</span><span class="sxs-lookup"><span data-stu-id="e4b42-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="e4b42-307">L’exemple suivant indique comment passer des arguments vers un type en utilisant `TypeFilterAttribute` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="e4b42-308">Filtres d'autorisation</span><span class="sxs-lookup"><span data-stu-id="e4b42-308">Authorization filters</span></span>

<span data-ttu-id="e4b42-309">Filtres d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="e4b42-309">Authorization filters:</span></span>

* <span data-ttu-id="e4b42-310">Sont les premiers filtres à être exécutés dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="e4b42-311">Contrôlent l’accès aux méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-311">Control access to action methods.</span></span>
* <span data-ttu-id="e4b42-312">Ont une méthode avant, mais pas de méthode après.</span><span class="sxs-lookup"><span data-stu-id="e4b42-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="e4b42-313">Les filtres d’autorisations personnalisés nécessitent une infrastructure d’autorisation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="e4b42-314">Préférez la configuration des stratégies d’autorisation ou l’écriture d’une stratégie d’autorisation personnalisée à l’écriture d’un filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e4b42-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="e4b42-315">Le filtre d'autorisations intégré :</span><span class="sxs-lookup"><span data-stu-id="e4b42-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="e4b42-316">Appelle le système d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e4b42-316">Calls the authorization system.</span></span>
* <span data-ttu-id="e4b42-317">N’autoriser les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-317">Does not authorize requests.</span></span>

<span data-ttu-id="e4b42-318">Ne levez **pas** d’exceptions dans les filtres d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="e4b42-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="e4b42-319">L’exception ne sera pas gérée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-319">The exception will not be handled.</span></span>
* <span data-ttu-id="e4b42-320">Les filtres d’exceptions ne gèreront pas l’exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="e4b42-321">Songez à émettre une stimulation quand un filtre d’autorisations se produit.</span><span class="sxs-lookup"><span data-stu-id="e4b42-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="e4b42-322">Découvrez plus d’informations sur [l’autorisation](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="e4b42-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="e4b42-323">Filtres de ressources</span><span class="sxs-lookup"><span data-stu-id="e4b42-323">Resource filters</span></span>

<span data-ttu-id="e4b42-324">Filtres de ressources :</span><span class="sxs-lookup"><span data-stu-id="e4b42-324">Resource filters:</span></span>

* <span data-ttu-id="e4b42-325">Implémentez l’interface <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="e4b42-326">L’exécution inclut dans un wrapper la majeure partie du pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="e4b42-327">Seuls les [filtres d’autorisations](#authorization-filters) s’exécutent avant les filtres de ressources.</span><span class="sxs-lookup"><span data-stu-id="e4b42-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="e4b42-328">Les filtres de ressources sont utiles pour court-circuiter la majeure partie du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="e4b42-329">Par exemple, un filtre de mise en cache peut éviter le reste du pipeline dans une correspondance dans le cache.</span><span class="sxs-lookup"><span data-stu-id="e4b42-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="e4b42-330">Exemples de filtre de ressources :</span><span class="sxs-lookup"><span data-stu-id="e4b42-330">Resource filter examples:</span></span>

* <span data-ttu-id="e4b42-331">[Le filtre de ressources de court-circuit](#short-circuiting-resource-filter) illustré précédemment.</span><span class="sxs-lookup"><span data-stu-id="e4b42-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="e4b42-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="e4b42-333">Il empêche la liaison de données d’accéder aux données de formulaire.</span><span class="sxs-lookup"><span data-stu-id="e4b42-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="e4b42-334">Il est utilisé pour les chargements de fichiers volumineux et pour empêcher que le formulaire de données ne soit lu en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e4b42-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="e4b42-335">Filtres d'action</span><span class="sxs-lookup"><span data-stu-id="e4b42-335">Action filters</span></span>

<span data-ttu-id="e4b42-336">Les filtres d’action ne s’appliquent **pas** aux Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="e4b42-337">Razor Pages prennent en charge <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span><span class="sxs-lookup"><span data-stu-id="e4b42-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="e4b42-338">Pour plus d’informations, consultez [Méthodes de filtre pour les pages Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="e4b42-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="e4b42-339">Filtres d'actions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-339">Action filters:</span></span>

* <span data-ttu-id="e4b42-340">Implémentez l’interface <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="e4b42-341">Leur exécution entoure l’exécution de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="e4b42-342">Le code suivant montre un exemple de filtre d’actions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="e4b42-343"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> fournit les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="e4b42-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> : active la lecture des entrées dans une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="e4b42-345"><xref:Microsoft.AspNetCore.Mvc.Controller> : permet de manipuler l’instance de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="e4b42-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> : la définition de `Result` court-circuite l’exécution de la méthode d’action et les filtres d’actions suivants.</span><span class="sxs-lookup"><span data-stu-id="e4b42-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="e4b42-347">Levée d’une exception dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="e4b42-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="e4b42-348">Empêche l’exécution des filtres suivants.</span><span class="sxs-lookup"><span data-stu-id="e4b42-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="e4b42-349">Contrairement au paramètre `Result`, est traité comme un échec plutôt que comme un résultat positif.</span><span class="sxs-lookup"><span data-stu-id="e4b42-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="e4b42-350"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> fournit `Controller` et `Result`, en plus des propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="e4b42-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> : sa valeur est true si l’exécution de l’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="e4b42-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> : sa valeur est non Null si l’action ou un filtre d’actions précédemment exécuté a lancé une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="e4b42-353">Paramètre de cette propriété sur null :</span><span class="sxs-lookup"><span data-stu-id="e4b42-353">Setting this property to null:</span></span>

  * <span data-ttu-id="e4b42-354">Gère effectivement une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="e4b42-355">`Result` est exécuté comme s’il avait été retourné à partir de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="e4b42-356">Pour un `IAsyncActionFilter`, un appel à <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> :</span><span class="sxs-lookup"><span data-stu-id="e4b42-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="e4b42-357">Exécute tous les filtres d’actions suivants et la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="e4b42-358">Renvoie `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="e4b42-359">Pour court-circuiter, attribuez <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> à une instance de résultat et n’appelez pas le `next` (le `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="e4b42-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="e4b42-360">L’infrastructure fournit un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> abstrait que vous pouvez placer dans une sous-classe.</span><span class="sxs-lookup"><span data-stu-id="e4b42-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="e4b42-361">Le filtre d’actions `OnActionExecuting` peut être utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="e4b42-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="e4b42-362">Valider l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="e4b42-362">Validate model state.</span></span>
* <span data-ttu-id="e4b42-363">Renvoyer une erreur si l’état n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="e4b42-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-364">La méthode `OnActionExecuted` s’exécute après la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="e4b42-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="e4b42-365">Et peut voir et manipuler les résultats de l’action via la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="e4b42-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> est défini sur true si l’exécution de l’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="e4b42-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> est défini sur une valeur non Null si l’action ou un filtre d’actions suivant a levé une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="e4b42-368">Le fait de définir `Exception` avec la valeur null :</span><span class="sxs-lookup"><span data-stu-id="e4b42-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="e4b42-369">Gère effectivement une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="e4b42-370">`ActionExecutedContext.Result` est exécuté comme s’il avait été retourné normalement à partir de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="e4b42-371">Filtres d'exception</span><span class="sxs-lookup"><span data-stu-id="e4b42-371">Exception filters</span></span>

<span data-ttu-id="e4b42-372">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-372">Exception filters:</span></span>

* <span data-ttu-id="e4b42-373">Implémentent <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="e4b42-374">Ils peuvent être utilisés pour implémenter des stratégies de gestion des erreurs communes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="e4b42-375">L’échantillon de filtre d’exceptions suivant utilise un affichage personnalisé des erreurs pour afficher des détails sur les exceptions qui se produisent pendant que l’application est en développement :</span><span class="sxs-lookup"><span data-stu-id="e4b42-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="e4b42-376">Le code suivant teste le filtre d’exception :</span><span class="sxs-lookup"><span data-stu-id="e4b42-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="e4b42-377">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-377">Exception filters:</span></span>

* <span data-ttu-id="e4b42-378">N’ont pas d’événements avant et après.</span><span class="sxs-lookup"><span data-stu-id="e4b42-378">Don't have before and after events.</span></span>
* <span data-ttu-id="e4b42-379">Implémentent <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="e4b42-380">Gèrent des exceptions non prises en charge qui se produisent dans Razor Page ou dans la création des contrôleurs, la [liaison de données](xref:mvc/models/model-binding), les filtres d’actions ou les méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="e4b42-381">N’interceptent **pas** les exceptions qui se produisent dans l’exécution des filtres de ressources, des filtres de résultats ou des résultats MVC.</span><span class="sxs-lookup"><span data-stu-id="e4b42-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="e4b42-382">Pour gérer une exception, définissez la propriété <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> sur `true` ou écrivez une réponse.</span><span class="sxs-lookup"><span data-stu-id="e4b42-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="e4b42-383">Ceci arrête la propagation de l’exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-383">This stops propagation of the exception.</span></span> <span data-ttu-id="e4b42-384">Un filtre d’exceptions ne peut pas changer une exception en « réussite ».</span><span class="sxs-lookup"><span data-stu-id="e4b42-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="e4b42-385">Seul un filtre d’actions peut le faire.</span><span class="sxs-lookup"><span data-stu-id="e4b42-385">Only an action filter can do that.</span></span>

<span data-ttu-id="e4b42-386">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-386">Exception filters:</span></span>

* <span data-ttu-id="e4b42-387">Conviennent pour intercepter des exceptions qui se produisent dans des actions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="e4b42-388">N’offrent pas la même souplesse que le middleware (intergiciel) de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="e4b42-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="e4b42-389">Privilégiez le middleware pour la gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="e4b42-390">Utilisez les filtres d’exceptions uniquement lorsque la gestion des erreurs *diffère* selon la méthode d’action appelée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="e4b42-391">Par exemple, votre application peut avoir des méthodes d’action à la fois pour des points de terminaison d’API et pour des affichages/HTML.</span><span class="sxs-lookup"><span data-stu-id="e4b42-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="e4b42-392">Les points de terminaison d’API peuvent retourner des informations d’erreur au format JSON, tandis que les actions basées sur une vue peuvent retourner une page d’erreur au format HTML.</span><span class="sxs-lookup"><span data-stu-id="e4b42-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="e4b42-393">Filtres de résultats</span><span class="sxs-lookup"><span data-stu-id="e4b42-393">Result filters</span></span>

<span data-ttu-id="e4b42-394">Filtres de résultats :</span><span class="sxs-lookup"><span data-stu-id="e4b42-394">Result filters:</span></span>

* <span data-ttu-id="e4b42-395">Implémenter une interface :</span><span class="sxs-lookup"><span data-stu-id="e4b42-395">Implement an interface:</span></span>
  * <span data-ttu-id="e4b42-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="e4b42-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="e4b42-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="e4b42-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="e4b42-398">Leur exécution entoure l’exécution de résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="e4b42-399">IResultFilter et IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="e4b42-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="e4b42-400">Le code suivant indique un filtre de résultats qui ajoute un en-tête HTTP :</span><span class="sxs-lookup"><span data-stu-id="e4b42-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="e4b42-401">Le type de résultat à exécuter dépend de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="e4b42-402">Une action qui retourne une vue comprend tout le traitement Razor dans le cadre de l’exécution du <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="e4b42-403">Une méthode d’API peut effectuer une sérialisation dans le cadre de l’exécution du résultat.</span><span class="sxs-lookup"><span data-stu-id="e4b42-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="e4b42-404">En savoir plus sur les [résultats des actions](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="e4b42-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="e4b42-405">Les filtres de résultats ne sont exécutés que lorsqu’une action ou un filtre d’action produit un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="e4b42-406">Les filtres de résultats ne sont pas exécutés dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="e4b42-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="e4b42-407">Un filtre d’autorisation ou des courts-circuits de filtre de ressources le pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="e4b42-408">Un filtre d’exception gère une exception en générant un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="e4b42-409">La méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> peut court-circuiter l’exécution du résultat d’action et les filtres de résultats suivants en définissant <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="e4b42-410">Écrivez dans l’objet de réponse quand vous court-circuitez pour éviter de générer une réponse vide.</span><span class="sxs-lookup"><span data-stu-id="e4b42-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="e4b42-411">Levée d’une exception dans `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="e4b42-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="e4b42-412">Empêche l’exécution du résultat de l’action et des filtres suivants.</span><span class="sxs-lookup"><span data-stu-id="e4b42-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="e4b42-413">Est traité comme un échec au lieu d’un résultat réussi.</span><span class="sxs-lookup"><span data-stu-id="e4b42-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="e4b42-414">Lorsque la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> s’exécute, la réponse a probablement déjà été envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="e4b42-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="e4b42-415">Si la réponse a déjà été envoyée au client, elle ne peut pas être modifiée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="e4b42-416">`ResultExecutedContext.Canceled` est défini sur `true` si l’exécution du résultat d’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="e4b42-417">`ResultExecutedContext.Exception` est défini sur une valeur non Null si le résultat d’action ou un filtre de résultats suivant a levé une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="e4b42-418">La définition de `Exception` sur null gère efficacement une exception et empêche la levée de l’exception plus tard dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="e4b42-419">Il n’existe aucun moyen fiable pour écrire des données dans une réponse lors de la gestion d’une exception dans un filtre de résultats.</span><span class="sxs-lookup"><span data-stu-id="e4b42-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="e4b42-420">Si les en-têtes ont été vidés pour le client lorsqu’un résultat d’action lance une exception, il n’existe aucun mécanisme fiable pour envoyer un code d’échec.</span><span class="sxs-lookup"><span data-stu-id="e4b42-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="e4b42-421">Pour un <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, un appel à `await next` sur le <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> exécute tous les filtres de résultats suivants et le résultat de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="e4b42-422">Pour court-circuiter, définissez [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) sur `true` et n’appelez pas `ResultExecutionDelegate` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="e4b42-423">L’infrastructure fournit un `ResultFilterAttribute` abstrait que vous pouvez placer dans une sous-classe.</span><span class="sxs-lookup"><span data-stu-id="e4b42-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="e4b42-424">La classe [AddHeaderAttribute](#add-header-attribute) ci-dessus est un exemple d’un attribut de filtre de résultats.</span><span class="sxs-lookup"><span data-stu-id="e4b42-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="e4b42-425">IAlwaysRunResultFilter et IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="e4b42-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="e4b42-426">Les interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> déclarent une implémentation <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> qui s’exécute pour tous les résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="e4b42-427">Cela comprend les résultats d’action produits par :</span><span class="sxs-lookup"><span data-stu-id="e4b42-427">This includes action results produced by:</span></span>

* <span data-ttu-id="e4b42-428">Filtres d’autorisation et filtres de ressources qui court-circuitent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="e4b42-429">Filtres d’exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-429">Exception filters.</span></span>

<span data-ttu-id="e4b42-430">Par exemple, le filtre suivant exécute et définit toujours un résultat d’action (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) avec un code d’état *422 Entité non traitée* en cas d’échec de la négociation de contenu :</span><span class="sxs-lookup"><span data-stu-id="e4b42-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="e4b42-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="e4b42-431">IFilterFactory</span></span>

<span data-ttu-id="e4b42-432">L'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="e4b42-433">Par conséquent, une instance de `IFilterFactory` peut être utilisée comme instance de `IFilterMetadata` n’importe où dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="e4b42-434">Quan se prépare à appeler le filtre, il tente de le caster en `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="e4b42-435">Si ce cast réussit, la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> est appelée pour créer l’instance `IFilterMetadata` qui sera appelée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="e4b42-436">La conception est flexible, car il n’est pas nécessaire de définir explicitement le pipeline de filtres exact quand l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="e4b42-437">Une autre approche pour la création de filtres est d’implémenter `IFilterFactory` à l’aide des implémentations d’attribut personnalisé :</span><span class="sxs-lookup"><span data-stu-id="e4b42-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="e4b42-438">Le filtre est appliqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="e4b42-439">Testez le code précédent en exécutant l' [exemple Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="e4b42-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="e4b42-440">Appeler les outils de développement F12.</span><span class="sxs-lookup"><span data-stu-id="e4b42-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="e4b42-441">Accédez à `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="e4b42-442">Les outils de développement F12 affichent les en-têtes de réponse suivants ajoutés par l’exemple de code :</span><span class="sxs-lookup"><span data-stu-id="e4b42-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="e4b42-443">**Auteur :** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="e4b42-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="e4b42-444">**globaladdheader :** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="e4b42-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="e4b42-445">**interne :** `My header`</span><span class="sxs-lookup"><span data-stu-id="e4b42-445">**internal:** `My header`</span></span>

<span data-ttu-id="e4b42-446">Le code précédent crée l’en-tête de réponse **Internal :** `My header`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="e4b42-447">IFilterFactory implémenté sur un attribut</span><span class="sxs-lookup"><span data-stu-id="e4b42-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="e4b42-448">Les filtres qui implémentent `IFilterFactory` sont utiles pour les filtres qui :</span><span class="sxs-lookup"><span data-stu-id="e4b42-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="e4b42-449">Ne nécessitent pas le passage de paramètres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="e4b42-450">Disposent de dépendances de constructeur qui doivent être remplies par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="e4b42-451">L'objet <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="e4b42-452">`IFilterFactory` expose la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> pour la création d’une instance <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="e4b42-453">`CreateInstance` charge le type spécifié à partir du conteneur de services (injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="e4b42-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="e4b42-454">Le code suivant illustre trois approches pour appliquer `[SampleActionFilter]` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="e4b42-455">Dans le code précédent, la décoration de la méthode avec `[SampleActionFilter]` est la meilleure approche pour appliquer `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="e4b42-456">Utilisation d’un intergiciel dans le pipeline de filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="e4b42-457">Les filtres de ressources fonctionnent comme un [intergiciel](xref:fundamentals/middleware/index) dans la mesure où ils entourent l’exécution de tout ce qui vient plus tard dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="e4b42-458">Toutefois, les filtres diffèrent des intergiciels (middleware) en ce sens qu’ils font partie du runtime, ce qui signifie qu’ils ont accès au contexte et aux constructions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="e4b42-459">Pour utiliser un intergiciel comme filtre, créez un type avec une méthode `Configure` qui spécifie l’intergiciel que vous voulez injecter dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="e4b42-460">Voici un exemple qui utilise l’intergiciel de localisation pour établir la culture actuelle d’une requête :</span><span class="sxs-lookup"><span data-stu-id="e4b42-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="e4b42-461">Utilisez le <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> pour exécuter l’intergiciel :</span><span class="sxs-lookup"><span data-stu-id="e4b42-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="e4b42-462">Les filtres d’intergiciels s’exécutent à la même étape du pipeline de filtres en tant que filtres de ressources, avant la liaison de modèle et après le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="e4b42-463">Actions suivantes</span><span class="sxs-lookup"><span data-stu-id="e4b42-463">Next actions</span></span>

* <span data-ttu-id="e4b42-464">Consultez [les méthodes de filtre pour Razor pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="e4b42-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="e4b42-465">Pour expérimenter les filtres, [téléchargez, testez et modifiez l’échantillon Github](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="e4b42-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e4b42-466">Par [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e4b42-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e4b42-467">Les *filtres* dans ASP.NET Core permettent d’exécuter du code avant ou après des étapes spécifiques dans le pipeline de traitement des requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="e4b42-468">Les filtres intégrés gèrent notamment les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="e4b42-469">Autorisation (empêcher un utilisateur non autorisé d’accéder à des ressources).</span><span class="sxs-lookup"><span data-stu-id="e4b42-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="e4b42-470">Mise en cache des réponses (court-circuitage du pipeline de requêtes pour retourner une réponse mise en cache).</span><span class="sxs-lookup"><span data-stu-id="e4b42-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="e4b42-471">Il est possible de créer des filtres personnalisés pour gérer les problèmes transversaux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="e4b42-472">Les exemples de problèmes transversaux incluent la gestion des erreurs, la mise en cache, la configuration, l’autorisation et la journalisation.</span><span class="sxs-lookup"><span data-stu-id="e4b42-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="e4b42-473">Les filtres évitent la duplication de code.</span><span class="sxs-lookup"><span data-stu-id="e4b42-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="e4b42-474">Par exemple, un filtre d’exceptions de gestion des erreurs peut servir à consolider la gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="e4b42-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="e4b42-475">Ce document s’applique aux Razor Pages, aux contrôleurs d’API et aux contrôleurs avec affichages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="e4b42-476">[Afficher ou télécharger l’échantillon](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([comment télécharger](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e4b42-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="e4b42-477">Fonctionnement des filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-477">How filters work</span></span>

<span data-ttu-id="e4b42-478">Les filtres s’exécutent dans le *pipeline des appels d’action ASP.NET Core*, parfois appelé *pipeline de filtres*.</span><span class="sxs-lookup"><span data-stu-id="e4b42-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="e4b42-479">Le pipeline de filtres s’exécute après la sélection par ASP.NET Core de l’action à exécuter.</span><span class="sxs-lookup"><span data-stu-id="e4b42-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La requête est traitée via un autre intergiciel, un intergiciel de routage, une sélection d’action et le pipeline d’appels d’action ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="e4b42-482">Types de filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-482">Filter types</span></span>

<span data-ttu-id="e4b42-483">Chaque type de filtre est exécuté dans une étape différente du pipeline de filtres :</span><span class="sxs-lookup"><span data-stu-id="e4b42-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="e4b42-484">Les [filtres d’autorisations](#authorization-filters) s’exécutent en premier et sont utilisés pour déterminer si l’utilisateur est autorisé pour la requête.</span><span class="sxs-lookup"><span data-stu-id="e4b42-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="e4b42-485">Les filtres d’autorisations peuvent court-circuiter le pipeline si la requête n’est pas autorisée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="e4b42-486">[Filtres de ressources](#resource-filters) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="e4b42-487">Exécuter après l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e4b42-487">Run after authorization.</span></span>  
  * <span data-ttu-id="e4b42-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> peut exécuter du code avant le reste du pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="e4b42-489">Par exemple, `OnResourceExecuting` peut exécuter du code avant la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="e4b42-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="e4b42-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> peut exécuter du code une fois que le reste du pipeline terminé.</span><span class="sxs-lookup"><span data-stu-id="e4b42-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="e4b42-491">Les [filtres d’actions](#action-filters) peuvent exécuter du code juste avant et juste après l’appel d’une méthode d’action individuelle.</span><span class="sxs-lookup"><span data-stu-id="e4b42-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="e4b42-492">Ils peuvent être utilisés pour manipuler les arguments passés à une action et le résultat retourné depuis l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="e4b42-493">Les filtres d’actions ne sont **pas** pris en charge dans les Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="e4b42-494">Les [filtres d’exceptions](#exception-filters) sont utilisés pour appliquer des stratégies globales à des exceptions non gérées qui se produisent avant que des éléments aient été écrits dans le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4b42-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="e4b42-495">Les [filtres de résultats](#result-filters) peuvent exécuter du code juste avant et juste après l’exécution des résultats d’une action individuelle.</span><span class="sxs-lookup"><span data-stu-id="e4b42-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="e4b42-496">Ils s’exécutent uniquement au terme de l’exécution de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="e4b42-497">Ils sont utiles pour la logique qui doit entourer l’exécution de la vue ou du formateur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="e4b42-498">Le diagramme suivant montre comment ces types de filtres interagissent dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La requête est traitée à travers les filtres d’autorisations, les filtres de ressources, la liaison de modèle, les filtres d’actions, l’exécution d’actions et la conversion des résultats d’actions, les filtres d’exceptions, les filtres de résultats et l’exécution de résultats.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="e4b42-501">Implémentation</span><span class="sxs-lookup"><span data-stu-id="e4b42-501">Implementation</span></span>

<span data-ttu-id="e4b42-502">Les filtres prennent en charge les implémentations synchrones et asynchrones via différentes définitions d’interface.</span><span class="sxs-lookup"><span data-stu-id="e4b42-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="e4b42-503">Les filtres synchrones peuvent exécuter du code avant (`On-Stage-Executing`) et après (`On-Stage-Executed`) leur étape de pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="e4b42-504">Par exemple, `OnActionExecuting` est appelé avant l’appel de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="e4b42-505">`OnActionExecuted` est appelé après le retour de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="e4b42-506">Les filtres asynchrones définissent une méthode `On-Stage-ExecutionAsync` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="e4b42-507">Dans le code précédent, le `SampleAsyncActionFilter` a un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) qui exécute la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="e4b42-508">Chacune des méthodes `On-Stage-ExecutionAsync` prennent un `FilterType-ExecutionDelegate` qui exécute l’étape de pipeline du filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="e4b42-509">Plusieurs étapes de filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-509">Multiple filter stages</span></span>

<span data-ttu-id="e4b42-510">Vous pouvez implémenter des interfaces pour plusieurs étapes de filtres dans une même classe.</span><span class="sxs-lookup"><span data-stu-id="e4b42-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="e4b42-511">Par exemple, la classe <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implémente `IActionFilter`, `IResultFilter` et leurs équivalents asynchrones.</span><span class="sxs-lookup"><span data-stu-id="e4b42-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="e4b42-512">Implémentez la version synchrone **ou bien** la version asynchrone d’une interface de filtre, mais **pas** les deux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="e4b42-513">Le runtime vérifie d’abord si le filtre implémente l’interface asynchrone et, le cas échéant, il appelle cette interface.</span><span class="sxs-lookup"><span data-stu-id="e4b42-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="e4b42-514">Dans le cas contraire, il appelle la ou les méthodes de l’interface synchrone.</span><span class="sxs-lookup"><span data-stu-id="e4b42-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="e4b42-515">Si des interfaces synchrones et asynchrones sont implémentées dans une classe, seule la méthode asynchrone est appelée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="e4b42-516">Quand vous utilisez des classes abstraites comme <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, vous devez remplacer seulement les méthodes synchrones ou la méthode asynchrone pour chaque type de filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="e4b42-517">Attributs de filtre intégrés</span><span class="sxs-lookup"><span data-stu-id="e4b42-517">Built-in filter attributes</span></span>

<span data-ttu-id="e4b42-518">ASP.NET Core inclut les filtres intégrés basés sur des attributs que vous pouvez définir dans une sous-classe et personnaliser.</span><span class="sxs-lookup"><span data-stu-id="e4b42-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="e4b42-519">Par exemple, le filtre de résultats suivant ajoute un en-tête à la réponse :</span><span class="sxs-lookup"><span data-stu-id="e4b42-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-520">Les attributs autorisent les filtres à accepter des arguments, comme indiqué dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="e4b42-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="e4b42-521">Appliquez le `AddHeaderAttribute` à un contrôleur ou une méthode d’action et spécifiez le nom et la valeur de l’en-tête HTTP :</span><span class="sxs-lookup"><span data-stu-id="e4b42-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="e4b42-522">Plusieurs des interfaces de filtre ont des attributs correspondants qui peuvent être utilisés comme classes de base pour des implémentations personnalisées.</span><span class="sxs-lookup"><span data-stu-id="e4b42-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="e4b42-523">Les attributs de filtre :</span><span class="sxs-lookup"><span data-stu-id="e4b42-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="e4b42-524">Étendues de filtre et ordre d’exécution</span><span class="sxs-lookup"><span data-stu-id="e4b42-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="e4b42-525">Un filtre peut être ajouté au pipeline à une des trois *étendues* :</span><span class="sxs-lookup"><span data-stu-id="e4b42-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="e4b42-526">À l’aide d’un attribut sur une action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="e4b42-527">À l’aide d’un attribut sur un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="e4b42-528">Dans le monde entier pour tous les contrôleurs et actions, comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="e4b42-529">Le code précédent ajoute trois filtres globalement à l’aide de la collection [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="e4b42-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="e4b42-530">Ordre d’exécution par défaut</span><span class="sxs-lookup"><span data-stu-id="e4b42-530">Default order of execution</span></span>

<span data-ttu-id="e4b42-531">Lorsqu’il existe plusieurs filtres *du même type*, l’étendue détermine l’ordre par défaut de l’exécution du filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="e4b42-532">Filtres globaux-filtres de classe surround.</span><span class="sxs-lookup"><span data-stu-id="e4b42-532">Global filters surround class filters.</span></span> <span data-ttu-id="e4b42-533">Les filtres de classe entourent les filtres de méthode.</span><span class="sxs-lookup"><span data-stu-id="e4b42-533">Class filters surround method filters.</span></span>

<span data-ttu-id="e4b42-534">En raison de l’imbrication de filtres, le code *après* des filtres s’exécute dans l’ordre inverse du code *avant*.</span><span class="sxs-lookup"><span data-stu-id="e4b42-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="e4b42-535">La séquence de filtre :</span><span class="sxs-lookup"><span data-stu-id="e4b42-535">The filter sequence:</span></span>

* <span data-ttu-id="e4b42-536">Le code *avant* des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="e4b42-537">Le code *avant* des filtres du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="e4b42-538">Le code *avant* des filtres de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="e4b42-539">Le code *après* des filtres de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="e4b42-540">Le code *après* des filtres du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="e4b42-541">Le code *après* des filtres globaux.</span><span class="sxs-lookup"><span data-stu-id="e4b42-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="e4b42-542">Voici un exemple qui illustre l’ordre dans lequel les méthodes de filtre sont appelées pour les filtres d’actions synchrones.</span><span class="sxs-lookup"><span data-stu-id="e4b42-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="e4b42-543">Séquence</span><span class="sxs-lookup"><span data-stu-id="e4b42-543">Sequence</span></span> | <span data-ttu-id="e4b42-544">Étendue de filtre</span><span class="sxs-lookup"><span data-stu-id="e4b42-544">Filter scope</span></span> | <span data-ttu-id="e4b42-545">Filter, méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="e4b42-546">1</span><span class="sxs-lookup"><span data-stu-id="e4b42-546">1</span></span> | <span data-ttu-id="e4b42-547">Global</span><span class="sxs-lookup"><span data-stu-id="e4b42-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="e4b42-548">2</span><span class="sxs-lookup"><span data-stu-id="e4b42-548">2</span></span> | <span data-ttu-id="e4b42-549">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="e4b42-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="e4b42-550">3</span><span class="sxs-lookup"><span data-stu-id="e4b42-550">3</span></span> | <span data-ttu-id="e4b42-551">Méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="e4b42-552">4</span><span class="sxs-lookup"><span data-stu-id="e4b42-552">4</span></span> | <span data-ttu-id="e4b42-553">Méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="e4b42-554">5</span><span class="sxs-lookup"><span data-stu-id="e4b42-554">5</span></span> | <span data-ttu-id="e4b42-555">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="e4b42-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="e4b42-556">6</span><span class="sxs-lookup"><span data-stu-id="e4b42-556">6</span></span> | <span data-ttu-id="e4b42-557">Global</span><span class="sxs-lookup"><span data-stu-id="e4b42-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="e4b42-558">Cette séquence montre que :</span><span class="sxs-lookup"><span data-stu-id="e4b42-558">This sequence shows:</span></span>

* <span data-ttu-id="e4b42-559">Le filtre de méthode est imbriqué dans le filtre de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="e4b42-560">Le filtre de contrôleur est imbriqué dans le filtre global.</span><span class="sxs-lookup"><span data-stu-id="e4b42-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="e4b42-561">Filtres au niveau de contrôleur et de Razor Page</span><span class="sxs-lookup"><span data-stu-id="e4b42-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="e4b42-562">Chaque contrôleur qui hérite de la classe de base <xref:Microsoft.AspNetCore.Mvc.Controller> inclut les méthodes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) et [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="e4b42-563">Ces méthodes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-563">These methods:</span></span>

* <span data-ttu-id="e4b42-564">Incluent dans un wrapper les filtres qui s’exécutent pour une action donnée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="e4b42-565">`OnActionExecuting` est appelé avant tous les filtres de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="e4b42-566">`OnActionExecuted` est appelé après tous les filtres d’actions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="e4b42-567">`OnActionExecutionAsync` est appelé avant tous les filtres de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="e4b42-568">Le code dans le filtre après `next` s’exécute après la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="e4b42-569">Par exemple, dans l’échantillon à télécharger, `MySampleActionFilter` est appliqué globalement au démarrage.</span><span class="sxs-lookup"><span data-stu-id="e4b42-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="e4b42-570">Voici le `TestController` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-570">The `TestController`:</span></span>

* <span data-ttu-id="e4b42-571">Applique la `SampleActionFilterAttribute` (`[SampleActionFilter]`) à l’action `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="e4b42-572">Remplace `OnActionExecuting` et `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="e4b42-573">La navigation vers `https://localhost:5001/Test/FilterTest2` exécute le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4b42-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="e4b42-574">Pour Razor Pages, consultez [Implémenter des filtres Razor Page en remplaçant les méthodes de filtre](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="e4b42-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="e4b42-575">Remplacement de l’ordre par défaut</span><span class="sxs-lookup"><span data-stu-id="e4b42-575">Overriding the default order</span></span>

<span data-ttu-id="e4b42-576">Vous pouvez remplacer la séquence d’exécution par défaut en implémentant <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="e4b42-577">`IOrderedFilter` expose la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> qui est prioritaire sur l’étendue afin de déterminer l’ordre d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e4b42-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="e4b42-578">Un filtre avec une valeur `Order` inférieure :</span><span class="sxs-lookup"><span data-stu-id="e4b42-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="e4b42-579">Exécute le code *avant* avant celui d’un filtre avec une valeur supérieure à `Order`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="e4b42-580">Exécute le code *après* après celui d’un filtre avec une valeur `Order` supérieure.</span><span class="sxs-lookup"><span data-stu-id="e4b42-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="e4b42-581">La propriété `Order` peut être définie avec un paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="e4b42-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="e4b42-582">Prenez en compte les mêmes 3 filtres d’actions indiqués dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="e4b42-583">Si la propriété `Order` du contrôleur et les filtres globaux sont définis sur 1 et 2 respectivement, l’ordre d’exécution est inversé.</span><span class="sxs-lookup"><span data-stu-id="e4b42-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="e4b42-584">Séquence</span><span class="sxs-lookup"><span data-stu-id="e4b42-584">Sequence</span></span> | <span data-ttu-id="e4b42-585">Étendue de filtre</span><span class="sxs-lookup"><span data-stu-id="e4b42-585">Filter scope</span></span> | <span data-ttu-id="e4b42-586">Propriété`Order`</span><span class="sxs-lookup"><span data-stu-id="e4b42-586">`Order` property</span></span> | <span data-ttu-id="e4b42-587">Filter, méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="e4b42-588">1</span><span class="sxs-lookup"><span data-stu-id="e4b42-588">1</span></span> | <span data-ttu-id="e4b42-589">Méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-589">Method</span></span> | <span data-ttu-id="e4b42-590">0</span><span class="sxs-lookup"><span data-stu-id="e4b42-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="e4b42-591">2</span><span class="sxs-lookup"><span data-stu-id="e4b42-591">2</span></span> | <span data-ttu-id="e4b42-592">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="e4b42-592">Controller</span></span> | <span data-ttu-id="e4b42-593">1</span><span class="sxs-lookup"><span data-stu-id="e4b42-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="e4b42-594">3</span><span class="sxs-lookup"><span data-stu-id="e4b42-594">3</span></span> | <span data-ttu-id="e4b42-595">Global</span><span class="sxs-lookup"><span data-stu-id="e4b42-595">Global</span></span> | <span data-ttu-id="e4b42-596">2</span><span class="sxs-lookup"><span data-stu-id="e4b42-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="e4b42-597">4</span><span class="sxs-lookup"><span data-stu-id="e4b42-597">4</span></span> | <span data-ttu-id="e4b42-598">Global</span><span class="sxs-lookup"><span data-stu-id="e4b42-598">Global</span></span> | <span data-ttu-id="e4b42-599">2</span><span class="sxs-lookup"><span data-stu-id="e4b42-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="e4b42-600">5</span><span class="sxs-lookup"><span data-stu-id="e4b42-600">5</span></span> | <span data-ttu-id="e4b42-601">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="e4b42-601">Controller</span></span> | <span data-ttu-id="e4b42-602">1</span><span class="sxs-lookup"><span data-stu-id="e4b42-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="e4b42-603">6</span><span class="sxs-lookup"><span data-stu-id="e4b42-603">6</span></span> | <span data-ttu-id="e4b42-604">Méthode</span><span class="sxs-lookup"><span data-stu-id="e4b42-604">Method</span></span> | <span data-ttu-id="e4b42-605">0</span><span class="sxs-lookup"><span data-stu-id="e4b42-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="e4b42-606">La propriété `Order` remplace l’étendue lors de la détermination de l’ordre dans lequel les filtres s’exécutent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="e4b42-607">Les filtres sont d’abord classés par ordre, puis l’étendue est utilisée pour couper les liens.</span><span class="sxs-lookup"><span data-stu-id="e4b42-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="e4b42-608">Tous les filtres intégrés implémentent `IOrderedFilter` et affectent 0 à la valeur `Order` par défaut.</span><span class="sxs-lookup"><span data-stu-id="e4b42-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="e4b42-609">Pour les filtres intégrés, l’étendue détermine l’ordre, sauf si `Order` est défini sur une valeur différente de zéro.</span><span class="sxs-lookup"><span data-stu-id="e4b42-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="e4b42-610">Annulation et court-circuit</span><span class="sxs-lookup"><span data-stu-id="e4b42-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="e4b42-611">Vous pouvez court-circuiter le pipeline de filtres en définissant la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> sur le paramètre <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> fourni à la méthode de filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="e4b42-612">Par exemple, le filtre de ressources suivant empêche l’exécution du reste du pipeline :</span><span class="sxs-lookup"><span data-stu-id="e4b42-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-613">Dans le code suivant, les filtres `ShortCircuitingResourceFilter` et `AddHeader` ciblent tous deux la méthode d’action `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="e4b42-614">Voici le `ShortCircuitingResourceFilter` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="e4b42-615">S’exécute en premier (puisqu’il s’agit d’un filtre de ressources et que `AddHeader` est un filtre d’action).</span><span class="sxs-lookup"><span data-stu-id="e4b42-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="e4b42-616">Court-circuite le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="e4b42-617">Le filtre `AddHeader` ne s’exécute donc jamais pour l’action `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="e4b42-618">Ce comportement est le même si les deux filtres sont appliqués au niveau de la méthode d’action, à condition que `ShortCircuitingResourceFilter` soit exécuté en premier.</span><span class="sxs-lookup"><span data-stu-id="e4b42-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="e4b42-619">`ShortCircuitingResourceFilter` s’exécute en premier en raison de son type de filtre ou de l’utilisation explicite de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="e4b42-620">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e4b42-620">Dependency injection</span></span>

<span data-ttu-id="e4b42-621">Les filtres peuvent être ajoutés par type ou par instance.</span><span class="sxs-lookup"><span data-stu-id="e4b42-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="e4b42-622">Si vous ajoutez une instance, celle-ci sera utilisée pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="e4b42-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="e4b42-623">Si un type est ajouté, il est activé par type.</span><span class="sxs-lookup"><span data-stu-id="e4b42-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="e4b42-624">Un filtre activé par type signifie :</span><span class="sxs-lookup"><span data-stu-id="e4b42-624">A type-activated filter means:</span></span>

* <span data-ttu-id="e4b42-625">Une instance est créée pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="e4b42-625">An instance is created for each request.</span></span>
* <span data-ttu-id="e4b42-626">Les dépendances de constructeur sont remplies par [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="e4b42-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="e4b42-627">Les filtres qui sont implémentés en tant qu’attributs et ajoutés directement à des classes de contrôleur ou à de méthodes d’action ne peut pas avoir de dépendances de constructeur fournies par [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4b42-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="e4b42-628">Les dépendances de constructeur ne peuvent pas être fournies par l’injection de dépendance, car :</span><span class="sxs-lookup"><span data-stu-id="e4b42-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="e4b42-629">Les paramètres du constructeur des attributs doivent être fournis là où ils sont appliqués.</span><span class="sxs-lookup"><span data-stu-id="e4b42-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="e4b42-630">Il s’agit d’une limitation de la façon dont les attributs fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="e4b42-631">Les filtres suivants prennent en charge les dépendances de constructeur fournies à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="e4b42-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="e4b42-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implémenté sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="e4b42-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="e4b42-633">Les filtres précédents peuvent être appliqués à une méthode de contrôleur ou d’action :</span><span class="sxs-lookup"><span data-stu-id="e4b42-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="e4b42-634">Des enregistreurs d’événements sont disponibles à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-634">Loggers are available from DI.</span></span> <span data-ttu-id="e4b42-635">Toutefois, évitez la création et l’utilisation de filtres à des fins purement d’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e4b42-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="e4b42-636">L’[enregistrement d’infrastructure intégrée](xref:fundamentals/logging/index) fournit généralement ce qui est nécessaire pour l’enregistrement.</span><span class="sxs-lookup"><span data-stu-id="e4b42-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="e4b42-637">Enregistrement ajouté aux filtres :</span><span class="sxs-lookup"><span data-stu-id="e4b42-637">Logging added to filters:</span></span>

* <span data-ttu-id="e4b42-638">Doit se concentrer sur les problèmes ou le comportement du domaine métier spécifique au filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="e4b42-639">Ne doit **pas** enregistrer des actions ou d’autres événements d’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="e4b42-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="e4b42-640">Les filtres intégrés enregistrent des actions et des événements d’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="e4b42-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="e4b42-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="e4b42-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="e4b42-642">Les types d’implémentation du filtre de services sont enregistrés dans `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="e4b42-643">Un <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> récupère une instance du filtre à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="e4b42-644">Le code suivant affiche le `AddHeaderResultServiceFilter` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="e4b42-645">Dans le code suivant, `AddHeaderResultServiceFilter` est ajouté au conteneur d’injection de dépendance :</span><span class="sxs-lookup"><span data-stu-id="e4b42-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="e4b42-646">Dans le code suivant, l’attribut `ServiceFilter` récupère une instance du filtre `AddHeaderResultServiceFilter` à partir de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="e4b42-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="e4b42-647">Lors de l’utilisation de `ServiceFilterAttribute`, définir [ServiceFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="e4b42-648">Conseille que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle elle a été créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="e4b42-649">Le runtime ASP.NET Core ne garantit pas :</span><span class="sxs-lookup"><span data-stu-id="e4b42-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="e4b42-650">Qu’une seule instance du filtre sera créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="e4b42-651">Le filtre ne sera pas demandé à nouveau à partir du conteneur d’injection de dépendance à un stade ultérieur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="e4b42-652">Évitez de l’utiliser avec un filtre qui dépend de services avec une durée de vie autre que singleton.</span><span class="sxs-lookup"><span data-stu-id="e4b42-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="e4b42-653">L'objet <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="e4b42-654">`IFilterFactory` expose la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> pour la création d’une instance <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="e4b42-655">`CreateInstance` charge le type spécifié à partir de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="e4b42-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="e4b42-656">TypeFilterAttribute</span></span>

<span data-ttu-id="e4b42-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> est similaire à <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, mais son type n’est pas résolu directement à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="e4b42-658">Il instancie le type en utilisant <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="e4b42-659">Étant donné que les types `TypeFilterAttribute` ne sont pas résolus directement à partir du conteneur d’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="e4b42-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="e4b42-660">Les types qui sont référencés avec `TypeFilterAttribute` ne doivent pas d’abord être inscrits auprès du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="e4b42-661">Leurs dépendances ne sont pas remplies par le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="e4b42-662">`TypeFilterAttribute` peut éventuellement accepter des arguments de constructeur pour le type.</span><span class="sxs-lookup"><span data-stu-id="e4b42-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="e4b42-663">Lors de l’utilisation de `TypeFilterAttribute`, définir [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="e4b42-664">Indique que l’instance de filtre *peut* être réutilisée en dehors de l’étendue de la requête dans laquelle elle a été créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="e4b42-665">Le runtime ASP.NET Core ne fournit aucune garantie qu’une seule instance du filtre sera créée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="e4b42-666">Évitez de l’utiliser avec un filtre qui dépend de services avec une durée de vie autre que singleton.</span><span class="sxs-lookup"><span data-stu-id="e4b42-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="e4b42-667">L’exemple suivant indique comment passer des arguments vers un type en utilisant `TypeFilterAttribute` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="e4b42-668">Filtres d'autorisation</span><span class="sxs-lookup"><span data-stu-id="e4b42-668">Authorization filters</span></span>

<span data-ttu-id="e4b42-669">Filtres d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="e4b42-669">Authorization filters:</span></span>

* <span data-ttu-id="e4b42-670">Sont les premiers filtres à être exécutés dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="e4b42-671">Contrôlent l’accès aux méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-671">Control access to action methods.</span></span>
* <span data-ttu-id="e4b42-672">Ont une méthode avant, mais pas de méthode après.</span><span class="sxs-lookup"><span data-stu-id="e4b42-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="e4b42-673">Les filtres d’autorisations personnalisés nécessitent une infrastructure d’autorisation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="e4b42-674">Préférez la configuration des stratégies d’autorisation ou l’écriture d’une stratégie d’autorisation personnalisée à l’écriture d’un filtre personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e4b42-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="e4b42-675">Le filtre d'autorisations intégré :</span><span class="sxs-lookup"><span data-stu-id="e4b42-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="e4b42-676">Appelle le système d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e4b42-676">Calls the authorization system.</span></span>
* <span data-ttu-id="e4b42-677">N’autoriser les requêtes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-677">Does not authorize requests.</span></span>

<span data-ttu-id="e4b42-678">Ne levez **pas** d’exceptions dans les filtres d’autorisations :</span><span class="sxs-lookup"><span data-stu-id="e4b42-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="e4b42-679">L’exception ne sera pas gérée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-679">The exception will not be handled.</span></span>
* <span data-ttu-id="e4b42-680">Les filtres d’exceptions ne gèreront pas l’exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="e4b42-681">Songez à émettre une stimulation quand un filtre d’autorisations se produit.</span><span class="sxs-lookup"><span data-stu-id="e4b42-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="e4b42-682">Découvrez plus d’informations sur [l’autorisation](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="e4b42-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="e4b42-683">Filtres de ressources</span><span class="sxs-lookup"><span data-stu-id="e4b42-683">Resource filters</span></span>

<span data-ttu-id="e4b42-684">Filtres de ressources :</span><span class="sxs-lookup"><span data-stu-id="e4b42-684">Resource filters:</span></span>

* <span data-ttu-id="e4b42-685">Implémentez l’interface <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="e4b42-686">L’exécution inclut dans un wrapper la majeure partie du pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="e4b42-687">Seuls les [filtres d’autorisations](#authorization-filters) s’exécutent avant les filtres de ressources.</span><span class="sxs-lookup"><span data-stu-id="e4b42-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="e4b42-688">Les filtres de ressources sont utiles pour court-circuiter la majeure partie du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="e4b42-689">Par exemple, un filtre de mise en cache peut éviter le reste du pipeline dans une correspondance dans le cache.</span><span class="sxs-lookup"><span data-stu-id="e4b42-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="e4b42-690">Exemples de filtre de ressources :</span><span class="sxs-lookup"><span data-stu-id="e4b42-690">Resource filter examples:</span></span>

* <span data-ttu-id="e4b42-691">[Le filtre de ressources de court-circuit](#short-circuiting-resource-filter) illustré précédemment.</span><span class="sxs-lookup"><span data-stu-id="e4b42-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="e4b42-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="e4b42-693">Il empêche la liaison de données d’accéder aux données de formulaire.</span><span class="sxs-lookup"><span data-stu-id="e4b42-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="e4b42-694">Il est utilisé pour les chargements de fichiers volumineux et pour empêcher que le formulaire de données ne soit lu en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e4b42-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="e4b42-695">Filtres d'action</span><span class="sxs-lookup"><span data-stu-id="e4b42-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4b42-696">Les filtres d’action ne s’appliquent **pas** aux Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e4b42-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="e4b42-697">Razor Pages prennent en charge <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span><span class="sxs-lookup"><span data-stu-id="e4b42-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="e4b42-698">Pour plus d’informations, consultez [Méthodes de filtre pour les pages Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="e4b42-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="e4b42-699">Filtres d'actions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-699">Action filters:</span></span>

* <span data-ttu-id="e4b42-700">Implémentez l’interface <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="e4b42-701">Leur exécution entoure l’exécution de méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="e4b42-702">Le code suivant montre un exemple de filtre d’actions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="e4b42-703"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> fournit les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="e4b42-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> : permet de lire les entrées vers une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="e4b42-705"><xref:Microsoft.AspNetCore.Mvc.Controller> : permet de manipuler l’instance de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4b42-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="e4b42-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> : la définition de `Result` court-circuite l’exécution de la méthode d’action et les filtres d’actions suivants.</span><span class="sxs-lookup"><span data-stu-id="e4b42-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="e4b42-707">Levée d’une exception dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="e4b42-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="e4b42-708">Empêche l’exécution des filtres suivants.</span><span class="sxs-lookup"><span data-stu-id="e4b42-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="e4b42-709">Contrairement au paramètre `Result`, est traité comme un échec plutôt que comme un résultat positif.</span><span class="sxs-lookup"><span data-stu-id="e4b42-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="e4b42-710"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> fournit `Controller` et `Result`, en plus des propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4b42-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="e4b42-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> : sa valeur est true si l’exécution de l’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="e4b42-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> : sa valeur est non Null si l’action ou un filtre d’actions précédemment exécuté a lancé une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="e4b42-713">Paramètre de cette propriété sur null :</span><span class="sxs-lookup"><span data-stu-id="e4b42-713">Setting this property to null:</span></span>

  * <span data-ttu-id="e4b42-714">Gère effectivement une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="e4b42-715">`Result` est exécuté comme s’il avait été retourné à partir de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="e4b42-716">Pour un `IAsyncActionFilter`, un appel à <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> :</span><span class="sxs-lookup"><span data-stu-id="e4b42-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="e4b42-717">Exécute tous les filtres d’actions suivants et la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="e4b42-718">Renvoie `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="e4b42-719">Pour court-circuiter, attribuez <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> à une instance de résultat et n’appelez pas le `next` (le `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="e4b42-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="e4b42-720">L’infrastructure fournit un <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> abstrait que vous pouvez placer dans une sous-classe.</span><span class="sxs-lookup"><span data-stu-id="e4b42-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="e4b42-721">Le filtre d’actions `OnActionExecuting` peut être utilisé pour :</span><span class="sxs-lookup"><span data-stu-id="e4b42-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="e4b42-722">Valider l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="e4b42-722">Validate model state.</span></span>
* <span data-ttu-id="e4b42-723">Renvoyer une erreur si l’état n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="e4b42-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="e4b42-724">La méthode `OnActionExecuted` s’exécute après la méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="e4b42-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="e4b42-725">Et peut voir et manipuler les résultats de l’action via la propriété <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="e4b42-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> est défini sur true si l’exécution de l’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="e4b42-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> est défini sur une valeur non Null si l’action ou un filtre d’actions suivant a levé une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="e4b42-728">Le fait de définir `Exception` avec la valeur null :</span><span class="sxs-lookup"><span data-stu-id="e4b42-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="e4b42-729">Gère effectivement une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="e4b42-730">`ActionExecutedContext.Result` est exécuté comme s’il avait été retourné normalement à partir de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="e4b42-731">Filtres d'exception</span><span class="sxs-lookup"><span data-stu-id="e4b42-731">Exception filters</span></span>

<span data-ttu-id="e4b42-732">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-732">Exception filters:</span></span>

* <span data-ttu-id="e4b42-733">Implémentent <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="e4b42-734">Ils peuvent être utilisés pour implémenter des stratégies de gestion des erreurs communes.</span><span class="sxs-lookup"><span data-stu-id="e4b42-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="e4b42-735">L’échantillon de filtre d’exceptions suivant utilise un affichage personnalisé des erreurs pour afficher des détails sur les exceptions qui se produisent pendant que l’application est en développement :</span><span class="sxs-lookup"><span data-stu-id="e4b42-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="e4b42-736">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-736">Exception filters:</span></span>

* <span data-ttu-id="e4b42-737">N’ont pas d’événements avant et après.</span><span class="sxs-lookup"><span data-stu-id="e4b42-737">Don't have before and after events.</span></span>
* <span data-ttu-id="e4b42-738">Implémentent <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="e4b42-739">Gèrent des exceptions non prises en charge qui se produisent dans Razor Page ou dans la création des contrôleurs, la [liaison de données](xref:mvc/models/model-binding), les filtres d’actions ou les méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="e4b42-740">N’interceptent **pas** les exceptions qui se produisent dans l’exécution des filtres de ressources, des filtres de résultats ou des résultats MVC.</span><span class="sxs-lookup"><span data-stu-id="e4b42-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="e4b42-741">Pour gérer une exception, définissez la propriété <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> sur `true` ou écrivez une réponse.</span><span class="sxs-lookup"><span data-stu-id="e4b42-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="e4b42-742">Ceci arrête la propagation de l’exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-742">This stops propagation of the exception.</span></span> <span data-ttu-id="e4b42-743">Un filtre d’exceptions ne peut pas changer une exception en « réussite ».</span><span class="sxs-lookup"><span data-stu-id="e4b42-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="e4b42-744">Seul un filtre d’actions peut le faire.</span><span class="sxs-lookup"><span data-stu-id="e4b42-744">Only an action filter can do that.</span></span>

<span data-ttu-id="e4b42-745">Les filtres d’exceptions :</span><span class="sxs-lookup"><span data-stu-id="e4b42-745">Exception filters:</span></span>

* <span data-ttu-id="e4b42-746">Conviennent pour intercepter des exceptions qui se produisent dans des actions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="e4b42-747">N’offrent pas la même souplesse que le middleware (intergiciel) de gestion des erreurs.</span><span class="sxs-lookup"><span data-stu-id="e4b42-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="e4b42-748">Privilégiez le middleware pour la gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="e4b42-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="e4b42-749">Utilisez les filtres d’exceptions uniquement lorsque la gestion des erreurs *diffère* selon la méthode d’action appelée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="e4b42-750">Par exemple, votre application peut avoir des méthodes d’action à la fois pour des points de terminaison d’API et pour des affichages/HTML.</span><span class="sxs-lookup"><span data-stu-id="e4b42-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="e4b42-751">Les points de terminaison d’API peuvent retourner des informations d’erreur au format JSON, tandis que les actions basées sur une vue peuvent retourner une page d’erreur au format HTML.</span><span class="sxs-lookup"><span data-stu-id="e4b42-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="e4b42-752">Filtres de résultats</span><span class="sxs-lookup"><span data-stu-id="e4b42-752">Result filters</span></span>

<span data-ttu-id="e4b42-753">Filtres de résultats :</span><span class="sxs-lookup"><span data-stu-id="e4b42-753">Result filters:</span></span>

* <span data-ttu-id="e4b42-754">Implémenter une interface :</span><span class="sxs-lookup"><span data-stu-id="e4b42-754">Implement an interface:</span></span>
  * <span data-ttu-id="e4b42-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="e4b42-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="e4b42-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ou <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="e4b42-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="e4b42-757">Leur exécution entoure l’exécution de résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="e4b42-758">IResultFilter et IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="e4b42-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="e4b42-759">Le code suivant indique un filtre de résultats qui ajoute un en-tête HTTP :</span><span class="sxs-lookup"><span data-stu-id="e4b42-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="e4b42-760">Le type de résultat à exécuter dépend de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="e4b42-761">Une action retournant un affichage inclut tous les traitements Razor dans le cadre de la <xref:Microsoft.AspNetCore.Mvc.ViewResult> à exécuter.</span><span class="sxs-lookup"><span data-stu-id="e4b42-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="e4b42-762">Une méthode d’API peut effectuer une sérialisation dans le cadre de l’exécution du résultat.</span><span class="sxs-lookup"><span data-stu-id="e4b42-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="e4b42-763">En savoir plus sur les [résultats des actions](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="e4b42-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="e4b42-764">Les filtres de résultats ne sont exécutés que lorsqu’une action ou un filtre d’action produit un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="e4b42-765">Les filtres de résultats ne sont pas exécutés dans les cas suivants :</span><span class="sxs-lookup"><span data-stu-id="e4b42-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="e4b42-766">Un filtre d’autorisation ou des courts-circuits de filtre de ressources le pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="e4b42-767">Un filtre d’exception gère une exception en générant un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="e4b42-768">La méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> peut court-circuiter l’exécution du résultat d’action et les filtres de résultats suivants en définissant <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> sur `true`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="e4b42-769">Écrivez dans l’objet de réponse quand vous court-circuitez pour éviter de générer une réponse vide.</span><span class="sxs-lookup"><span data-stu-id="e4b42-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="e4b42-770">La levée d’une exception dans `IResultFilter.OnResultExecuting` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="e4b42-771">Empêche l’exécution du résultat d’action et des filtres suivants.</span><span class="sxs-lookup"><span data-stu-id="e4b42-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="e4b42-772">Est traitée comme une erreur et non comme une réussite.</span><span class="sxs-lookup"><span data-stu-id="e4b42-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="e4b42-773">Lorsque la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> s’exécute, la réponse a probablement déjà été envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="e4b42-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="e4b42-774">Si la réponse a déjà été envoyée au client, elle ne peut plus être modifiée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="e4b42-775">`ResultExecutedContext.Canceled` est défini sur `true` si l’exécution du résultat d’action a été court-circuitée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="e4b42-776">`ResultExecutedContext.Exception` est défini sur une valeur non Null si le résultat d’action ou un filtre de résultats suivant a levé une exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="e4b42-777">Le paramètre de `Exception` sur Null gère effectivement une exception et évite à l’exception d’être à nouveau levée par ASP.NET Core plus loin dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="e4b42-778">Il n’existe aucun moyen fiable pour écrire des données dans une réponse lors de la gestion d’une exception dans un filtre de résultats.</span><span class="sxs-lookup"><span data-stu-id="e4b42-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="e4b42-779">Si les en-têtes ont été vidés pour le client lorsqu’un résultat d’action lance une exception, il n’existe aucun mécanisme fiable pour envoyer un code d’échec.</span><span class="sxs-lookup"><span data-stu-id="e4b42-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="e4b42-780">Pour un <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, un appel à `await next` sur le <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> exécute tous les filtres de résultats suivants et le résultat de l’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="e4b42-781">Pour court-circuiter, définissez [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) sur `true` et n’appelez pas `ResultExecutionDelegate` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="e4b42-782">L’infrastructure fournit un `ResultFilterAttribute` abstrait que vous pouvez placer dans une sous-classe.</span><span class="sxs-lookup"><span data-stu-id="e4b42-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="e4b42-783">La classe [AddHeaderAttribute](#add-header-attribute) ci-dessus est un exemple d’un attribut de filtre de résultats.</span><span class="sxs-lookup"><span data-stu-id="e4b42-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="e4b42-784">IAlwaysRunResultFilter et IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="e4b42-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="e4b42-785">Les interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> et <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> déclarent une implémentation <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> qui s’exécute pour tous les résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="e4b42-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="e4b42-786">Cela comprend les résultats d’action produits par :</span><span class="sxs-lookup"><span data-stu-id="e4b42-786">This includes action results produced by:</span></span>

* <span data-ttu-id="e4b42-787">Filtres d’autorisation et filtres de ressources qui court-circuitent.</span><span class="sxs-lookup"><span data-stu-id="e4b42-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="e4b42-788">Filtres d’exception.</span><span class="sxs-lookup"><span data-stu-id="e4b42-788">Exception filters.</span></span>

<span data-ttu-id="e4b42-789">Par exemple, le filtre suivant exécute et définit toujours un résultat d’action (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) avec un code d’état *422 Entité non traitée* en cas d’échec de la négociation de contenu :</span><span class="sxs-lookup"><span data-stu-id="e4b42-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="e4b42-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="e4b42-790">IFilterFactory</span></span>

<span data-ttu-id="e4b42-791">L'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="e4b42-792">Par conséquent, une instance de `IFilterFactory` peut être utilisée comme instance de `IFilterMetadata` n’importe où dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="e4b42-793">Quan se prépare à appeler le filtre, il tente de le caster en `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="e4b42-794">Si ce cast réussit, la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> est appelée pour créer l’instance `IFilterMetadata` qui sera appelée.</span><span class="sxs-lookup"><span data-stu-id="e4b42-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="e4b42-795">La conception est flexible, car il n’est pas nécessaire de définir explicitement le pipeline de filtres exact quand l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="e4b42-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="e4b42-796">Une autre approche pour la création de filtres est d’implémenter `IFilterFactory` à l’aide des implémentations d’attribut personnalisé :</span><span class="sxs-lookup"><span data-stu-id="e4b42-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="e4b42-797">Le code précédent peut être testé en exécutant l’[échantillon de téléchargement](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) :</span><span class="sxs-lookup"><span data-stu-id="e4b42-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="e4b42-798">Appeler les outils de développement F12.</span><span class="sxs-lookup"><span data-stu-id="e4b42-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="e4b42-799">Accédez à `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="e4b42-800">Les outils de développement F12 affichent les en-têtes de réponse suivants ajoutés par l’exemple de code :</span><span class="sxs-lookup"><span data-stu-id="e4b42-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="e4b42-801">**Auteur :** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="e4b42-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="e4b42-802">**globaladdheader :** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="e4b42-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="e4b42-803">**interne :** `My header`</span><span class="sxs-lookup"><span data-stu-id="e4b42-803">**internal:** `My header`</span></span>

<span data-ttu-id="e4b42-804">Le code précédent crée l’en-tête de réponse **Internal :** `My header`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="e4b42-805">IFilterFactory implémenté sur un attribut</span><span class="sxs-lookup"><span data-stu-id="e4b42-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="e4b42-806">Les filtres qui implémentent `IFilterFactory` sont utiles pour les filtres qui :</span><span class="sxs-lookup"><span data-stu-id="e4b42-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="e4b42-807">Ne nécessitent pas le passage de paramètres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="e4b42-808">Disposent de dépendances de constructeur qui doivent être remplies par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4b42-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="e4b42-809">L'objet <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implémente l'objet <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="e4b42-810">`IFilterFactory` expose la méthode <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> pour la création d’une instance <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="e4b42-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="e4b42-811">`CreateInstance` charge le type spécifié à partir du conteneur de services (injection de dépendances).</span><span class="sxs-lookup"><span data-stu-id="e4b42-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="e4b42-812">Le code suivant illustre trois approches pour appliquer `[SampleActionFilter]` :</span><span class="sxs-lookup"><span data-stu-id="e4b42-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="e4b42-813">Dans le code précédent, la décoration de la méthode avec `[SampleActionFilter]` est la meilleure approche pour appliquer `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="e4b42-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="e4b42-814">Utilisation d’un intergiciel dans le pipeline de filtres</span><span class="sxs-lookup"><span data-stu-id="e4b42-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="e4b42-815">Les filtres de ressources fonctionnent comme un [intergiciel](xref:fundamentals/middleware/index) dans la mesure où ils entourent l’exécution de tout ce qui vient plus tard dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="e4b42-816">Les filtres diffèrent cependant d’un intergiciel dans la mesure où ils font partie du runtime ASP.NET Core, ce qui signifie qu’ils ont accès au contexte et aux constructions ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4b42-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="e4b42-817">Pour utiliser un intergiciel comme filtre, créez un type avec une méthode `Configure` qui spécifie l’intergiciel que vous voulez injecter dans le pipeline de filtres.</span><span class="sxs-lookup"><span data-stu-id="e4b42-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="e4b42-818">Voici un exemple qui utilise l’intergiciel de localisation pour établir la culture actuelle d’une requête :</span><span class="sxs-lookup"><span data-stu-id="e4b42-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="e4b42-819">Utilisez le <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> pour exécuter l’intergiciel :</span><span class="sxs-lookup"><span data-stu-id="e4b42-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="e4b42-820">Les filtres d’intergiciels s’exécutent à la même étape du pipeline de filtres en tant que filtres de ressources, avant la liaison de modèle et après le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="e4b42-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="e4b42-821">Actions suivantes</span><span class="sxs-lookup"><span data-stu-id="e4b42-821">Next actions</span></span>

* <span data-ttu-id="e4b42-822">Consultez [les méthodes de filtre pour Razor pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="e4b42-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="e4b42-823">Pour expérimenter les filtres, [téléchargez, testez et modifiez l’échantillon Github](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="e4b42-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
