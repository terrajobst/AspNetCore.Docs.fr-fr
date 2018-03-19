---
title: Mise en page
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a><span data-ttu-id="87a5e-102">Mise en page</span><span class="sxs-lookup"><span data-stu-id="87a5e-102">Layout</span></span>

<span data-ttu-id="87a5e-103">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="87a5e-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="87a5e-104">Les vues ont souvent des objets visuels et des éléments de programme en commun.</span><span class="sxs-lookup"><span data-stu-id="87a5e-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="87a5e-105">Dans cet article, vous allez apprendre à utiliser des dispositions communes, à partager des directives et à exécuter le code commun avant d’afficher les vues dans votre application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="87a5e-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="87a5e-106">Qu’est-ce qu’une disposition ?</span><span class="sxs-lookup"><span data-stu-id="87a5e-106">What is a Layout</span></span>

<span data-ttu-id="87a5e-107">La plupart des applications web ont une disposition commune pour offrir aux utilisateurs une expérience homogène quand ils naviguent de page en page.</span><span class="sxs-lookup"><span data-stu-id="87a5e-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="87a5e-108">En général, la disposition inclut des éléments d’interface utilisateur communs à toute l’application, tels que l’en-tête, des éléments de menu ou de navigation et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="87a5e-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Exemple de disposition de page](layout/_static/page-layout.png)

<span data-ttu-id="87a5e-110">Les structures HTML communes comme les scripts et les feuilles de style sont aussi fréquemment utilisées par bon nombre de pages dans une application.</span><span class="sxs-lookup"><span data-stu-id="87a5e-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="87a5e-111">Tous ces éléments partagés peuvent être définis dans un fichier de *disposition*, qui peut ensuite être référencé par n’importe quelle vue utilisée dans l’application.</span><span class="sxs-lookup"><span data-stu-id="87a5e-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="87a5e-112">Les dispositions réduisent la répétition de code dans les vues, selon le [principe Ne vous répétez pas (DRY, Don’t Repeat Yourself)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="87a5e-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="87a5e-113">Par convention, la disposition par défaut d’une application ASP.NET est nommée `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="87a5e-114">Le modèle de projet Visual Studio ASP.NET Core MVC stocke ce fichier de disposition dans le dossier `Views/Shared` :</span><span class="sxs-lookup"><span data-stu-id="87a5e-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Dossier Vues dans l’Explorateur de solutions](layout/_static/web-project-views.png)

<span data-ttu-id="87a5e-116">Cette disposition définit un modèle général pour les vues dans l’application.</span><span class="sxs-lookup"><span data-stu-id="87a5e-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="87a5e-117">Les applications ne nécessitent pas toujours une disposition, mais elles peuvent aussi définir plusieurs dispositions, avec des vues différentes spécifiant des dispositions différentes.</span><span class="sxs-lookup"><span data-stu-id="87a5e-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="87a5e-118">Exemple `_Layout.cshtml` :</span><span class="sxs-lookup"><span data-stu-id="87a5e-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="87a5e-119">Spécification d’une disposition</span><span class="sxs-lookup"><span data-stu-id="87a5e-119">Specifying a Layout</span></span>

