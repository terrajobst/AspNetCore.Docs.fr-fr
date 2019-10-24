---
title: Conventions de routes et d’applications pour les pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment les conventions du fournisseur de modèles de routes et d’applications vous permettent de gérer le routage, la découverte et le traitement des pages.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779218"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="97380-103">Conventions de routes et d’applications pour les pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97380-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="97380-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="97380-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="97380-105">Découvrez comment utiliser les [conventions du fournisseur de modèles de routes et d’applications](xref:mvc/controllers/application-model#conventions) pour contrôler le routage, la découverte et le traitement des pages dans les applications à base de pages Razor.</span><span class="sxs-lookup"><span data-stu-id="97380-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="97380-106">Quand vous devez configurer des routages de pages personnalisés pour des pages individuelles, configurez le routage vers les pages avec la convention [AddPageRoute](#configure-a-page-route) décrite plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="97380-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="97380-107">Pour spécifier un itinéraire de page, ajouter des segments de routage ou ajouter des paramètres à un itinéraire, utilisez la directive `@page` de la page.</span><span class="sxs-lookup"><span data-stu-id="97380-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="97380-108">Pour plus d’informations, consultez [itinéraires personnalisés](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="97380-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="97380-109">Il existe des mots réservés qui ne peuvent pas être utilisés en tant que segments de routage ou noms de paramètres.</span><span class="sxs-lookup"><span data-stu-id="97380-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="97380-110">Pour plus d’informations, consultez [routage : noms de routage réservés](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="97380-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="97380-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97380-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="97380-112">Scénario</span><span class="sxs-lookup"><span data-stu-id="97380-112">Scenario</span></span> | <span data-ttu-id="97380-113">L’exemple montre...</span><span class="sxs-lookup"><span data-stu-id="97380-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="97380-114">Conventions de modèle</span><span class="sxs-lookup"><span data-stu-id="97380-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="97380-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="97380-115">Conventions.Add</span></span><ul><li><span data-ttu-id="97380-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="97380-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="97380-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="97380-119">Ajouter un modèle de route et un en-tête aux pages d’une application.</span><span class="sxs-lookup"><span data-stu-id="97380-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="97380-120">Conventions d’actions de routage de pages</span><span class="sxs-lookup"><span data-stu-id="97380-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="97380-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="97380-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="97380-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="97380-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="97380-124">Ajouter un modèle de route aux pages d’un dossier et à une page unique.</span><span class="sxs-lookup"><span data-stu-id="97380-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="97380-125">Conventions d’actions de modèle de page</span><span class="sxs-lookup"><span data-stu-id="97380-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="97380-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="97380-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="97380-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="97380-128">ConfigureFilter (classe de filtre, expression lambda ou fabrique de filtres)</span><span class="sxs-lookup"><span data-stu-id="97380-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="97380-129">Ajouter un en-tête dans les pages d’un dossier, ajouter un en-tête dans une page unique et configurer une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) pour ajouter un en-tête dans les pages d’une application.</span><span class="sxs-lookup"><span data-stu-id="97380-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="97380-130">Les conventions d’Razor Pages sont ajoutées et configurées à l’aide de la méthode d’extension <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> pour <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> sur la collection de services de la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="97380-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="97380-131">Les exemples de convention suivants sont expliqués plus loin dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="97380-131">The following convention examples are explained later in this topic:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a><span data-ttu-id="97380-132">Ordre de routage</span><span class="sxs-lookup"><span data-stu-id="97380-132">Route order</span></span>

<span data-ttu-id="97380-133">Les itinéraires spécifient un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> pour le traitement (correspondance d’itinéraires).</span><span class="sxs-lookup"><span data-stu-id="97380-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="97380-134">Trier</span><span class="sxs-lookup"><span data-stu-id="97380-134">Order</span></span>            | <span data-ttu-id="97380-135">Comportement</span><span class="sxs-lookup"><span data-stu-id="97380-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="97380-136">-1</span><span class="sxs-lookup"><span data-stu-id="97380-136">-1</span></span>               | <span data-ttu-id="97380-137">L’itinéraire est traité avant le traitement des autres itinéraires.</span><span class="sxs-lookup"><span data-stu-id="97380-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="97380-138">0</span><span class="sxs-lookup"><span data-stu-id="97380-138">0</span></span>                | <span data-ttu-id="97380-139">L’ordre n’est pas spécifié (valeur par défaut).</span><span class="sxs-lookup"><span data-stu-id="97380-139">Order isn't specified (default value).</span></span> <span data-ttu-id="97380-140">Si vous n’affectez pas de `Order` (`Order = null`), la `Order` d’itinéraire est définie par défaut sur 0 (zéro) pour le traitement.</span><span class="sxs-lookup"><span data-stu-id="97380-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="97380-141">1,2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="97380-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="97380-142">Spécifie l’ordre de traitement des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="97380-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="97380-143">Le traitement des itinéraires est établi par Convention :</span><span class="sxs-lookup"><span data-stu-id="97380-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="97380-144">Les itinéraires sont traités dans l’ordre séquentiel (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="97380-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="97380-145">Lorsque les itinéraires ont le même `Order`, l’itinéraire le plus spécifique est mis en correspondance en premier, suivi d’itinéraires moins spécifiques.</span><span class="sxs-lookup"><span data-stu-id="97380-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="97380-146">Lorsque les itinéraires avec le même `Order` et le même nombre de paramètres correspondent à une URL de demande, les itinéraires sont traités dans l’ordre dans lequel ils sont ajoutés à la <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="97380-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="97380-147">Si possible, évitez de dépendre d’un ordre de traitement des itinéraires établi.</span><span class="sxs-lookup"><span data-stu-id="97380-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="97380-148">En règle générale, le routage sélectionne l’itinéraire correct avec la correspondance d’URL.</span><span class="sxs-lookup"><span data-stu-id="97380-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="97380-149">Si vous devez définir les propriétés de `Order` de routage pour acheminer les demandes correctement, le schéma de routage de l’application est sans doute confus pour les clients et fragiles à gérer.</span><span class="sxs-lookup"><span data-stu-id="97380-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="97380-150">Cherchez à simplifier le schéma de routage de l’application.</span><span class="sxs-lookup"><span data-stu-id="97380-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="97380-151">L’exemple d’application requiert un ordre de traitement des itinéraires explicites pour illustrer plusieurs scénarios de routage à l’aide d’une seule application.</span><span class="sxs-lookup"><span data-stu-id="97380-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="97380-152">Toutefois, vous devez essayer d’éviter de définir des `Order` de routage dans des applications de production.</span><span class="sxs-lookup"><span data-stu-id="97380-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="97380-153">Le routage de Razor Pages et celui du contrôleur MVC partagent une implémentation.</span><span class="sxs-lookup"><span data-stu-id="97380-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="97380-154">Des informations sur l’ordre des itinéraires dans les rubriques MVC sont disponibles dans [routage vers actions du contrôleur : classement des itinéraires des attributs](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="97380-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="97380-155">Conventions de modèle</span><span class="sxs-lookup"><span data-stu-id="97380-155">Model conventions</span></span>

<span data-ttu-id="97380-156">Ajoutez un délégué pour <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> pour ajouter des [conventions de modèle](xref:mvc/controllers/application-model#conventions) qui s’appliquent à Razor pages.</span><span class="sxs-lookup"><span data-stu-id="97380-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="97380-157">Ajouter une convention de modèle de routage à toutes les pages</span><span class="sxs-lookup"><span data-stu-id="97380-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="97380-158">Utilisez <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> à la collection d’instances de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> appliquées pendant la construction du modèle de routage de la page.</span><span class="sxs-lookup"><span data-stu-id="97380-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="97380-159">L’exemple d’application ajoute un modèle de routage `{globalTemplate?}` à toutes les pages de l’application :</span><span class="sxs-lookup"><span data-stu-id="97380-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="97380-160">La propriété <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> a la valeur `1`.</span><span class="sxs-lookup"><span data-stu-id="97380-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="97380-161">Cela garantit le comportement de correspondance d’itinéraire suivant dans l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="97380-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="97380-162">Un modèle de routage pour `TheContactPage/{text?}` est ajouté plus loin dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="97380-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="97380-163">L’itinéraire de la page de contact a un ordre par défaut de `null` (`Order = 0`), de sorte qu’il corresponde avant le modèle de routage `{globalTemplate?}`.</span><span class="sxs-lookup"><span data-stu-id="97380-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="97380-164">Un modèle de route `{aboutTemplate?}` est ajouté plus loin dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="97380-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="97380-165">Le modèle `{aboutTemplate?}` reçoit l’ordre `Order` avec la valeur `2`.</span><span class="sxs-lookup"><span data-stu-id="97380-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="97380-166">Quand la page About est demandée sur `/About/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["aboutTemplate"]` (`Order = 2`) en raison de la définition de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="97380-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="97380-167">Un modèle de route `{otherPagesTemplate?}` est ajouté plus loin dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="97380-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="97380-168">Le modèle `{otherPagesTemplate?}` reçoit l’ordre `Order` avec la valeur `2`.</span><span class="sxs-lookup"><span data-stu-id="97380-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="97380-169">Quand une page du dossier *pages/OtherPages* est demandée avec un paramètre d’itinéraire (par exemple, `/OtherPages/Page1/RouteDataValue`), « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) en raison de la définition de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="97380-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="97380-170">Dans la mesure du possible, ne définissez pas le `Order`, ce qui donne `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="97380-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="97380-171">Utilisez le routage pour sélectionner l’itinéraire correct.</span><span class="sxs-lookup"><span data-stu-id="97380-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="97380-172">Razor Pages options, telles que l’ajout de <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, sont ajoutées lorsque MVC est ajouté à la collection de services dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="97380-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="97380-173">Pour obtenir un exemple, consultez [l’exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="97380-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="97380-174">Demandez la page About de l’exemple sur `localhost:5000/About/GlobalRouteValue` et examinez le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![La page About est demandée avec un segment de routage pour GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="97380-177">Ajouter une convention de modèle d’application à toutes les pages</span><span class="sxs-lookup"><span data-stu-id="97380-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="97380-178">Utilisez <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> à la collection d’instances de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> appliquées pendant la construction du modèle d’application de la page.</span><span class="sxs-lookup"><span data-stu-id="97380-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="97380-179">Pour illustrer cette convention et bien d’autres, plus loin dans cette rubrique, l’exemple d’application inclut une classe `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="97380-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="97380-180">Le constructeur de classe accepte une chaîne `name` et un tableau de chaînes `values`.</span><span class="sxs-lookup"><span data-stu-id="97380-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="97380-181">Ces valeurs sont utilisées dans sa méthode `OnResultExecuting` pour définir un en-tête de réponse.</span><span class="sxs-lookup"><span data-stu-id="97380-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="97380-182">La classe complète est affichée dans la section [Conventions d’actions de modèle de page](#page-model-action-conventions), plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="97380-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="97380-183">L’exemple d’application utilise la classe `AddHeaderAttribute` pour ajouter un en-tête, `GlobalHeader`, à toutes les pages de l’application :</span><span class="sxs-lookup"><span data-stu-id="97380-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="97380-184">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="97380-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="97380-185">Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Les en-têtes de réponse de la page About montrent que GlobalHeader a été ajouté.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="97380-187">Ajouter une convention de modèle de gestionnaire à toutes les pages</span><span class="sxs-lookup"><span data-stu-id="97380-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="97380-188">Utilisez <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> à la collection d’instances de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> appliquées pendant la construction du modèle de gestionnaire de page.</span><span class="sxs-lookup"><span data-stu-id="97380-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="97380-189">*Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="97380-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="97380-190">Conventions d’actions de routage de pages</span><span class="sxs-lookup"><span data-stu-id="97380-190">Page route action conventions</span></span>

<span data-ttu-id="97380-191">Le fournisseur de modèles de routage par défaut qui dérive de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> appelle des conventions qui sont conçues pour fournir des points d’extensibilité pour la configuration des itinéraires de page.</span><span class="sxs-lookup"><span data-stu-id="97380-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="97380-192">Convention de modèle de routage de dossier</span><span class="sxs-lookup"><span data-stu-id="97380-192">Folder route model convention</span></span>

<span data-ttu-id="97380-193">Utilisez <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> qui appelle une action sur le <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> pour toutes les pages du dossier spécifié.</span><span class="sxs-lookup"><span data-stu-id="97380-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="97380-194">L’exemple d’application utilise <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> pour ajouter un modèle de routage `{otherPagesTemplate?}` aux pages du dossier *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="97380-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="97380-195">La propriété <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> a la valeur `2`.</span><span class="sxs-lookup"><span data-stu-id="97380-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="97380-196">Cela permet de s’assurer que le modèle de `{globalTemplate?}` (défini plus haut dans la rubrique pour `1`) est prioritaire pour la première position de valeur de données de route lorsqu’une valeur de route unique est fournie.</span><span class="sxs-lookup"><span data-stu-id="97380-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="97380-197">Si une page du dossier *pages/OtherPages* est demandée avec une valeur de paramètre d’itinéraire (par exemple, `/OtherPages/Page1/RouteDataValue`), « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) en raison de la définition de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="97380-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="97380-198">Dans la mesure du possible, ne définissez pas le `Order`, ce qui donne `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="97380-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="97380-199">Utilisez le routage pour sélectionner l’itinéraire correct.</span><span class="sxs-lookup"><span data-stu-id="97380-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="97380-200">Demandez la page Page1 de l’exemple sur `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` et examinez le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![La page Page1 du dossier OtherPages est demandée avec un segment de routage pour GlobalRouteValue et OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="97380-203">Convention de modèle de routage de page</span><span class="sxs-lookup"><span data-stu-id="97380-203">Page route model convention</span></span>

<span data-ttu-id="97380-204">Utilisez <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> qui appelle une action sur le <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> pour la page avec le nom spécifié.</span><span class="sxs-lookup"><span data-stu-id="97380-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="97380-205">L’exemple d’application utilise `AddPageRouteModelConvention` pour ajouter un modèle de routage `{aboutTemplate?}` à la page About :</span><span class="sxs-lookup"><span data-stu-id="97380-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="97380-206">La propriété <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> a la valeur `2`.</span><span class="sxs-lookup"><span data-stu-id="97380-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="97380-207">Cela permet de s’assurer que le modèle de `{globalTemplate?}` (défini plus haut dans la rubrique pour `1`) est prioritaire pour la première position de valeur de données de route lorsqu’une valeur de route unique est fournie.</span><span class="sxs-lookup"><span data-stu-id="97380-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="97380-208">Si la page about est demandée avec une valeur de paramètre d’itinéraire à `/About/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["aboutTemplate"]` (`Order = 2`) en raison de la définition de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="97380-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="97380-209">Dans la mesure du possible, ne définissez pas le `Order`, ce qui donne `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="97380-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="97380-210">Utilisez le routage pour sélectionner l’itinéraire correct.</span><span class="sxs-lookup"><span data-stu-id="97380-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="97380-211">Demandez la page About de l’exemple sur `localhost:5000/About/GlobalRouteValue/AboutRouteValue` et examinez le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![La page About est demandée avec des segments de routage pour GlobalRouteValue et AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="97380-214">Utiliser un transformateur de paramètre pour personnaliser les itinéraires de page</span><span class="sxs-lookup"><span data-stu-id="97380-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="97380-215">Les itinéraires de page générés par ASP.NET Core peuvent être personnalisés à l’aide d’un transformateur de paramètres.</span><span class="sxs-lookup"><span data-stu-id="97380-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="97380-216">Un transformateur de paramètre implémente `IOutboundParameterTransformer` et transforme la valeur des paramètres.</span><span class="sxs-lookup"><span data-stu-id="97380-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="97380-217">Par exemple, un transformateur de paramètre `SlugifyParameterTransformer` personnalisé transforme la valeur de la route `SubscriptionManagement` en `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="97380-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="97380-218">La Convention de modèle de routage de page `PageRouteTransformerConvention` applique un transformateur de paramètre aux segments de nom de fichier et de dossier des itinéraires de page générés automatiquement dans une application.</span><span class="sxs-lookup"><span data-stu-id="97380-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="97380-219">Par exemple, le fichier Razor Pages sur */pages/SubscriptionManagement/viewall.cshtml* aurait son itinéraire réécrit de `/SubscriptionManagement/ViewAll` à `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="97380-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="97380-220">`PageRouteTransformerConvention` transforme uniquement les segments générés automatiquement d’un itinéraire de page provenant du Razor Pages du dossier et du nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="97380-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="97380-221">Elle ne transforme pas les segments de routage ajoutés avec la directive `@page`.</span><span class="sxs-lookup"><span data-stu-id="97380-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="97380-222">La Convention ne transforme pas non plus les routes ajoutées par <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="97380-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="97380-223">Le `PageRouteTransformerConvention` est inscrit en tant qu’option dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="97380-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a><span data-ttu-id="97380-224">Configurer un routage de page</span><span class="sxs-lookup"><span data-stu-id="97380-224">Configure a page route</span></span>

<span data-ttu-id="97380-225">Utilisez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> pour configurer un itinéraire vers une page au niveau du chemin de page spécifié.</span><span class="sxs-lookup"><span data-stu-id="97380-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="97380-226">Les liens générés qui pointent vers la page utilisent le routage que vous avez spécifié.</span><span class="sxs-lookup"><span data-stu-id="97380-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="97380-227">`AddPageRoute` utilise `AddPageRouteModelConvention` pour établir le routage.</span><span class="sxs-lookup"><span data-stu-id="97380-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="97380-228">L’exemple d’application crée un routage vers `/TheContactPage` pour *Contact.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="97380-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="97380-229">La page Contact est également accessible sur `/Contact` via son routage par défaut.</span><span class="sxs-lookup"><span data-stu-id="97380-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="97380-230">Le routage personnalisé de l’exemple d’application vers la page Contact permet un segment de routage `text` facultatif (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="97380-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="97380-231">Cette page inclut également ce segment facultatif dans sa directive `@page`, au cas où le visiteur accèderait à la page via le routage `/Contact` :</span><span class="sxs-lookup"><span data-stu-id="97380-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="97380-232">Notez que l’URL générée pour le lien **Contact** dans la page affichée reflète le routage mis à jour :</span><span class="sxs-lookup"><span data-stu-id="97380-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Lien Contact de l’exemple d’application dans la barre de navigation](razor-pages-conventions/_static/contact-link.png)

