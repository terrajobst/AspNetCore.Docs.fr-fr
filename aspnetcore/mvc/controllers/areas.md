---
title: Zones dans ASP.NET Core
author: rick-anderson
description: Découvrez les zones, fonctionnalité d’ASP.NET MVC utilisée pour organiser des fonctionnalités connexes dans un groupe sous la forme d’un espace de noms distinct (pour le routage) et d’une structure de dossiers (pour les vues).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833525"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="7cac6-103">Zones dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7cac6-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="7cac6-104">Par [Dhananjay Kumar](https://twitter.com/debug_mode) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7cac6-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7cac6-105">Les zones sont une fonctionnalité d’ASP.NET qui servent à organiser les fonctionnalités associées dans un groupe sous la forme d’un espace de noms (pour le routage) et d’une structure de dossiers (pour les vues) distincts.</span><span class="sxs-lookup"><span data-stu-id="7cac6-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="7cac6-106">L’utilisation de zones a pour effet de créer une hiérarchie pour les besoins du routage en ajoutant un autre paramètre de route, `area`, à `controller` et `action` ou une Razor Page `page`.</span><span class="sxs-lookup"><span data-stu-id="7cac6-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="7cac6-107">Les zones offrent un moyen de partitionner une application web ASP.NET Core en groupes fonctionnels plus petits, chacun avec son propre ensemble de Razor Pages, de contrôleurs, de vues et de modèles.</span><span class="sxs-lookup"><span data-stu-id="7cac6-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="7cac6-108">Une zone est en réalité une structure au sein d’une application.</span><span class="sxs-lookup"><span data-stu-id="7cac6-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="7cac6-109">Dans un projet web ASP.NET Core, les composants logiques comme Pages, Controller et View sont conservés dans différents dossiers.</span><span class="sxs-lookup"><span data-stu-id="7cac6-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="7cac6-110">Le runtime ASP.NET Core utilise des conventions de nommage pour créer la relation entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="7cac6-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="7cac6-111">Pour une application volumineuse, il peut être avantageux de partitionner l’application en différentes zones de fonctionnalités de premier niveau.</span><span class="sxs-lookup"><span data-stu-id="7cac6-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="7cac6-112">Par exemple, pour une application d’e-commerce à plusieurs entités, il peut s’agir de la caisse, de la facturation et de la recherche.</span><span class="sxs-lookup"><span data-stu-id="7cac6-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="7cac6-113">Chacune de ces unités a sa propre zone pour accueillir les vues, les contrôleurs, les Razor Pages et les modèles.</span><span class="sxs-lookup"><span data-stu-id="7cac6-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="7cac6-114">Envisagez d’utiliser des zones dans un projet quand :</span><span class="sxs-lookup"><span data-stu-id="7cac6-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="7cac6-115">L’application est constituée de plusieurs composants fonctionnels globaux qui peuvent être logiquement séparés.</span><span class="sxs-lookup"><span data-stu-id="7cac6-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="7cac6-116">Vous pouvez partitionner l’application de façon à pouvoir travailler indépendamment sur chaque zone fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="7cac6-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="7cac6-117">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7cac6-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="7cac6-118">L’exemple de code téléchargeable fournit une application de base pour tester les zones.</span><span class="sxs-lookup"><span data-stu-id="7cac6-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="7cac6-119">Zones pour contrôleurs avec vues</span><span class="sxs-lookup"><span data-stu-id="7cac6-119">Areas for controllers with views</span></span>

