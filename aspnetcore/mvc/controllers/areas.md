---
title: Zones dans ASP.NET Core
author: rick-anderson
description: Découvrez les zones, fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312216"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="eea27-103">Zones dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eea27-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="eea27-104">Par [Dhananjay Kumar](https://twitter.com/debug_mode) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eea27-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eea27-105">Les zones sont une fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).</span><span class="sxs-lookup"><span data-stu-id="eea27-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="eea27-106">L’utilisation de zones crée une hiérarchie qui permet le routage par ajout d’un autre paramètre de route, `area`, à `controller` et à `action`.</span><span class="sxs-lookup"><span data-stu-id="eea27-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="eea27-107">Les zones permettent de partitionner une application web ASP.NET Core MVC de grande taille en regroupements fonctionnels plus petits.</span><span class="sxs-lookup"><span data-stu-id="eea27-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="eea27-108">Une zone est en réalité une structure MVC à l’intérieur d’une application.</span><span class="sxs-lookup"><span data-stu-id="eea27-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="eea27-109">Dans un projet MVC, les composants logiques, comme Modèle, Contrôleur et Vue, sont conservés dans des dossiers différents, et MVC utilise des conventions de nommage pour créer la relation entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="eea27-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="eea27-110">Pour une application de grande taille, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de haut niveau.</span><span class="sxs-lookup"><span data-stu-id="eea27-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="eea27-111">Par exemple, considérons une application de e-commerce avec plusieurs entités métier, comme la commande, la facturation, la recherche, etc. Chacune de ces unités a ses propres vues, contrôleurs et modèles de composant logique.</span><span class="sxs-lookup"><span data-stu-id="eea27-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="eea27-112">Dans ce scénario, vous pouvez utiliser des zones pour partitionner physiquement les composants métier dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="eea27-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="eea27-113">Une zone peut être définie comme une unité fonctionnelle plus petite dans un projet ASP.NET Core MVC avec son propre ensemble de contrôleurs, de vues et de modèles.</span><span class="sxs-lookup"><span data-stu-id="eea27-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="eea27-114">Envisagez l’utilisation de zones dans un projet MVC quand :</span><span class="sxs-lookup"><span data-stu-id="eea27-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="eea27-115">Votre application est constituée de plusieurs composants fonctionnels de haut niveau qui doivent être logiquement séparés</span><span class="sxs-lookup"><span data-stu-id="eea27-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="eea27-116">Vous voulez partitionner votre projet MVC de façon à pouvoir travailler indépendamment sur chaque zone fonctionnelle</span><span class="sxs-lookup"><span data-stu-id="eea27-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="eea27-117">Caractéristiques des zones :</span><span class="sxs-lookup"><span data-stu-id="eea27-117">Area features:</span></span>

* <span data-ttu-id="eea27-118">Une application ASP.NET Core MVC peut contenir n’importe quel nombre de zones.</span><span class="sxs-lookup"><span data-stu-id="eea27-118">An ASP.NET Core MVC app can have any number of areas.</span></span>

* <span data-ttu-id="eea27-119">Chaque zone a ses propres contrôleurs, modèles et vues.</span><span class="sxs-lookup"><span data-stu-id="eea27-119">Each area has its own controllers, models, and views.</span></span>

* <span data-ttu-id="eea27-120">Les zones vous permettent d’organiser des projets MVC de grande taille en plusieurs composants généraux sur lesquels vous pouvez travailler indépendamment.</span><span class="sxs-lookup"><span data-stu-id="eea27-120">Areas allow you to organize large MVC projects into multiple high-level components that can be worked on independently.</span></span>

* <span data-ttu-id="eea27-121">Les zones prennent en charge plusieurs contrôleurs de même nom, à condition que ces contrôleurs soient dans des *zones* différentes.</span><span class="sxs-lookup"><span data-stu-id="eea27-121">Areas support multiple controllers with the same name, as long as they have different *areas*.</span></span>

