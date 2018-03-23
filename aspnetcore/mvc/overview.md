---
title: Vue d’ensemble d’ASP.NET Core MVC
author: ardalis
description: ASP.NET Core MVC est une infrastructure riche pour la création d’applications web et d'API à l’aide du modèle de conception Model-View-Controller.
manager: wpickett
ms.author: riande
ms.date: 01/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/overview
ms.openlocfilehash: 16fd1b5e71cde4364f02640f504d42218ed680df
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="e3b80-103">Vue d’ensemble d’ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e3b80-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="e3b80-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e3b80-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e3b80-105">ASP.NET Core MVC est une infrastructure riche pour la création d’applications web et d'APIs à l’aide du modèle de conception Model-View-Controller.</span><span class="sxs-lookup"><span data-stu-id="e3b80-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="e3b80-106">Quel est le modèle de conception MVC ?</span><span class="sxs-lookup"><span data-stu-id="e3b80-106">What is the MVC pattern?</span></span>

<span data-ttu-id="e3b80-107">Le modèle d’architecture Model-View-Controller (MVC) sépare une application en trois groupes de composants principaux : les modèles, les vues et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="e3b80-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="e3b80-108">Ce modèle permet d’effectuer la [séparation des préoccupations](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="e3b80-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="e3b80-109">En utilisant ce modèle, les demandes de l’utilisateur sont acheminées vers un contrôleur qui a la responsabilité de fonctionner avec le modèle pour effectuer des actions de l’utilisateur et/ou de récupérer les résultats de requêtes.</span><span class="sxs-lookup"><span data-stu-id="e3b80-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="e3b80-110">Le contrôleur choisit la vue à afficher à l’utilisateur et lui fournit toutes les données de modèle dont elle a besoin.</span><span class="sxs-lookup"><span data-stu-id="e3b80-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="e3b80-111">Le diagramme suivant montre les trois composants principaux et les références entre eux :</span><span class="sxs-lookup"><span data-stu-id="e3b80-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Modèle MVC](overview/_static/mvc.png)