<span data-ttu-id="87a5e-120">Les vues Razor ont une propriété `Layout`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="87a5e-121">Chaque vue spécifie une disposition en définissant cette propriété :</span><span class="sxs-lookup"><span data-stu-id="87a5e-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="87a5e-122">La disposition spécifiée peut utiliser un chemin complet (par exemple, `/Views/Shared/_Layout.cshtml`) ou un nom partiel (par exemple, `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="87a5e-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="87a5e-123">Quand un nom partiel est fourni, le moteur de vue Razor recherche le fichier de disposition en suivant son processus de détection habituel.</span><span class="sxs-lookup"><span data-stu-id="87a5e-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="87a5e-124">Il recherche d’abord le dossier associé au contrôleur, puis le dossier `Shared`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="87a5e-125">Ce processus de détection est le même que celui utilisé pour détecter les [vues partielles](partial.md).</span><span class="sxs-lookup"><span data-stu-id="87a5e-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="87a5e-126">Par défaut, chaque disposition doit appeler `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="87a5e-127">À chaque appel de `RenderBody`, le contenu de la vue est affiché.</span><span class="sxs-lookup"><span data-stu-id="87a5e-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="87a5e-128">Sections</span><span class="sxs-lookup"><span data-stu-id="87a5e-128">Sections</span></span>

<span data-ttu-id="87a5e-129">Une disposition peut éventuellement faire référence à une ou plusieurs *sections*, en appelant `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="87a5e-130">Les sections sont un moyen d’organiser certains éléments dans la page.</span><span class="sxs-lookup"><span data-stu-id="87a5e-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="87a5e-131">Chaque appel à `RenderSection` peut spécifier si cette section est obligatoire ou facultative.</span><span class="sxs-lookup"><span data-stu-id="87a5e-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="87a5e-132">Si une section obligatoire est introuvable, une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="87a5e-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="87a5e-133">Chacune des vues spécifient le contenu à afficher dans une section à l’aide de la syntaxe Razor `@section`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="87a5e-134">Si une vue définit une section, elle doit être affichée (sinon, une erreur se produit).</span><span class="sxs-lookup"><span data-stu-id="87a5e-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="87a5e-135">Exemple de définition `@section` dans une vue :</span><span class="sxs-lookup"><span data-stu-id="87a5e-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="87a5e-136">Dans le code ci-dessus, des scripts de validation sont ajoutés à la section `scripts` dans une vue qui contient un formulaire.</span><span class="sxs-lookup"><span data-stu-id="87a5e-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="87a5e-137">Il est possible que d’autres vues dans la même application ne nécessitent pas de scripts supplémentaires. Pour ces vues, il n’y a donc pas besoin de définir une section de scripts.</span><span class="sxs-lookup"><span data-stu-id="87a5e-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="87a5e-138">Les sections définies dans une vue sont disponibles uniquement dans sa page de disposition la plus proche.</span><span class="sxs-lookup"><span data-stu-id="87a5e-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="87a5e-139">Elles ne peuvent pas être référencées à partir de vues partielles, de composants de vue ou d’autres parties du système de vue.</span><span class="sxs-lookup"><span data-stu-id="87a5e-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="87a5e-140">Ignorer des sections</span><span class="sxs-lookup"><span data-stu-id="87a5e-140">Ignoring sections</span></span>