<span data-ttu-id="7cac6-120">Une application web ASP.NET Core type qui utilise des zones, des contrôleurs et des vues est constituée des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="7cac6-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="7cac6-121">Une [structure de dossiers Zone](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="7cac6-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="7cac6-122">Des contrôleurs décorés avec l’attribut [&lbrack;Area&rbrack;](#attribute) pour associer le contrôleur à la zone : [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="7cac6-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="7cac6-123">La [route de zone ajoutée au démarrage](#add-area-route) : [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="7cac6-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="7cac6-124">Structure de dossiers Zone</span><span class="sxs-lookup"><span data-stu-id="7cac6-124">Area folder structure</span></span>
<span data-ttu-id="7cac6-125">Imaginez une application qui contient deux groupes logiques, *Produits* et *Services*.</span><span class="sxs-lookup"><span data-stu-id="7cac6-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="7cac6-126">En utilisant des zones, la structure de dossiers se présenterait comme suit :</span><span class="sxs-lookup"><span data-stu-id="7cac6-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="7cac6-127">Nom du projet</span><span class="sxs-lookup"><span data-stu-id="7cac6-127">Project name</span></span>
  * <span data-ttu-id="7cac6-128">Zones (Areas)</span><span class="sxs-lookup"><span data-stu-id="7cac6-128">Areas</span></span>
    * <span data-ttu-id="7cac6-129">Produits</span><span class="sxs-lookup"><span data-stu-id="7cac6-129">Products</span></span>
      * <span data-ttu-id="7cac6-130">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="7cac6-130">Controllers</span></span>
        * <span data-ttu-id="7cac6-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="7cac6-131">HomeController.cs</span></span>
        * <span data-ttu-id="7cac6-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="7cac6-132">ManageController.cs</span></span>
      * <span data-ttu-id="7cac6-133">Affichages</span><span class="sxs-lookup"><span data-stu-id="7cac6-133">Views</span></span>
        * <span data-ttu-id="7cac6-134">Accueil</span><span class="sxs-lookup"><span data-stu-id="7cac6-134">Home</span></span>
          * <span data-ttu-id="7cac6-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="7cac6-135">Index.cshtml</span></span>
        * <span data-ttu-id="7cac6-136">Gérer</span><span class="sxs-lookup"><span data-stu-id="7cac6-136">Manage</span></span>
          * <span data-ttu-id="7cac6-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="7cac6-137">Index.cshtml</span></span>
          * <span data-ttu-id="7cac6-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="7cac6-138">About.cshtml</span></span>
    * <span data-ttu-id="7cac6-139">Services</span><span class="sxs-lookup"><span data-stu-id="7cac6-139">Services</span></span>
      * <span data-ttu-id="7cac6-140">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="7cac6-140">Controllers</span></span>
        * <span data-ttu-id="7cac6-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="7cac6-141">HomeController.cs</span></span>
      * <span data-ttu-id="7cac6-142">Affichages</span><span class="sxs-lookup"><span data-stu-id="7cac6-142">Views</span></span>
        * <span data-ttu-id="7cac6-143">Accueil</span><span class="sxs-lookup"><span data-stu-id="7cac6-143">Home</span></span>
          * <span data-ttu-id="7cac6-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="7cac6-144">Index.cshtml</span></span>

<span data-ttu-id="7cac6-145">Si la disposition précédente est classique dans les cas où des zones sont utilisées, seuls les fichiers de vues sont nécessaires pour utiliser cette structure de dossiers.</span><span class="sxs-lookup"><span data-stu-id="7cac6-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="7cac6-146">La détection de vues recherche un fichier de vue de zone correspondant dans l’ordre suivant :</span><span class="sxs-lookup"><span data-stu-id="7cac6-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="7cac6-147">L’emplacement des dossiers autres que Vues comme *Contrôleurs* et *Modèles* **n’a pas** d’importance.</span><span class="sxs-lookup"><span data-stu-id="7cac6-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="7cac6-148">Par exemple, les dossiers *Contrôleurs* et *Modèles* ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="7cac6-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="7cac6-149">Le contenu de *Contrôleurs* et *Modèles* est constitué de code qui est compilé dans un fichier .dll.</span><span class="sxs-lookup"><span data-stu-id="7cac6-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="7cac6-150">Le contenu de *Vues* n’est pas compilée tant qu’aucune demande n’a été émise pour la vue en question.</span><span class="sxs-lookup"><span data-stu-id="7cac6-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="7cac6-151">Associer le contrôleur à une zone</span><span class="sxs-lookup"><span data-stu-id="7cac6-151">Associate the controller with an Area</span></span>

<span data-ttu-id="7cac6-152">Les contrôleurs de zone sont désignés à l’aide de l’attribut [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) :</span><span class="sxs-lookup"><span data-stu-id="7cac6-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="7cac6-153">Ajouter une route de zone</span><span class="sxs-lookup"><span data-stu-id="7cac6-153">Add Area route</span></span>

<span data-ttu-id="7cac6-154">Les routes de zones utilisent généralement un routage conventionnel plutôt qu’un routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="7cac6-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="7cac6-155">Le routage conventionnel est dépendant de l’ordre.</span><span class="sxs-lookup"><span data-stu-id="7cac6-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="7cac6-156">En général, les routes avec des zones doivent être placées plus haut dans la table de routage, car elles sont plus spécifiques que les routes sans zone.</span><span class="sxs-lookup"><span data-stu-id="7cac6-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="7cac6-157">`{area:...}` peut être utilisé comme jeton dans les modèles de route si l’espace d’url est uniforme dans toutes les zones :</span><span class="sxs-lookup"><span data-stu-id="7cac6-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="7cac6-158">Dans le code précédent, `exists` applique une contrainte qui impose à la route de correspondre à une zone.</span><span class="sxs-lookup"><span data-stu-id="7cac6-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="7cac6-159">Pour ajouter un routage à des zones, le mécanisme le moins compliqué consiste à utiliser `{area:...}`.</span><span class="sxs-lookup"><span data-stu-id="7cac6-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="7cac6-160">Le code suivant utilise <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> pour créer deux routes de zone nommées :</span><span class="sxs-lookup"><span data-stu-id="7cac6-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="7cac6-161">Si vous utilisez `MapAreaRoute` avec ASP.NET Core 2.2, prenez connaissance de [ce problème sur GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="7cac6-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="7cac6-162">Pour plus d’informations, consultez [Routage de zones](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="7cac6-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="7cac6-163">Génération de liens avec zones</span><span class="sxs-lookup"><span data-stu-id="7cac6-163">Link Generation with Areas</span></span>

<span data-ttu-id="7cac6-164">Le code suivant tiré de l’[exemple de code téléchargeable](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) illustre une génération de liens avec la zone spécifiée :</span><span class="sxs-lookup"><span data-stu-id="7cac6-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="7cac6-165">Les liens générés par le code précédent sont valides où que ce soit dans l’application.</span><span class="sxs-lookup"><span data-stu-id="7cac6-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="7cac6-166">L’exemple de code téléchargeable comprend une [vue partielle](xref:mvc/views/partial) qui contient les liens précédents et les mêmes liens sans spécification de la zone.</span><span class="sxs-lookup"><span data-stu-id="7cac6-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="7cac6-167">La vue partielle étant référencée dans le [fichier de disposition](), chaque page de l’application affiche les liens générés.</span><span class="sxs-lookup"><span data-stu-id="7cac6-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="7cac6-168">Les liens générés sans spécification de la zone sont valides uniquement quand ils sont référencés dans une page contenue dans la même zone et le même contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7cac6-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="7cac6-169">Quand la zone ou le contrôleur n’est pas spécifié, le routage dépend des valeurs *ambiantes*.</span><span class="sxs-lookup"><span data-stu-id="7cac6-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="7cac6-170">Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens.</span><span class="sxs-lookup"><span data-stu-id="7cac6-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="7cac6-171">Dans de nombreux cas, pour l’exemple d’application, l’utilisation des valeurs ambiantes génère des liens incorrects.</span><span class="sxs-lookup"><span data-stu-id="7cac6-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="7cac6-172">Pour plus d’informations, consultez [Routage vers des actions de contrôleur](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="7cac6-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="7cac6-173">Disposition partagée pour les zones en utilisant le fichier _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="7cac6-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="7cac6-174">Pour partager une disposition commune pour l’ensemble de l’application, déplacez *_ViewStart.cshtml* dans le dossier racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="7cac6-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="7cac6-175">Changer le dossier de zone par défaut où sont stockées les vues</span><span class="sxs-lookup"><span data-stu-id="7cac6-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="7cac6-176">Le code suivant remplace le dossier de zone par défaut `"Areas"` par `"MyAreas"` :</span><span class="sxs-lookup"><span data-stu-id="7cac6-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="7cac6-177">Zones de publication</span><span class="sxs-lookup"><span data-stu-id="7cac6-177">Publishing Areas</span></span>

<span data-ttu-id="7cac6-178">Tous les fichiers `*.cshtml` et `wwwroot/**` sont publiés en sortie quand `<Project Sdk="Microsoft.NET.Sdk.Web">` est inclus dans le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7cac6-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