<span data-ttu-id="eea27-122">Examinons un exemple pour illustrer la façon dont les zones sont créées et utilisées.</span><span class="sxs-lookup"><span data-stu-id="eea27-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="eea27-123">Supposons que vous avez une application de magasin avec deux regroupements distincts de contrôleurs et de vues : Produits et Services.</span><span class="sxs-lookup"><span data-stu-id="eea27-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="eea27-124">Voici une structure de dossiers classique pour ce type d’application avec des zones MVC :</span><span class="sxs-lookup"><span data-stu-id="eea27-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="eea27-125">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="eea27-125">Project name</span></span>

  * <span data-ttu-id="eea27-126">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="eea27-126">Areas</span></span>

    * <span data-ttu-id="eea27-127">Produits</span><span class="sxs-lookup"><span data-stu-id="eea27-127">Products</span></span>

      * <span data-ttu-id="eea27-128">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="eea27-128">Controllers</span></span>

        * <span data-ttu-id="eea27-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="eea27-129">HomeController.cs</span></span>

        * <span data-ttu-id="eea27-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="eea27-130">ManageController.cs</span></span>

      * <span data-ttu-id="eea27-131">Affichages</span><span class="sxs-lookup"><span data-stu-id="eea27-131">Views</span></span>

        * <span data-ttu-id="eea27-132">Accueil</span><span class="sxs-lookup"><span data-stu-id="eea27-132">Home</span></span>

          * <span data-ttu-id="eea27-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="eea27-133">Index.cshtml</span></span>

        * <span data-ttu-id="eea27-134">Gérer</span><span class="sxs-lookup"><span data-stu-id="eea27-134">Manage</span></span>

          * <span data-ttu-id="eea27-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="eea27-135">Index.cshtml</span></span>

    * <span data-ttu-id="eea27-136">Services</span><span class="sxs-lookup"><span data-stu-id="eea27-136">Services</span></span>

      * <span data-ttu-id="eea27-137">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="eea27-137">Controllers</span></span>

        * <span data-ttu-id="eea27-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="eea27-138">HomeController.cs</span></span>

      * <span data-ttu-id="eea27-139">Affichages</span><span class="sxs-lookup"><span data-stu-id="eea27-139">Views</span></span>

        * <span data-ttu-id="eea27-140">Accueil</span><span class="sxs-lookup"><span data-stu-id="eea27-140">Home</span></span>

          * <span data-ttu-id="eea27-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="eea27-141">Index.cshtml</span></span>

<span data-ttu-id="eea27-142">Quand MVC tente d’afficher une vue dans une zone, par défaut, il tente de chercher aux emplacements suivants :</span><span class="sxs-lookup"><span data-stu-id="eea27-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="eea27-143">Il s’agit des emplacements par défaut, qui peuvent être changés via `AreaViewLocationFormats` sur `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="eea27-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="eea27-144">Par exemple, dans le code ci-dessous, le nom de dossier « Areas » a été changé en « Categories ».</span><span class="sxs-lookup"><span data-stu-id="eea27-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="eea27-145">Une chose à noter est que la structure du dossier *Vues* est la seule qui est considérée comme importante ici, et que le contenu du reste des dossiers, comme *Contrôleurs* et *Modèles* **n’a pas** d’importance.</span><span class="sxs-lookup"><span data-stu-id="eea27-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="eea27-146">Par exemple, il n’est pas du tout nécessaire d’avoir des dossiers *Contrôleurs* et *Modèles*.</span><span class="sxs-lookup"><span data-stu-id="eea27-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="eea27-147">Ceci fonctionne, car le contenu de *Contrôleurs* et de *Modèles* est simplement du code qui est compilé dans un fichier .dll, alors que le contenu de *Vues* ne l’est pas tant qu’une demande n’est pas adressée à cette vue.</span><span class="sxs-lookup"><span data-stu-id="eea27-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="eea27-148">Une fois que vous avez défini la hiérarchie des dossiers, vous devez indiquer à MVC que chaque contrôleur est associé à une zone.</span><span class="sxs-lookup"><span data-stu-id="eea27-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="eea27-149">Vous faites cela en décorant le nom du contrôleur avec l’attribut `[Area]`.</span><span class="sxs-lookup"><span data-stu-id="eea27-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="eea27-150">Configurez une définition de route qui fonctionne avec vos zones nouvellement créées.</span><span class="sxs-lookup"><span data-stu-id="eea27-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="eea27-151">L’article [Router vers les actions du contrôleur](routing.md) explique en détail comment créer des définitions de route, notamment l’utilisation de routes conventionnelles par rapport aux routes d’attributs.</span><span class="sxs-lookup"><span data-stu-id="eea27-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="eea27-152">Dans cet exemple, nous allons utiliser une route conventionnelle.</span><span class="sxs-lookup"><span data-stu-id="eea27-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="eea27-153">Pour cela, ouvrez le fichier *Startup.cs* et modifiez-le en ajoutant la définition de route nommée `areaRoute` ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="eea27-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="eea27-154">Avec un accès à `http://<yourApp>/products`, la méthode d’action `Index` de `HomeController` dans la zone `Products` sera appelée.</span><span class="sxs-lookup"><span data-stu-id="eea27-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="eea27-155">Génération de liens</span><span class="sxs-lookup"><span data-stu-id="eea27-155">Link Generation</span></span>