![Le lien Contact dans la page HTML affichée indique que l’attribut href a la valeur ’/TheContactPage’](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="97380-235">Visitez la page Contact via son routage ordinaire, `/Contact`, ou via le routage personnalisé, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="97380-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="97380-236">Si vous indiquez un segment de routage `text` supplémentaire, la page affiche le segment HTML que vous fournissez :</span><span class="sxs-lookup"><span data-stu-id="97380-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Exemple dans le navigateur Edge d’un segment de routage ‘text’ facultatif fourni pour ‘TextValue’ dans l’URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="97380-239">Conventions d’actions de modèle de page</span><span class="sxs-lookup"><span data-stu-id="97380-239">Page model action conventions</span></span>

<span data-ttu-id="97380-240">Le fournisseur de modèles de page par défaut qui implémente <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> appelle des conventions conçues pour fournir des points d’extensibilité pour la configuration des modèles de page.</span><span class="sxs-lookup"><span data-stu-id="97380-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="97380-241">Ces conventions sont utiles durant la génération et la modification de scénarios de découverte et de traitement de pages.</span><span class="sxs-lookup"><span data-stu-id="97380-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="97380-242">Pour les exemples de cette section, l’exemple d’application utilise une classe `AddHeaderAttribute`, qui est un <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, qui applique un en-tête de réponse :</span><span class="sxs-lookup"><span data-stu-id="97380-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="97380-243">À l’aide de conventions, l’exemple montre comment appliquer l’attribut à toutes les pages d’un dossier et à une seule page.</span><span class="sxs-lookup"><span data-stu-id="97380-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="97380-244">**Convention de modèle d’application de dossier**</span><span class="sxs-lookup"><span data-stu-id="97380-244">**Folder app model convention**</span></span>

<span data-ttu-id="97380-245">Utilisez <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> qui appelle une action sur les instances de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> pour toutes les pages du dossier spécifié.</span><span class="sxs-lookup"><span data-stu-id="97380-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="97380-246">L’exemple illustre l’utilisation de `AddFolderApplicationModelConvention` en ajoutant un en-tête, `OtherPagesHeader`, aux pages situées dans le dossier *OtherPages* de l’application :</span><span class="sxs-lookup"><span data-stu-id="97380-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="97380-247">Demandez la page Page1 de l’exemple sur `localhost:5000/OtherPages/Page1` et examinez les en-têtes pour voir le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Les en-têtes de réponse de la page OtherPages/Page1 montrent que OtherPagesHeader a été ajouté.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="97380-249">**Convention de modèle d’application de page**</span><span class="sxs-lookup"><span data-stu-id="97380-249">**Page app model convention**</span></span>

<span data-ttu-id="97380-250">Utilisez <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> pour créer et ajouter un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> qui appelle une action sur le <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> pour la page avec le nom spécifié.</span><span class="sxs-lookup"><span data-stu-id="97380-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="97380-251">L’exemple illustre l’utilisation de `AddPageApplicationModelConvention` en ajoutant un en-tête, `AboutHeader`, à la page About :</span><span class="sxs-lookup"><span data-stu-id="97380-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="97380-252">Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Les en-têtes de réponse de la page About montrent que AboutHeader a été ajouté.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="97380-254">**Configurer un filtre**</span><span class="sxs-lookup"><span data-stu-id="97380-254">**Configure a filter**</span></span>

<span data-ttu-id="97380-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configure le filtre spécifié à appliquer.</span><span class="sxs-lookup"><span data-stu-id="97380-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="97380-256">Vous pouvez implémenter une classe de filtre. Toutefois, l’exemple d’application montre comment implémenter un filtre dans une expression lambda, laquelle est implémentée en arrière-plan en tant que fabrique qui retourne un filtre :</span><span class="sxs-lookup"><span data-stu-id="97380-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="97380-257">Le modèle d’application de page est utilisé pour vérifier le chemin relatif des segments qui mènent à la page Page2 dans le dossier *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="97380-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="97380-258">Si la condition est satisfaite, un en-tête est ajouté.</span><span class="sxs-lookup"><span data-stu-id="97380-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="97380-259">Sinon, `EmptyFilter` est appliqué.</span><span class="sxs-lookup"><span data-stu-id="97380-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="97380-260">`EmptyFilter` est un [filtre d’action](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="97380-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="97380-261">Étant donné que les filtres d’action sont ignorés par Razor Pages, le `EmptyFilter` n’a aucun effet comme prévu si le chemin d’accès ne contient pas de `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="97380-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="97380-262">Demandez la page Page2 de l’exemple sur `localhost:5000/OtherPages/Page2` et examinez les en-têtes pour voir le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header est ajouté à la réponse pour Page2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="97380-264">**Configurer une fabrique de filtres**</span><span class="sxs-lookup"><span data-stu-id="97380-264">**Configure a filter factory**</span></span>

<span data-ttu-id="97380-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configure la fabrique spécifiée pour appliquer des [filtres](xref:mvc/controllers/filters) à tous les Razor pages.</span><span class="sxs-lookup"><span data-stu-id="97380-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="97380-266">L’exemple d’application illustre l’utilisation d’une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) en ajoutant un en-tête, `FilterFactoryHeader`, et deux valeurs aux pages de l’application :</span><span class="sxs-lookup"><span data-stu-id="97380-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="97380-267">*AddHeaderWithFactory.cs* :</span><span class="sxs-lookup"><span data-stu-id="97380-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="97380-268">Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :</span><span class="sxs-lookup"><span data-stu-id="97380-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Les en-têtes de réponse de la page About montrent que deux en-têtes FilterFactoryHeader ont été ajoutés.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="97380-270">Filtres MVC et filtre de page (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="97380-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="97380-271">Les [filtres d’action](xref:mvc/controllers/filters#action-filters) MVC sont ignorés par les pages Razor, car les pages Razor utilisent des méthodes de gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="97380-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="97380-272">Vous pouvez utiliser d’autres types de filtre MVC : [Autorisation](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Ressource](xref:mvc/controllers/filters#resource-filters) et [Résultat](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="97380-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="97380-273">Pour plus d’informations, consultez la rubrique [Filtres](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="97380-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="97380-274">Le filtre de page (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) est un filtre qui s’applique à Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="97380-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="97380-275">Pour plus d’informations, consultez [Méthodes de filtre pour les pages Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="97380-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97380-276">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="97380-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