<span data-ttu-id="87a5e-141">Par défaut, le corps et toutes les sections dans une page de contenu doivent intégralement être affichés par la page de disposition.</span><span class="sxs-lookup"><span data-stu-id="87a5e-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="87a5e-142">Le moteur de vue Razor s’assure que c’est bien le cas en vérifiant que le corps et chaque section ont été affichés.</span><span class="sxs-lookup"><span data-stu-id="87a5e-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="87a5e-143">Pour indiquer au moteur de vue d’ignorer le corps ou les sections, appelez les méthodes `IgnoreBody` et `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="87a5e-144">Le corps et toutes les sections dans une page Razor doivent être soit affichés, soit ignorés.</span><span class="sxs-lookup"><span data-stu-id="87a5e-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="87a5e-145">Importation de directives partagées</span><span class="sxs-lookup"><span data-stu-id="87a5e-145">Importing Shared Directives</span></span>

<span data-ttu-id="87a5e-146">Les vues peuvent utiliser des directives Razor pour diverses opérations, par exemple, importer des espaces de noms ou effectuer une [injection de dépendances](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="87a5e-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="87a5e-147">Les directives partagées par plusieurs vues peuvent être spécifiées dans un fichier `_ViewImports.cshtml` commun.</span><span class="sxs-lookup"><span data-stu-id="87a5e-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="87a5e-148">Le fichier `_ViewImports` prend en charge les directives suivantes :</span><span class="sxs-lookup"><span data-stu-id="87a5e-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="87a5e-149">Le fichier ne prend pas en charge les autres fonctionnalités Razor, telles que les fonctions et les définitions de section.</span><span class="sxs-lookup"><span data-stu-id="87a5e-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="87a5e-150">Exemple de fichier `_ViewImports.cshtml` :</span><span class="sxs-lookup"><span data-stu-id="87a5e-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="87a5e-151">Le fichier `_ViewImports.cshtml` pour une application ASP.NET Core MVC est généralement créé dans le dossier `Views`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="87a5e-152">Un fichier `_ViewImports.cshtml` peut être créé dans un autre dossier, auquel cas il s’applique uniquement aux vues contenues dans ce dossier et ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="87a5e-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="87a5e-153">Les fichiers `_ViewImports` sont traités d’abord au niveau racine, puis pour chaque dossier jusqu’à l’emplacement de la vue proprement dite. Les paramètres spécifiés au niveau racine peuvent dont être remplacés par les paramètres définis au niveau du dossier.</span><span class="sxs-lookup"><span data-stu-id="87a5e-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="87a5e-154">Par exemple, si le fichier `_ViewImports.cshtml` au niveau racine spécifie `@model` et `@addTagHelper`, mais qu’un autre fichier `_ViewImports.cshtml` dans le dossier associé au contrôleur de la vue spécifie un `@model` différent et un `@addTagHelper` supplémentaire, la vue a accès aux deux Tag Helpers et utilise le dernier `@model`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="87a5e-155">Si plusieurs fichiers `_ViewImports.cshtml` sont exécutés pour une vue, le comportement combiné des directives incluses dans les fichiers `ViewImports.cshtml` est le suivant :</span><span class="sxs-lookup"><span data-stu-id="87a5e-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="87a5e-156">`@addTagHelper`, `@removeTagHelper` : les deux directives sont exécutées, dans l’ordre</span><span class="sxs-lookup"><span data-stu-id="87a5e-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="87a5e-157">`@tagHelperPrefix` : la directive la plus proche de la vue se substitue aux autres</span><span class="sxs-lookup"><span data-stu-id="87a5e-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="87a5e-158">`@model` : la directive la plus proche de la vue se substitue aux autres</span><span class="sxs-lookup"><span data-stu-id="87a5e-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="87a5e-159">`@inherits` : la directive la plus proche de la vue se substitue aux autres</span><span class="sxs-lookup"><span data-stu-id="87a5e-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="87a5e-160">`@using` : toutes les directives sont incluses ; les doublons sont ignorés</span><span class="sxs-lookup"><span data-stu-id="87a5e-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="87a5e-161">`@inject` : pour chaque propriété, la plus proche de la vue se substitue aux autres propriétés ayant le même nom</span><span class="sxs-lookup"><span data-stu-id="87a5e-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="87a5e-162">Exécution du code avant chaque vue</span><span class="sxs-lookup"><span data-stu-id="87a5e-162">Running Code Before Each View</span></span>

<span data-ttu-id="87a5e-163">Si vous avez du code à exécuter avant chaque vue, vous devez l’ajouter au fichier `_ViewStart.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="87a5e-164">Par convention, le fichier `_ViewStart.cshtml` se trouve dans le dossier `Views`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="87a5e-165">Les instructions contenues dans `_ViewStart.cshtml` sont exécutées avant chaque vue complète (donc hors dispositions et vues partielles).</span><span class="sxs-lookup"><span data-stu-id="87a5e-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="87a5e-166">Comme [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` a une structure hiérarchique.</span><span class="sxs-lookup"><span data-stu-id="87a5e-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="87a5e-167">Si un fichier `_ViewStart.cshtml` est défini dans le dossier de vue associé au contrôleur, il est exécuté après celui qui est défini à la racine du dossier `Views` (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="87a5e-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="87a5e-168">Exemple de fichier `_ViewStart.cshtml` :</span><span class="sxs-lookup"><span data-stu-id="87a5e-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="87a5e-169">Le fichier ci-dessus spécifie que toutes les vues doivent utiliser la disposition `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="87a5e-170">Les fichiers `_ViewStart.cshtml` et `_ViewImports.cshtml` ne sont généralement pas placés dans le dossier `/Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="87a5e-171">Les versions de ces fichiers qui sont au niveau de l’application doivent être placées directement dans le dossier `/Views`.</span><span class="sxs-lookup"><span data-stu-id="87a5e-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
