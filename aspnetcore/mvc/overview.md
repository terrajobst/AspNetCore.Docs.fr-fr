---
title: "Vue d’ensemble du modèle MVC d’ASP.NET Core"
author: ardalis
description: "Découvrez ASP.NET Core MVC, un puissant framework qui vous permet de générer des applications web et des API à l’aide du modèle de conception Model-View-Controller."
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
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="42c67-103">Vue d’ensemble du modèle MVC d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42c67-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="42c67-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="42c67-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="42c67-105">ASP.NET Core MVC est un puissant framework qui vous permet de générer des applications web et des API à l’aide du modèle de conception Model-View-Controller.</span><span class="sxs-lookup"><span data-stu-id="42c67-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="42c67-106">Qu’est-ce que le modèle MVC ?</span><span class="sxs-lookup"><span data-stu-id="42c67-106">What is the MVC pattern?</span></span>

<span data-ttu-id="42c67-107">Le modèle architectural MVC (Model-View-Controller) sépare une application en trois groupes principaux de composants : les modèles, les vues et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="42c67-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="42c67-108">Ce modèle permet d’obtenir une [séparation des responsabilités](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="42c67-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="42c67-109">Avec ce modèle, les requêtes des utilisateurs sont acheminées vers un contrôleur chargé d’interagir avec le modèle pour effectuer des actions utilisateur et/ou récupérer les résultats des requêtes.</span><span class="sxs-lookup"><span data-stu-id="42c67-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="42c67-110">Le contrôleur choisit la vue à afficher à l’utilisateur et lui fournit les données de modèle dont il a besoin.</span><span class="sxs-lookup"><span data-stu-id="42c67-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="42c67-111">Le diagramme suivant montre les trois composants principaux et leurs relations entre eux :</span><span class="sxs-lookup"><span data-stu-id="42c67-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Modèle MVC](overview/_static/mvc.png)

