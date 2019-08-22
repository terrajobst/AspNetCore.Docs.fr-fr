---
title: Zones dans ASP.NET Core
author: rick-anderson
description: Découvrez les zones, fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).
ms.author: riande
ms.date: 08/16/2019
uid: mvc/controllers/areas
ms.openlocfilehash: d0af3092776ee09469c879fffd3047c50b1a59b4
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545809"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="5e551-103">Zones dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e551-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="5e551-104">Par [Dhananjay Kumar](https://twitter.com/debug_mode) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5e551-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5e551-105">Les zones sont une fonctionnalité d’ASP.NET qui servent à organiser les fonctionnalités associées dans un groupe sous la forme d’un espace de noms (pour le routage) et d’une structure de dossiers (pour les vues) distincts.</span><span class="sxs-lookup"><span data-stu-id="5e551-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="5e551-106">L’utilisation de zones a pour effet de créer une hiérarchie pour les besoins du routage en ajoutant un autre paramètre de route, `area`, à `controller` et `action` ou une Razor Page `page`.</span><span class="sxs-lookup"><span data-stu-id="5e551-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="5e551-107">Les zones offrent un moyen de partitionner une application web ASP.NET Core en groupes fonctionnels plus petits, chacun avec son propre ensemble de Razor Pages, de contrôleurs, de vues et de modèles.</span><span class="sxs-lookup"><span data-stu-id="5e551-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="5e551-108">Une zone est en réalité une structure au sein d’une application.</span><span class="sxs-lookup"><span data-stu-id="5e551-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="5e551-109">Dans un projet web ASP.NET Core, les composants logiques comme Pages, Controller et View sont conservés dans différents dossiers.</span><span class="sxs-lookup"><span data-stu-id="5e551-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="5e551-110">Le runtime ASP.NET Core utilise des conventions de nommage pour créer la relation entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="5e551-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="5e551-111">Pour une application volumineuse, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de premier niveau.</span><span class="sxs-lookup"><span data-stu-id="5e551-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="5e551-112">Par exemple, pour une application d’e-commerce à plusieurs entités, il peut s’agir de la caisse, de la facturation et de la recherche.</span><span class="sxs-lookup"><span data-stu-id="5e551-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="5e551-113">Chacune de ces unités a sa propre zone pour accueillir les vues, les contrôleurs, les Razor Pages et les modèles.</span><span class="sxs-lookup"><span data-stu-id="5e551-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="5e551-114">Envisagez d’utiliser des zones dans un projet quand :</span><span class="sxs-lookup"><span data-stu-id="5e551-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="5e551-115">L’application est constituée de plusieurs composants fonctionnels globaux qui peuvent être logiquement séparés.</span><span class="sxs-lookup"><span data-stu-id="5e551-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="5e551-116">Vous pouvez partitionner l’application de façon à pouvoir travailler indépendamment sur chaque zone fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="5e551-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="5e551-117">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5e551-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5e551-118">L’exemple de code téléchargeable fournit une application de base pour tester les zones.</span><span class="sxs-lookup"><span data-stu-id="5e551-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="5e551-119">Si vous utilisez Razor Pages, consultez [Zones avec Razor Pages](#areas-with-razor-pages) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="5e551-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="5e551-120">Zones pour contrôleurs avec vues</span><span class="sxs-lookup"><span data-stu-id="5e551-120">Areas for controllers with views</span></span>

<span data-ttu-id="5e551-121">Une application web ASP.NET Core type qui utilise des zones, des contrôleurs et des vues est constituée des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="5e551-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="5e551-122">Une [structure de dossiers Zone](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="5e551-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="5e551-123">Des contrôleurs décorés avec l’attribut [&lbrack;Area&rbrack;](#attribute) pour associer le contrôleur à la zone :</span><span class="sxs-lookup"><span data-stu-id="5e551-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="5e551-124">La [route de zone ajoutée au démarrage](#add-area-route) :</span><span class="sxs-lookup"><span data-stu-id="5e551-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="5e551-125">Structure de dossiers Zone</span><span class="sxs-lookup"><span data-stu-id="5e551-125">Area folder structure</span></span>

<span data-ttu-id="5e551-126">Imaginez une application qui contient deux groupes logiques, *Produits* et *Services*.</span><span class="sxs-lookup"><span data-stu-id="5e551-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="5e551-127">En utilisant des zones, la structure de dossiers se présenterait comme suit :</span><span class="sxs-lookup"><span data-stu-id="5e551-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="5e551-128">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="5e551-128">Project name</span></span>
  * <span data-ttu-id="5e551-129">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="5e551-129">Areas</span></span>
    * <span data-ttu-id="5e551-130">Produits</span><span class="sxs-lookup"><span data-stu-id="5e551-130">Products</span></span>
      * <span data-ttu-id="5e551-131">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="5e551-131">Controllers</span></span>
        * <span data-ttu-id="5e551-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="5e551-132">HomeController.cs</span></span>
        * <span data-ttu-id="5e551-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="5e551-133">ManageController.cs</span></span>
      * <span data-ttu-id="5e551-134">Affichages</span><span class="sxs-lookup"><span data-stu-id="5e551-134">Views</span></span>
        * <span data-ttu-id="5e551-135">Accueil</span><span class="sxs-lookup"><span data-stu-id="5e551-135">Home</span></span>
          * <span data-ttu-id="5e551-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5e551-136">Index.cshtml</span></span>
        * <span data-ttu-id="5e551-137">Gérer</span><span class="sxs-lookup"><span data-stu-id="5e551-137">Manage</span></span>
          * <span data-ttu-id="5e551-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5e551-138">Index.cshtml</span></span>
          * <span data-ttu-id="5e551-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="5e551-139">About.cshtml</span></span>
    * <span data-ttu-id="5e551-140">Services</span><span class="sxs-lookup"><span data-stu-id="5e551-140">Services</span></span>
      * <span data-ttu-id="5e551-141">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="5e551-141">Controllers</span></span>
        * <span data-ttu-id="5e551-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="5e551-142">HomeController.cs</span></span>
      * <span data-ttu-id="5e551-143">Affichages</span><span class="sxs-lookup"><span data-stu-id="5e551-143">Views</span></span>
        * <span data-ttu-id="5e551-144">Accueil</span><span class="sxs-lookup"><span data-stu-id="5e551-144">Home</span></span>
          * <span data-ttu-id="5e551-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5e551-145">Index.cshtml</span></span>

<span data-ttu-id="5e551-146">Si la disposition précédente est classique dans les cas où des zones sont utilisées, seuls les fichiers de vues sont nécessaires pour utiliser cette structure de dossiers.</span><span class="sxs-lookup"><span data-stu-id="5e551-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="5e551-147">La détection de vues recherche un fichier de vue de zone correspondant dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="5e551-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="5e551-148">L’emplacement des dossiers autres que Vues comme *Contrôleurs* et *Modèles* **n’a pas** d’importance.</span><span class="sxs-lookup"><span data-stu-id="5e551-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="5e551-149">Par exemple, les dossiers *Contrôleurs* et *Modèles* ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="5e551-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="5e551-150">Le contenu de *Contrôleurs* et *Modèles* est constitué de code qui est compilé dans un fichier .dll.</span><span class="sxs-lookup"><span data-stu-id="5e551-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="5e551-151">Le contenu de *Vues* n’est pas compilée tant qu’aucune demande n’a été émise pour la vue en question.</span><span class="sxs-lookup"><span data-stu-id="5e551-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="5e551-152">Associer le contrôleur à une zone</span><span class="sxs-lookup"><span data-stu-id="5e551-152">Associate the controller with an Area</span></span>

<span data-ttu-id="5e551-153">Les contrôleurs de zone sont désignés à l’aide de l’attribut [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) :</span><span class="sxs-lookup"><span data-stu-id="5e551-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="5e551-154">Ajouter une route de zone</span><span class="sxs-lookup"><span data-stu-id="5e551-154">Add Area route</span></span>

<span data-ttu-id="5e551-155">Les routes de zones utilisent généralement un routage conventionnel plutôt qu’un routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="5e551-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="5e551-156">Le routage conventionnel est dépendant de l’ordre.</span><span class="sxs-lookup"><span data-stu-id="5e551-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="5e551-157">En général, les routes avec des zones doivent être placées plus haut dans la table de routage, car elles sont plus spécifiques que les routes sans zone.</span><span class="sxs-lookup"><span data-stu-id="5e551-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="5e551-158">`{area:...}` peut être utilisé comme jeton dans les modèles de route si l’espace d’url est uniforme dans toutes les zones :</span><span class="sxs-lookup"><span data-stu-id="5e551-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="5e551-159">Dans le code précédent, `exists` applique une contrainte qui impose à la route de correspondre à une zone.</span><span class="sxs-lookup"><span data-stu-id="5e551-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="5e551-160">Pour ajouter un routage à des zones, le mécanisme le moins compliqué consiste à utiliser `{area:...}`.</span><span class="sxs-lookup"><span data-stu-id="5e551-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="5e551-161">Le code suivant utilise <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> pour créer deux routes de zone nommées :</span><span class="sxs-lookup"><span data-stu-id="5e551-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="5e551-162">Si vous utilisez `MapAreaRoute` avec ASP.NET Core 2.2, prenez connaissance de [ce problème sur GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="5e551-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="5e551-163">Pour plus d’informations, consultez [Routage de zones](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="5e551-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="5e551-164">Génération de liens avec zones MVC</span><span class="sxs-lookup"><span data-stu-id="5e551-164">Link generation with MVC areas</span></span>

<span data-ttu-id="5e551-165">Le code suivant tiré de l’[exemple de code téléchargeable](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) illustre une génération de liens avec la zone spécifiée :</span><span class="sxs-lookup"><span data-stu-id="5e551-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="5e551-166">Les liens générés par le code précédent sont valides où que ce soit dans l’application.</span><span class="sxs-lookup"><span data-stu-id="5e551-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="5e551-167">L’exemple de code téléchargeable comprend une [vue partielle](xref:mvc/views/partial) qui contient les liens précédents et les mêmes liens sans spécification de la zone.</span><span class="sxs-lookup"><span data-stu-id="5e551-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="5e551-168">La vue partielle étant référencée dans le [fichier de disposition](xref:mvc/views/layout), chaque page de l’application affiche les liens générés.</span><span class="sxs-lookup"><span data-stu-id="5e551-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="5e551-169">Les liens générés sans spécification de la zone sont valides uniquement quand ils sont référencés dans une page contenue dans la même zone et le même contrôleur.</span><span class="sxs-lookup"><span data-stu-id="5e551-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="5e551-170">Quand la zone ou le contrôleur n’est pas spécifié, le routage dépend des valeurs *ambiantes*.</span><span class="sxs-lookup"><span data-stu-id="5e551-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="5e551-171">Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens.</span><span class="sxs-lookup"><span data-stu-id="5e551-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="5e551-172">Dans de nombreux cas, pour l’exemple d’application, l’utilisation des valeurs ambiantes génère des liens incorrects.</span><span class="sxs-lookup"><span data-stu-id="5e551-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="5e551-173">Pour plus d’informations, consultez [Routage vers des actions de contrôleur](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="5e551-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="5e551-174">Disposition partagée pour les zones en utilisant le fichier _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="5e551-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="5e551-175">Pour partager une disposition commune pour l’ensemble de l’application, déplacez *_ViewStart.cshtml* dans le dossier racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e551-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="5e551-176">_ViewImports.cshtml</span><span class="sxs-lookup"><span data-stu-id="5e551-176">_ViewImports.cshtml</span></span>

<span data-ttu-id="5e551-177">Dans son emplacement standard, */Views/_ViewImports.cshtml* ne s’applique pas aux zones.</span><span class="sxs-lookup"><span data-stu-id="5e551-177">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="5e551-178">Pour utiliser des [Tag Helpers](xref:mvc/views/tag-helpers/intro) courants, `@using` ou `@inject` dans votre zone, vérifiez qu’un fichier *_ViewImports.cshtml* correct [s’applique à vos vues de zone](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="5e551-178">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="5e551-179">Si vous souhaitez obtenir le même comportement dans toutes vos vues, déplacez */Views/_ViewImports.cshtml* vers la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e551-179">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="5e551-180">Changer le dossier de zone par défaut où sont stockées les vues</span><span class="sxs-lookup"><span data-stu-id="5e551-180">Change default area folder where views are stored</span></span>

<span data-ttu-id="5e551-181">Le code suivant remplace le dossier de zone par défaut `"Areas"` par `"MyAreas"` :</span><span class="sxs-lookup"><span data-stu-id="5e551-181">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="5e551-182">Zones avec Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e551-182">Areas with Razor Pages</span></span>

<span data-ttu-id="5e551-183">Les zones avec Razor Pages requièrent un dossier *Areas/<area name>/Pages* à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e551-183">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="5e551-184">La structure de dossiers suivante est utilisée avec [l’exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) :</span><span class="sxs-lookup"><span data-stu-id="5e551-184">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="5e551-185">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="5e551-185">Project name</span></span>
  * <span data-ttu-id="5e551-186">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="5e551-186">Areas</span></span>
    * <span data-ttu-id="5e551-187">Produits</span><span class="sxs-lookup"><span data-stu-id="5e551-187">Products</span></span>
      * <span data-ttu-id="5e551-188">Pages</span><span class="sxs-lookup"><span data-stu-id="5e551-188">Pages</span></span>
        * <span data-ttu-id="5e551-189">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="5e551-189">_ViewImports</span></span>
        * <span data-ttu-id="5e551-190">À propos de</span><span class="sxs-lookup"><span data-stu-id="5e551-190">About</span></span>
        * <span data-ttu-id="5e551-191">Index</span><span class="sxs-lookup"><span data-stu-id="5e551-191">Index</span></span>
    * <span data-ttu-id="5e551-192">Services</span><span class="sxs-lookup"><span data-stu-id="5e551-192">Services</span></span>
      * <span data-ttu-id="5e551-193">Pages</span><span class="sxs-lookup"><span data-stu-id="5e551-193">Pages</span></span>
        * <span data-ttu-id="5e551-194">Gérer</span><span class="sxs-lookup"><span data-stu-id="5e551-194">Manage</span></span>
          * <span data-ttu-id="5e551-195">À propos de</span><span class="sxs-lookup"><span data-stu-id="5e551-195">About</span></span>
          * <span data-ttu-id="5e551-196">Index</span><span class="sxs-lookup"><span data-stu-id="5e551-196">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="5e551-197">Génération de liens avec Razor Pages et des zones</span><span class="sxs-lookup"><span data-stu-id="5e551-197">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="5e551-198">Le code suivant tiré de [l’exemple de code téléchargeable](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) illustre une génération de liens avec la zone spécifiée (par exemple, `asp-area="Products"`) :</span><span class="sxs-lookup"><span data-stu-id="5e551-198">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="5e551-199">Les liens générés par le code précédent sont valides où que ce soit dans l’application.</span><span class="sxs-lookup"><span data-stu-id="5e551-199">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="5e551-200">L’exemple de code téléchargeable comprend une [vue partielle](xref:mvc/views/partial) qui contient les liens précédents et les mêmes liens sans spécification de la zone.</span><span class="sxs-lookup"><span data-stu-id="5e551-200">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="5e551-201">La vue partielle étant référencée dans le [fichier de disposition](xref:mvc/views/layout), chaque page de l’application affiche les liens générés.</span><span class="sxs-lookup"><span data-stu-id="5e551-201">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="5e551-202">Les liens générés sans spécification de la zone sont valides uniquement quand ils sont référencés dans une page contenue dans la même zone.</span><span class="sxs-lookup"><span data-stu-id="5e551-202">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="5e551-203">Quand la zone n’est pas spécifiée, le routage dépend des valeurs *ambiantes*.</span><span class="sxs-lookup"><span data-stu-id="5e551-203">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="5e551-204">Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens.</span><span class="sxs-lookup"><span data-stu-id="5e551-204">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="5e551-205">Dans de nombreux cas, pour l’exemple d’application, l’utilisation des valeurs ambiantes génère des liens incorrects.</span><span class="sxs-lookup"><span data-stu-id="5e551-205">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="5e551-206">Prenons l’exemple des liens générés à partir de l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="5e551-206">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="5e551-207">Pour le code précédent :</span><span class="sxs-lookup"><span data-stu-id="5e551-207">For the preceding code:</span></span>

* <span data-ttu-id="5e551-208">Le lien généré à partir de `<a asp-page="/Manage/About">` est correct uniquement lorsque la dernière requête concernait une page dans la zone `Services`.</span><span class="sxs-lookup"><span data-stu-id="5e551-208">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="5e551-209">Par exemple, `/Services/Manage/`, `/Services/Manage/Index` ou `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="5e551-209">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="5e551-210">Le lien généré à partir de `<a asp-page="/About">` est correct uniquement lorsque la dernière requête concernait une page dans `/Home`.</span><span class="sxs-lookup"><span data-stu-id="5e551-210">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="5e551-211">Le code est issu du [téléchargement de l’exemple](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="5e551-211">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="5e551-212">Importer l’espace de noms et les Tag Helpers avec le fichier _ViewImports</span><span class="sxs-lookup"><span data-stu-id="5e551-212">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="5e551-213">Un fichier *_ViewImports.cshtml* peut être ajouté à chaque dossier *Pages* pour importer l’espace de noms et des Tag Helpers à chaque page Razor dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="5e551-213">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="5e551-214">Considérez la zone *Services* de l’exemple de code, qui ne contient pas de fichier *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5e551-214">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="5e551-215">Le balisage suivant montre la Razor Page */Services/Manage/About* :</span><span class="sxs-lookup"><span data-stu-id="5e551-215">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="5e551-216">Dans le balisage précédent :</span><span class="sxs-lookup"><span data-stu-id="5e551-216">In the preceding markup:</span></span>

* <span data-ttu-id="5e551-217">Le nom de domaine complet doit être utilisé pour spécifier le modèle (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="5e551-217">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="5e551-218">Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) sont activés par `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="5e551-218">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="5e551-219">Dans l’exemple de téléchargement, la zone Products contient le fichier *_ViewImports.cshtml* suivant :</span><span class="sxs-lookup"><span data-stu-id="5e551-219">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="5e551-220">Le balisage suivant montre Razor Page */Products/About* :</span><span class="sxs-lookup"><span data-stu-id="5e551-220">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="5e551-221">Dans le fichier précédent, l’espace de noms et la directive `@addTagHelper` sont importés dans le fichier par le fichier *Areas/Products/Pages/_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5e551-221">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="5e551-222">Pour plus d’informations, consultez [Gestion de l’étendue des Tag Helpers](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) et [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="5e551-222">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="5e551-223">Disposition partagée pour les zones Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5e551-223">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="5e551-224">Pour partager une disposition commune pour l’ensemble de l’application, déplacez *_ViewStart.cshtml* dans le dossier racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e551-224">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="5e551-225">Zones de publication</span><span class="sxs-lookup"><span data-stu-id="5e551-225">Publishing Areas</span></span>

<span data-ttu-id="5e551-226">Tous les fichiers \*.cshtml et autres fichiers dans le répertoire *wwwroot* sont publiés en sortie quand `<Project Sdk="Microsoft.NET.Sdk.Web">` est inclus dans le fichier \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="5e551-226">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
