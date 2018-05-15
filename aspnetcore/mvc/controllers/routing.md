---
title: Routage vers les actions du contrôleur dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ASP.NET Core MVC utilise le middleware (intergiciel) de routage pour mettre en correspondance les URL des requêtes entrantes et les mapper à des actions.
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/routing
ms.openlocfilehash: 28fe62128d0a094fa08e866a270aed26080b1e51
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="bc54e-103">Routage vers les actions du contrôleur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc54e-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="bc54e-104">Par [Ryan Nowak](https://github.com/rynowak) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc54e-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bc54e-105">ASP.NET Core MVC utilise [l’intergiciel](xref:fundamentals/middleware/index) de routage pour mettre en correspondance les URL des requêtes entrantes et les mapper à des actions.</span><span class="sxs-lookup"><span data-stu-id="bc54e-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="bc54e-106">Les routes sont définies dans le code de démarrage ou dans des attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="bc54e-107">Les routes décrivent comment les chemins des URL doivent être mis en correspondance avec les actions.</span><span class="sxs-lookup"><span data-stu-id="bc54e-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="bc54e-108">Les routes sont également utilisées pour générer des URL (pour les liens) envoyés dans les réponses.</span><span class="sxs-lookup"><span data-stu-id="bc54e-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="bc54e-109">Les actions sont routées de façon conventionnelle ou routées par attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="bc54e-110">Le fait de placer une route sur le contrôleur ou sur l’action les rend « routés par attribut ».</span><span class="sxs-lookup"><span data-stu-id="bc54e-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="bc54e-111">Pour plus d’informations, consultez [Routage mixte](#routing-mixed-ref-label).</span><span class="sxs-lookup"><span data-stu-id="bc54e-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="bc54e-112">Ce document explique les interactions entre MVC et le routage, et comment les applications MVC classiques utilisent les fonctionnalités du routage.</span><span class="sxs-lookup"><span data-stu-id="bc54e-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="bc54e-113">Pour plus d’informations sur le routage avancé, consultez [Routage](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="bc54e-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="bc54e-114">Configuration de l’intergiciel de routage</span><span class="sxs-lookup"><span data-stu-id="bc54e-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="bc54e-115">Dans votre méthode *Configure*, vous pouvez voir un code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="bc54e-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="bc54e-116">À l’intérieur de l’appel à `UseMvc`, `MapRoute` est utilisé pour créer une route, nous allons appeler route `default`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="bc54e-117">La plupart des applications MVC utilisent une route avec un modèle similaire à la route `default`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="bc54e-118">Le modèle de route `"{controller=Home}/{action=Index}/{id?}"` peut correspondre à un chemin d’URL comme `/Products/Details/5` et il extrait les valeurs `{ controller = Products, action = Details, id = 5 }` de la route en décomposant le chemin en jetons.</span><span class="sxs-lookup"><span data-stu-id="bc54e-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="bc54e-119">MVC tente de trouver un contrôleur nommé `ProductsController` et d’exécuter l’action `Details` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="bc54e-120">Notez que dans cet exemple, la liaison de modèle utilise la valeur de `id = 5` pour définir le paramètre `id` sur `5` lors de l’appel de cette action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="bc54e-121">Pour plus d’informations, consultez [Liaison de modèle](../models/model-binding.md).</span><span class="sxs-lookup"><span data-stu-id="bc54e-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="bc54e-122">Utilisation de la route `default` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="bc54e-123">Le modèle de route :</span><span class="sxs-lookup"><span data-stu-id="bc54e-123">The route template:</span></span>

* <span data-ttu-id="bc54e-124">`{controller=Home}` définit `Home` comme `controller` par défaut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="bc54e-125">`{action=Index}` définit `Index` comme `action` par défaut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="bc54e-126">`{id?}` définit `id` comme étant facultatif.</span><span class="sxs-lookup"><span data-stu-id="bc54e-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="bc54e-127">Les paramètres de route par défaut et facultatifs n’ont pas besoin d’être présents dans le chemin d’URL pour qu’une correspondance soit établie.</span><span class="sxs-lookup"><span data-stu-id="bc54e-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="bc54e-128">Pour une description détaillée de la syntaxe du modèle de route, consultez [Informations de référence sur le modèle de route](../../fundamentals/routing.md#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="bc54e-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="bc54e-129">`"{controller=Home}/{action=Index}/{id?}"` peut correspondre au chemin d’URL `/` et produit les valeurs de route `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="bc54e-130">Les valeurs de `controller` et `action` utilisent les valeurs par défaut, et `id` ne produit pas de valeur, car il n’existe pas de segment correspondant dans le chemin d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="bc54e-131">MVC utilisez ces valeurs de route pour sélectionner `HomeController` et l’action `Index` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="bc54e-132">Avec cette définition de contrôleur et ce modèle de route, l’action `HomeController.Index` est exécutée pour tous les chemins d’URL suivants :</span><span class="sxs-lookup"><span data-stu-id="bc54e-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="bc54e-133">La méthode pratique `UseMvcWithDefaultRoute` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="bc54e-134">Peut être utilisée pour remplacer :</span><span class="sxs-lookup"><span data-stu-id="bc54e-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="bc54e-135">`UseMvc` et `UseMvcWithDefaultRoute` ajoutent une instance de `RouterMiddleware` au pipeline de l’intergiciel.</span><span class="sxs-lookup"><span data-stu-id="bc54e-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="bc54e-136">MVC n’interagit pas directement avec l’intergiciel et utilise le routage pour gérer les requêtes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="bc54e-137">MVC est connecté aux routes via une instance de `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="bc54e-138">Le code de `UseMvc` est similaire à ceci :</span><span class="sxs-lookup"><span data-stu-id="bc54e-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="bc54e-139">`UseMvc` ne définit directement aucune route, il ajoute un espace réservé à la collection de routes pour la route `attribute`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="bc54e-140">La surcharge `UseMvc(Action<IRouteBuilder>)` vous permet d’ajouter vos propres routes et prend également en charge le routage par attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="bc54e-141">`UseMvc` et toutes ses variations ajoutent un espace réservé pour la route d’attribut ; le routage par attribut est toujours disponible, quelle que soit la façon dont vous configurez `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="bc54e-142">`UseMvcWithDefaultRoute` définit une route par défaut et prend en charge le routage par attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="bc54e-143">La section [Routage par attribut](#attribute-routing-ref-label) comprend plus de détails sur le routage par attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="bc54e-144">Routage conventionnel</span><span class="sxs-lookup"><span data-stu-id="bc54e-144">Conventional routing</span></span>

<span data-ttu-id="bc54e-145">La route `default` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="bc54e-146">est un exemple de *routage conventionnel*.</span><span class="sxs-lookup"><span data-stu-id="bc54e-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="bc54e-147">Nous appelons ce style *routage conventionnel*, car il établit une *convention* pour les chemins d’URL :</span><span class="sxs-lookup"><span data-stu-id="bc54e-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="bc54e-148">Le premier segment du chemin correspond au nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="bc54e-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="bc54e-149">Le deuxième correspond au nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-149">the second maps to the action name.</span></span>

* <span data-ttu-id="bc54e-150">Le troisième segment est utilisé pour un `id` facultatif utilisé pour le mappage à une entité du modèle.</span><span class="sxs-lookup"><span data-stu-id="bc54e-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="bc54e-151">En utilisant cette route `default`, le chemin d’URL `/Products/List` mappe à l’action `ProductsController.List`, et `/Blog/Article/17` mappe à `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="bc54e-152">Ce mappage est basé **uniquement** sur les noms de contrôleur et d’action ; il n’est pas basé sur les espaces de noms, les emplacements des fichiers sources ou les paramètres de méthode.</span><span class="sxs-lookup"><span data-stu-id="bc54e-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="bc54e-153">L’utilisation du routage conventionnel avec la route par défaut vous permet de créer rapidement l’application sans avoir à élaborer un nouveau modèle d’URL pour chaque action que vous définissez.</span><span class="sxs-lookup"><span data-stu-id="bc54e-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="bc54e-154">Pour une application avec des actions de style CRUD, le fait de disposer d’une cohérence pour les URL entre vos contrôleurs peut simplifier votre code et rendre votre interface utilisateur plus prévisible.</span><span class="sxs-lookup"><span data-stu-id="bc54e-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="bc54e-155">`id` est défini comme étant facultatif par le modèle de route, ce qui signifie que vos actions peuvent s’exécuter sans l’ID fourni dans le cadre de l’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="bc54e-156">En général, si `id` est omis de l’URL, il est défini sur `0` par la liaison de modèle : aucune entité correspondant à `id == 0` n’est donc trouvée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc54e-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="bc54e-157">Le routage par attribut vous donne un contrôle précis pour rendre le code obligatoire pour certaines actions et pas pour d’autres.</span><span class="sxs-lookup"><span data-stu-id="bc54e-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="bc54e-158">Par convention, la documentation inclut des paramètres facultatifs comme `id` quand ils sont susceptibles d’apparaître dans une utilisation correcte.</span><span class="sxs-lookup"><span data-stu-id="bc54e-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="bc54e-159">Routes multiples</span><span class="sxs-lookup"><span data-stu-id="bc54e-159">Multiple routes</span></span>

<span data-ttu-id="bc54e-160">Vous pouvez ajouter plusieurs routes dans `UseMvc` en ajoutant plusieurs appels à `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="bc54e-161">Ceci vous permet de définir plusieurs conventions ou d’ajouter des routes conventionnelles qui sont dédiées à une action spécifique, comme :</span><span class="sxs-lookup"><span data-stu-id="bc54e-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="bc54e-162">La route `blog` est ici une *route conventionnelle dédiée*, ce qui signifie qu’elle utilise le système de routage conventionnel, mais qu’elle est dédiée à une action spécifique.</span><span class="sxs-lookup"><span data-stu-id="bc54e-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="bc54e-163">Comme `controller` et `action` n’apparaissent pas dans le modèle de route en tant que paramètres, ils peuvent seulement avoir les valeurs par défaut et par conséquent, cette route sera toujours mappée à l’action `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="bc54e-164">Les routes de la collection de routes sont ordonnées et elles sont traitées dans l’ordre où elles sont ajoutées.</span><span class="sxs-lookup"><span data-stu-id="bc54e-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="bc54e-165">Ainsi, dans cet exemple, la route `blog` est essayée avant la route `default`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-166">Les *routes conventionnelles dédiées* utilisent souvent des paramètres de route fourre-tout, comme `{*article}`, pour capturer la partie restante du chemin d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="bc54e-167">Ceci peut rendre une route « trop globale », ce qui signifie qu’elle correspond à des URL qui devaient être mises en correspondance par d’autres routes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="bc54e-168">Pour résoudre ce problème, placez les routes « globales » plus loin dans la table de routage.</span><span class="sxs-lookup"><span data-stu-id="bc54e-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="bc54e-169">Processus de repli</span><span class="sxs-lookup"><span data-stu-id="bc54e-169">Fallback</span></span>

<span data-ttu-id="bc54e-170">Dans le cadre du traitement des requêtes, MVC vérifie que les valeurs de route peuvent être utilisées pour rechercher un contrôleur et une action dans votre application.</span><span class="sxs-lookup"><span data-stu-id="bc54e-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="bc54e-171">Si les valeurs de route ne correspondent pas à une action, la route n’est pas considérée comme une correspondance, et la route suivante est essayée.</span><span class="sxs-lookup"><span data-stu-id="bc54e-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="bc54e-172">Ceci est appelé *processus de repli* et est conçu pour simplifier les cas où des routes conventionnelles se chevauchent.</span><span class="sxs-lookup"><span data-stu-id="bc54e-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="bc54e-173">Résolution des ambiguïtés pour les actions</span><span class="sxs-lookup"><span data-stu-id="bc54e-173">Disambiguating actions</span></span>

<span data-ttu-id="bc54e-174">Quand deux actions correspondent via le routage, MVC doit résoudre l’ambiguïté pour choisir le « meilleur » candidat ou sinon lever une exception.</span><span class="sxs-lookup"><span data-stu-id="bc54e-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="bc54e-175">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bc54e-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="bc54e-176">Ce contrôleur définit deux actions qui correspondraient au chemin d’URL `/Products/Edit/17` et les données de routage `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="bc54e-177">Il s’agit d’un modèle classique pour les contrôleurs MVC, où `Edit(int)` montre un formulaire pour modifier un produit, et où `Edit(int, Product)` traite le formulaire envoyé.</span><span class="sxs-lookup"><span data-stu-id="bc54e-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="bc54e-178">Pour rendre cela possible, MVC doit choisir `Edit(int, Product)` quand la requête est `POST` HTTP, et `Edit(int)` quand le verbe HTTP est autre chose.</span><span class="sxs-lookup"><span data-stu-id="bc54e-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="bc54e-179">`HttpPostAttribute` (`[HttpPost]`) est une implémentation de `IActionConstraint` qui permet la sélection de l’action seulement quand le verbe HTTP est `POST`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="bc54e-180">La présence d’une `IActionConstraint` fait de `Edit(int, Product)` une correspondance « meilleure » que `Edit(int)`, de sorte que `Edit(int, Product)` est essayé en premier.</span><span class="sxs-lookup"><span data-stu-id="bc54e-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="bc54e-181">Vous devez écrire des implémentations personnalisées de `IActionConstraint` seulement dans des scénarios spécifiques, mais il est important de comprendre le rôle d’attributs comme `HttpPostAttribute` ; des attributs similaires sont définis pour les autres verbes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc54e-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="bc54e-182">Dans le routage conventionnel, il est courant que des actions utilisent un même nom d’action quand elles font partie d’un flux de travail `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="bc54e-183">L’avantage de ce modèle devient plus évident après la lecture de la section [Présentation d’IActionConstraint](#understanding-iactionconstraint).</span><span class="sxs-lookup"><span data-stu-id="bc54e-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="bc54e-184">Si plusieurs routes correspondent et que MVC ne peut pas trouver une « meilleure » route, elle lève une `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="bc54e-185">Noms des routes</span><span class="sxs-lookup"><span data-stu-id="bc54e-185">Route names</span></span>

<span data-ttu-id="bc54e-186">Les chaînes `"blog"` et `"default"` dans les exemples suivants sont des noms de routes :</span><span class="sxs-lookup"><span data-stu-id="bc54e-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="bc54e-187">Les noms de routes donnent aux routes des noms logiques : le nom de route peut ainsi être utilisé pour la génération des URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="bc54e-188">Ceci simplifie considérablement la création d’URL quand l’ordonnancement des routes peut rendre compliquée la génération des URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="bc54e-189">Les noms de routes doivent être unique à l’échelle de l’application.</span><span class="sxs-lookup"><span data-stu-id="bc54e-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="bc54e-190">Les noms de routes n’ont pas d’impact sur la correspondance des URL ni sur le traitement des requêtes ; ils sont utilisés seulement pour la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="bc54e-191">La section [Routage](xref:fundamentals/routing) contient des informations plus détaillées sur la génération d’URL, notamment la génération d’URL dans les helpers spécifiques à MVC.</span><span class="sxs-lookup"><span data-stu-id="bc54e-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="bc54e-192">Routage par attributs</span><span class="sxs-lookup"><span data-stu-id="bc54e-192">Attribute routing</span></span>

<span data-ttu-id="bc54e-193">Le routage par attributs utilise un ensemble d’attributs pour mapper les actions directement aux modèles de routes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="bc54e-194">Dans l’exemple suivant, `app.UseMvc();` est utilisé dans la méthode `Configure` et aucune route n’est passée.</span><span class="sxs-lookup"><span data-stu-id="bc54e-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="bc54e-195">`HomeController` va trouver des correspondances avec un ensemble d’URL similaires aux correspondances qui seraient trouvées par la route par défaut `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="bc54e-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="bc54e-196">L’action `HomeController.Index()` sera exécutée pour tous les chemins d’URL `/`, `/Home` ou `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-197">Cet exemple met en évidence une différence importante en termes de programmation entre le routage par attributs et le routage conventionnel.</span><span class="sxs-lookup"><span data-stu-id="bc54e-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="bc54e-198">Le routage par attributs nécessite des entrées en plus grand nombre pour spécifier une route ; la route conventionnelle par défaut gère les routes de façon plus succincte.</span><span class="sxs-lookup"><span data-stu-id="bc54e-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="bc54e-199">Cependant, le routage par attributs permet (et nécessite) un contrôle précis des modèles de routes qui s’appliquent à chaque action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="bc54e-200">Avec le routage par attributs, le nom du contrôleur et les noms des actions ne jouent **aucun** rôle dans la sélection de l’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="bc54e-201">Cet exemple trouve une correspondance avec les mêmes URL que l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="bc54e-201">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="bc54e-202">Les modèles de routes ci-dessus ne définissent pas de paramètres de route pour `action`, `area` et `controller`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="bc54e-203">En fait, ces paramètres de route ne sont pas autorisés dans les routes par attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="bc54e-204">Comme le modèle de route est déjà associé à une action, cela n’aurait pas de sens d’analyser le nom de l’action à partir de l’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="bc54e-205">Routage par attributs avec des attributs Http[Verbe]</span><span class="sxs-lookup"><span data-stu-id="bc54e-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="bc54e-206">Le routage par attributs peut aussi utiliser les attributs `Http[Verb]`, comme `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="bc54e-207">Tous ces attributs peuvent accepter un modèle de route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="bc54e-208">Cet exemple montre deux actions qui correspondent au même modèle de route :</span><span class="sxs-lookup"><span data-stu-id="bc54e-208">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="bc54e-209">Pour un chemin d’URL comme `/products`, l’action `ProductsApi.ListProducts` est exécutée quand le verbe HTTP est `GET`, et `ProductsApi.CreateProduct` est exécutée quand le verbe HTTP est `POST`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="bc54e-210">Le routage par attributs recherche d’abord une correspondance de l’URL par rapport à l’ensemble des modèles de routes défini par les attributs de la route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="bc54e-211">Une fois qu’un modèle de route correspond, les contraintes de `IActionConstraint` sont appliquées pour déterminer quelles actions peuvent être exécutées.</span><span class="sxs-lookup"><span data-stu-id="bc54e-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="bc54e-212">Lors de la création d’une API REST, il est rare de vouloir utiliser `[Route(...)]` sur une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="bc54e-213">Il est préférable d’utiliser les `Http*Verb*Attributes` plus spécifiques pour plus de précision quant à ce qui est pris en charge par votre API.</span><span class="sxs-lookup"><span data-stu-id="bc54e-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="bc54e-214">Les clients des API REST doivent normalement connaître les chemins et les verbes HTTP qui correspondent à des opérations logiques spécifiques.</span><span class="sxs-lookup"><span data-stu-id="bc54e-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="bc54e-215">Dans la mesure où une route d’attribut s’applique à une action spécifique, il est facile de placer les paramètres nécessaires dans la définition du modèle de route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="bc54e-216">Dans cet exemple, `id` est obligatoire dans le chemin d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="bc54e-217">L’action `ProductsApi.GetProduct(int)` est exécutée pour un chemin d’URL comme `/products/3`, mais pas pour un chemin d’URL comme `/products`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="bc54e-218">Consultez [Routage](../../fundamentals/routing.md) pour obtenir une description complète des modèles de routes et des options associées.</span><span class="sxs-lookup"><span data-stu-id="bc54e-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="bc54e-219">Nom des routes</span><span class="sxs-lookup"><span data-stu-id="bc54e-219">Route Name</span></span>

<span data-ttu-id="bc54e-220">Le code suivant définit un *nom de route* `Products_List` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="bc54e-221">Les noms de routes peuvent être utilisés pour générer une URL basée sur une route spécifique.</span><span class="sxs-lookup"><span data-stu-id="bc54e-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="bc54e-222">Les noms de routes n’ont pas d’impact sur le comportement de mise en correspondance des URL du routage et ils sont utilisés seulement pour la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="bc54e-223">Les noms de routes doivent être unique à l’échelle de l’application.</span><span class="sxs-lookup"><span data-stu-id="bc54e-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-224">Comparez ceci avec la *route par défaut* conventionnelle, qui définit le paramètre `id` comme étant facultatif (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="bc54e-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="bc54e-225">Cette possibilité de spécifier les API avec précision présente des avantages, par exemple de permettre de diriger `/products` et `/products/5` vers des actions différentes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="bc54e-226">Combinaison de routes</span><span class="sxs-lookup"><span data-stu-id="bc54e-226">Combining routes</span></span>

<span data-ttu-id="bc54e-227">Pour rendre le routage par attributs moins répétitif, les attributs de route sont combinés avec des attributs de route sur les actions individuelles.</span><span class="sxs-lookup"><span data-stu-id="bc54e-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="bc54e-228">Les modèles de routes définis sur le contrôleur sont ajoutés à des modèles de routes sur les actions.</span><span class="sxs-lookup"><span data-stu-id="bc54e-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="bc54e-229">Placer un attribut de route sur le contrôleur a pour effet que **toutes** les actions du contrôleur utilisent le routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="bc54e-230">Dans cet exemple, le chemin d’URL `/products` peut être mis en correspondance avec `ProductsApi.ListProducts` et le chemin d’URL `/products/5` peut être mis en correspondance avec `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="bc54e-231">Ces deux actions correspondent seulement à HTTP `GET`, car elles sont décorées avec `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-231">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="bc54e-232">Les modèles de routes appliqués à une action qui commencent par `/` ne sont pas combinés avec les modèles de routes appliqués au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="bc54e-232">Route templates applied to an action that begin with a `/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="bc54e-233">Cet exemple met en correspondance avec un ensemble de chemins d’URL similaires à la *route par défaut*.</span><span class="sxs-lookup"><span data-stu-id="bc54e-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="bc54e-234">Ordonnancement des routes d’attributs</span><span class="sxs-lookup"><span data-stu-id="bc54e-234">Ordering attribute routes</span></span>

<span data-ttu-id="bc54e-235">Contrairement aux routes conventionnelles qui s’exécutent dans un ordre défini, le routage par attributs génère une arborescence et établit une correspondance simultanément avec toutes les routes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="bc54e-236">Il se comporte comme-si les entrées des routes étaient placées dans un ordre idéal ; les routes les plus spécifiques ont ainsi plus de probabilités de s’exécuter avant les routes plus générales.</span><span class="sxs-lookup"><span data-stu-id="bc54e-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="bc54e-237">Par exemple, une route comme `blog/search/{topic}` est plus spécifique qu’une route comme `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="bc54e-238">D’un point de vue logique, la route `blog/search/{topic}` « s’exécute » par défaut en premier, car il s’agit du seul ordre sensé.</span><span class="sxs-lookup"><span data-stu-id="bc54e-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="bc54e-239">Avec le routage conventionnel, le développeur est responsable du placement des routes dans l’ordre souhaité.</span><span class="sxs-lookup"><span data-stu-id="bc54e-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="bc54e-240">Les routes d’attribut peuvent configurer un ordre en utilisant la propriété `Order` de tous les attributs de route fournis par le framework.</span><span class="sxs-lookup"><span data-stu-id="bc54e-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="bc54e-241">Les routes sont traitées selon un ordre croissant de la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="bc54e-242">L’ordre par défaut est `0`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-242">The default order is `0`.</span></span> <span data-ttu-id="bc54e-243">La définition d’une route avec `Order = -1` fait que cette route « s’exécute » avant les routes qui ne définissent pas d’ordre.</span><span class="sxs-lookup"><span data-stu-id="bc54e-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="bc54e-244">La définition d’une route avec `Order = 1` fait que cette route « s’exécute » après l’ordre des routes par défaut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="bc54e-245">Évitez de dépendre de `Order`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="bc54e-246">Si votre espace d’URL nécessite des valeurs d’ordre explicites pour router correctement, il est probable qu’il prête également à confusion pour les clients.</span><span class="sxs-lookup"><span data-stu-id="bc54e-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="bc54e-247">D’une façon générale, le routage par attributs sélectionne la route correcte avec la mise en correspondance d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="bc54e-248">Si l’ordre par défaut utilisé pour la génération d’URL ne fonctionne pas, l’utilisation à titre de remplacement d’un nom de route est généralement plus simple que d’appliquer la propriété `Order`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="bc54e-249">Remplacement de jetons dans les modèles de routes ([contrôleur], [action], [zone])</span><span class="sxs-lookup"><span data-stu-id="bc54e-249">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="bc54e-250">Pour plus de commodité, les routes d’attribut prennent en charge *le remplacement de jetons*, qui se fait via la mise entre crochets d’un jeton (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="bc54e-250">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="bc54e-251">Les jetons `[action]`, `[area]`, et `[controller]` sont remplacés par les valeurs du nom d’action, du nom de la zone et du nom du contrôleur de l’action où la route est définie.</span><span class="sxs-lookup"><span data-stu-id="bc54e-251">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="bc54e-252">Dans cet exemple, les actions peuvent être mises en correspondance avec les chemins d’URL comme décrit dans les commentaires :</span><span class="sxs-lookup"><span data-stu-id="bc54e-252">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="bc54e-253">Le remplacement des jetons se produit à la dernière étape de la création des routes d’attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-253">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="bc54e-254">L’exemple ci-dessus se comporte comme le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc54e-254">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="bc54e-255">Les routes d’attribut peuvent aussi être combinées avec l’héritage.</span><span class="sxs-lookup"><span data-stu-id="bc54e-255">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="bc54e-256">Combiné avec le remplacement de jetons, c’est particulièrement puissant.</span><span class="sxs-lookup"><span data-stu-id="bc54e-256">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="bc54e-257">Le remplacement des jetons s’applique aussi aux noms de routes définis par des routes d’attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-257">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="bc54e-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]` génère un nom de route unique pour chaque action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="bc54e-259">Pour faire correspondre le délimiteur littéral de remplacement de jetons `[` ou `]`, placez-le en échappement en répétant le caractère (`[[` ou `]]`).</span><span class="sxs-lookup"><span data-stu-id="bc54e-259">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="bc54e-260">Routes multiples</span><span class="sxs-lookup"><span data-stu-id="bc54e-260">Multiple Routes</span></span>

<span data-ttu-id="bc54e-261">Le routage par attributs prend en charge la définition de plusieurs routes pour atteindre la même action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-261">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="bc54e-262">L’utilisation la plus courante de ceci est d’imiter le comportement de la *route conventionnelle par défaut*, comme le montre l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="bc54e-262">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="bc54e-263">Le fait de placer plusieurs attributs de route sur le contrôleur signifie que chacun d’eux se combine avec chacun des attributs de route sur les méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-263">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="bc54e-264">Quand plusieurs attributs de route (qui implémentent `IActionConstraint`) sont placés sur une action, chaque contrainte d’action se combine avec le modèle de route de l’attribut qui l’a défini.</span><span class="sxs-lookup"><span data-stu-id="bc54e-264">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="bc54e-265">Si utiliser plusieurs routes sur des actions peut paître puissant, il est préférable de conserver l’espace d’URL de votre application simple et bien définie.</span><span class="sxs-lookup"><span data-stu-id="bc54e-265">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="bc54e-266">Utilisez plusieurs routes sur les actions seulement là où c’est nécessaire, par exemple pour prendre en charge des clients existants.</span><span class="sxs-lookup"><span data-stu-id="bc54e-266">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="bc54e-267">Spécification facultative de paramètres, de valeurs par défaut et de contraintes pour les routes d’attribut</span><span class="sxs-lookup"><span data-stu-id="bc54e-267">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="bc54e-268">Les routes d’attribut prennent en charge la même syntaxe inline que les routes conventionnelles pour spécifier des paramètres, des valeurs par défaut et des contraintes facultatifs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-268">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="bc54e-269">Pour une description détaillée de la syntaxe du modèle de route, consultez [Informations de référence sur le modèle de route](../../fundamentals/routing.md#route-template-reference).</span><span class="sxs-lookup"><span data-stu-id="bc54e-269">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="bc54e-270">Attributs de route personnalisés avec `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="bc54e-270">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="bc54e-271">Tous les attributs de route fournis dans le framework (`[Route(...)]`, `[HttpGet(...)]`, etc.) implémentent l’interface `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-271">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="bc54e-272">MVC recherche les attributs sur les classes de contrôleur et les méthodes d’action quand l’application démarre et utilise ceux qui implémentent `IRouteTemplateProvider` pour créer l’ensemble initial des routes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-272">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="bc54e-273">Vous pouvez implémenter `IRouteTemplateProvider` pour définir vos propres attributs de route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-273">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="bc54e-274">Chaque `IRouteTemplateProvider` vous permet de définir une route avec un modèle, un nom et un ordre de route personnalisés :</span><span class="sxs-lookup"><span data-stu-id="bc54e-274">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="bc54e-275">L’attribut de l’exemple ci-dessus définit automatiquement `Template` sur `"api/[controller]"` quand `[MyApiController]` est appliqué.</span><span class="sxs-lookup"><span data-stu-id="bc54e-275">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="bc54e-276">Utilisation d’un modèle d’application pour personnaliser les routes d’attribut</span><span class="sxs-lookup"><span data-stu-id="bc54e-276">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="bc54e-277">Le *modèle d’application* est un modèle d’objet créé au démarrage avec toutes les métadonnées utilisée par MVC pour router et exécuter vos actions.</span><span class="sxs-lookup"><span data-stu-id="bc54e-277">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="bc54e-278">Le *modèle d’application* inclut toutes les données collectées à partir des attributs de route (via `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="bc54e-278">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="bc54e-279">Vous pouvez écrire des *conventions* pour modifier le modèle d’application au moment du démarrage pour personnaliser le comporte du routage.</span><span class="sxs-lookup"><span data-stu-id="bc54e-279">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="bc54e-280">Cette section montre un exemple simple de personnalisation du routage avec le modèle d’application.</span><span class="sxs-lookup"><span data-stu-id="bc54e-280">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="bc54e-281">Routage mixte : routage conventionnel et routage par attributs</span><span class="sxs-lookup"><span data-stu-id="bc54e-281">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="bc54e-282">Les applications MVC peuvent combiner l’utilisation du routage conventionnel et du routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-282">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="bc54e-283">Il est courant d’utiliser des routes conventionnelles pour les contrôleurs délivrant des pages HTML pour les navigateurs, et le routage par attributs pour les contrôleurs délivrant des API REST.</span><span class="sxs-lookup"><span data-stu-id="bc54e-283">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="bc54e-284">Les actions sont routées de façon conventionnelle ou routées par attribut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-284">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="bc54e-285">Le fait de placer une route sur le contrôleur ou sur l’action les rend « routés par attribut ».</span><span class="sxs-lookup"><span data-stu-id="bc54e-285">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="bc54e-286">Les actions qui définissent des routes d’attribut ne sont pas accessibles via les routes conventionnelles et vice versa.</span><span class="sxs-lookup"><span data-stu-id="bc54e-286">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="bc54e-287">**Tout** attribut de route sur le contrôleur a pour effet que toutes les actions du contrôleur sont routées par attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-287">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-288">Ce qui distingue les deux types de systèmes de routage est le processus appliqué une fois qu’une URL correspond à un modèle de route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-288">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="bc54e-289">Dans le routage conventionnel, les valeurs de route de la correspondance sont utilisées pour choisir l’action et le contrôleur dans une table de recherche comprenant toutes les actions routées de façon conventionnelle.</span><span class="sxs-lookup"><span data-stu-id="bc54e-289">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="bc54e-290">Dans le routage par attributs, chaque modèle est déjà associé à une action, et aucune recherche supplémentaire n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="bc54e-290">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="bc54e-291">Génération des URL</span><span class="sxs-lookup"><span data-stu-id="bc54e-291">URL Generation</span></span>

<span data-ttu-id="bc54e-292">Les applications MVC peuvent utiliser les fonctionnalités de génération d’URL de routage pour générer des liens URL vers des actions.</span><span class="sxs-lookup"><span data-stu-id="bc54e-292">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="bc54e-293">La génération d’URL élimine le codage en dur des URL, ce qui rend votre code plus robuste et plus facile à maintenir.</span><span class="sxs-lookup"><span data-stu-id="bc54e-293">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="bc54e-294">Cette section se concentre sur les fonctionnalités de génération d’URL fournies par MVC et couvre seulement les principes de base du fonctionnement de la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-294">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="bc54e-295">Pour une description détaillée de la génération d’URL, consultez [Routage](../../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="bc54e-295">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="bc54e-296">L’interface `IUrlHelper` est l’élément d’infrastructure sous-jacent entre MVC et le routage pour la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-296">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="bc54e-297">Vous pouvez trouver une instance de `IUrlHelper` disponible via la propriété `Url` dans les composants controllers, views et view.</span><span class="sxs-lookup"><span data-stu-id="bc54e-297">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="bc54e-298">Dans cet exemple, l’interface `IUrlHelper` est utilisée via la propriété `Controller.Url` pour générer une URL vers une autre action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-298">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="bc54e-299">Si l’application utilise la route conventionnelle par défaut, la valeur de la variable `url` est la chaîne de chemin d’URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-299">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="bc54e-300">Ce chemin d’URL est créé par le routage en combinant les valeurs de route de la requête en cours (valeurs ambiantes) avec les valeurs passées à `Url.Action`, et en remplaçant les valeurs dans le modèle de route :</span><span class="sxs-lookup"><span data-stu-id="bc54e-300">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="bc54e-301">La valeur de chaque paramètre de route du modèle de route est remplacée en établissant une correspondance avec les valeurs et les valeurs ambiantes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-301">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="bc54e-302">Un paramètre de route qui n’a pas de valeur peut utiliser une valeur par défaut s’il en a une, ou il peut être ignoré s’il est facultatif (comme dans le cas de `id` dans cet exemple).</span><span class="sxs-lookup"><span data-stu-id="bc54e-302">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="bc54e-303">La génération d’URL échoue si un paramètre de route obligatoire n’a pas de valeur correspondante.</span><span class="sxs-lookup"><span data-stu-id="bc54e-303">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="bc54e-304">Si la génération d’URL échoue pour une route, la route suivante est essayée, ceci jusqu’à ce que toutes les routes aient été essayées ou qu’une correspondance soit trouvée.</span><span class="sxs-lookup"><span data-stu-id="bc54e-304">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="bc54e-305">L’exemple de `Url.Action` ci-dessus suppose un routage conventionnel, mais la génération d’URL fonctionne de façon similaire avec le routage par attributs, même si les concepts sont différents.</span><span class="sxs-lookup"><span data-stu-id="bc54e-305">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="bc54e-306">Avec le routage conventionnel, les valeurs de route sont utilisées pour développer un modèle, et les valeurs de route pour `controller` et `action` apparaissent généralement dans ce modèle : ceci fonctionne car les URL mises en correspondance par le routage adhèrent à une *convention*.</span><span class="sxs-lookup"><span data-stu-id="bc54e-306">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="bc54e-307">Dans le routage par attributs, les valeurs de route pour `controller` et `action` ne sont pas autorisées à apparaître dans le modèle : au lieu de cela, elles sont utilisées pour rechercher le modèle à utiliser.</span><span class="sxs-lookup"><span data-stu-id="bc54e-307">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="bc54e-308">Cet exemple utilise le routage par attributs :</span><span class="sxs-lookup"><span data-stu-id="bc54e-308">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="bc54e-309">MVC génère une table de recherche de toutes les actions routées par attributs et recherche une correspondance avec les valeurs de `controller` et de `action` pour sélectionner le modèle de route à utiliser pour la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-309">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="bc54e-310">Dans l’exemple ci-dessus, `custom/url/to/destination` est généré.</span><span class="sxs-lookup"><span data-stu-id="bc54e-310">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="bc54e-311">Génération des URL par nom d’action</span><span class="sxs-lookup"><span data-stu-id="bc54e-311">Generating URLs by action name</span></span>

<span data-ttu-id="bc54e-312">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="bc54e-312">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="bc54e-313">`Action`) et toutes les surcharges associées sont tous basés sur cette idée que vous voulez spécifier ce à quoi vous liez en spécifiant un nom de contrôleur et un nom d’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-313">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-314">Quand vous utilisez `Url.Action`, les valeurs de route actuelles pour `controller` et `action` sont spécifiées pour vous : la valeur de `controller` et de `action` font partie des *valeurs ambiantes* **et des** *valeurs*.</span><span class="sxs-lookup"><span data-stu-id="bc54e-314">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="bc54e-315">La méthode `Url.Action` utilise toujours les valeurs actuelles de `action` et de `controller`, et génère un chemin d’URL qui route vers l’action actuelle.</span><span class="sxs-lookup"><span data-stu-id="bc54e-315">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="bc54e-316">Le routage essaye d’utiliser les valeurs dans les valeurs ambiantes pour renseigner les informations que vous n’avez pas fournies lors de la génération d’une URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-316">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="bc54e-317">En utilisant une route comme `{a}/{b}/{c}/{d}` et les valeurs ambiantes `{ a = Alice, b = Bob, c = Carol, d = David }`, le routage a suffisamment d’informations pour générer une URL sans aucune autre valeur supplémentaire, car tous les paramètres de route ont une valeur.</span><span class="sxs-lookup"><span data-stu-id="bc54e-317">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="bc54e-318">Si vous avez ajouté la valeur `{ d = Donovan }`, la valeur `{ d = David }` est ignorée, et le chemin d’URL généré est `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-318">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="bc54e-319">Les chemins d’URL sont hiérarchiques.</span><span class="sxs-lookup"><span data-stu-id="bc54e-319">URL paths are hierarchical.</span></span> <span data-ttu-id="bc54e-320">Dans l’exemple ci-dessus, si vous avez ajouté la valeur `{ c = Cheryl }`, les deux valeurs `{ c = Carol, d = David }` sont ignorées.</span><span class="sxs-lookup"><span data-stu-id="bc54e-320">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="bc54e-321">Dans ce cas nous n’avons plus de valeur pour `d` et la génération d’URL échoue.</span><span class="sxs-lookup"><span data-stu-id="bc54e-321">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="bc54e-322">Vous devez spécifier la valeur souhaitée de `c` et de `d`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-322">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="bc54e-323">Vous vous attendez peut-être à rencontrer ce problème avec la route par défaut (`{controller}/{action}/{id?}`), mais vous rencontrerez rarement ce comportement en pratique, car `Url.Action` spécifie toujours explicitement une valeur pour `controller` et pour `action`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-323">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="bc54e-324">Les surcharges plus longues de `Url.Action` prennent également un objet supplémentaire de *valeurs de route*  pour fournir des valeurs pour les paramètres de route autres que `controller` et `action`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-324">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="bc54e-325">Vous verrez ceci plus couramment utilisé avec un `id` comme `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-325">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="bc54e-326">Par convention, l’objet de *valeurs de route* objet est généralement un objet de type anonyme, mais il peut également être un `IDictionary<>` ou un *objet .NET traditionnel*.</span><span class="sxs-lookup"><span data-stu-id="bc54e-326">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="bc54e-327">Toutes les valeurs de route supplémentaires qui ne correspondent pas aux paramètres de route sont placées dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="bc54e-327">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="bc54e-328">Pour créer une URL absolue, utilisez une surcharge qui accepte un `protocol` :`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="bc54e-328">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="bc54e-329">Génération des URL par route</span><span class="sxs-lookup"><span data-stu-id="bc54e-329">Generating URLs by route</span></span>

<span data-ttu-id="bc54e-330">Le code ci-dessus a montré la génération d’une URL en passant le nom du contrôleur et le nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-330">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="bc54e-331">`IUrlHelper` fournit également la famille de méthodes `Url.RouteUrl`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-331">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="bc54e-332">Ces méthodes sont similaires à `Url.Action`, mais elle ne copient pas les valeurs actuelles de `action` et de `controller` vers les valeurs de route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-332">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="bc54e-333">L’utilisation la plus courante est de spécifier un nom de route pour utiliser une route spécifique pour générer l’URL, généralement *sans* spécifier un nom de contrôleur ou d’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-333">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="bc54e-334">Génération des URL en HTML</span><span class="sxs-lookup"><span data-stu-id="bc54e-334">Generating URLs in HTML</span></span>

<span data-ttu-id="bc54e-335">`IHtmlHelper` fournit les méthodes `HtmlHelper` `Html.BeginForm` et `Html.ActionLink` pour générer respectivement les éléments `<form>` et `<a>`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-335">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="bc54e-336">Ces méthodes utilisent la méthode `Url.Action` pour générer une URL et ils acceptent les arguments similaires.</span><span class="sxs-lookup"><span data-stu-id="bc54e-336">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="bc54e-337">Les pendants de `Url.RouteUrl` pour `HtmlHelper` sont `Html.BeginRouteForm` et `Html.RouteLink`, qui ont des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="bc54e-337">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="bc54e-338">Les TagHelpers génèrent des URL via le TagHelper `form` et le TagHelper `<a>`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-338">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="bc54e-339">Ils utilisent tous les deux `IUrlHelper` pour leur implémentation.</span><span class="sxs-lookup"><span data-stu-id="bc54e-339">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="bc54e-340">Pour plus d’informations, consultez [Utilisation des formulaires](../views/working-with-forms.md).</span><span class="sxs-lookup"><span data-stu-id="bc54e-340">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="bc54e-341">Dans les vues, `IUrlHelper` est disponible via la propriété `Url` pour toute génération d’URL ad hoc non couverte par ce qui figure ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="bc54e-341">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="bc54e-342">Génération des URL dans les résultats d’action</span><span class="sxs-lookup"><span data-stu-id="bc54e-342">Generating URLS in Action Results</span></span>

<span data-ttu-id="bc54e-343">Les exemples précédents ont montré l’utilisation de `IUrlHelper` dans un contrôleur, alors que l’utilisation la plus courante dans un contrôleur est de générer une URL dans le cadre d’un résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-343">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="bc54e-344">Les classes de base `ControllerBase` et `Controller` fournissent des méthodes pratiques pour les résultats d’action qui référencent une autre action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-344">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="bc54e-345">Une utilisation typique est de rediriger après acceptation de l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bc54e-345">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="bc54e-346">Les méthodes de fabrique de résultats d’action suivent un modèle similaire aux méthodes sur `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-346">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="bc54e-347">Cas spécial pour les routes conventionnelles dédiées</span><span class="sxs-lookup"><span data-stu-id="bc54e-347">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="bc54e-348">Le routage conventionnel peut utiliser un type spécial de définition de route appelé *route conventionnelle dédiée*.</span><span class="sxs-lookup"><span data-stu-id="bc54e-348">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="bc54e-349">Dans l’exemple ci-dessous, la route nommée `blog` est une route conventionnelle dédiée.</span><span class="sxs-lookup"><span data-stu-id="bc54e-349">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="bc54e-350">En utilisant ces définitions de route, `Url.Action("Index", "Home")` génère le chemin d’URL `/` avec la route `default`, mais pourquoi ?</span><span class="sxs-lookup"><span data-stu-id="bc54e-350">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="bc54e-351">Vous pouvez deviner que les valeurs de route `{ controller = Home, action = Index }` seraient suffisantes pour générer une URL avec `blog`, et que le résultat serait `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-351">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="bc54e-352">Les routes conventionnelles dédiées s’appuient sur un comportement spécial des valeurs par défaut qui n’ont pas de paramètre de route correspondant qui empêche la route d’être « trop globale » avec la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-352">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="bc54e-353">Dans ce cas, les valeurs par défaut sont `{ controller = Blog, action = Article }`, et ni `controller` ni `action` n’apparaissent comme paramètre de route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-353">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="bc54e-354">Quand le routage effectue une génération d’URL, les valeurs fournies doivent correspondre aux valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="bc54e-354">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="bc54e-355">La génération d’URL avec `blog` échoue, car les valeurs `{ controller = Home, action = Index }` ne correspondent pas à `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-355">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="bc54e-356">Le routage essaye alors d’utiliser `default`, ce qui réussit.</span><span class="sxs-lookup"><span data-stu-id="bc54e-356">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="bc54e-357">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="bc54e-357">Areas</span></span>

<span data-ttu-id="bc54e-358">Les [zones](areas.md) sont une fonctionnalité de MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms de routage distinct (pour les actions de contrôleur) et d’une structure de dossiers (pour les vues).</span><span class="sxs-lookup"><span data-stu-id="bc54e-358">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="bc54e-359">L’utilisation de zones permet à une application d’avoir plusieurs contrôleurs portant le même nom, pour autant qu’ils soient dans des *zones* différentes.</span><span class="sxs-lookup"><span data-stu-id="bc54e-359">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="bc54e-360">L’utilisation de zones crée une hiérarchie qui permet le routage par ajout d’un autre paramètre de route, `area`, à `controller` et à `action`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-360">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="bc54e-361">Cette section explique comment le routage interagit avec les zones. Pour plus d’informations sur l’utilisation des zones avec des vues, consultez [Zones](areas.md).</span><span class="sxs-lookup"><span data-stu-id="bc54e-361">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="bc54e-362">L’exemple suivant configure MVC pour utiliser la route conventionnelle par défaut et une *route de zone* pour une zone nommée `Blog` :</span><span class="sxs-lookup"><span data-stu-id="bc54e-362">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="bc54e-363">Lors de la mise en correspondance d’un chemin d’URL comme `/Manage/Users/AddUser`, la première route produit les valeurs de route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-363">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="bc54e-364">La valeur de route `area` est produite par une valeur par défaut pour `area` ; en fait, la route créée par `MapAreaRoute` est équivalente à la suivante :</span><span class="sxs-lookup"><span data-stu-id="bc54e-364">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="bc54e-365">`MapAreaRoute` crée une route avec à la fois une valeur par défaut et une contrainte pour `area` en utilisant le nom de la zone fournie, dans ce cas `Blog`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-365">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="bc54e-366">La valeur par défaut garantit que la route produit toujours `{ area = Blog, ... }`, et la contrainte nécessite la valeur `{ area = Blog, ... }` pour la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-366">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="bc54e-367">Le routage conventionnel est dépendant de l’ordre.</span><span class="sxs-lookup"><span data-stu-id="bc54e-367">Conventional routing is order-dependent.</span></span> <span data-ttu-id="bc54e-368">En général, les routes avec des zones doivent être placées plus haut dans la table de routage, car elles sont plus spécifiques que les routes sans zone.</span><span class="sxs-lookup"><span data-stu-id="bc54e-368">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="bc54e-369">Avec l’exemple ci-dessus, les valeurs de route seraient mises en correspondance avec l’action suivante :</span><span class="sxs-lookup"><span data-stu-id="bc54e-369">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="bc54e-370">`AreaAttribute` est ce qui indique qu’un contrôleur fait partie d’une zone : nous disons que ce contrôleur est dans la zone `Blog`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-370">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="bc54e-371">Les contrôleurs sans attribut `[Area]` ne sont membres d’aucune zone et ne sont **pas** trouvés en correspondance quand la valeur de route `area` est fournie par le routage.</span><span class="sxs-lookup"><span data-stu-id="bc54e-371">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="bc54e-372">Dans l’exemple suivant, seul le premier contrôleur répertorié peut correspondre aux valeurs de route `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-372">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="bc54e-373">L’espace de noms de chaque contrôleur est montré ici par souci d’exhaustivité, sans quoi les contrôleurs auraient un conflit de noms et généreraient une erreur de compilateur.</span><span class="sxs-lookup"><span data-stu-id="bc54e-373">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="bc54e-374">Les espaces de noms de classe n’ont pas d’effet sur le routage de MVC.</span><span class="sxs-lookup"><span data-stu-id="bc54e-374">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="bc54e-375">Les deux premiers contrôleurs sont membres de zones, et ils sont trouvés en correspondance seulement quand le nom de leur zone respective est fourni par la valeur de route `area`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-375">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="bc54e-376">Le troisième contrôleur n’est membre d’aucune zone et peut être trouvé en correspondance seulement quand aucune valeur pour `area` n’est fournie par le routage.</span><span class="sxs-lookup"><span data-stu-id="bc54e-376">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-377">En termes de mise en correspondance avec *aucune valeur*, l’absence de la valeur `area` est identique à une valeur null ou de chaîne vide pour `area`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-377">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="bc54e-378">Lors de l’exécution d’une action à l’intérieur d’une zone, la valeur de route pour `area` est disponible en tant que *valeur ambiante*, que le routage peut utiliser pour la génération d’URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-378">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="bc54e-379">Cela signifie que par défaut, les zones agissent *par attraction* pour la génération d’URL, comme le montre l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="bc54e-379">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="bc54e-380">Présentation d’IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="bc54e-380">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="bc54e-381">Cette section est une présentation détaillée des mécanismes internes du framework et de la façon dont MVC choisit une action à exécuter.</span><span class="sxs-lookup"><span data-stu-id="bc54e-381">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="bc54e-382">Une application classique n’a pas besoin d’une `IActionConstraint` personnalisée.</span><span class="sxs-lookup"><span data-stu-id="bc54e-382">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="bc54e-383">Vous avez probablement déjà utilisé `IActionConstraint` même si vous n’êtes pas familiarisé avec l’interface.</span><span class="sxs-lookup"><span data-stu-id="bc54e-383">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="bc54e-384">L’attribut `[HttpGet]` et les attributs `[Http-VERB]` similaires implémentent `IActionConstraint` de façon à limiter l’exécution d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="bc54e-384">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="bc54e-385">Dans l’hypothèse d’une route conventionnelle par défaut, le chemin d’URL `/Products/Edit` produirait les valeurs `{ controller = Products, action = Edit }`, qui seraient en correspondance avec **les deux** actions montrées ici.</span><span class="sxs-lookup"><span data-stu-id="bc54e-385">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="bc54e-386">Dans la terminologie `IActionConstraint`, nous pouvons dire que ces deux actions sont considérées comme candidates, car elles correspondent toutes deux aux données de la route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-386">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="bc54e-387">Quand `HttpGetAttribute` s’exécute, il indique que *Edit()* est une correspondance pour *GET* et qu’il n’est une correspondance pour aucun des autres verbes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc54e-387">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="bc54e-388">L’action `Edit(...)` n’a aucune contrainte définie et elle ne correspond donc à aucun verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc54e-388">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="bc54e-389">Par conséquent, dans l’hypothèse d’un `POST`, seul `Edit(...)` est en correspondance.</span><span class="sxs-lookup"><span data-stu-id="bc54e-389">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="bc54e-390">Cependant, pour un `GET`, les deux actions peuvent néanmoins être en correspondance, mais une action avec `IActionConstraint` est toujours considérée comme étant *meilleure*  qu’une action sans.</span><span class="sxs-lookup"><span data-stu-id="bc54e-390">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="bc54e-391">Par conséquent, comme `Edit()` a `[HttpGet]`, elle est considérée comme étant plus spécifique et est sélectionnée si les deux actions peuvent correspondre.</span><span class="sxs-lookup"><span data-stu-id="bc54e-391">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="bc54e-392">D’un point de vue conceptuel, `IActionConstraint` est une forme de *surcharge*, mais au lieu de surcharger des méthodes portant le même nom, elle surcharge entre des actions qui correspondent à la même URL.</span><span class="sxs-lookup"><span data-stu-id="bc54e-392">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="bc54e-393">Le routage par attributs utilise également `IActionConstraint` et peut aboutir à des actions de différents contrôleurs, toutes considérées comme candidates.</span><span class="sxs-lookup"><span data-stu-id="bc54e-393">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="bc54e-394">Implémentation d’IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="bc54e-394">Implementing IActionConstraint</span></span>

<span data-ttu-id="bc54e-395">La façon la plus simple d’implémenter une `IActionConstraint` est de créer une classe dérivée de `System.Attribute` et de la placer sur vos actions et vos contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-395">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="bc54e-396">MVC découvre automatiquement les `IActionConstraint` qui sont appliquées en tant qu’attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-396">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="bc54e-397">Vous pouvez utiliser le modèle d’application pour appliquer des contraintes, et il s’agit probablement de l’approche la plus souple, car elle vous permet de programmer des méta-informations indiquant comment elles sont appliquées.</span><span class="sxs-lookup"><span data-stu-id="bc54e-397">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="bc54e-398">Dans l’exemple suivant, une contrainte choisit une action en fonction d’un *code pays* provenant des données de la route.</span><span class="sxs-lookup"><span data-stu-id="bc54e-398">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="bc54e-399">[Exemple complet sur GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="bc54e-399">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="bc54e-400">Vous êtes responsable de l’implémentation de la méthode `Accept` et du choix d’un « ordre » pour l’exécution de la contrainte.</span><span class="sxs-lookup"><span data-stu-id="bc54e-400">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="bc54e-401">Dans ce cas, la méthode `Accept` retourne `true` pour indiquer que l’action est une correspondance quand la valeur de la route `country` correspond à la valeur.</span><span class="sxs-lookup"><span data-stu-id="bc54e-401">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="bc54e-402">Ceci diffère d’un `RouteValueAttribute`, car elle permet à défaut d’appliquer une action sans attributs.</span><span class="sxs-lookup"><span data-stu-id="bc54e-402">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="bc54e-403">L’exemple montre que si vous définissez une action `en-US`, un code pays comme `fr-FR` passe à défaut à un contrôleur plus générique auquel `[CountrySpecific(...)]` n’est pas appliqué.</span><span class="sxs-lookup"><span data-stu-id="bc54e-403">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="bc54e-404">La propriété `Order` décide de *l’étape* dont la contrainte fait partie.</span><span class="sxs-lookup"><span data-stu-id="bc54e-404">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="bc54e-405">Les contraintes d’action s’exécutent dans des groupes en fonction de `Order`.</span><span class="sxs-lookup"><span data-stu-id="bc54e-405">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="bc54e-406">Par exemple, tous les attributs de méthode HTTP fournis par le framework utilisent la même valeur pour `Order`, de façon à ce qu’ils s’exécutent dans la même étape.</span><span class="sxs-lookup"><span data-stu-id="bc54e-406">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="bc54e-407">Vous pouvez avoir autant d’étapes que nécessaire pour implémenter les stratégies souhaitées.</span><span class="sxs-lookup"><span data-stu-id="bc54e-407">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="bc54e-408">Pour décider d’une valeur pour `Order`, déterminez si votre contrainte doit ou non être appliquée avant les méthodes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc54e-408">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="bc54e-409">Les nombres les plus petits s’exécutent en premier.</span><span class="sxs-lookup"><span data-stu-id="bc54e-409">Lower numbers run first.</span></span>