<span data-ttu-id="42c67-113">Cette délimitation des responsabilités vous aide à mettre à l’échelle la complexité de l’application, car il est plus facile de coder, déboguer et tester une chose (modèle, vue ou contrôleur) qui a un seul travail (et suit le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="42c67-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="42c67-114">Il est plus difficile de mettre à jour, tester et déboguer du code dont les dépendances sont réparties sur deux de ces domaines ou plus.</span><span class="sxs-lookup"><span data-stu-id="42c67-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="42c67-115">Par exemple, la logique de l’interface utilisateur a tendance à changer plus fréquemment que la logique métier.</span><span class="sxs-lookup"><span data-stu-id="42c67-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="42c67-116">Si le code de présentation et la logique métier sont combinés en un seul objet, l’objet contenant la logique métier doit être modifié chaque fois que l’interface utilisateur change.</span><span class="sxs-lookup"><span data-stu-id="42c67-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="42c67-117">Cela introduit souvent des erreurs et nécessite de retester la logique métier après chaque changement minimal de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="42c67-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="42c67-118">La vue et le contrôleur dépendent tous deux du modèle.</span><span class="sxs-lookup"><span data-stu-id="42c67-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="42c67-119">Toutefois, le modèle ne dépend ni de la vue ni du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="42c67-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="42c67-120">Il s’agit de l’un des principaux avantages de la séparation.</span><span class="sxs-lookup"><span data-stu-id="42c67-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="42c67-121">Cette séparation permet de générer et de tester le modèle indépendamment de la présentation visuelle.</span><span class="sxs-lookup"><span data-stu-id="42c67-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="42c67-122">Responsabilités du modèle</span><span class="sxs-lookup"><span data-stu-id="42c67-122">Model Responsibilities</span></span>

<span data-ttu-id="42c67-123">Le modèle d’une application MVC représente l’état de l’application, ainsi que la logique métier ou les opérations à effectuer.</span><span class="sxs-lookup"><span data-stu-id="42c67-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="42c67-124">La logique métier doit être encapsulée dans le modèle, ainsi que toute autre logique d’implémentation pour la persistance de l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="42c67-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="42c67-125">En général, les vues fortement typées utilisent des types ViewModel conçus pour contenir les données à afficher sur cette vue.</span><span class="sxs-lookup"><span data-stu-id="42c67-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="42c67-126">Le contrôleur crée et remplit ces instances de ViewModel à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="42c67-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="42c67-127">Il existe de nombreuses façons d’organiser le modèle dans une application qui utilise le modèle architectural MVC.</span><span class="sxs-lookup"><span data-stu-id="42c67-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="42c67-128">Pour en savoir plus, consultez les informations sur les [différents genres de types de modèle](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="42c67-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="42c67-129">Responsabilités de la vue</span><span class="sxs-lookup"><span data-stu-id="42c67-129">View Responsibilities</span></span>

<span data-ttu-id="42c67-130">Les vues sont responsables de la présentation du contenu via l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="42c67-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="42c67-131">Elles utilisent le [moteur de vue Razor](#razor-view-engine) pour incorporer du code .NET dans les balises HTML.</span><span class="sxs-lookup"><span data-stu-id="42c67-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="42c67-132">Il doit exister une logique minimale dans les vues, et cette logique doit être liée à la présentation du contenu.</span><span class="sxs-lookup"><span data-stu-id="42c67-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="42c67-133">Si vous avez besoin d’exécuter une grande partie de la logique dans les fichiers de vue pour afficher les données d’un modèle complexe, utilisez un [composant de vue](views/view-components.md), ViewModel ou un modèle de vue pour simplifier la vue.</span><span class="sxs-lookup"><span data-stu-id="42c67-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="42c67-134">Responsabilités du contrôleur</span><span class="sxs-lookup"><span data-stu-id="42c67-134">Controller Responsibilities</span></span>

<span data-ttu-id="42c67-135">Les contrôleurs sont des composants qui gèrent l’interaction avec l’utilisateur, fonctionnent avec le modèle et, au final, sélectionnent une vue à afficher.</span><span class="sxs-lookup"><span data-stu-id="42c67-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="42c67-136">Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.</span><span class="sxs-lookup"><span data-stu-id="42c67-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="42c67-137">Dans le modèle MVC, le contrôleur est le point d’entrée initial. Il est responsable de la sélection des types de modèle à utiliser et de la vue à afficher (ce qui explique son nom, car il contrôle la manière dont l’application répond à une requête donnée).</span><span class="sxs-lookup"><span data-stu-id="42c67-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="42c67-138">Vous devez éviter les excès de responsabilités pour ne pas rendre les contrôleurs trop complexes.</span><span class="sxs-lookup"><span data-stu-id="42c67-138">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="42c67-139">Pour éviter que la logique du contrôleur ne devienne trop complexe, utilisez le [principe de responsabilité unique](http://deviq.com/single-responsibility-principle/) afin d’envoyer (push) la logique métier hors du contrôleur vers le modèle de domaine.</span><span class="sxs-lookup"><span data-stu-id="42c67-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="42c67-140">Si vous constatez que le contrôleur effectue souvent les mêmes genres d’action, vous pouvez suivre le [principe DRY (Ne vous répétez pas)](http://deviq.com/don-t-repeat-yourself/) en plaçant ces actions usuelles dans des [filtres](#filters).</span><span class="sxs-lookup"><span data-stu-id="42c67-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="42c67-141">Qu’est-ce qu’ASP.NET Core MVC ?</span><span class="sxs-lookup"><span data-stu-id="42c67-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="42c67-142">Le framework ASP.NET Core MVC est un framework de présentation léger, open source et hautement testable, optimisé pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42c67-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="42c67-143">ASP.NET Core MVC permet de créer des sites web dynamiques à l’aide de modèles en délimitant clairement les responsabilités.</span><span class="sxs-lookup"><span data-stu-id="42c67-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="42c67-144">Il vous donne un contrôle total des balises, prend en charge le développement TDD et utilise les dernières normes du web.</span><span class="sxs-lookup"><span data-stu-id="42c67-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="42c67-145">Fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="42c67-145">Features</span></span>

<span data-ttu-id="42c67-146">ASP.NET Core MVC inclut les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="42c67-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="42c67-147">Routage</span><span class="sxs-lookup"><span data-stu-id="42c67-147">Routing</span></span>](#routing)
* [<span data-ttu-id="42c67-148">Liaison de modèles</span><span class="sxs-lookup"><span data-stu-id="42c67-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="42c67-149">Validation du modèle</span><span class="sxs-lookup"><span data-stu-id="42c67-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="42c67-150">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="42c67-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="42c67-151">Filtres</span><span class="sxs-lookup"><span data-stu-id="42c67-151">Filters</span></span>](#filters)
* [<span data-ttu-id="42c67-152">Zones</span><span class="sxs-lookup"><span data-stu-id="42c67-152">Areas</span></span>](#areas)
* [<span data-ttu-id="42c67-153">API web</span><span class="sxs-lookup"><span data-stu-id="42c67-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="42c67-154">Testabilité</span><span class="sxs-lookup"><span data-stu-id="42c67-154">Testability</span></span>](#testability)
* [<span data-ttu-id="42c67-155">Moteur de vue Razor</span><span class="sxs-lookup"><span data-stu-id="42c67-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="42c67-156">Vues fortement typées</span><span class="sxs-lookup"><span data-stu-id="42c67-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="42c67-157">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="42c67-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="42c67-158">Composants de vues</span><span class="sxs-lookup"><span data-stu-id="42c67-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="42c67-159">Routage</span><span class="sxs-lookup"><span data-stu-id="42c67-159">Routing</span></span>

<span data-ttu-id="42c67-160">ASP.NET Core MVC est basé sur le [routage d’ASP.NET Core](../fundamentals/routing.md), un puissant composant de mappage d’URL qui vous permet de générer des applications ayant des URL compréhensibles et pouvant faire l’objet de recherches.</span><span class="sxs-lookup"><span data-stu-id="42c67-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="42c67-161">Cela vous permet de définir des modèles de nommage d’URL pour votre application, qui soient bien adaptés au SEO (optimisation du référencement d’un site auprès d’un moteur de recherche) et à la génération de liens, indépendamment de la façon dont les fichiers sont organisés sur votre serveur web.</span><span class="sxs-lookup"><span data-stu-id="42c67-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="42c67-162">Vous pouvez définir vos routages à l’aide d’une syntaxe de modèle de routage pratique qui prend en charge les contraintes des valeurs de routage, les valeurs par défaut et les valeurs facultatives.</span><span class="sxs-lookup"><span data-stu-id="42c67-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="42c67-163">*Le routage basé sur les conventions* vous permet de définir globalement les formats d’URL acceptés par votre application, ainsi que le mappage de chacun de ces formats à une méthode d’action spécifique sur un contrôleur donné.</span><span class="sxs-lookup"><span data-stu-id="42c67-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="42c67-164">Quand une requête entrante est reçue, le moteur de routage analyse l’URL et la fait correspondre à l’un des formats d’URL définis, puis il appelle la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="42c67-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="42c67-165">Le *routage d’attributs* vous permet de spécifier des informations de routage en décorant vos contrôleurs et vos actions avec des attributs qui définissent les routages de votre application.</span><span class="sxs-lookup"><span data-stu-id="42c67-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="42c67-166">Cela signifie que vos définitions de routage sont placées à côté du contrôleur et de l’action auxquels elles sont associées.</span><span class="sxs-lookup"><span data-stu-id="42c67-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="42c67-167">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="42c67-167">Model binding</span></span>

<span data-ttu-id="42c67-168">La liaison de données [ASP.NET Core MVC](models/model-binding.md) convertit les données de requêtes clientes (valeurs de formulaire, données de routage, paramètres de chaîne de requête, en-têtes HTTP) en objets que le contrôleur peut prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="42c67-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="42c67-169">Ainsi, la logique du contrôleur n’a pas besoin d’identifier les données de requête entrantes ; elle utilise simplement les données en tant que paramètres de ses méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="42c67-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="42c67-170">Validation de modèle</span><span class="sxs-lookup"><span data-stu-id="42c67-170">Model validation</span></span>

<span data-ttu-id="42c67-171">ASP.NET Core MVC prend en charge la [validation](models/validation.md) en décorant votre objet de modèle avec des attributs de validation d’annotation de données.</span><span class="sxs-lookup"><span data-stu-id="42c67-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="42c67-172">Les attributs de validation sont vérifiés côté client avant que les valeurs ne soient postées sur le serveur, ainsi que sur le serveur avant l’appel de l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="42c67-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="42c67-173">Action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="42c67-173">A controller action:</span></span>

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

<span data-ttu-id="42c67-174">Le framework gère la validation des données de requête à la fois sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="42c67-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="42c67-175">La logique de validation spécifiée pour les types de modèle est ajoutée aux vues affichées en tant qu’annotations discrètes, et est appliquée dans le navigateur à l’aide de la [validation jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="42c67-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="42c67-176">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="42c67-176">Dependency injection</span></span>

<span data-ttu-id="42c67-177">ASP.NET Core offre une prise en charge intégrée de l’[injection de dépendances](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="42c67-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="42c67-178">Dans ASP.NET Core MVC, les [contrôleurs](controllers/dependency-injection.md) peuvent demander les services nécessaires via leurs constructeurs, ce qui leur permet de suivre le [principe des dépendances explicites](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="42c67-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="42c67-179">Votre application peut également utiliser l’[injection de dépendances dans les fichiers de vue](views/dependency-injection.md), à l’aide de la directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="42c67-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="42c67-180">Filtres</span><span class="sxs-lookup"><span data-stu-id="42c67-180">Filters</span></span>

<span data-ttu-id="42c67-181">Les [filtres](controllers/filters.md) permettent aux développeurs d’intégrer des problèmes transversaux, par exemple la prise en charge des exceptions ou les autorisations.</span><span class="sxs-lookup"><span data-stu-id="42c67-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="42c67-182">Les filtres permettent d’exécuter une logique de prétraitement et de posttraitement personnalisée pour les méthodes d’action. Vous pouvez les configurer pour qu’ils se lancent à certaines étapes du pipeline d’exécution d’une requête donnée.</span><span class="sxs-lookup"><span data-stu-id="42c67-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="42c67-183">Vous pouvez appliquer les filtres aux contrôleurs ou aux actions en tant qu’attributs (ou vous pouvez les exécuter de manière globale).</span><span class="sxs-lookup"><span data-stu-id="42c67-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="42c67-184">Plusieurs filtres (tels que `Authorize`) sont inclus dans le framework.</span><span class="sxs-lookup"><span data-stu-id="42c67-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="42c67-185">Zones</span><span class="sxs-lookup"><span data-stu-id="42c67-185">Areas</span></span>

<span data-ttu-id="42c67-186">Les [zones](controllers/areas.md) permettent de partitionner une grande application web ASP.NET Core MVC en regroupements opérationnels plus petits.</span><span class="sxs-lookup"><span data-stu-id="42c67-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="42c67-187">Une zone est une structure MVC à l’intérieur d’une application.</span><span class="sxs-lookup"><span data-stu-id="42c67-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="42c67-188">Dans un projet MVC, les composants logiques, tels que Modèle, Contrôleur et Vue, sont conservés dans des dossiers distincts. MVC utilise des conventions de nommage pour créer la relation entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="42c67-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="42c67-189">Pour une grande application, il peut être avantageux de partitionner celle-ci en différentes zones de fonctionnalités générales.</span><span class="sxs-lookup"><span data-stu-id="42c67-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="42c67-190">Prenons le cas d’une application d’e-commerce avec plusieurs divisions, par exemple le paiement, la facturation, la recherche, etc. Chacune de ces divisions a ses propres vues, contrôleurs et modèles de composants logiques.</span><span class="sxs-lookup"><span data-stu-id="42c67-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="42c67-191">API web</span><span class="sxs-lookup"><span data-stu-id="42c67-191">Web APIs</span></span>

<span data-ttu-id="42c67-192">En plus d’être une excellente plateforme pour la création de sites web, ASP.NET Core MVC prend en charge la génération d’API web.</span><span class="sxs-lookup"><span data-stu-id="42c67-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="42c67-193">Vous pouvez créer des services accessibles à un large éventail de clients, notamment les navigateurs et les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="42c67-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="42c67-194">Le framework inclut la prise en charge de la négociation de contenu HTTP, ainsi que la prise en charge intégrée des [données de mise en forme](models/formatting.md) au format JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="42c67-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="42c67-195">Écrivez des [formateurs personnalisés](advanced/custom-formatters.md) pour ajouter la prise en charge de vos propres formats.</span><span class="sxs-lookup"><span data-stu-id="42c67-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="42c67-196">Utilisez la génération de liens pour activer la prise en charge hypermédia.</span><span class="sxs-lookup"><span data-stu-id="42c67-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="42c67-197">Activez facilement la prise en charge de [CORS (Cross-Origin Resource Sharing)](http://www.w3.org/TR/cors/) pour que vos API web puissent être partagées entre plusieurs applications web.</span><span class="sxs-lookup"><span data-stu-id="42c67-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="42c67-198">Testabilité</span><span class="sxs-lookup"><span data-stu-id="42c67-198">Testability</span></span>

<span data-ttu-id="42c67-199">Le framework utilise les interfaces et l’injection de dépendances, ce qui le rend particulièrement adapté aux tests unitaires. De plus, le framework inclut des fonctionnalités (par exemple un fournisseur TestHost et InMemory pour Entity Framework) qui facilitent aussi les [tests d’intégration](../testing/integration-testing.md).</span><span class="sxs-lookup"><span data-stu-id="42c67-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="42c67-200">Pour en savoir plus, consultez les informations sur le [test de la logique du contrôleur](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="42c67-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="42c67-201">Moteur de vue Razor</span><span class="sxs-lookup"><span data-stu-id="42c67-201">Razor view engine</span></span>

<span data-ttu-id="42c67-202">Les [vues ASP.NET Core MVC](views/overview.md) utilisent le [moteur de vue Razor](views/razor.md) pour afficher les vues.</span><span class="sxs-lookup"><span data-stu-id="42c67-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="42c67-203">Razor est un langage de balisage de modèles compact, expressif et fluide qui permet de définir des vues avec du code C# incorporé.</span><span class="sxs-lookup"><span data-stu-id="42c67-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="42c67-204">Razor est utilisé pour générer dynamiquement du contenu web sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="42c67-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="42c67-205">Vous pouvez mélanger sans problème du code serveur avec du contenu et du code côté client.</span><span class="sxs-lookup"><span data-stu-id="42c67-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="42c67-206">À l’aide du moteur de vue Razor, vous pouvez définir des [dispositions](views/layout.md), des [vues partielles](views/partial.md) et des sections remplaçables.</span><span class="sxs-lookup"><span data-stu-id="42c67-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="42c67-207">Vues fortement typées</span><span class="sxs-lookup"><span data-stu-id="42c67-207">Strongly typed views</span></span>

<span data-ttu-id="42c67-208">Les vues Razor dans MVC peuvent être fortement typées en fonction de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="42c67-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="42c67-209">Les contrôleurs peuvent passer un modèle fortement typé à des vues, ce qui leur permet de disposer du contrôle de type et de la prise en charge d’IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="42c67-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="42c67-210">Par exemple, la vue suivante affiche un modèle de type `IEnumerable<Product>` :</span><span class="sxs-lookup"><span data-stu-id="42c67-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="42c67-211">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="42c67-211">Tag Helpers</span></span>

<span data-ttu-id="42c67-212">Les [Tag Helpers](views/tag-helpers/intro.md) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.</span><span class="sxs-lookup"><span data-stu-id="42c67-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="42c67-213">Vous pouvez utiliser des Tag Helpers pour définir des balises personnalisées (par exemple `<environment>`), ou pour modifier le comportement de balises existantes (par exemple `<label>`).</span><span class="sxs-lookup"><span data-stu-id="42c67-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="42c67-214">Les Tag Helpers se lient à des éléments spécifiques en fonction du nom de l’élément et de ses attributs.</span><span class="sxs-lookup"><span data-stu-id="42c67-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="42c67-215">Ils offrent les avantages du rendu côté serveur tout en conservant une expérience utilisateur d’édition HTML.</span><span class="sxs-lookup"><span data-stu-id="42c67-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="42c67-216">Il existe de nombreux Tag Helpers pour les tâches courantes (par exemple la création de formulaires ou de liens, le chargement de ressources, etc.) et bien d’autres encore, dans les dépôts publics GitHub et sous forme de packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="42c67-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="42c67-217">Les Tag Helpers sont créés en C# et ciblent les éléments HTML en fonction du nom de l’élément, du nom de l’attribut ou de la balise parente.</span><span class="sxs-lookup"><span data-stu-id="42c67-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="42c67-218">Par exemple, le Tag Helper Link intégré permet de créer un lien vers l’action `Login` de `AccountsController` :</span><span class="sxs-lookup"><span data-stu-id="42c67-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="42c67-219">Le `EnvironmentTagHelper` permet d’inclure différents scripts dans vos vues (par exemple bruts ou minimisés) en fonction de l’environnement d’exécution, Développement, Préproduction ou Production :</span><span class="sxs-lookup"><span data-stu-id="42c67-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="42c67-220">Les Tag Helpers fournissent une expérience utilisateur de développement HTML et un riche environnement IntelliSense pour la création de balises HTML et Razor.</span><span class="sxs-lookup"><span data-stu-id="42c67-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="42c67-221">La plupart des Tag Helpers intégrés ciblent les éléments HTML existants et fournissent des attributs côté serveur pour l’élément.</span><span class="sxs-lookup"><span data-stu-id="42c67-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="42c67-222">Composants de vues</span><span class="sxs-lookup"><span data-stu-id="42c67-222">View Components</span></span>

<span data-ttu-id="42c67-223">Les [composants de vues](views/view-components.md) vous permettent de compresser la logique de rendu et de la réutiliser dans l’application.</span><span class="sxs-lookup"><span data-stu-id="42c67-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="42c67-224">Ils sont similaires aux [vues partielles](views/partial.md), mais avec une logique associée.</span><span class="sxs-lookup"><span data-stu-id="42c67-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
