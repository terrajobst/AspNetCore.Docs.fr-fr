---
title: Vues dans ASP.NET Core MVC
author: ardalis
description: Découvrez comment les vues gèrent la présentation des données et les interactions utilisateur dans les applications ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 12/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/overview
ms.openlocfilehash: b9af2068aec4326585eb2a8994399a16461db3be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2018
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="aed19-103">Vues dans ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="aed19-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="aed19-104">Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="aed19-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="aed19-105">Ce document explique l’utilisation des vues dans les applications ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="aed19-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="aed19-106">Pour plus d’informations sur les pages Razor, consultez [Présentation des pages Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="aed19-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="aed19-107">Selon le schéma MVC (**M**odèle-**V**ue-**C**ontrôleur, la *vue* gère la présentation des données et les interactions utilisateur dans l’application.</span><span class="sxs-lookup"><span data-stu-id="aed19-107">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="aed19-108">Une vue est un modèle HTML dans lequel est incorporé un [balisage Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="aed19-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="aed19-109">Le balisage Razor est du code qui interagit avec le balisage HTML pour générer une page web qui est envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="aed19-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="aed19-110">Dans ASP.NET Core MVC, les vues sont des fichiers *.cshtml* qui utilisent le [langage de programmation C#](/dotnet/csharp/) dans le balisage Razor.</span><span class="sxs-lookup"><span data-stu-id="aed19-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="aed19-111">En règle générale, les fichiers de vue sont regroupés dans des dossiers nommés correspondant aux différents [contrôleurs](xref:mvc/controllers/actions) de l’application.</span><span class="sxs-lookup"><span data-stu-id="aed19-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="aed19-112">Ces dossiers sont eux-mêmes regroupés sous le dossier *Vues* situé à la racine de l’application :</span><span class="sxs-lookup"><span data-stu-id="aed19-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Dossier Vues développé dans l’Explorateur de solutions de Visual Studio, avec le dossier Accueil ouvert contenant les fichiers About.cshtml, Contact.cshtml et Index.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="aed19-114">Le contrôleur *Home* est représenté par un dossier *Home* à l’intérieur du dossier *vues*.</span><span class="sxs-lookup"><span data-stu-id="aed19-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="aed19-115">Le dossier *Home* contient les vues pour les pages web *About*, *Contact*, et *Index* (page d’accueil).</span><span class="sxs-lookup"><span data-stu-id="aed19-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="aed19-116">Quand un utilisateur demande une de ces trois pages Web, les actions de contrôleur dans le contrôleur *Home* sont utilisées afin de déterminer laquelle des trois vues est utilisée pour générer et retourner une page Web à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aed19-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="aed19-117">Utilisez [dispositions](xref:mvc/views/layout) pour fournir des sections cohérentes de page Web et de réduire la redondance du code.</span><span class="sxs-lookup"><span data-stu-id="aed19-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="aed19-118">Les dispositions contiennent souvent l’en-tête, des éléments de menu et de navigation, et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="aed19-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="aed19-119">L’en-tête et le pied de page contiennent généralement des balises standard pour de nombreux éléments de métadonnées et des liens vers des ressources de script et de style.</span><span class="sxs-lookup"><span data-stu-id="aed19-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="aed19-120">Les dispositions vous aident à éviter ce balisage réutilisable dans vos vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="aed19-121">Les [vues partielles](xref:mvc/views/partial) réduisent la répétition de code grâce à la gestion des parties réutilisables dans les vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="aed19-122">Par exemple, une vue partielle est utile dans le cas d’une biographie d’auteur publiée sur un site web de blog qui doit s’afficher dans plusieurs vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="aed19-123">Une biographie d’auteur présente un contenu de vue standard et ne nécessite pas d’exécution de code particulier pour générer le contenu à afficher sur la page web.</span><span class="sxs-lookup"><span data-stu-id="aed19-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="aed19-124">Le contenu de la biographie d’auteur est fourni à la vue uniquement par la liaison de données. C’est pourquoi l’utilisation d’une vue partielle pour ce type de contenu est la solution la plus appropriée.</span><span class="sxs-lookup"><span data-stu-id="aed19-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="aed19-125">Les [composants de vue](xref:mvc/views/view-components) sont similaires aux vues partielles dans le sens où ils vous permettent aussi de réduire la répétition de code. La différence est qu’ils sont plutôt conçus pour du contenu de vue qui nécessite l’exécution de code sur le serveur pour afficher la page web.</span><span class="sxs-lookup"><span data-stu-id="aed19-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="aed19-126">Les composants de vue sont utiles quand le contenu affiché doit interagir avec une base de données, comme c’est le cas pour un panier d’achat sur un site web.</span><span class="sxs-lookup"><span data-stu-id="aed19-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="aed19-127">Les composants de vue ne dépendent pas d’une liaison de données pour pouvoir générer la sortie d’une page web.</span><span class="sxs-lookup"><span data-stu-id="aed19-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="aed19-128">Avantages de l’utilisation des vues</span><span class="sxs-lookup"><span data-stu-id="aed19-128">Benefits of using views</span></span>

<span data-ttu-id="aed19-129">Les vues permettent d’établir une [ **S**eparation **o**f **C**oncerns (SoC) conception](http://deviq.com/separation-of-concerns/) au sein d’une application MVC en séparant le balisage de l'interface utilisateur des autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="aed19-129">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="aed19-130">Suivre les principes de la conception SoC rend votre application modulaire, ce qui offre plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="aed19-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="aed19-131">L’application est plus facile à gérer, car elle est mieux organisée.</span><span class="sxs-lookup"><span data-stu-id="aed19-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="aed19-132">Les vues sont généralement regroupées en fonction de l’application.</span><span class="sxs-lookup"><span data-stu-id="aed19-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="aed19-133">Cela rend plus facile à trouver les vues associées lorsque vous travaillez sur une fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="aed19-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="aed19-134">Les parties de l’application sont faiblement couplées.</span><span class="sxs-lookup"><span data-stu-id="aed19-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="aed19-135">Vous pouvez créer et mettre à jour des vues de l’application séparément dans les composants de logique métier et d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="aed19-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="aed19-136">Vous pouvez modifier les vues de l’application sans nécessairement devoir mettre à jour des autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="aed19-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="aed19-137">Il est plus facile de tester les parties de l’interface utilisateur de l’application, car les vues constituent des unités individuelles séparées.</span><span class="sxs-lookup"><span data-stu-id="aed19-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="aed19-138">En raison d’une meilleure organisation, le risque est moins grand de répéter accidentellement des sections de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aed19-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="aed19-139">Création d’une vue</span><span class="sxs-lookup"><span data-stu-id="aed19-139">Creating a view</span></span>

<span data-ttu-id="aed19-140">Les vues qui sont spécifiques à un contrôleur sont créées dans le dossier *Views/[nom du contrôleur]*.</span><span class="sxs-lookup"><span data-stu-id="aed19-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="aed19-141">Les vues qui sont partagées entre les contrôleurs sont placées dans le dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="aed19-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="aed19-142">Pour créer une vue, ajoutez un nouveau fichier et donnez-lui le même nom que son action de contrôleur associée avec l’extension de fichier *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aed19-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="aed19-143">Pour créer une vue qui correspond à l'action *About* dans le contrôleur *Home*, créez un fichier *About.cshtml* dans le dossier *Views/Home* :</span><span class="sxs-lookup"><span data-stu-id="aed19-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="aed19-144">La syntaxe *Razor* commence par le symbole `@`.</span><span class="sxs-lookup"><span data-stu-id="aed19-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="aed19-145">Exécutez des instructions c# en plaçant du code c# dans des [blocs de code Razor](xref:mvc/views/razor#razor-code-blocks) délimités par des accolades (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="aed19-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="aed19-146">Par exemple, consultez l’affectation de « About » pour `ViewData["Title"]` ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="aed19-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="aed19-147">Vous pouvez afficher des valeurs dans le code HTML en référençant simplement la valeur avec le symbole `@`.</span><span class="sxs-lookup"><span data-stu-id="aed19-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="aed19-148">Consultez le contenu des éléments `<h2>` et `<h3>` ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="aed19-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="aed19-149">Le contenu de la vue ci-dessus n'est qu’une partie de la page Web entière qui est restituée à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aed19-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="aed19-150">Le reste de la mise en page et d’autres aspects courants de la vue sont spécifiés dans d’autres fichiers de vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="aed19-151">Pour plus d’informations, consultez la [rubrique de présentation](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="aed19-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="aed19-152">Comment les contrôleurs spécifient les vues</span><span class="sxs-lookup"><span data-stu-id="aed19-152">How controllers specify views</span></span>

<span data-ttu-id="aed19-153">Les vues sont généralement retournées par des actions sous forme d’objet [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), qui est un type d’objet [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="aed19-153">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="aed19-154">Votre méthode d’action peut créer et retourner un `ViewResult` directement, mais cela n’est pas le plus fréquent.</span><span class="sxs-lookup"><span data-stu-id="aed19-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="aed19-155">Étant donné que la plupart des contrôleurs héritent de [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), utilisez simplement la méthode d’assistance `View` pour retourner le `ViewResult` :</span><span class="sxs-lookup"><span data-stu-id="aed19-155">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="aed19-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="aed19-156">*HomeController.cs*</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="aed19-157">Lorsque cette action est retournée, la vue *About.cshtml* indiquée dans la dernière section est restituée en tant que page Web :</span><span class="sxs-lookup"><span data-stu-id="aed19-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Page About affichée dans le navigateur Edge](overview/_static/about-page.png)

<span data-ttu-id="aed19-159">La méthode helper `View` a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="aed19-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="aed19-160">Vous pouvez éventuellement spécifier :</span><span class="sxs-lookup"><span data-stu-id="aed19-160">You can optionally specify:</span></span>

* <span data-ttu-id="aed19-161">Une vue explicite à retourner :</span><span class="sxs-lookup"><span data-stu-id="aed19-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="aed19-162">Un [modèle](xref:mvc/models/model-binding) à passer à la vue :</span><span class="sxs-lookup"><span data-stu-id="aed19-162">A [model](xref:mvc/models/model-binding) to pass to the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="aed19-163">Une vue et un modèle :</span><span class="sxs-lookup"><span data-stu-id="aed19-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="aed19-164">Détection de la vue</span><span class="sxs-lookup"><span data-stu-id="aed19-164">View discovery</span></span>

<span data-ttu-id="aed19-165">Lorsqu’une action retourne une vue, un processus appelé *détection de la vue* a lieu.</span><span class="sxs-lookup"><span data-stu-id="aed19-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="aed19-166">Ce processus détermine quel fichier est utilisé en fonction du nom de la vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="aed19-167">Le comportement par défaut de la méthode `View` (`return View();`) est de retourner une vue du même nom que la méthode d’action à partir de laquelle elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="aed19-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="aed19-168">Par exemple, le nom de la méthode `ActionResult` *About* du contrôleur est utilisé pour rechercher un fichier de vue nommé *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aed19-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="aed19-169">Le Runtime commence par rechercher la vue dans le dossier *Vues/[nom_contrôleur]*.</span><span class="sxs-lookup"><span data-stu-id="aed19-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="aed19-170">S’il ne trouve pas de vue correspondante, il recherche ensuite la vue dans le dossier *Partagé*.</span><span class="sxs-lookup"><span data-stu-id="aed19-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="aed19-171">Peu importe si vous retournez implicitement `ViewResult` avec `return View();` ou si vous passez explicitement le nom de la vue à la méthode `View` avec `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="aed19-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="aed19-172">Dans les deux cas, la détection de la vue recherche un fichier de vue correspondant dans cet ordre :</span><span class="sxs-lookup"><span data-stu-id="aed19-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="aed19-173">*Vues /\[ControllerName] /\[ViewName] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="aed19-173">*Views/\[ControllerName]/\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="aed19-174">*Vues/Partagé/\[nom_vue].cshtml*</span><span class="sxs-lookup"><span data-stu-id="aed19-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="aed19-175">Vous pouvez spécifier le chemin du fichier de vue au lieu du nom de la vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="aed19-176">Si vous utilisez un chemin absolu à partir de la racine de l’application (commençant éventuellement par « / » ou « ~/ »), vous devez indiquer l’extension *.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="aed19-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="aed19-177">Vous pouvez également utiliser un chemin relatif pour spécifier des vues situées dans des répertoires différents. Dans ce cas, n’indiquez pas l’extension *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aed19-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="aed19-178">Dans `HomeController`, vous pouvez retourner la vue *Index* du dossier de vues *Gérer* avec un chemin relatif :</span><span class="sxs-lookup"><span data-stu-id="aed19-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="aed19-179">De même, vous pouvez indiquer le dossier spécifique du contrôleur actif avec le préfixe « ./ » :</span><span class="sxs-lookup"><span data-stu-id="aed19-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="aed19-180">Les [vues partielles](xref:mvc/views/partial) et les [composants de vue](xref:mvc/views/view-components) utilisent des mécanismes de détection presque identiques.</span><span class="sxs-lookup"><span data-stu-id="aed19-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="aed19-181">Vous pouvez personnaliser la convention par défaut de recherche des vues dans l’application à l’aide d’un [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander) personnalisé.</span><span class="sxs-lookup"><span data-stu-id="aed19-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="aed19-182">La détection des vues recherche les fichiers de vue d’après le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="aed19-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="aed19-183">Si le système de fichiers sous-jacent respecte la casse, les noms de vue respectent probablement la casse eux aussi.</span><span class="sxs-lookup"><span data-stu-id="aed19-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="aed19-184">Pour garantir la compatibilité entre les systèmes d’exploitation, utilisez la même casse pour les noms de contrôleur et d’action et pour les noms de leurs dossiers et fichiers de vues associés.</span><span class="sxs-lookup"><span data-stu-id="aed19-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="aed19-185">Si vous utilisez un système de fichiers qui respecte la casse et que vous voyez un message d’erreur indiquant qu’un fichier de vue est introuvable, vérifiez que le nom du fichier de vue demandé et le nom du fichier de vue réel ont une casse identique.</span><span class="sxs-lookup"><span data-stu-id="aed19-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="aed19-186">Suivez les bonnes pratiques en matière d’organisation de la structure des fichiers de vue. Votre structure doit refléter au mieux les relations entre les contrôleurs, les actions et les vues pour être plus claire et facile à gérer.</span><span class="sxs-lookup"><span data-stu-id="aed19-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="aed19-187">Passage de données aux vues</span><span class="sxs-lookup"><span data-stu-id="aed19-187">Passing data to views</span></span>

<span data-ttu-id="aed19-188">Vous pouvez passer des données pour les vues à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="aed19-188">You can pass data to views using several approaches.</span></span> <span data-ttu-id="aed19-189">L’approche la plus fiable consiste à spécifier un [modèle](xref:mvc/models/model-binding) type dans la vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-189">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="aed19-190">Ce modèle est communément appelé un *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="aed19-190">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="aed19-191">Vous passez une instance du type viewmodel à la vue à partir de l’action.</span><span class="sxs-lookup"><span data-stu-id="aed19-191">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="aed19-192">L’utilisation d’un ViewModel pour passer des données à une vue vous permet d’effectuer un contrôle de type *fort* dans la vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-192">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="aed19-193">Le terme *typage fort* (ou *fortement typé*) signifie que chaque variable et constante a un type défini explicitement (par exemple, `string`, `int` ou `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="aed19-193">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="aed19-194">La validité des types utilisés dans une vue est contrôlée au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="aed19-194">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="aed19-195">[Visual Studio](https://www.visualstudio.com/vs/) et [Visual Studio Code](https://code.visualstudio.com/) répertorient les membres de classe fortement typés à l’aide d’une fonctionnalité appelée [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="aed19-195">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="aed19-196">Quand vous voulez afficher les propriétés d’un ViewModel, tapez le nom de variable pour le ViewModel suivi d’un point (`.`).</span><span class="sxs-lookup"><span data-stu-id="aed19-196">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="aed19-197">Cela vous permet d’écrire du code plus rapidement et avec moins d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="aed19-197">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="aed19-198">Spécifiez un modèle à l’aide de la directive `@model`.</span><span class="sxs-lookup"><span data-stu-id="aed19-198">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="aed19-199">Utilisez le modèle avec `@Model` :</span><span class="sxs-lookup"><span data-stu-id="aed19-199">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="aed19-200">Pour fournir le modèle à la vue, le contrôleur le passe en tant que paramètre :</span><span class="sxs-lookup"><span data-stu-id="aed19-200">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="aed19-201">Il n’existe aucune restriction sur les types de modèles que vous pouvez fournir à une vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-201">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="aed19-202">Nous vous recommandons d’utiliser des modèles de vue **P****O****C****O**avec peu ou pas de comportements (méthodes) définis.</span><span class="sxs-lookup"><span data-stu-id="aed19-202">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="aed19-203">En règle générale, les classes viewmodel sont soit stockées dans le dossier *Models* ou dans un dossier *ViewModels* distinct à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="aed19-203">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="aed19-204">Le viewmodel *Adress* utilisé dans l’exemple ci-dessus est un viewmodel POCO stocké dans un fichier nommé *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="aed19-204">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> <span data-ttu-id="aed19-205">Rien ne vous empêche d’utiliser les mêmes classes pour vos types de ViewModel et vos types de modèle métier.</span><span class="sxs-lookup"><span data-stu-id="aed19-205">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="aed19-206">Toutefois, l’utilisation de modèles distincts vous permet de changer les vues indépendamment de la logique métier et des composants d’accès aux données de votre application.</span><span class="sxs-lookup"><span data-stu-id="aed19-206">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="aed19-207">La séparation des modèles et des ViewModel est également un gage de sécurité si vous avez des modèles qui utilisent la [liaison de données](xref:mvc/models/model-binding) et la [validation](xref:mvc/models/validation) pour les données envoyées à l’application par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aed19-207">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="aed19-208">Données faiblement typées (ViewData et ViewBag)</span><span class="sxs-lookup"><span data-stu-id="aed19-208">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="aed19-209">Remarque : `ViewBag` n’est pas disponible dans les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="aed19-209">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="aed19-210">En plus des vues fortement typées, les vues ont accès à une collection de données *faiblement typées* (ou *librement typées*).</span><span class="sxs-lookup"><span data-stu-id="aed19-210">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="aed19-211">Contrairement aux types forts, les *types faibles* (ou *types libres*) ne nécessitent pas de déclarer explicitement le type de données utilisé.</span><span class="sxs-lookup"><span data-stu-id="aed19-211">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="aed19-212">Vous pouvez utiliser la collection de données faiblement typées pour passer de petites quantités de données entre les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-212">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="aed19-213">Passer des données entre...</span><span class="sxs-lookup"><span data-stu-id="aed19-213">Passing data between a ...</span></span>                        | <span data-ttu-id="aed19-214">Exemple</span><span class="sxs-lookup"><span data-stu-id="aed19-214">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="aed19-215">Un contrôleur et une vue</span><span class="sxs-lookup"><span data-stu-id="aed19-215">Controller and a view</span></span>                             | <span data-ttu-id="aed19-216">Remplissage d’une liste déroulante avec des données.</span><span class="sxs-lookup"><span data-stu-id="aed19-216">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="aed19-217">Une vue et une [disposition](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="aed19-217">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="aed19-218">Définition du contenu de l’élément **\<title>** dans la disposition à partir d’un fichier de vue.</span><span class="sxs-lookup"><span data-stu-id="aed19-218">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="aed19-219">Une [vue partielle](xref:mvc/views/partial) et une vue</span><span class="sxs-lookup"><span data-stu-id="aed19-219">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="aed19-220">Widget qui affiche des données en fonction de la page web demandée par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aed19-220">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="aed19-221">Cette collection peut être référencée par les propriétés `ViewData` ou `ViewBag` sur les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-221">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="aed19-222">La propriété `ViewData` est un dictionnaire d’objets faiblement typés.</span><span class="sxs-lookup"><span data-stu-id="aed19-222">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="aed19-223">La propriété `ViewBag` est un wrapper autour de `ViewData` qui fournit des propriétés dynamiques pour la collection `ViewData` sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="aed19-223">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="aed19-224">`ViewData` et `ViewBag` sont résolues dynamiquement au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="aed19-224">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="aed19-225">Dans la mesure où elles n’effectuent pas de contrôle de type à la compilation, ces deux propriétés sont généralement davantage sujettes aux erreurs qu’un ViewModel.</span><span class="sxs-lookup"><span data-stu-id="aed19-225">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="aed19-226">Pour cette raison, certains développeurs préfèrent ne jamais utiliser les propriétés `ViewData` et `ViewBag`, ou les utiliser le moins possible.</span><span class="sxs-lookup"><span data-stu-id="aed19-226">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="aed19-227">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="aed19-227">**ViewData**</span></span>

<span data-ttu-id="aed19-228">`ViewData` est un objet [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) accessible à l’aide de clés `string`.</span><span class="sxs-lookup"><span data-stu-id="aed19-228">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="aed19-229">Vous pouvez stocker et utiliser des données de type chaîne directement, sans avoir à les caster, mais vous devez effectuer un cast des autres valeurs de l’objet `ViewData` vers des types spécifiques lors de leur extraction.</span><span class="sxs-lookup"><span data-stu-id="aed19-229">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="aed19-230">Vous pouvez utiliser `ViewData` pour passer des données des contrôleurs aux vues et au sein même des vues, y compris les [vues partielles](xref:mvc/views/partial) et les [dispositions](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="aed19-230">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="aed19-231">L’exemple suivant utilise un objet `ViewData` dans une action pour définir les valeurs d’un message d’accueil et d’une adresse :</span><span class="sxs-lookup"><span data-stu-id="aed19-231">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="aed19-232">Utilisation des données dans une vue :</span><span class="sxs-lookup"><span data-stu-id="aed19-232">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="aed19-233">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="aed19-233">**ViewBag**</span></span>

<span data-ttu-id="aed19-234">Remarque : `ViewBag` n’est pas disponible dans les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="aed19-234">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="aed19-235">`ViewBag` est un objet [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) qui fournit un accès dynamique aux objets stockés dans `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="aed19-235">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="aed19-236">`ViewBag` est parfois plus pratique à utiliser, car il ne nécessite pas de cast.</span><span class="sxs-lookup"><span data-stu-id="aed19-236">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="aed19-237">L’exemple suivant montre comment utiliser `ViewBag` pour obtenir le même résultat qu’avec l’objet `ViewData` ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="aed19-237">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="aed19-238">**Utilisation simultanée de ViewData et ViewBag**</span><span class="sxs-lookup"><span data-stu-id="aed19-238">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="aed19-239">Remarque : `ViewBag` n’est pas disponible dans les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="aed19-239">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="aed19-240">Comme `ViewData` et `ViewBag` font référence à la même collection `ViewData` sous-jacente, vous pouvez utiliser `ViewData` et `ViewBag` simultanément, en les combinant et en les associant pour lire et écrire des valeurs.</span><span class="sxs-lookup"><span data-stu-id="aed19-240">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="aed19-241">Définissez le titre avec `ViewBag` et la description avec `ViewData` au début d’une vue *About.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="aed19-241">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="aed19-242">Lisez les propriétés, mais inversez l’utilisation de `ViewData` et `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="aed19-242">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="aed19-243">Dans le fichier *_Layout.cshtml*, obtenez le titre et la description avec `ViewData` et `ViewBag`, respectivement :</span><span class="sxs-lookup"><span data-stu-id="aed19-243">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="aed19-244">Souvenez-vous que les chaînes ne nécessitent pas de cast pour `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="aed19-244">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="aed19-245">Vous pouvez utiliser `@ViewData["Title"]` sans cast.</span><span class="sxs-lookup"><span data-stu-id="aed19-245">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="aed19-246">L’utilisation simultanée de `ViewData` et `ViewBag` est possible, de la même manière que la combinaison et l’association des propriétés de lecture et d’écriture.</span><span class="sxs-lookup"><span data-stu-id="aed19-246">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="aed19-247">Le balisage suivant est affiché :</span><span class="sxs-lookup"><span data-stu-id="aed19-247">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="aed19-248">**Récapitulatif des différences entre ViewData et ViewBag**</span><span class="sxs-lookup"><span data-stu-id="aed19-248">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="aed19-249">`ViewBag` n’est pas disponible dans les pages Razor.</span><span class="sxs-lookup"><span data-stu-id="aed19-249">`ViewBag` isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="aed19-250">Dérivé de [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), cet objet fournit des propriétés de dictionnaire potentiellement utiles, telles que `ContainsKey`, `Add`, `Remove` et `Clear`.</span><span class="sxs-lookup"><span data-stu-id="aed19-250">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="aed19-251">Les clés contenues dans le dictionnaire sont des chaînes ; les espaces blancs sont donc autorisés.</span><span class="sxs-lookup"><span data-stu-id="aed19-251">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="aed19-252">Exemple : `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="aed19-252">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="aed19-253">Les autres types que `string` doivent être castés dans la vue pour utiliser `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="aed19-253">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="aed19-254">Dérivé de [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), cet objet permet de créer des propriétés dynamiques avec la notation par points (`@ViewBag.SomeKey = <value or object>`). Aucun cast n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="aed19-254">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="aed19-255">La syntaxe de `ViewBag` facilite son ajout aux contrôleurs et aux vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-255">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="aed19-256">Simplifie la vérification des valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="aed19-256">Simpler to check for null values.</span></span> <span data-ttu-id="aed19-257">Exemple : `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="aed19-257">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="aed19-258">**Quand utiliser ViewData ou ViewBag ?**</span><span class="sxs-lookup"><span data-stu-id="aed19-258">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="aed19-259">`ViewData` et `ViewBag` constituent deux approches appropriées pour passer de petites quantités de données entre les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-259">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="aed19-260">Choisissez celle qui vous convient le mieux.</span><span class="sxs-lookup"><span data-stu-id="aed19-260">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="aed19-261">Vous pouvez combiner et associer les objets `ViewData` et `ViewBag`. Toutefois, il est recommandé d’utiliser une seule approche pour faciliter la lecture et la gestion du code.</span><span class="sxs-lookup"><span data-stu-id="aed19-261">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="aed19-262">Les deux approches sont résolues dynamiquement au moment de l’exécution et sont donc susceptibles d’engendrer des erreurs d’exécution.</span><span class="sxs-lookup"><span data-stu-id="aed19-262">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="aed19-263">C’est la raison pour laquelle certains développeurs préfèrent ne pas les utiliser.</span><span class="sxs-lookup"><span data-stu-id="aed19-263">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="aed19-264">Vues dynamiques</span><span class="sxs-lookup"><span data-stu-id="aed19-264">Dynamic views</span></span>

<span data-ttu-id="aed19-265">Les vues qui ne déclarent pas de modèle de type à l’aide de `@model` mais qui reçoivent une instance de modèle (par exemple, `return View(Address);`) peuvent référencer dynamiquement les propriétés de l’instance :</span><span class="sxs-lookup"><span data-stu-id="aed19-265">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="aed19-266">Cette fonctionnalité offre beaucoup de flexibilité, mais elle ne fournit pas de protection de la compilation ou la fonction IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="aed19-266">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="aed19-267">Si la propriété n’existe pas, la génération de la page web échoue au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="aed19-267">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="aed19-268">Autres fonctionnalités de vue</span><span class="sxs-lookup"><span data-stu-id="aed19-268">More view features</span></span>

<span data-ttu-id="aed19-269">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent d’ajouter facilement un comportement côté serveur dans des balises HTML existantes.</span><span class="sxs-lookup"><span data-stu-id="aed19-269">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="aed19-270">Leur utilisation vous évite d’avoir à écrire du code personnalisé ou des méthodes d’assistance dans les vues.</span><span class="sxs-lookup"><span data-stu-id="aed19-270">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="aed19-271">Les Tag Helpers sont appliqués comme attributs aux éléments HTML et sont ignorés par les éditeurs qui ne peuvent pas les traiter.</span><span class="sxs-lookup"><span data-stu-id="aed19-271">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="aed19-272">Vous pouvez ainsi modifier et afficher le balisage des vues dans divers outils.</span><span class="sxs-lookup"><span data-stu-id="aed19-272">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="aed19-273">Générer un balisage HTML personnalisé peut être obtenu avec de nombreux helpers HTML intégrés.</span><span class="sxs-lookup"><span data-stu-id="aed19-273">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="aed19-274">Une logique plus complexe de l’interface utilisateur peut être gérée par des [composants de vue](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="aed19-274">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="aed19-275">Les composants de vue fournissent la même séparation de responsabilité (SoC) que les contrôleurs et les vues offrent.</span><span class="sxs-lookup"><span data-stu-id="aed19-275">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="aed19-276">Il peuvent éliminer le besoin d'utiliser des actions ou des vues qui traitent les données utilisées par les éléments d’interface couramment utilisés.</span><span class="sxs-lookup"><span data-stu-id="aed19-276">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="aed19-277">Comme de nombreux autres aspects d’ASP.NET Core, les vues prennent en charge [l’injection de dépendances](xref:fundamentals/dependency-injection), ce qui permet aux services d’être [injectés dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="aed19-277">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