* <span data-ttu-id="eea27-156">Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action au sein du même contrôleur</span><span class="sxs-lookup"><span data-stu-id="eea27-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="eea27-157">Supposons que le chemin de la requête actuelle soit `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="eea27-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="eea27-158">Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="eea27-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="eea27-159">Syntaxe de TagHelper : `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="eea27-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="eea27-160">Notez que nous ne devons pas fournir ici les valeurs « area » et « controller », car elles sont déjà disponibles dans le contexte de la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="eea27-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="eea27-161">Ces types de valeurs sont appelées valeurs `ambient`.</span><span class="sxs-lookup"><span data-stu-id="eea27-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="eea27-162">Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action sur un autre contrôleur</span><span class="sxs-lookup"><span data-stu-id="eea27-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="eea27-163">Supposons que le chemin de la requête actuelle soit `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="eea27-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="eea27-164">Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="eea27-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="eea27-165">Syntaxe de TagHelper : `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="eea27-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="eea27-166">Notez qu’ici la valeur ambiante d’une « area » (zone) est utilisée, mais que la valeur « controller » est spécifiée explicitement ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="eea27-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="eea27-167">Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action sur un autre contrôleur et une autre zone</span><span class="sxs-lookup"><span data-stu-id="eea27-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="eea27-168">Supposons que le chemin de la requête actuelle soit `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="eea27-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="eea27-169">Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="eea27-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="eea27-170">Syntaxe de TagHelper : `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="eea27-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="eea27-171">Notez qu’ici aucune valeur ambiante n’est utilisée.</span><span class="sxs-lookup"><span data-stu-id="eea27-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="eea27-172">Génération de liens depuis une action dans un contrôleur basé sur une zone vers une autre action sur un autre contrôleur mais **pas** dans une zone</span><span class="sxs-lookup"><span data-stu-id="eea27-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="eea27-173">Syntaxe de HtmlHelper : `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="eea27-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="eea27-174">Syntaxe de TagHelper : `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="eea27-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="eea27-175">Comme nous voulons générer des liens vers une action de contrôleur qui n’est pas basé sur une zone, nous vidons ici la valeur ambiante pour « area » (zone).</span><span class="sxs-lookup"><span data-stu-id="eea27-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="eea27-176">Zones de publication</span><span class="sxs-lookup"><span data-stu-id="eea27-176">Publishing Areas</span></span>

<span data-ttu-id="eea27-177">Tous les fichiers `*.cshtml` et `wwwroot/**` sont publiés en sortie quand `<Project Sdk="Microsoft.NET.Sdk.Web">` est inclus dans le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="eea27-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