<span data-ttu-id="e3b80-113">Cette délimitation des responsabilités vous aide à mettre à l’échelle la complexité de l’application, car il est plus facile de coder, déboguer et tester une chose (modèle, vue ou contrôleur) qui a un seul travail (et suit le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="e3b80-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="e3b80-114">Il est plus difficile de mettre à jour, tester et déboguer du code qui a des dépendances réparties sur deux ou plusieurs de ces trois zones.</span><span class="sxs-lookup"><span data-stu-id="e3b80-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="e3b80-115">Par exemple, la logique de l’interface utilisateur a tendance à changer plus fréquemment que la logique métier.</span><span class="sxs-lookup"><span data-stu-id="e3b80-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="e3b80-116">Si le code de présentation et la logique métier sont combinés en un seul objet, l’objet contenant la logique métier doit être modifié chaque fois que l’interface utilisateur change.</span><span class="sxs-lookup"><span data-stu-id="e3b80-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="e3b80-117">Cela introduit souvent des erreurs et nécessite de retester la logique métier après chaque changement minimal de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e3b80-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="e3b80-118">La vue et le contrôleur dépendent du modèle.</span><span class="sxs-lookup"><span data-stu-id="e3b80-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="e3b80-119">Toutefois, le modèle ne dépend ni de la vue, ni du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e3b80-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="e3b80-120">Il s’agit d’un des principaux avantages de la séparation.</span><span class="sxs-lookup"><span data-stu-id="e3b80-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="e3b80-121">Cette séparation permet au modèle d'être généré et testé indépendamment de la présentation visuelle.</span><span class="sxs-lookup"><span data-stu-id="e3b80-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="e3b80-122">Responsabilités du modèle</span><span class="sxs-lookup"><span data-stu-id="e3b80-122">Model Responsibilities</span></span>

<span data-ttu-id="e3b80-123">Le modèle dans une application MVC représente l’état de l’application et de n'importe quelles logiques métier ou opérations qui doivent être effectuées par celui-ci.</span><span class="sxs-lookup"><span data-stu-id="e3b80-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="e3b80-124">La logique métier doit être encapsulée dans le modèle, ainsi que toute la logique d’implémentation de la persistance de l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="e3b80-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="e3b80-125">En général, les vues fortement typées utilisent des types ViewModel conçus pour contenir les données à afficher sur cette vue.</span><span class="sxs-lookup"><span data-stu-id="e3b80-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="e3b80-126">Le contrôleur crée et remplit ces instances de ViewModel à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="e3b80-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="e3b80-127">Il existe de nombreuses façons d’organiser le modèle dans une application qui utilise le modèle d'architecture MVC.</span><span class="sxs-lookup"><span data-stu-id="e3b80-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="e3b80-128">En savoir plus sur les [différents types de types de modèles](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="e3b80-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="e3b80-129">Responsabilités de la vue</span><span class="sxs-lookup"><span data-stu-id="e3b80-129">View Responsibilities</span></span>

<span data-ttu-id="e3b80-130">Les vues sont chargées de présenter du contenu via l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e3b80-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="e3b80-131">Elles utilisent le [moteur d’affichage Razor](#razor-view-engine) pour incorporer le code .NET dans le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="e3b80-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="e3b80-132">Il doit y avoir un minimum de logique dans les vues, et toute logique présente doit être liée à la présentation du contenu.</span><span class="sxs-lookup"><span data-stu-id="e3b80-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="e3b80-133">Si vous trouvez nécessaire d’effectuer une grande partie de la logique dans l’affichage des fichiers pour afficher des données à partir d’un modèle complexe, envisagez d’utiliser un [composant de vue (View Component)](views/view-components.md), un ViewModel, ou un modèle d’affichage (view template) pour simplifier l’affichage.</span><span class="sxs-lookup"><span data-stu-id="e3b80-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="e3b80-134">Responsabilités du contrôleur</span><span class="sxs-lookup"><span data-stu-id="e3b80-134">Controller Responsibilities</span></span>

<span data-ttu-id="e3b80-135">Les contrôleurs sont les composants qui gèrent l'interaction avec l’utilisateur, travaillent avec le modèle et finalement sélectionnent une vue à restituer.</span><span class="sxs-lookup"><span data-stu-id="e3b80-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="e3b80-136">Dans une application MVC, la vue affiche uniquement les informations ; le contrôleur gère et répond à la saisie de l’utilisateur et à l’interaction.</span><span class="sxs-lookup"><span data-stu-id="e3b80-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="e3b80-137">Dans le modèle MVC, le contrôleur est le point d’entrée initial et est chargé de sélectionner les types de modèle avec lequels travailler et la vue à restituer (d'où son nom - il contrôle la manière dont l’application répond à une requête donnée).</span><span class="sxs-lookup"><span data-stu-id="e3b80-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="e3b80-138">Vous devez éviter les excès de responsabilités pour ne pas rendre les contrôleurs trop complexes.</span><span class="sxs-lookup"><span data-stu-id="e3b80-138">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="e3b80-139">Pour empêcher la logique du contrôleur de devenir trop complexe, utilisez le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/) enlevant de la logique métier en dehors du contrôleur pour la placer dans le modèle de domaine.</span><span class="sxs-lookup"><span data-stu-id="e3b80-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="e3b80-140">Si vous trouvez que les actions du contrôleur effectuent fréquemment les mêmes types d’actions, vous pouvez suivre le [principe DRY (Don't Repeat Yourself : Ne vous répétez pas) ](http://deviq.com/don-t-repeat-yourself/) en déplaçant ces actions courantes dans des [filtres](#filters).</span><span class="sxs-lookup"><span data-stu-id="e3b80-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="e3b80-141">Nouveautés d’ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e3b80-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="e3b80-142">L’infrastructure ASP.NET MVC Core est un framework de présentation léger, open source, facilement testable et optimisé pour une utilisation avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3b80-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="e3b80-143">ASP.NET Core MVC offre un fonctionnement basé sur des patterns pour créer des sites Web dynamiques qui permettent une séparation claire des préoccupations.</span><span class="sxs-lookup"><span data-stu-id="e3b80-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="e3b80-144">Il vous donne un contrôle total sur le balisage, prend en charge les développements TDD et utilise les standards web les plus récents.</span><span class="sxs-lookup"><span data-stu-id="e3b80-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="e3b80-145">Fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="e3b80-145">Features</span></span>

<span data-ttu-id="e3b80-146">ASP.NET Core MVC inclut les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e3b80-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="e3b80-147">Le routage</span><span class="sxs-lookup"><span data-stu-id="e3b80-147">Routing</span></span>](#routing)
* [<span data-ttu-id="e3b80-148">Liaison de modèles</span><span class="sxs-lookup"><span data-stu-id="e3b80-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="e3b80-149">Validation du modèle</span><span class="sxs-lookup"><span data-stu-id="e3b80-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="e3b80-150">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e3b80-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="e3b80-151">Les filtres</span><span class="sxs-lookup"><span data-stu-id="e3b80-151">Filters</span></span>](#filters)
* [<span data-ttu-id="e3b80-152">Les zones (areas)</span><span class="sxs-lookup"><span data-stu-id="e3b80-152">Areas</span></span>](#areas)
* [<span data-ttu-id="e3b80-153">Les APIs Web</span><span class="sxs-lookup"><span data-stu-id="e3b80-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="e3b80-154">La testabilité</span><span class="sxs-lookup"><span data-stu-id="e3b80-154">Testability</span></span>](#testability)
* [<span data-ttu-id="e3b80-155">Moteur de vue Razor</span><span class="sxs-lookup"><span data-stu-id="e3b80-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="e3b80-156">Les vues fortement typées</span><span class="sxs-lookup"><span data-stu-id="e3b80-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="e3b80-157">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="e3b80-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="e3b80-158">Les composants de vues (View components)</span><span class="sxs-lookup"><span data-stu-id="e3b80-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="e3b80-159">Routage</span><span class="sxs-lookup"><span data-stu-id="e3b80-159">Routing</span></span>

<span data-ttu-id="e3b80-160">ASP.NET Core MVC est construit sur [le routage d'ASP.NET Core](../fundamentals/routing.md), un composant de mappage d’URL puissant qui vous permet de créer des applications ayant des URLs compréhensibles et découvrables.</span><span class="sxs-lookup"><span data-stu-id="e3b80-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="e3b80-161">Cela vous permet de définir les patterns de noms d’URL de votre application qui fonctionnent bien pour l’optimisation des moteurs de recherche (SEO) et pour la génération de lien, sans tenir compte de la façon dont les fichiers sont organisés sur votre serveur web.</span><span class="sxs-lookup"><span data-stu-id="e3b80-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="e3b80-162">Vous pouvez définir vos routes à l’aide d’une syntaxe pratique de modèle de routes (route template) qui prend en charge les contraintes de valeur, les valeurs par défaut et les valeurs facultatives de routes.</span><span class="sxs-lookup"><span data-stu-id="e3b80-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="e3b80-163">*Le routage basé sur une convention* vous permet de définir globalement les formats d'URL que votre application accepte et comment chacun de ces formats est mappé à une méthode d’action spécifique sur un contrôleur donné.</span><span class="sxs-lookup"><span data-stu-id="e3b80-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="e3b80-164">Lorsqu’une demande entrante est reçue, le moteur de routage analyse l’URL et la fait correspondre à l’un des formats d’URL définis et appelle ensuite la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="e3b80-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="e3b80-165">*Le routage par attribut (Attribute routing)* vous permet de spécifier des informations de routage en décorant vos contrôleurs et vos actions avec les attributs qui définissent les routes de votre application.</span><span class="sxs-lookup"><span data-stu-id="e3b80-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="e3b80-166">Cela signifie que vos définitions de route sont placées dans le contrôleur au-dessus de l’action avec laquelle elles sont associées.</span><span class="sxs-lookup"><span data-stu-id="e3b80-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="e3b80-167">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="e3b80-167">Model binding</span></span>

<span data-ttu-id="e3b80-168">La [liaison de modèle](models/model-binding.md) ASP.NET Core MVC convertit les données de requête client (les valeurs de formulaire, les données de routage, les paramètres de chaîne de requête, les en-têtes HTTP) en objets que le contrôleur peut traiter.</span><span class="sxs-lookup"><span data-stu-id="e3b80-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="e3b80-169">Par conséquent, votre logique de contrôleur ne doit pas nécessairement faire le travail d’identifier les données de requête entrante; Il a simplement les données en tant que paramètres dans ses méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="e3b80-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="e3b80-170">Validation du modèle</span><span class="sxs-lookup"><span data-stu-id="e3b80-170">Model validation</span></span>

<span data-ttu-id="e3b80-171">ASP.NET Core MVC prend en charge la [validation](models/validation.md) en décorant votre objet de modèle avec des attributs de validation de données d’annotation.</span><span class="sxs-lookup"><span data-stu-id="e3b80-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="e3b80-172">Les attributs de validation sont vérifiés côté client avant que les valeurs ne soient publiées sur le serveur, ainsi que sur le serveur avant que l’action du contrôleur ne soit appelée.</span><span class="sxs-lookup"><span data-stu-id="e3b80-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="e3b80-173">Action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e3b80-173">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="e3b80-174">Le framework gère la validation des données de requête à la fois sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e3b80-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="e3b80-175">La logique de validation spécifiée sur les types de modèle est ajoutée aux vues rendues sous forme d’annotations discrètes (unobtrusive) et est réalisée dans le navigateur avec [jQuery Validation](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="e3b80-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="e3b80-176">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e3b80-176">Dependency injection</span></span>

<span data-ttu-id="e3b80-177">ASP.NET Core a une prise en charge intégrée pour l'[injection de dépendance (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e3b80-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="e3b80-178">Dans ASP.NET Core MVC, les [contrôleurs](controllers/dependency-injection.md) peuvent faire appel à des services nécessaires via leurs constructeurs, ce qui leur permet de suivre le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="e3b80-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="e3b80-179">Votre application peut également utiliser l'[injection de dépendance dans les fichiers de vue](views/dependency-injection.md), à l’aide de la directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="e3b80-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="e3b80-180">Filtres</span><span class="sxs-lookup"><span data-stu-id="e3b80-180">Filters</span></span>

<span data-ttu-id="e3b80-181">Les [filtres](controllers/filters.md) aident les développeurs à encapsuler des problèmes transversaux, tels que la gestion des exceptions ou l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="e3b80-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="e3b80-182">Les filtres permettent une logique personnalisée en cours d’exécution et de post-traitement pour les méthodes d’action et peuvent être configurés pour s’exécuter à certains endroits dans le pipeline de l’exécution pour une demande donnée.</span><span class="sxs-lookup"><span data-stu-id="e3b80-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="e3b80-183">Les filtres peuvent être appliqués aux contrôleurs ou aux actions sous forme d’attributs (ou peuvent être exécutés de manière globale).</span><span class="sxs-lookup"><span data-stu-id="e3b80-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="e3b80-184">Plusieurs filtres (tels que `Authorize`) sont inclus dans le framework.</span><span class="sxs-lookup"><span data-stu-id="e3b80-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="e3b80-185">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="e3b80-185">Areas</span></span>

<span data-ttu-id="e3b80-186">Les [zones](controllers/areas.md) fournissent un moyen de partitionner une application Web ASP.NET Core MVC volumineuse en regroupements fonctionnels plus petits.</span><span class="sxs-lookup"><span data-stu-id="e3b80-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="e3b80-187">Une zone est en réalité une structure MVC à l’intérieur d’une application.</span><span class="sxs-lookup"><span data-stu-id="e3b80-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="e3b80-188">Dans un projet MVC, les composants logiques tels que les modèles, les contrôleurs et les vues sont conservés dans des dossiers différents et MVC utilise les conventions de nommage pour créer la relation entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="e3b80-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="e3b80-189">Pour une application volumineuse, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de premier niveau.</span><span class="sxs-lookup"><span data-stu-id="e3b80-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="e3b80-190">Par exemple, une application de commerce électronique avec plusieurs entités, telles que le paiement, le facturation et la recherche, etc. Chacune de ces unités a ses propres composants logiques de vues, contrôleurs et modèles.</span><span class="sxs-lookup"><span data-stu-id="e3b80-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="e3b80-191">API web</span><span class="sxs-lookup"><span data-stu-id="e3b80-191">Web APIs</span></span>

<span data-ttu-id="e3b80-192">En plus de constituer une plate-forme idéale pour la création de sites web, ASP.NET Core MVC a une très bonne prise en charge pour la génération d'API Web.</span><span class="sxs-lookup"><span data-stu-id="e3b80-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="e3b80-193">Vous pouvez créer des services accessibles à un large éventail de clients, notamment les navigateurs et les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="e3b80-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="e3b80-194">Le framework inclut le support de la négociation de contenu HTTP avec prise en charge intégrée pour la [mise en forme des données](models/formatting.md) en JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="e3b80-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="e3b80-195">Vous pouvez écrire des [formateurs personnalisés](advanced/custom-formatters.md) pour ajouter la prise en charge de vos propres formats.</span><span class="sxs-lookup"><span data-stu-id="e3b80-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="e3b80-196">Utilisez la génération de lien pour activer la prise en charge de liens hypermédia.</span><span class="sxs-lookup"><span data-stu-id="e3b80-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="e3b80-197">Activez facilement la prise en charge du [partage de ressources cross-origin (CORS)](http://www.w3.org/TR/cors/) afin que vos APIs Web puissent être partagées entre plusieurs applications Web.</span><span class="sxs-lookup"><span data-stu-id="e3b80-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="e3b80-198">Testabilité</span><span class="sxs-lookup"><span data-stu-id="e3b80-198">Testability</span></span>

<span data-ttu-id="e3b80-199">L'utilisation des interfaces et de l'injection de dépendances de l’infrastructure conviennent parfaitement aux tests unitaires et l’infrastructure comprend des fonctionnalités (par exemple, un fournisseur TestHost et InMemory pour Entity Framework) qui rendent les [tests d’intégration](../testing/integration-testing.md) rapides et simples.</span><span class="sxs-lookup"><span data-stu-id="e3b80-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="e3b80-200">En savoir plus sur [logique de test de contrôleur](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="e3b80-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="e3b80-201">Moteur de vue Razor</span><span class="sxs-lookup"><span data-stu-id="e3b80-201">Razor view engine</span></span>

<span data-ttu-id="e3b80-202">[Les vues ASP.NET Core MVC](views/overview.md) utilisent le [moteur d’affichage Razor](views/razor.md) pour restituer les vues.</span><span class="sxs-lookup"><span data-stu-id="e3b80-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="e3b80-203">Razor est un langage de balisage de modèle compact, expressif et fluide pour définir des vues à l’aide de code c# incorporé.</span><span class="sxs-lookup"><span data-stu-id="e3b80-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="e3b80-204">Razor est utilisé pour générer dynamiquement le contenu web sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e3b80-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="e3b80-205">Vous pouvez mélangez proprement du code serveur avec du contenu et du code côté client.</span><span class="sxs-lookup"><span data-stu-id="e3b80-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="e3b80-206">En utilisant le moteur d’affichage Razor, vous pouvez définir des [dispositions](views/layout.md), des [vues partielles](views/partial.md) et des sections remplaçables.</span><span class="sxs-lookup"><span data-stu-id="e3b80-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="e3b80-207">Vues fortement typées</span><span class="sxs-lookup"><span data-stu-id="e3b80-207">Strongly typed views</span></span>

<span data-ttu-id="e3b80-208">Les vues Razor dans MVC peuvent être fortement typées en fonction de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="e3b80-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="e3b80-209">les contrôleurs peuvent passer un modèle fortement typé aux vues en permettant la vérification du type et le support de l'IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e3b80-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="e3b80-210">Par exemple, la vue suivante affiche un modèle de type `IEnumerable<Product>` :</span><span class="sxs-lookup"><span data-stu-id="e3b80-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="e3b80-211">Tag helpers</span><span class="sxs-lookup"><span data-stu-id="e3b80-211">Tag Helpers</span></span>

<span data-ttu-id="e3b80-212">Les [Tag helpers](views/tag-helpers/intro.md) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b80-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="e3b80-213">Vous pouvez utiliser des Tag helpers pour définir des balises personnalisées (par exemple, `<environment>`) ou pour modifier le comportement de balises existantes (par exemple, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="e3b80-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="e3b80-214">Les Tag helpers associent des éléments spécifiques en fonction du nom de l’élément et des ses attributs.</span><span class="sxs-lookup"><span data-stu-id="e3b80-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="e3b80-215">Ils fournissent les avantages de rendu côté serveur tout en conservant la possibilité d'éditer le HTML.</span><span class="sxs-lookup"><span data-stu-id="e3b80-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="e3b80-216">Il existe de nombreux Tag helpers intégrés pour les tâches courantes - telles que la création de formulaires, des liens, de chargement de ressources et plus - et bien d'autres sont disponibles dans les dépôts GitHub publics et sous forme de NuGet packages.</span><span class="sxs-lookup"><span data-stu-id="e3b80-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="e3b80-217">Les Tag helpers sont créés en c#, et ils ciblent des éléments HTML en fonction de la balise parente, du nom d’attribut ou du nom de l’élément.</span><span class="sxs-lookup"><span data-stu-id="e3b80-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="e3b80-218">Par exemple, la fonction intégrée LinkTagHelper peut être utilisée pour créer un lien vers l'action `Login` du `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="e3b80-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="e3b80-219">L'`EnvironmentTagHelper` peut être utilisé pour inclure des scripts différents dans vos vues (par exemple, bruts ou minifiés) en fonction de l’environnement d’exécution, par exemple le développement, le staging ou la production :</span><span class="sxs-lookup"><span data-stu-id="e3b80-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="e3b80-220">Les Tag helpers fournissent une expérience de développement HTML conviviale et un environnement IntelliSense riche pour la création de balises HTML et Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b80-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="e3b80-221">La plupart des Tag helpers intégrés ciblent des éléments HTML existants et fournissent des attributs côté serveur pour l’élément.</span><span class="sxs-lookup"><span data-stu-id="e3b80-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="e3b80-222">View components</span><span class="sxs-lookup"><span data-stu-id="e3b80-222">View Components</span></span>

<span data-ttu-id="e3b80-223">Les [View components](views/view-components.md) vous permettent d’empaqueter la logique de rendu et la réutiliser dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="e3b80-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="e3b80-224">Ils ressemblent aux [vues partielles](views/partial.md), mais avec de la logique associée.</span><span class="sxs-lookup"><span data-stu-id="e3b80-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
