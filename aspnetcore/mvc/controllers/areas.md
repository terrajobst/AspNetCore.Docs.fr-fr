---
title: Zones dans ASP.NET Core
author: rick-anderson
description: Découvrez les zones, fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 8904d217a18fff65113ae3469efe60258d20d5f0
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400643"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="373cf-103">Zones dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="373cf-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="373cf-104">Par [Dhananjay Kumar](https://twitter.com/debug_mode) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="373cf-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="373cf-105">Les zones sont une fonctionnalité d’ASP.NET qui servent à organiser les fonctionnalités associées dans un groupe sous la forme d’un espace de noms (pour le routage) et d’une structure de dossiers (pour les vues) distincts.</span><span class="sxs-lookup"><span data-stu-id="373cf-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="373cf-106">L’utilisation de zones a pour effet de créer une hiérarchie pour les besoins du routage en ajoutant un autre paramètre de route, `area`, à `controller` et `action` ou une Razor Page `page`.</span><span class="sxs-lookup"><span data-stu-id="373cf-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="373cf-107">Les zones offrent un moyen de partitionner une application web ASP.NET Core en groupes fonctionnels plus petits, chacun avec son propre ensemble de Razor Pages, de contrôleurs, de vues et de modèles.</span><span class="sxs-lookup"><span data-stu-id="373cf-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="373cf-108">Une zone est en réalité une structure au sein d’une application.</span><span class="sxs-lookup"><span data-stu-id="373cf-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="373cf-109">Dans un projet web ASP.NET Core, les composants logiques comme Pages, Controller et View sont conservés dans différents dossiers.</span><span class="sxs-lookup"><span data-stu-id="373cf-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="373cf-110">Le runtime ASP.NET Core utilise des conventions de nommage pour créer la relation entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="373cf-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="373cf-111">Pour une application volumineuse, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de premier niveau.</span><span class="sxs-lookup"><span data-stu-id="373cf-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="373cf-112">Par exemple, pour une application d’e-commerce à plusieurs entités, il peut s’agir de la caisse, de la facturation et de la recherche.</span><span class="sxs-lookup"><span data-stu-id="373cf-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="373cf-113">Chacune de ces unités a sa propre zone pour accueillir les vues, les contrôleurs, les Razor Pages et les modèles.</span><span class="sxs-lookup"><span data-stu-id="373cf-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="373cf-114">Envisagez d’utiliser des zones dans un projet quand :</span><span class="sxs-lookup"><span data-stu-id="373cf-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="373cf-115">L’application est constituée de plusieurs composants fonctionnels globaux qui peuvent être logiquement séparés.</span><span class="sxs-lookup"><span data-stu-id="373cf-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="373cf-116">Vous pouvez partitionner l’application de façon à pouvoir travailler indépendamment sur chaque zone fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="373cf-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="373cf-117">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="373cf-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="373cf-118">L’exemple de code téléchargeable fournit une application de base pour tester les zones.</span><span class="sxs-lookup"><span data-stu-id="373cf-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="373cf-119">Si vous utilisez Razor Pages, consultez [Zones avec Razor Pages](#areas-with-razor-pages) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="373cf-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="373cf-120">Zones pour contrôleurs avec vues</span><span class="sxs-lookup"><span data-stu-id="373cf-120">Areas for controllers with views</span></span>

<span data-ttu-id="373cf-121">Une application web ASP.NET Core type qui utilise des zones, des contrôleurs et des vues est constituée des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="373cf-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="373cf-122">Une [structure de dossiers Zone](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="373cf-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="373cf-123">Des contrôleurs décorés avec l’attribut [&lbrack;Area&rbrack;](#attribute) pour associer le contrôleur à la zone : [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="373cf-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="373cf-124">La [route de zone ajoutée au démarrage](#add-area-route) : [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="373cf-124">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

### <a name="area-folder-structure"></a><span data-ttu-id="373cf-125">Structure de dossiers Zone</span><span class="sxs-lookup"><span data-stu-id="373cf-125">Area folder structure</span></span>
<span data-ttu-id="373cf-126">Imaginez une application qui contient deux groupes logiques, *Produits* et *Services*.</span><span class="sxs-lookup"><span data-stu-id="373cf-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="373cf-127">En utilisant des zones, la structure de dossiers se présenterait comme suit :</span><span class="sxs-lookup"><span data-stu-id="373cf-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="373cf-128">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="373cf-128">Project name</span></span>
  * <span data-ttu-id="373cf-129">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="373cf-129">Areas</span></span>
    * <span data-ttu-id="373cf-130">Produits</span><span class="sxs-lookup"><span data-stu-id="373cf-130">Products</span></span>
      * <span data-ttu-id="373cf-131">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="373cf-131">Controllers</span></span>
        * <span data-ttu-id="373cf-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="373cf-132">HomeController.cs</span></span>
        * <span data-ttu-id="373cf-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="373cf-133">ManageController.cs</span></span>
      * <span data-ttu-id="373cf-134">Affichages</span><span class="sxs-lookup"><span data-stu-id="373cf-134">Views</span></span>
        * <span data-ttu-id="373cf-135">Accueil</span><span class="sxs-lookup"><span data-stu-id="373cf-135">Home</span></span>
          * <span data-ttu-id="373cf-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="373cf-136">Index.cshtml</span></span>
        * <span data-ttu-id="373cf-137">Gérer</span><span class="sxs-lookup"><span data-stu-id="373cf-137">Manage</span></span>
          * <span data-ttu-id="373cf-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="373cf-138">Index.cshtml</span></span>
          * <span data-ttu-id="373cf-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="373cf-139">About.cshtml</span></span>
    * <span data-ttu-id="373cf-140">Services</span><span class="sxs-lookup"><span data-stu-id="373cf-140">Services</span></span>
      * <span data-ttu-id="373cf-141">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="373cf-141">Controllers</span></span>
        * <span data-ttu-id="373cf-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="373cf-142">HomeController.cs</span></span>
      * <span data-ttu-id="373cf-143">Affichages</span><span class="sxs-lookup"><span data-stu-id="373cf-143">Views</span></span>
        * <span data-ttu-id="373cf-144">Accueil</span><span class="sxs-lookup"><span data-stu-id="373cf-144">Home</span></span>
          * <span data-ttu-id="373cf-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="373cf-145">Index.cshtml</span></span>

<span data-ttu-id="373cf-146">Si la disposition précédente est classique dans les cas où des zones sont utilisées, seuls les fichiers de vues sont nécessaires pour utiliser cette structure de dossiers.</span><span class="sxs-lookup"><span data-stu-id="373cf-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="373cf-147">La détection de vues recherche un fichier de vue de zone correspondant dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="373cf-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="373cf-148">L’emplacement des dossiers autres que Vues comme *Contrôleurs* et *Modèles* **n’a pas** d’importance.</span><span class="sxs-lookup"><span data-stu-id="373cf-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="373cf-149">Par exemple, les dossiers *Contrôleurs* et *Modèles* ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="373cf-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="373cf-150">Le contenu de *Contrôleurs* et *Modèles* est constitué de code qui est compilé dans un fichier .dll.</span><span class="sxs-lookup"><span data-stu-id="373cf-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="373cf-151">Le contenu de *Vues* n’est pas compilée tant qu’aucune demande n’a été émise pour la vue en question.</span><span class="sxs-lookup"><span data-stu-id="373cf-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="373cf-152">Associer le contrôleur à une zone</span><span class="sxs-lookup"><span data-stu-id="373cf-152">Associate the controller with an Area</span></span>

<span data-ttu-id="373cf-153">Les contrôleurs de zone sont désignés à l’aide de l’attribut [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) :</span><span class="sxs-lookup"><span data-stu-id="373cf-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="373cf-154">Ajouter une route de zone</span><span class="sxs-lookup"><span data-stu-id="373cf-154">Add Area route</span></span>

<span data-ttu-id="373cf-155">Les routes de zones utilisent généralement un routage conventionnel plutôt qu’un routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="373cf-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="373cf-156">Le routage conventionnel est dépendant de l’ordre.</span><span class="sxs-lookup"><span data-stu-id="373cf-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="373cf-157">En général, les routes avec des zones doivent être placées plus haut dans la table de routage, car elles sont plus spécifiques que les routes sans zone.</span><span class="sxs-lookup"><span data-stu-id="373cf-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="373cf-158">`{area:...}` peut être utilisé comme jeton dans les modèles de route si l’espace d’url est uniforme dans toutes les zones :</span><span class="sxs-lookup"><span data-stu-id="373cf-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="373cf-159">Dans le code précédent, `exists` applique une contrainte qui impose à la route de correspondre à une zone.</span><span class="sxs-lookup"><span data-stu-id="373cf-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="373cf-160">Pour ajouter un routage à des zones, le mécanisme le moins compliqué consiste à utiliser `{area:...}`.</span><span class="sxs-lookup"><span data-stu-id="373cf-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="373cf-161">Le code suivant utilise <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> pour créer deux routes de zone nommées :</span><span class="sxs-lookup"><span data-stu-id="373cf-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="373cf-162">Si vous utilisez `MapAreaRoute` avec ASP.NET Core 2.2, prenez connaissance de [ce problème sur GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="373cf-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="373cf-163">Pour plus d’informations, consultez [Routage de zones](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="373cf-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="373cf-164">Génération de liens avec zones MVC</span><span class="sxs-lookup"><span data-stu-id="373cf-164">Link generation with MVC areas</span></span>

<span data-ttu-id="373cf-165">Le code suivant tiré de l’[exemple de code téléchargeable](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) illustre une génération de liens avec la zone spécifiée :</span><span class="sxs-lookup"><span data-stu-id="373cf-165">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="373cf-166">Les liens générés par le code précédent sont valides où que ce soit dans l’application.</span><span class="sxs-lookup"><span data-stu-id="373cf-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="373cf-167">L’exemple de code téléchargeable comprend une [vue partielle](xref:mvc/views/partial) qui contient les liens précédents et les mêmes liens sans spécification de la zone.</span><span class="sxs-lookup"><span data-stu-id="373cf-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="373cf-168">La vue partielle étant référencée dans le [fichier de disposition](xref:mvc/views/layout), chaque page de l’application affiche les liens générés.</span><span class="sxs-lookup"><span data-stu-id="373cf-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="373cf-169">Les liens générés sans spécification de la zone sont valides uniquement quand ils sont référencés dans une page contenue dans la même zone et le même contrôleur.</span><span class="sxs-lookup"><span data-stu-id="373cf-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="373cf-170">Quand la zone ou le contrôleur n’est pas spécifié, le routage dépend des valeurs *ambiantes*.</span><span class="sxs-lookup"><span data-stu-id="373cf-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="373cf-171">Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens.</span><span class="sxs-lookup"><span data-stu-id="373cf-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="373cf-172">Dans de nombreux cas, pour l’exemple d’application, l’utilisation des valeurs ambiantes génère des liens incorrects.</span><span class="sxs-lookup"><span data-stu-id="373cf-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="373cf-173">Pour plus d’informations, consultez [Routage vers des actions de contrôleur](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="373cf-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="373cf-174">Disposition partagée pour les zones en utilisant le fichier _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="373cf-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="373cf-175">Pour partager une disposition commune pour l’ensemble de l’application, déplacez *_ViewStart.cshtml* dans le dossier racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="373cf-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="373cf-176">Changer le dossier de zone par défaut où sont stockées les vues</span><span class="sxs-lookup"><span data-stu-id="373cf-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="373cf-177">Le code suivant remplace le dossier de zone par défaut `"Areas"` par `"MyAreas"` :</span><span class="sxs-lookup"><span data-stu-id="373cf-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="373cf-178">Zones avec Razor Pages</span><span class="sxs-lookup"><span data-stu-id="373cf-178">Areas with Razor Pages</span></span>

<span data-ttu-id="373cf-179">Les zones avec Razor Pages requièrent un dossier *Areas/&lt;nom de zone&gt;/Pages* à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="373cf-179">Areas with Razor Pages require and *Areas/&lt;area name&gt;/Pages* folder in the root of the app.</span></span> <span data-ttu-id="373cf-180">La structure de dossiers suivante est utilisée avec le [téléchargement de l’exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)</span><span class="sxs-lookup"><span data-stu-id="373cf-180">The following folder structure is used with the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)</span></span>

* <span data-ttu-id="373cf-181">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="373cf-181">Project name</span></span>
  * <span data-ttu-id="373cf-182">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="373cf-182">Areas</span></span>
    * <span data-ttu-id="373cf-183">Produits</span><span class="sxs-lookup"><span data-stu-id="373cf-183">Products</span></span>
      * <span data-ttu-id="373cf-184">Pages</span><span class="sxs-lookup"><span data-stu-id="373cf-184">Pages</span></span>
        * <span data-ttu-id="373cf-185">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="373cf-185">_ViewImports</span></span>
        * <span data-ttu-id="373cf-186">À propos de</span><span class="sxs-lookup"><span data-stu-id="373cf-186">About</span></span>
        * <span data-ttu-id="373cf-187">Index</span><span class="sxs-lookup"><span data-stu-id="373cf-187">Index</span></span>
    * <span data-ttu-id="373cf-188">Services</span><span class="sxs-lookup"><span data-stu-id="373cf-188">Services</span></span>
      * <span data-ttu-id="373cf-189">Pages</span><span class="sxs-lookup"><span data-stu-id="373cf-189">Pages</span></span>
        * <span data-ttu-id="373cf-190">Gérer</span><span class="sxs-lookup"><span data-stu-id="373cf-190">Manage</span></span>
          * <span data-ttu-id="373cf-191">À propos de</span><span class="sxs-lookup"><span data-stu-id="373cf-191">About</span></span>
          * <span data-ttu-id="373cf-192">Index</span><span class="sxs-lookup"><span data-stu-id="373cf-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="373cf-193">Génération de liens avec Razor Pages et des zones</span><span class="sxs-lookup"><span data-stu-id="373cf-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="373cf-194">Le code suivant tiré de [l’exemple de code téléchargeable](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) illustre une génération de liens avec la zone spécifiée (par exemple, `asp-area="Products"`) :</span><span class="sxs-lookup"><span data-stu-id="373cf-194">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="373cf-195">Les liens générés par le code précédent sont valides où que ce soit dans l’application.</span><span class="sxs-lookup"><span data-stu-id="373cf-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="373cf-196">L’exemple de code téléchargeable comprend une [vue partielle](xref:mvc/views/partial) qui contient les liens précédents et les mêmes liens sans spécification de la zone.</span><span class="sxs-lookup"><span data-stu-id="373cf-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="373cf-197">La vue partielle étant référencée dans le [fichier de disposition](xref:mvc/views/layout), chaque page de l’application affiche les liens générés.</span><span class="sxs-lookup"><span data-stu-id="373cf-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="373cf-198">Les liens générés sans spécification de la zone sont valides uniquement quand ils sont référencés dans une page contenue dans la même zone.</span><span class="sxs-lookup"><span data-stu-id="373cf-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="373cf-199">Quand la zone n’est pas spécifiée, le routage dépend des valeurs *ambiantes*.</span><span class="sxs-lookup"><span data-stu-id="373cf-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="373cf-200">Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens.</span><span class="sxs-lookup"><span data-stu-id="373cf-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="373cf-201">Dans de nombreux cas, pour l’exemple d’application, l’utilisation des valeurs ambiantes génère des liens incorrects.</span><span class="sxs-lookup"><span data-stu-id="373cf-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="373cf-202">Prenons l’exemple des liens générés à partir de l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="373cf-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="373cf-203">Pour le code précédent :</span><span class="sxs-lookup"><span data-stu-id="373cf-203">For the preceding code:</span></span>

* <span data-ttu-id="373cf-204">Le lien généré à partir de `<a asp-page="/Manage/About">` est correct uniquement lorsque la dernière requête concernait une page dans la zone `Services`.</span><span class="sxs-lookup"><span data-stu-id="373cf-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="373cf-205">Par exemple, `/Services/Manage/`, `/Services/Manage/Index` ou `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="373cf-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="373cf-206">Le lien généré à partir de `<a asp-page="/About">` est correct uniquement lorsque la dernière requête concernait une page dans `/Home`.</span><span class="sxs-lookup"><span data-stu-id="373cf-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="373cf-207">Le code est issu du [téléchargement de l’exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="373cf-207">The code is from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-viewimports-file"></a><span data-ttu-id="373cf-208">Importer l’espace de noms et les Tag Helpers avec le fichier _ViewImports</span><span class="sxs-lookup"><span data-stu-id="373cf-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="373cf-209">Un fichier *_ViewImports* peut être ajouté à chaque dossier *Pages* de la zone pour importer l’espace de noms et les Tag Helpers dans chaque Razor Page du dossier.</span><span class="sxs-lookup"><span data-stu-id="373cf-209">A *_ViewImports* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="373cf-210">Observez la zone *Services* de l’exemple de code, qui ne contient pas de fichier *_ViewImports*.</span><span class="sxs-lookup"><span data-stu-id="373cf-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports* file.</span></span> <span data-ttu-id="373cf-211">Le balisage suivant montre la Razor Page */Services/Manage/About* :</span><span class="sxs-lookup"><span data-stu-id="373cf-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="373cf-212">Dans le balisage précédent :</span><span class="sxs-lookup"><span data-stu-id="373cf-212">In the preceding markup:</span></span>

* <span data-ttu-id="373cf-213">Le nom de domaine complet doit être utilisé pour spécifier le modèle (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="373cf-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="373cf-214">Les [Tag Helpers]() sont activés par `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="373cf-214">[Tag Helpers]() are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="373cf-215">Dans le téléchargement de l’exemple, la zone Products contient le fichier *_ViewImports* suivant :</span><span class="sxs-lookup"><span data-stu-id="373cf-215">In the sample download, the Products area contains the following *_ViewImports* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="373cf-216">Le balisage suivant montre la Razor Page */Products/About* : [!code-cshtml [](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="373cf-216">The following markup shows the */Products/About* Razor Page: [!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]</span></span>

<span data-ttu-id="373cf-217">Dans le fichier précédent, l’espace de noms et la directive `@addTagHelper` sont importés dans le fichier par le fichier *Areas/Products/Pages/_ViewImports.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="373cf-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="373cf-218">Pour plus d’informations, consultez [Gestion de l’étendue des Tag Helpers](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) et [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="373cf-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="373cf-219">Disposition partagée pour les zones Razor Pages</span><span class="sxs-lookup"><span data-stu-id="373cf-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="373cf-220">Pour partager une disposition commune pour l’ensemble de l’application, déplacez *_ViewStart.cshtml* dans le dossier racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="373cf-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="373cf-221">Zones de publication</span><span class="sxs-lookup"><span data-stu-id="373cf-221">Publishing Areas</span></span>

<span data-ttu-id="373cf-222">Tous les fichiers `*.cshtml` et `wwwroot/**` sont publiés en sortie quand `<Project Sdk="Microsoft.NET.Sdk.Web">` est inclus dans le fichier .csproj\*.</span><span class="sxs-lookup"><span data-stu-id="373cf-222">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the.csproj\* file.</span></span>