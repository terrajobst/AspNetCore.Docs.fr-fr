---
title: "Vues de base d’ASP.NET MVC"
author: ardalis
description: "Découvrez comment gérer les vues de présentation des données de l’application et l’interaction utilisateur dans ASP.NET MVC de base."
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: c0a1f475941f3389e9aa1f5bb7819bef491b2cae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="7446a-103">Vues de base d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7446a-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="7446a-104">Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7446a-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7446a-105">Ce document explique les vues utilisées dans les applications ASP.NET MVC de base.</span><span class="sxs-lookup"><span data-stu-id="7446a-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="7446a-106">Pour plus d’informations sur les Pages Razor, consultez [Introduction aux Pages Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="7446a-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="7446a-107">Dans le **M**odèle -**V**UE -**C**ontroller (MVC), modèle, le *vue* gère l’interaction utilisateur et de présentation des données de l’application.</span><span class="sxs-lookup"><span data-stu-id="7446a-107">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="7446a-108">Une vue est un modèle HTML incorporé [balisage de Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="7446a-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="7446a-109">Balisage de Razor est un code qui interagit avec le balisage HTML pour générer une page Web qui est envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="7446a-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="7446a-110">Dans ASP.NET MVC de base, les vues sont *.cshtml* fichiers qui utilisent le [langage de programmation c#](/dotnet/csharp/) dans le balisage de Razor.</span><span class="sxs-lookup"><span data-stu-id="7446a-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="7446a-111">En règle générale, afficher les fichiers sont regroupés dans des dossiers nommés pour chacune de l’application [contrôleurs](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="7446a-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="7446a-112">Les dossiers sont stockées dans un *vues* dossier à la racine de l’application :</span><span class="sxs-lookup"><span data-stu-id="7446a-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Dossier de vues dans l’Explorateur de solutions de Visual Studio est ouvert avec le dossier de base ouvert pour afficher les fichiers About.cshtml, Contact.cshtml et Index.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="7446a-114">Le *accueil* contrôleur est représenté par un *accueil* dossier à l’intérieur de la *vues* dossier.</span><span class="sxs-lookup"><span data-stu-id="7446a-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="7446a-115">Le *accueil* dossier contient les vues pour la *sur*, *Contact*, et *Index* des pages Web (page d’accueil).</span><span class="sxs-lookup"><span data-stu-id="7446a-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="7446a-116">Quand un utilisateur demande une de ces trois des pages Web, les actions de contrôleur dans le *accueil* contrôleur déterminer lequel des trois vues est utilisé pour générer et retourner une page Web à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7446a-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="7446a-117">Utilisez [dispositions](xref:mvc/views/layout) pour fournir des sections de la page Web cohérente et de réduire la redondance du code.</span><span class="sxs-lookup"><span data-stu-id="7446a-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="7446a-118">Dispositions contiennent souvent l’en-tête, des éléments de menu et de navigation et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="7446a-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="7446a-119">L’en-tête et le pied de page contiennent généralement de balisage standard pour de nombreux éléments de métadonnées et des liens vers des ressources de script et de style.</span><span class="sxs-lookup"><span data-stu-id="7446a-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="7446a-120">Dispositions de vous aident à éviter ce balisage réutilisable dans vos vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="7446a-121">[Les vues partielles](xref:mvc/views/partial) réduire la duplication de code grâce à la gestion de parties réutilisables de vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="7446a-122">Par exemple, une vue partielle est utile pour biographie de l’auteur sur un site Web de blog qui apparaît dans plusieurs vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="7446a-123">Biographie de l’auteur est d’ordinaire afficher le contenu et ne nécessite pas le code à exécuter afin de produire le contenu de la page Web.</span><span class="sxs-lookup"><span data-stu-id="7446a-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="7446a-124">Créer du contenu biographie est disponible à l’affichage par la liaison de modèle uniquement, par conséquent, à l’aide d’une vue partielle pour ce type de contenu est idéale.</span><span class="sxs-lookup"><span data-stu-id="7446a-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="7446a-125">[Affichage des composants](xref:mvc/views/view-components) sont des vues similaires sur la valeur partielle dans la mesure où ils vous permettent de réduire le code répétitif, mais ils sont appropriés pour l’affichage du contenu qui nécessite que du code à exécuter sur le serveur pour restituer la page Web.</span><span class="sxs-lookup"><span data-stu-id="7446a-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="7446a-126">Affichage des composants sont utiles lorsque le contenu rendu requiert une interaction de base de données, comme pour un site Web du panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="7446a-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="7446a-127">Affichage des composants ne sont pas limitées à la liaison de modèle afin de produire la sortie de la page Web.</span><span class="sxs-lookup"><span data-stu-id="7446a-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="7446a-128">Avantages de l’utilisation de vues</span><span class="sxs-lookup"><span data-stu-id="7446a-128">Benefits of using views</span></span>

<span data-ttu-id="7446a-129">Les vues permettent d’établir une [ **S**eparation **o**f **C**oncerns (SoC) conception](http://deviq.com/separation-of-concerns/) au sein d’une application MVC en séparant le balisage d’interface utilisateur à partir de autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="7446a-129">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="7446a-130">Suivant SoC conception rend votre application modulaire, ce qui offre plusieurs avantages :</span><span class="sxs-lookup"><span data-stu-id="7446a-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="7446a-131">L’application est plus facile à gérer, car il est mieux organisée.</span><span class="sxs-lookup"><span data-stu-id="7446a-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="7446a-132">Les vues sont généralement regroupés en fonction de l’application.</span><span class="sxs-lookup"><span data-stu-id="7446a-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="7446a-133">Cela rend plus facile à trouver les vues associées lorsque vous travaillez sur une fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="7446a-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="7446a-134">Les parties de l’application sont faiblement couplés.</span><span class="sxs-lookup"><span data-stu-id="7446a-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="7446a-135">Vous pouvez générer et mettre à jour des vues de l’application séparément dans les composants accès logique et les données d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="7446a-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="7446a-136">Vous pouvez modifier les vues de l’application sans nécessairement devoir mettre à jour des autres parties de l’application.</span><span class="sxs-lookup"><span data-stu-id="7446a-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="7446a-137">Il est plus facile de tester des parties de l’interface utilisateur de l’application, car les vues sont des unités distinctes.</span><span class="sxs-lookup"><span data-stu-id="7446a-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="7446a-138">En raison d’une meilleure organisation, il est moins probable que vous allez accidentellement sections de répétitions de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7446a-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="7446a-139">Création d’une vue</span><span class="sxs-lookup"><span data-stu-id="7446a-139">Creating a view</span></span>

<span data-ttu-id="7446a-140">Les vues qui sont spécifiques à un contrôleur sont créées dans le *vues / [nom du contrôleur]* dossier.</span><span class="sxs-lookup"><span data-stu-id="7446a-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="7446a-141">Les vues qui sont partagées entre les contrôleurs sont placées dans le *Views/Shared* dossier.</span><span class="sxs-lookup"><span data-stu-id="7446a-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="7446a-142">Pour créer une vue, ajoutez un nouveau fichier et lui donner le même nom que son action de contrôleur associé avec le *.cshtml* extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="7446a-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="7446a-143">Pour créer une vue qui correspond à la *sur* action dans le *accueil* contrôleur, créez un *About.cshtml* de fichiers dans le *Views/Home*dossier :</span><span class="sxs-lookup"><span data-stu-id="7446a-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="7446a-144">*Razor* balisage commence par la `@` symbole.</span><span class="sxs-lookup"><span data-stu-id="7446a-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="7446a-145">Les instructions c# exécution en plaçant c# du code dans [blocs de code Razor](xref:mvc/views/razor#razor-code-blocks) définir à off par des accolades (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="7446a-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="7446a-146">Par exemple, consultez l’affectation de « About » pour `ViewData["Title"]` ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="7446a-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="7446a-147">Vous pouvez afficher des valeurs dans le code HTML en référençant simplement la valeur avec le `@` symbole.</span><span class="sxs-lookup"><span data-stu-id="7446a-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="7446a-148">Consultez le contenu de la `<h2>` et `<h3>` éléments ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="7446a-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="7446a-149">Le contenu de la vue ci-dessus n'est qu’une partie de la page Web entière qui est restituée à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7446a-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="7446a-150">Le reste de la mise en page et d’autres aspects courantes de la vue sont spécifiés dans d’autres fichiers de vue.</span><span class="sxs-lookup"><span data-stu-id="7446a-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="7446a-151">Pour plus d’informations, consultez la [rubrique de présentation](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="7446a-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="7446a-152">La façon dont les contrôleurs spécifient les vues</span><span class="sxs-lookup"><span data-stu-id="7446a-152">How controllers specify views</span></span>

<span data-ttu-id="7446a-153">Les vues sont généralement retournées à partir d’actions en tant qu’un [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), qui est un type de [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="7446a-153">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="7446a-154">Votre méthode d’action peut créer et renvoyer un `ViewResult` directement, mais qui n’est pas généralement effectué.</span><span class="sxs-lookup"><span data-stu-id="7446a-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="7446a-155">Étant donné que la plupart des contrôleurs héritent de [contrôleur](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), vous utilisez simplement le `View` méthode d’assistance pour retourner le `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="7446a-155">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="7446a-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="7446a-156">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="7446a-157">Lorsque cette action est retournée, la *About.cshtml* indiqué dans la dernière section de vue est restituée en tant que la page Web suivante :</span><span class="sxs-lookup"><span data-stu-id="7446a-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Sur la page rendue dans le navigateur Edge](overview/_static/about-page.png)

<span data-ttu-id="7446a-159">Le `View` méthode d’assistance a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="7446a-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="7446a-160">Vous pouvez éventuellement spécifier :</span><span class="sxs-lookup"><span data-stu-id="7446a-160">You can optionally specify:</span></span>

* <span data-ttu-id="7446a-161">Un affichage explicite pour renvoyer :</span><span class="sxs-lookup"><span data-stu-id="7446a-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="7446a-162">A [modèle](xref:mvc/models/model-binding) à passer à la la vue :</span><span class="sxs-lookup"><span data-stu-id="7446a-162">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="7446a-163">Une vue et un modèle :</span><span class="sxs-lookup"><span data-stu-id="7446a-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="7446a-164">Détection de la vue</span><span class="sxs-lookup"><span data-stu-id="7446a-164">View discovery</span></span>

<span data-ttu-id="7446a-165">Lorsqu’une action retourne une vue, un processus appelé *détection de la vue* a lieu.</span><span class="sxs-lookup"><span data-stu-id="7446a-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="7446a-166">Ce processus détermine l’afficher le fichier est utilisé en fonction du nom de la vue.</span><span class="sxs-lookup"><span data-stu-id="7446a-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="7446a-167">Le comportement par défaut de la `View` (méthode) (`return View();`) doit retourner une vue avec le même nom que la méthode d’action à partir de laquelle elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="7446a-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="7446a-168">Par exemple, le *sur* `ActionResult` nom de la méthode du contrôleur est utilisé pour rechercher un fichier de vue nommé *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7446a-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="7446a-169">Tout d’abord, le runtime recherche le *vues / [nom du contrôleur]* dossier pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="7446a-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="7446a-170">S’il ne trouve pas une vue correspondante, il recherche le *Shared* dossier pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="7446a-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="7446a-171">Peu importe si vous retournez implicitement le `ViewResult` avec `return View();` ou explicitement passer le nom d’affichage pour le `View` méthode avec `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="7446a-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="7446a-172">Dans les deux cas, afficher les recherches de découverte pour un fichier de vue correspondant dans cet ordre :</span><span class="sxs-lookup"><span data-stu-id="7446a-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="7446a-173">*Views/\[ControllerName]\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="7446a-173">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="7446a-174">*Views/Shared/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="7446a-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="7446a-175">Un chemin d’accès du fichier de vue peut être fourni au lieu d’un nom de la vue.</span><span class="sxs-lookup"><span data-stu-id="7446a-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="7446a-176">Si vous utilisez un chemin d’accès absolu, commençant à la racine de l’application (éventuellement en commençant par « / » ou « ~ / »), le *.cshtml* extension doit être spécifiée :</span><span class="sxs-lookup"><span data-stu-id="7446a-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="7446a-177">Vous pouvez également utiliser un chemin d’accès relatif pour spécifier les vues dans des répertoires différents sans le *.cshtml* extension.</span><span class="sxs-lookup"><span data-stu-id="7446a-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="7446a-178">À l’intérieur de la `HomeController`, vous pouvez retourner le *Index* afficher de votre *gérer* vues avec un chemin d’accès relatif :</span><span class="sxs-lookup"><span data-stu-id="7446a-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="7446a-179">De même, vous pouvez indiquer le répertoire spécifique du contrôleur actif avec le «. / » préfixe :</span><span class="sxs-lookup"><span data-stu-id="7446a-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="7446a-180">[Les vues partielles](xref:mvc/views/partial) et [affichage des composants](xref:mvc/views/view-components) utilisent des mécanismes de découverte similaires (mais non identiques).</span><span class="sxs-lookup"><span data-stu-id="7446a-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="7446a-181">Vous pouvez personnaliser la convention par défaut pour comment les vues sont situés dans l’application à l’aide d’un [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="7446a-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="7446a-182">Découverte de la vue s’appuie sur la recherche d’afficher les fichiers par nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="7446a-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="7446a-183">Si le système de fichiers sous-jacent respecte la casse, les noms des vues sont probablement respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="7446a-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="7446a-184">Pour la compatibilité entre les systèmes d’exploitation, respecter la casse entre le contrôleur et les noms d’actions et associé à afficher les dossiers et les noms de fichiers.</span><span class="sxs-lookup"><span data-stu-id="7446a-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="7446a-185">Si vous rencontrez une erreur Impossible de trouver un fichier de vue lorsque vous travaillez avec un système de fichiers respectant la casse, vérifiez que la casse identique dans le fichier de la vue demandée et le nom de fichier réel de l’affichage.</span><span class="sxs-lookup"><span data-stu-id="7446a-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="7446a-186">Suivre la meilleure pratique d’organiser la structure de fichiers pour les vues afin de refléter les relations entre les contrôleurs, les actions et les vues à la facilité de maintenance et de clarté.</span><span class="sxs-lookup"><span data-stu-id="7446a-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="7446a-187">Passage de données à des vues</span><span class="sxs-lookup"><span data-stu-id="7446a-187">Passing data to views</span></span>

<span data-ttu-id="7446a-188">Vous pouvez passer des données pour les vues à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="7446a-188">You can pass data to views using several approaches.</span></span> <span data-ttu-id="7446a-189">L’approche la plus fiable consiste à spécifier un [modèle](xref:mvc/models/model-binding) type dans la vue.</span><span class="sxs-lookup"><span data-stu-id="7446a-189">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="7446a-190">Ce modèle est communément appelé une *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="7446a-190">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="7446a-191">Vous passez une instance du type viewmodel à la vue à partir de l’action.</span><span class="sxs-lookup"><span data-stu-id="7446a-191">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="7446a-192">À l’aide d’un viewmodel pour passer des données à une vue permet de tirer parti de la vue de *fort* la vérification du type.</span><span class="sxs-lookup"><span data-stu-id="7446a-192">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="7446a-193">*Un typage fort* (ou *fortement typé*) signifie que chaque variable et constante a un type défini explicitement (par exemple, `string`, `int`, ou `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="7446a-193">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="7446a-194">La validité des types utilisés dans une vue est vérifiée au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="7446a-194">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="7446a-195">[Visual Studio](https://www.visualstudio.com/vs/) et [Visual Studio Code](https://code.visualstudio.com/) répertorient les membres de classe fortement typée à l’aide d’une fonctionnalité appelée [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="7446a-195">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="7446a-196">Lorsque vous souhaitez afficher les propriétés d’un viewmodel, tapez le nom de variable pour le viewmodel suivi d’un point (`.`).</span><span class="sxs-lookup"><span data-stu-id="7446a-196">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="7446a-197">Cela vous permet d’écrire du code plus rapidement avec moins d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="7446a-197">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="7446a-198">Spécifier un modèle à l’aide de la `@model` la directive.</span><span class="sxs-lookup"><span data-stu-id="7446a-198">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="7446a-199">Utiliser le modèle avec `@Model`:</span><span class="sxs-lookup"><span data-stu-id="7446a-199">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="7446a-200">Pour fournir le modèle à la vue, le contrôleur transmet en tant que paramètre :</span><span class="sxs-lookup"><span data-stu-id="7446a-200">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="7446a-201">Il n’existe aucune restriction sur les types de modèles que vous pouvez fournir à une vue.</span><span class="sxs-lookup"><span data-stu-id="7446a-201">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="7446a-202">Nous vous recommandons d’utiliser **P**brut **O**%ld **C**LR **O**ViewModel objet (POCO) avec peu ou pas comportement (méthodes) défini.</span><span class="sxs-lookup"><span data-stu-id="7446a-202">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="7446a-203">En règle générale, les classes viewmodel sont soit stockés dans le *modèles* dossier ou distinct *ViewModel* dossier à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="7446a-203">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="7446a-204">Le *adresse* viewmodel utilisé dans l’exemple ci-dessus est un viewmodel POCO stocké dans un fichier nommé *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="7446a-204">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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
> <span data-ttu-id="7446a-205">Rien ne vous empêche d’utiliser les mêmes classes pour vos types viewmodel et vos types de modèle d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="7446a-205">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="7446a-206">Toutefois, à l’aide des modèles distincts permet à vos vues varier indépendamment à partir de la logique métier et les données des accès des parties de votre application.</span><span class="sxs-lookup"><span data-stu-id="7446a-206">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="7446a-207">Séparation des modèles et ViewModel offre également des avantages de sécurité lorsque les modèles utilisent [liaison de modèle](xref:mvc/models/model-binding) et [validation](xref:mvc/models/validation) pour les données envoyées à l’application par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7446a-207">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="7446a-208">Données faiblement typée (ViewData et ViewBag)</span><span class="sxs-lookup"><span data-stu-id="7446a-208">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="7446a-209">Remarque : `ViewBag` n’est pas disponible dans les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="7446a-209">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="7446a-210">Outre les vues fortement typées, vues ont accès à un *faiblement typée* (également appelé *faiblement typé*) collecte des données.</span><span class="sxs-lookup"><span data-stu-id="7446a-210">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="7446a-211">Contrairement aux types forts, *types faibles* (ou *de perdre des types*) signifie que vous ne déclariez pas explicitement le type de données que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="7446a-211">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="7446a-212">Vous pouvez utiliser la collection de données faiblement typé pour le passage de petites quantités de données vers et depuis les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-212">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="7446a-213">Passer des données entre un...</span><span class="sxs-lookup"><span data-stu-id="7446a-213">Passing data between a ...</span></span>                        | <span data-ttu-id="7446a-214">Exemple</span><span class="sxs-lookup"><span data-stu-id="7446a-214">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="7446a-215">Contrôleur et une vue</span><span class="sxs-lookup"><span data-stu-id="7446a-215">Controller and a view</span></span>                             | <span data-ttu-id="7446a-216">Remplissage d’une liste déroulante avec des données.</span><span class="sxs-lookup"><span data-stu-id="7446a-216">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="7446a-217">Affichage et un [mode](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="7446a-217">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="7446a-218">Définition de la  **\<titre >** contenu de l’élément dans la vue mise en page à partir d’un fichier de vue.</span><span class="sxs-lookup"><span data-stu-id="7446a-218">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="7446a-219">[Vue partielle](xref:mvc/views/partial) et une vue</span><span class="sxs-lookup"><span data-stu-id="7446a-219">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="7446a-220">Un widget qui affiche des données en fonction de la page Web demandée par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7446a-220">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="7446a-221">Cette collection peut être référencée par le biais du `ViewData` ou `ViewBag` propriétés sur les contrôleurs et vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-221">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="7446a-222">Le `ViewData` propriété est un dictionnaire d’objets de faiblement typée.</span><span class="sxs-lookup"><span data-stu-id="7446a-222">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="7446a-223">Le `ViewBag` propriété est un wrapper autour de `ViewData` qui fournit des propriétés dynamiques pour sous-jacent `ViewData` collection.</span><span class="sxs-lookup"><span data-stu-id="7446a-223">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="7446a-224">`ViewData`et `ViewBag` sont résolues dynamiquement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="7446a-224">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="7446a-225">Dans la mesure où ils n’offrent pas de vérification de type lors de la compilation, les deux sont généralement plus sujettes aux erreurs que l’utilisation d’un modèle de vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-225">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="7446a-226">Pour cette raison, certains développeurs préfèrent minimale ou jamais utiliser `ViewData` et `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="7446a-226">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="7446a-227">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="7446a-227">**ViewData**</span></span>

<span data-ttu-id="7446a-228">`ViewData`est un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) objet accédé via `string` clés.</span><span class="sxs-lookup"><span data-stu-id="7446a-228">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="7446a-229">Données de chaîne peuvent être stockées et utilisées directement, sans la nécessité d’un cast, mais vous devez effectuer un cast des autres `ViewData` valeurs à des types spécifiques de l’objet lorsque vous extrayez les.</span><span class="sxs-lookup"><span data-stu-id="7446a-229">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="7446a-230">Vous pouvez utiliser `ViewData` pour passer des données à partir des contrôleurs vers des affichages et dans les affichages, y compris [vues partielles](xref:mvc/views/partial) et [dispositions](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="7446a-230">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="7446a-231">Voici un exemple qui définit les valeurs pour un message d’accueil et une adresse à l’aide `ViewData` dans une action :</span><span class="sxs-lookup"><span data-stu-id="7446a-231">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="7446a-232">Utiliser les données dans une vue :</span><span class="sxs-lookup"><span data-stu-id="7446a-232">Work with the data in a view:</span></span>

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

<span data-ttu-id="7446a-233">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="7446a-233">**ViewBag**</span></span>

<span data-ttu-id="7446a-234">Remarque : `ViewBag` n’est pas disponible dans les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="7446a-234">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="7446a-235">`ViewBag`est un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objet qui fournit un accès dynamique pour les objets stockés dans `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="7446a-235">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="7446a-236">`ViewBag`peut être plus pratique d’utiliser, car il ne nécessite un transtypage.</span><span class="sxs-lookup"><span data-stu-id="7446a-236">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="7446a-237">L’exemple suivant montre comment utiliser `ViewBag` avec le même résultat qu’à l’aide de `ViewData` ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="7446a-237">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

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

<span data-ttu-id="7446a-238">**À l’aide de ViewData et ViewBag simultanément**</span><span class="sxs-lookup"><span data-stu-id="7446a-238">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="7446a-239">Remarque : `ViewBag` n’est pas disponible dans les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="7446a-239">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="7446a-240">Étant donné que `ViewData` et `ViewBag` font référence au même sous-jacent `ViewData` collection, vous pouvez utiliser les deux `ViewData` et `ViewBag` et combiner entre eux lors de la lecture et l’écriture de valeurs.</span><span class="sxs-lookup"><span data-stu-id="7446a-240">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="7446a-241">Définir le titre à l’aide `ViewBag` et la description à l’aide `ViewData` en haut d’un *About.cshtml* vue :</span><span class="sxs-lookup"><span data-stu-id="7446a-241">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="7446a-242">Lire les propriétés, mais l’inverse de l’utilisation de `ViewData` et `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="7446a-242">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="7446a-243">Dans le *_Layout.cshtml* de fichiers, d’obtenir le titre à l’aide de `ViewData` et obtenez la description en utilisant `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="7446a-243">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="7446a-244">Souvenez-vous que les chaînes ne nécessitent un cast pour `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="7446a-244">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="7446a-245">Vous pouvez utiliser `@ViewData["Title"]` sans cast.</span><span class="sxs-lookup"><span data-stu-id="7446a-245">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="7446a-246">L’utilisation des deux `ViewData` et `ViewBag` sur les travaux de même temps, en tant que fait mélangeant et en lire et écrire les propriétés.</span><span class="sxs-lookup"><span data-stu-id="7446a-246">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="7446a-247">Le balisage suivant est rendu :</span><span class="sxs-lookup"><span data-stu-id="7446a-247">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="7446a-248">**Résumé des différences entre ViewData et ViewBag**</span><span class="sxs-lookup"><span data-stu-id="7446a-248">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="7446a-249">`ViewBag`n’est pas disponible dans les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="7446a-249">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="7446a-250">Dérive de [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), de sorte qu’elle possède des propriétés du dictionnaire qui peuvent être utiles, telles que `ContainsKey`, `Add`, `Remove`, et `Clear`.</span><span class="sxs-lookup"><span data-stu-id="7446a-250">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="7446a-251">Clés du dictionnaire sont des chaînes, un espace blanc est autorisé.</span><span class="sxs-lookup"><span data-stu-id="7446a-251">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="7446a-252">Exemple : `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="7446a-252">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="7446a-253">Tout type autre qu’un `string` doit être effectué dans la vue à utiliser `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="7446a-253">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="7446a-254">Dérive de [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), donc il permet la création de propriétés dynamiques à l’aide de la notation par points (`@ViewBag.SomeKey = <value or object>`), et aucune conversion n’est requise.</span><span class="sxs-lookup"><span data-stu-id="7446a-254">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="7446a-255">La syntaxe de `ViewBag` rend plus rapide de les ajouter à des contrôleurs et vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-255">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="7446a-256">La plus simple vérifier les valeurs null.</span><span class="sxs-lookup"><span data-stu-id="7446a-256">Simpler to check for null values.</span></span> <span data-ttu-id="7446a-257">Exemple : `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="7446a-257">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="7446a-258">**Quand utiliser ViewData ou ViewBag**</span><span class="sxs-lookup"><span data-stu-id="7446a-258">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="7446a-259">Les deux `ViewData` et `ViewBag` sont également des approches valides pour le passage de petites quantités de données entre les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-259">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="7446a-260">Le choix de l’application à utiliser est selon la préférence.</span><span class="sxs-lookup"><span data-stu-id="7446a-260">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="7446a-261">Vous pouvez combiner des `ViewData` et `ViewBag` objets, toutefois, le code est plus facile à lire et à maintenir avec une approche utilisée régulièrement.</span><span class="sxs-lookup"><span data-stu-id="7446a-261">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="7446a-262">Les deux approches sont résolues dynamiquement lors de l’exécution et donc enclin à provoquer des erreurs d’exécution.</span><span class="sxs-lookup"><span data-stu-id="7446a-262">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="7446a-263">Certaines équipes de développement de les évitent.</span><span class="sxs-lookup"><span data-stu-id="7446a-263">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="7446a-264">Vues dynamiques</span><span class="sxs-lookup"><span data-stu-id="7446a-264">Dynamic views</span></span>

<span data-ttu-id="7446a-265">Les vues qui ne déclarent pas un modèle de type à l’aide de `@model` mais qui ont passé à une instance de modèle (par exemple, `return View(Address);`) peuvent référencer dynamiquement les propriétés de l’instance :</span><span class="sxs-lookup"><span data-stu-id="7446a-265">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="7446a-266">Cette fonctionnalité offre une souplesse, mais ne propose pas de protection de la compilation ou d’IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="7446a-266">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="7446a-267">Si la propriété n’existe pas, la génération de la page Web échoue lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="7446a-267">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="7446a-268">D’autres fonctionnalités d’affichage</span><span class="sxs-lookup"><span data-stu-id="7446a-268">More view features</span></span>

<span data-ttu-id="7446a-269">[Programmes d’assistance de balise](xref:mvc/views/tag-helpers/intro) rendent facile d’ajouter le comportement côté serveur pour les balises HTML existants.</span><span class="sxs-lookup"><span data-stu-id="7446a-269">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="7446a-270">À l’aide de programmes d’assistance de balise évite d’avoir à écrire du code personnalisé ou des programmes d’assistance dans les vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-270">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="7446a-271">Programmes d’assistance de balise sont appliquées en tant qu’attributs aux éléments HTML et sont ignorés par les éditeurs qui ne peut pas les traiter.</span><span class="sxs-lookup"><span data-stu-id="7446a-271">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="7446a-272">Cela vous permet de modifier et de restituer le balisage de vue dans une variété d’outils.</span><span class="sxs-lookup"><span data-stu-id="7446a-272">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="7446a-273">Génération d’un balisage HTML personnalisé peut être obtenue avec nombreux programmes d’assistance de HTML intégrés.</span><span class="sxs-lookup"><span data-stu-id="7446a-273">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="7446a-274">Logique de l’interface utilisateur plus complexe peut être gérée par [affichage des composants](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="7446a-274">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="7446a-275">Affichage des composants fournissent le même SoC qui contrôleurs et offrent des vues.</span><span class="sxs-lookup"><span data-stu-id="7446a-275">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="7446a-276">Il peuvent éliminer la nécessité pour les actions et les vues qui traitent les données utilisées par les éléments d’interface courants.</span><span class="sxs-lookup"><span data-stu-id="7446a-276">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="7446a-277">Comme de nombreux aspects d’ASP.NET Core, vues prennent en charge [injection de dépendance](xref:fundamentals/dependency-injection), autorisant les services soient [injectées dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7446a-277">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
