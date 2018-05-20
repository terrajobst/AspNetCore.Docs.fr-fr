---
title: Composants de vue dans ASP.NET Core
author: rick-anderson
description: Découvrez comment les composants de vue sont utilisés dans ASP.NET Core et comment les ajouter à des applications.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: cdf44fc15ac64497b2589e8b7b289beb94c5b2c4
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="a72dd-103">Composants de vue dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a72dd-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="a72dd-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a72dd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a72dd-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a72dd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="a72dd-106">Composants de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-106">View components</span></span>

<span data-ttu-id="a72dd-107">Les composants de vue sont similaires aux vues partielles, mais ils sont beaucoup plus puissants.</span><span class="sxs-lookup"><span data-stu-id="a72dd-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="a72dd-108">Les composants de vue n’utilisent pas la liaison de données ; ils dépendent uniquement des données fournies en réponse à l’appel d’un composant de vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="a72dd-109">Cet article a été écrit avec ASP.NET Core MVC, mais les composants de vue fonctionnent également avec les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="a72dd-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="a72dd-110">Un composant de vue a les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="a72dd-110">A view component:</span></span>

* <span data-ttu-id="a72dd-111">Il effectue le rendu d’un bloc de code au lieu d’une réponse entière.</span><span class="sxs-lookup"><span data-stu-id="a72dd-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="a72dd-112">Il garantit la même « séparation des préoccupations » et offre les mêmes avantages de testabilité qu’entre un contrôleur et une vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="a72dd-113">Il peut avoir des paramètres et une logique métier.</span><span class="sxs-lookup"><span data-stu-id="a72dd-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="a72dd-114">Il est généralement appelé à partir d’une page de disposition.</span><span class="sxs-lookup"><span data-stu-id="a72dd-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="a72dd-115">Les composants de vue sont conçus pour être utilisés là où vous avez une logique de rendu réutilisable qui est trop complexe pour une vue partielle. Ils ciblent les vues suivantes, par exemple :</span><span class="sxs-lookup"><span data-stu-id="a72dd-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="a72dd-116">Menus de navigation dynamiques</span><span class="sxs-lookup"><span data-stu-id="a72dd-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="a72dd-117">Nuage de mots clés (pour l’interrogation de la base de données)</span><span class="sxs-lookup"><span data-stu-id="a72dd-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="a72dd-118">Panneau de connexion</span><span class="sxs-lookup"><span data-stu-id="a72dd-118">Login panel</span></span>
* <span data-ttu-id="a72dd-119">Panier d’achat</span><span class="sxs-lookup"><span data-stu-id="a72dd-119">Shopping cart</span></span>
* <span data-ttu-id="a72dd-120">Articles récemment publiés</span><span class="sxs-lookup"><span data-stu-id="a72dd-120">Recently published articles</span></span>
* <span data-ttu-id="a72dd-121">Contenu de la barre latérale dans un blog standard</span><span class="sxs-lookup"><span data-stu-id="a72dd-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="a72dd-122">Panneau de connexion inclus dans chaque page pour afficher les liens de connexion ou déconnexion, en fonction de l’état de connexion de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="a72dd-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="a72dd-123">Un composant de vue a deux éléments : sa classe (généralement dérivée de [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) et le résultat qu’il retourne (en général, une vue).</span><span class="sxs-lookup"><span data-stu-id="a72dd-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="a72dd-124">Comme les contrôleurs, un composant de vue peut être un OCT, mais la plupart des développeurs préfèrent utiliser les méthodes et propriétés dérivées de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="a72dd-125">Création d’un composant de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-125">Creating a view component</span></span>

<span data-ttu-id="a72dd-126">Cette section présente les exigences générales relatives à la création d’un composant de vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="a72dd-127">Plus loin dans cet article, nous décrirons chaque étape en détail et nous créerons un composant de vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="a72dd-128">Classe de composant de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-128">The view component class</span></span>

<span data-ttu-id="a72dd-129">Vous pouvez créer une classe de composant de vue à l’aide d’une des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a72dd-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="a72dd-130">En la dérivant de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="a72dd-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="a72dd-131">En décorant une classe avec l’attribut `[ViewComponent]` ou en dérivant la classe d’une classe définie avec l’attribut `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="a72dd-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="a72dd-132">En créant une classe dont le nom se termine par le suffixe *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="a72dd-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="a72dd-133">Comme les contrôleurs, les composants de vue doivent être des classes publiques, non imbriquées et non abstraites.</span><span class="sxs-lookup"><span data-stu-id="a72dd-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="a72dd-134">Le nom du composant de vue correspond au nom de la classe sans le suffixe ViewComponent.</span><span class="sxs-lookup"><span data-stu-id="a72dd-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="a72dd-135">Il peut également être spécifié explicitement à l’aide de la propriété `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="a72dd-136">Une classe de composant de vue :</span><span class="sxs-lookup"><span data-stu-id="a72dd-136">A view component class:</span></span>

* <span data-ttu-id="a72dd-137">Prend entièrement en charge [l’injection de dépendances](../../fundamentals/dependency-injection.md) dans le constructeur</span><span class="sxs-lookup"><span data-stu-id="a72dd-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="a72dd-138">N’intervient pas dans le cycle de vie du contrôleur, ce qui signifie que vous ne pouvez pas utiliser de [filtres](../controllers/filters.md) dans un composant de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="a72dd-139">Méthodes d’un composant de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-139">View component methods</span></span>

<span data-ttu-id="a72dd-140">Un composant de vue définit sa logique dans une méthode `InvokeAsync` qui retourne un `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="a72dd-141">Les paramètres sont fournis directement en réponse à l’appel du composant de vue ; ils ne proviennent pas de la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="a72dd-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="a72dd-142">Un composant de vue ne traite jamais une requête directement.</span><span class="sxs-lookup"><span data-stu-id="a72dd-142">A view component never directly handles a request.</span></span> <span data-ttu-id="a72dd-143">En règle générale, un composant de vue initialise un modèle et le passe à une vue en appelant la méthode `View`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="a72dd-144">En résumé, les méthodes d’un composant de vue :</span><span class="sxs-lookup"><span data-stu-id="a72dd-144">In summary, view component methods:</span></span>

* <span data-ttu-id="a72dd-145">Définissent une méthode `InvokeAsync` qui retourne un `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="a72dd-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="a72dd-146">Permettent d’initialiser un modèle et de le passer à une vue en appelant la méthode `View` `ViewComponent`</span><span class="sxs-lookup"><span data-stu-id="a72dd-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="a72dd-147">Utilisent des paramètres fournis par la méthode appelante, pas par HTTP (il n’y a pas de liaison de données)</span><span class="sxs-lookup"><span data-stu-id="a72dd-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="a72dd-148">Ne sont pas accessibles directement comme point de terminaison HTTP et sont appelées de votre code (généralement dans une vue).</span><span class="sxs-lookup"><span data-stu-id="a72dd-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="a72dd-149">Un composant de vue ne traite jamais une requête directement</span><span class="sxs-lookup"><span data-stu-id="a72dd-149">A view component never handles a request</span></span>
* <span data-ttu-id="a72dd-150">Sont surchargées sur la signature, plutôt que des détails de la requête HTTP en cours</span><span class="sxs-lookup"><span data-stu-id="a72dd-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="a72dd-151">Chemin de recherche de la vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-151">View search path</span></span>

<span data-ttu-id="a72dd-152">Le Runtime recherche la vue dans les chemins suivants :</span><span class="sxs-lookup"><span data-stu-id="a72dd-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="a72dd-153">Views/\<nom_contrôleur>/Components/\<nom_composant_vue>/\<nom_vue></span><span class="sxs-lookup"><span data-stu-id="a72dd-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="a72dd-154">Views/Shared/Components/\<nom_composant_vue>/\<nom_vue></span><span class="sxs-lookup"><span data-stu-id="a72dd-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="a72dd-155">Le nom de la vue par défaut pour un composant de vue est *Default*. Votre fichier de vue est donc normalement appelé *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="a72dd-156">Vous pouvez spécifier un nom de vue différent quand vous créez le résultat du composant de vue ou quand vous appelez la méthode `View`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="a72dd-157">Nous vous recommandons de nommer le fichier de vue *Default.cshtml* et d’utiliser le chemin *Views/Shared/Components/\<nom_composant_vue>/\<nom_vue>*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="a72dd-158">Le composant de vue `PriorityList` montré dans cet exemple utilise le chemin *Views/Shared/Components/PriorityList/Default.cshtml* pour la vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="a72dd-159">Appel d’un composant de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-159">Invoking a view component</span></span>

<span data-ttu-id="a72dd-160">Pour utiliser le composant de vue, appelez-le dans une vue, comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="a72dd-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="a72dd-161">Les paramètres sont passés à la méthode `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="a72dd-162">Le composant de vue `PriorityList` décrit dans cet article est appelé du fichier de vue *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="a72dd-163">Dans le code suivant, la méthode `InvokeAsync` est appelée avec deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="a72dd-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="a72dd-164">Appel d’un composant de vue en tant que Tag Helper</span><span class="sxs-lookup"><span data-stu-id="a72dd-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="a72dd-165">Dans ASP.NET Core 1.1 et les versions ultérieures, vous pouvez appeler un composant de vue en tant que [Tag Helper](xref:mvc/views/tag-helpers/intro) :</span><span class="sxs-lookup"><span data-stu-id="a72dd-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="a72dd-166">Les paramètres de méthode et de classe de casse Pascal pour les Tag Helpers sont convertis en [casse kebab en minuscules](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101) (mots séparés par des tirets).</span><span class="sxs-lookup"><span data-stu-id="a72dd-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="a72dd-167">Le Tag Helper utilisé pour appeler un composant de vue contient l’élément `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="a72dd-168">Le composant de vue est spécifié de la façon suivante :</span><span class="sxs-lookup"><span data-stu-id="a72dd-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="a72dd-169">Remarque : Pour pouvoir utiliser un composant de vue en tant que Tag Helper, vous devez inscrire l’assembly contenant le composant de vue à l’aide de la directive `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="a72dd-170">Par exemple, si votre composant de vue se trouve dans un assembly appelé MyWebApp, ajoutez la directive suivante dans le fichier `_ViewImports.cshtml` :</span><span class="sxs-lookup"><span data-stu-id="a72dd-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="a72dd-171">Vous pouvez inscrire un composant de vue en tant que Tag Helper dans n’importe quel fichier qui fait référence à ce composant.</span><span class="sxs-lookup"><span data-stu-id="a72dd-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="a72dd-172">Pour plus d’informations sur l’inscription des Tag Helpers, consultez [Gestion de l’étendue des Tag Helpers](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope).</span><span class="sxs-lookup"><span data-stu-id="a72dd-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="a72dd-173">Méthode `InvokeAsync` utilisée dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="a72dd-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="a72dd-174">Dans le balisage Tag Helper :</span><span class="sxs-lookup"><span data-stu-id="a72dd-174">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="a72dd-175">Dans l’exemple ci-dessus, le composant de vue `PriorityList` devient `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="a72dd-176">Les paramètres sont passés au composant de vue sous forme d’attributs dans la casse kebab en minuscules.</span><span class="sxs-lookup"><span data-stu-id="a72dd-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="a72dd-177">Appel d’un composant de vue directement à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="a72dd-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="a72dd-178">Les composants de vue sont généralement appelés depuis une vue, mais ils peuvent aussi être appelés directement d’une méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a72dd-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="a72dd-179">Les composants de vue ne définissent pas de points de terminaison comme le font les contrôleurs, mais vous pouvez facilement implémenter une action de contrôleur qui retourne le contenu d’un `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-179">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="a72dd-180">Dans cet exemple, le composant de vue est appelé directement du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="a72dd-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="a72dd-181">Procédure pas à pas : Création d’un composant de vue simple</span><span class="sxs-lookup"><span data-stu-id="a72dd-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="a72dd-182">[Téléchargez](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), générez et testez le code de démarrage.</span><span class="sxs-lookup"><span data-stu-id="a72dd-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="a72dd-183">Il s’agit d’un projet simple avec un contrôleur `Todo` qui affiche une liste de tâches *Todo*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Liste des tâches Todo](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="a72dd-185">Ajouter une classe ViewComponent</span><span class="sxs-lookup"><span data-stu-id="a72dd-185">Add a ViewComponent class</span></span>

<span data-ttu-id="a72dd-186">Créez un dossier *ViewComponents* et ajoutez la classe `PriorityListViewComponent` suivante :</span><span class="sxs-lookup"><span data-stu-id="a72dd-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="a72dd-187">Remarques sur le code :</span><span class="sxs-lookup"><span data-stu-id="a72dd-187">Notes on the code:</span></span>

* <span data-ttu-id="a72dd-188">Les classes ViewComponent peuvent être ajoutées à **n’importe** quel dossier dans le projet.</span><span class="sxs-lookup"><span data-stu-id="a72dd-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="a72dd-189">Étant donné que le nom de classe PriorityList**ViewComponent** se termine par le suffixe **ViewComponent**, le Runtime utilise la chaîne « PriorityList » pour référencer le composant de la classe à partir d’une vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="a72dd-190">J’expliquerai ce point plus en détail plus tard.</span><span class="sxs-lookup"><span data-stu-id="a72dd-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="a72dd-191">L’attribut `[ViewComponent]` peut changer le nom utilisé pour faire référence à un composant de vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="a72dd-192">Par exemple, nous pourrions avoir nommé la classe `XYZ` et appliqué l’attribut `ViewComponent` :</span><span class="sxs-lookup"><span data-stu-id="a72dd-192">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="a72dd-193">L’attribut `[ViewComponent]` ci-dessus indique au sélecteur du composant de vue d’utiliser le nom `PriorityList` pour rechercher les vues associées au composant et d’utiliser la chaîne « PriorityList » pour faire référence au composant de la classe à partir d’une vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="a72dd-194">J’expliquerai ce point plus en détail plus tard.</span><span class="sxs-lookup"><span data-stu-id="a72dd-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="a72dd-195">Le composant utilise [l’injection de dépendances](../../fundamentals/dependency-injection.md) pour rendre le contexte de données disponible.</span><span class="sxs-lookup"><span data-stu-id="a72dd-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="a72dd-196">`InvokeAsync` expose une méthode qui peut être appelée à partir d’une vue et qui peut prendre un nombre arbitraire d’arguments.</span><span class="sxs-lookup"><span data-stu-id="a72dd-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="a72dd-197">La méthode `InvokeAsync` retourne l’ensemble des tâches `ToDo` qui correspondent aux paramètres `isDone` et `maxPriority` spécifiés.</span><span class="sxs-lookup"><span data-stu-id="a72dd-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="a72dd-198">Créer la vue Razor du composant de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-198">Create the view component Razor view</span></span>

* <span data-ttu-id="a72dd-199">Créez le dossier *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="a72dd-200">Ce dossier **doit** être nommé *Components*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="a72dd-201">Créez le dossier *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="a72dd-202">Ce nom de dossier doit correspondre au nom de la classe du composant de vue, ou au nom de la classe sans le suffixe (dans le cas où vous avez suivi la convention et utilisé le suffixe *ViewComponent* dans le nom de la classe).</span><span class="sxs-lookup"><span data-stu-id="a72dd-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="a72dd-203">Si vous avez utilisé l’attribut `ViewComponent`, le nom de la classe doit correspondre à la désignation de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="a72dd-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="a72dd-204">Créez une vue Razor *Views/Shared/Components/PriorityList/Default.cshtml* : [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="a72dd-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="a72dd-205">La vue Razor affiche une liste des tâches `TodoItem` retournées.</span><span class="sxs-lookup"><span data-stu-id="a72dd-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="a72dd-206">Si la méthode `InvokeAsync` du composant de vue ne passe pas le nom de la vue (comme dans notre exemple), le nom de vue *Default* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="a72dd-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="a72dd-207">Je vous montrerai comment passer le nom de la vue plus tard dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="a72dd-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="a72dd-208">Pour substituer le style par défaut d’un contrôleur spécifique, ajoutez une vue dans le dossier des vues du contrôleur (par exemple, *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="a72dd-209">Si le composant de vue est spécifique au contrôleur, vous pouvez l’ajouter au dossier du contrôleur (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="a72dd-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="a72dd-210">Ajoutez une balise `div` contenant un appel au composant de liste des priorités à la fin du fichier *Views/Todo/index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a72dd-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="a72dd-211">Le balisage `@await Component.InvokeAsync` montre la syntaxe utilisée pour appeler un composant de vue.</span><span class="sxs-lookup"><span data-stu-id="a72dd-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="a72dd-212">Le premier argument est le nom du composant à appeler.</span><span class="sxs-lookup"><span data-stu-id="a72dd-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="a72dd-213">Les paramètres suivants sont passés au composant.</span><span class="sxs-lookup"><span data-stu-id="a72dd-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="a72dd-214">`InvokeAsync` peut prendre un nombre arbitraire d’arguments.</span><span class="sxs-lookup"><span data-stu-id="a72dd-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="a72dd-215">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="a72dd-215">Test the app.</span></span> <span data-ttu-id="a72dd-216">L’image suivante montre la liste des tâches ToDo et les tâches prioritaires :</span><span class="sxs-lookup"><span data-stu-id="a72dd-216">The following image shows the ToDo list and the priority items:</span></span>

![liste des tâches ToDo et tâches prioritaires](view-components/_static/pi.png)

<span data-ttu-id="a72dd-218">Vous pouvez également appeler le composant de vue directement à partir du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="a72dd-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![tâches prioritaires retournées par IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="a72dd-220">Spécification d’un nom de vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-220">Specifying a view name</span></span>

<span data-ttu-id="a72dd-221">Un composant de vue complexe peut avoir besoin de spécifier une autre vue que la vue par défaut dans certaines conditions.</span><span class="sxs-lookup"><span data-stu-id="a72dd-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="a72dd-222">Le code suivant montre comment spécifier la vue « PVC » à l’aide de la méthode `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="a72dd-223">Mettez à jour la méthode `InvokeAsync` dans la classe `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="a72dd-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="a72dd-224">Copiez le fichier *Views/Shared/Components/PriorityList/Default.cshtml* dans une vue nommée *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="a72dd-225">Ajoutez un titre pour indiquer que la vue PVC est utilisée.</span><span class="sxs-lookup"><span data-stu-id="a72dd-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="a72dd-226">Mettez à jour *Views/TodoList/Index.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a72dd-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="a72dd-227">Exécutez l’application et vérifiez la vue PVC.</span><span class="sxs-lookup"><span data-stu-id="a72dd-227">Run the app and verify PVC view.</span></span>

![Composant de vue des priorités](view-components/_static/pvc.png)

<span data-ttu-id="a72dd-229">Si la vue PVC ne s’affiche pas, vérifiez que vous appelez le composant de vue avec une priorité supérieure ou égale à 4.</span><span class="sxs-lookup"><span data-stu-id="a72dd-229">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="a72dd-230">Vérifier le chemin de la vue</span><span class="sxs-lookup"><span data-stu-id="a72dd-230">Examine the view path</span></span>

* <span data-ttu-id="a72dd-231">Changez le paramètre de priorité à une priorité inférieure ou égale à 3 pour empêcher l’affichage de la vue des priorités.</span><span class="sxs-lookup"><span data-stu-id="a72dd-231">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="a72dd-232">Renommez temporairement *Views/Todo/Components/PriorityList/Default.cshtml* en *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="a72dd-233">Testez l’application. Vous obtenez l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="a72dd-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="a72dd-234">Copiez *Views/Todo/Components/PriorityList/1Default.cshtml* dans *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="a72dd-235">Ajoutez du balisage à la vue Todo *Shared* du composant de vue pour indiquer que la vue provient du dossier *Shared*.</span><span class="sxs-lookup"><span data-stu-id="a72dd-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="a72dd-236">Testez la vue de composant **Shared**.</span><span class="sxs-lookup"><span data-stu-id="a72dd-236">Test the **Shared** component view.</span></span>

![Affichage de tâches ToDo avec la vue de composant Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="a72dd-238">Éviter les chaînes magiques</span><span class="sxs-lookup"><span data-stu-id="a72dd-238">Avoiding magic strings</span></span>

<span data-ttu-id="a72dd-239">Pour éviter d’éventuels problèmes au moment de la compilation, vous pouvez remplacer le nom du composant de vue codé en dur par le nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="a72dd-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="a72dd-240">Créez le composant de vue sans le suffixe « ViewComponent » :</span><span class="sxs-lookup"><span data-stu-id="a72dd-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="a72dd-241">Ajoutez une instruction `using` dans votre fichier de vue Razor et utilisez l’opérateur `nameof` :</span><span class="sxs-lookup"><span data-stu-id="a72dd-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="a72dd-242">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a72dd-242">Additional resources</span></span>

* [<span data-ttu-id="a72dd-243">Injection de dépendances dans les vues</span><span class="sxs-lookup"><span data-stu-id="a72dd-243">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
