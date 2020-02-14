---
title: Dispositions de Blazor ASP.NET Core
author: guardrex
description: Découvrez comment créer des composants de disposition réutilisables pour les applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 8e7294f6b66d34781473522a71f929ed5f9c33f2
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213374"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a><span data-ttu-id="18d6a-103">Dispositions de Blazor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="18d6a-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="18d6a-104">Par [Rainer Stropek](https://www.timecockpit.com) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="18d6a-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="18d6a-105">Certains éléments de l’application, tels que les menus, les messages de copyright et les logos de l’entreprise, font généralement partie de la mise en page globale de l’application et sont utilisés par chaque composant de l’application.</span><span class="sxs-lookup"><span data-stu-id="18d6a-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="18d6a-106">La copie du code de ces éléments dans tous les composants d’une application n’est pas une approche efficace&mdash;chaque fois que l’un des éléments requiert une mise à jour, chaque composant doit être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="18d6a-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="18d6a-107">Une telle duplication est difficile à gérer et peut entraîner une incohérence du contenu au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="18d6a-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="18d6a-108">Les *dispositions* résolvent ce problème.</span><span class="sxs-lookup"><span data-stu-id="18d6a-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="18d6a-109">Techniquement, une disposition est simplement un autre composant.</span><span class="sxs-lookup"><span data-stu-id="18d6a-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="18d6a-110">Une disposition est définie dans un modèle Razor ou dans C# du code et peut utiliser la [liaison de données](xref:blazor/components#data-binding), l' [injection de dépendances](xref:blazor/dependency-injection)et d’autres scénarios de composants.</span><span class="sxs-lookup"><span data-stu-id="18d6a-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="18d6a-111">Pour transformer un *composant* en une *disposition*, le composant :</span><span class="sxs-lookup"><span data-stu-id="18d6a-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="18d6a-112">Hérite de `LayoutComponentBase`, qui définit une propriété `Body` pour le contenu rendu à l’intérieur de la disposition.</span><span class="sxs-lookup"><span data-stu-id="18d6a-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="18d6a-113">Utilise le `@Body` syntaxe Razor pour spécifier l’emplacement dans la balise de mise en page où le contenu est affiché.</span><span class="sxs-lookup"><span data-stu-id="18d6a-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="18d6a-114">L’exemple de code suivant montre le modèle Razor d’un composant de disposition, *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="18d6a-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="18d6a-115">La disposition hérite `LayoutComponentBase` et définit la `@Body` entre la barre de navigation et le pied de page :</span><span class="sxs-lookup"><span data-stu-id="18d6a-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="18d6a-116">Dans une application basée sur l’un des modèles d’application Blazor, le composant `MainLayout` (*MainLayout. Razor*) se trouve dans le dossier *partagé* de l’application.</span><span class="sxs-lookup"><span data-stu-id="18d6a-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="18d6a-117">Disposition par défaut</span><span class="sxs-lookup"><span data-stu-id="18d6a-117">Default layout</span></span>

<span data-ttu-id="18d6a-118">Spécifiez la disposition de l’application par défaut dans le composant `Router` dans le fichier *app. Razor* de l’application.</span><span class="sxs-lookup"><span data-stu-id="18d6a-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="18d6a-119">Le composant `Router` suivant, fourni par les modèles de Blazor par défaut, définit la disposition par défaut sur le composant `MainLayout` :</span><span class="sxs-lookup"><span data-stu-id="18d6a-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="18d6a-120">Pour fournir une disposition par défaut pour le contenu de `NotFound`, spécifiez un `LayoutView` pour le contenu de `NotFound` :</span><span class="sxs-lookup"><span data-stu-id="18d6a-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="18d6a-121">Pour plus d’informations sur le composant `Router`, consultez <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="18d6a-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

<span data-ttu-id="18d6a-122">La spécification de la disposition comme disposition par défaut dans le routeur est une pratique utile, car elle peut être remplacée par composant ou par dossier.</span><span class="sxs-lookup"><span data-stu-id="18d6a-122">Specifying the layout as a default layout in the router is a useful practice because it can be overridden on a per-component or per-folder basis.</span></span> <span data-ttu-id="18d6a-123">Préférez utiliser le routeur pour définir la disposition par défaut de l’application, car il s’agit de la technique la plus générale.</span><span class="sxs-lookup"><span data-stu-id="18d6a-123">Prefer using the router to set the app's default layout because it's the most general technique.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="18d6a-124">Spécifier une disposition dans un composant</span><span class="sxs-lookup"><span data-stu-id="18d6a-124">Specify a layout in a component</span></span>

<span data-ttu-id="18d6a-125">Utilisez la directive Razor `@layout` pour appliquer une disposition à un composant.</span><span class="sxs-lookup"><span data-stu-id="18d6a-125">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="18d6a-126">Le compilateur convertit `@layout` en `LayoutAttribute`, qui est appliqué à la classe de composant.</span><span class="sxs-lookup"><span data-stu-id="18d6a-126">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="18d6a-127">Le contenu du composant `MasterList` suivant est inséré dans le `MasterLayout` à la position de `@Body`:</span><span class="sxs-lookup"><span data-stu-id="18d6a-127">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

<span data-ttu-id="18d6a-128">La spécification de la disposition directement dans un composant remplace un ensemble de *dispositions par défaut* dans le routeur ou une directive de `@layout` importée à partir de *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="18d6a-128">Specifying the layout directly in a component overrides a *default layout* set in the router or an `@layout` directive imported from *_Imports.razor*.</span></span>

## <a name="centralized-layout-selection"></a><span data-ttu-id="18d6a-129">Sélection de la disposition centralisée</span><span class="sxs-lookup"><span data-stu-id="18d6a-129">Centralized layout selection</span></span>

<span data-ttu-id="18d6a-130">Chaque dossier d’une application peut éventuellement contenir un fichier modèle nommé *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="18d6a-130">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="18d6a-131">Le compilateur comprend les directives spécifiées dans le fichier Imports de tous les modèles Razor dans le même dossier et de manière récursive dans tous ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="18d6a-131">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="18d6a-132">Par conséquent, un fichier *_Imports. Razor* contenant `@layout MyCoolLayout` garantit que tous les composants d’un dossier utilisent `MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="18d6a-132">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="18d6a-133">Il n’est pas nécessaire d’ajouter à plusieurs reprises `@layout MyCoolLayout` à tous les fichiers *. Razor* dans le dossier et les sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="18d6a-133">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="18d6a-134">les directives de `@using` sont également appliquées aux composants de la même façon.</span><span class="sxs-lookup"><span data-stu-id="18d6a-134">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="18d6a-135">Le fichier *_Imports. Razor* suivant importe les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="18d6a-135">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="18d6a-136">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="18d6a-136">`MyCoolLayout`.</span></span>
* <span data-ttu-id="18d6a-137">Tous les composants Razor dans le même dossier et dans tous les sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="18d6a-137">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="18d6a-138">Espace de noms `BlazorApp1.Data`.</span><span class="sxs-lookup"><span data-stu-id="18d6a-138">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="18d6a-139">Le fichier *_Imports. Razor* est semblable au [fichier _ViewImports. cshtml pour les vues et les pages Razor,](xref:mvc/views/layout#importing-shared-directives) mais appliqué spécifiquement aux fichiers du composant Razor.</span><span class="sxs-lookup"><span data-stu-id="18d6a-139">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="18d6a-140">La spécification d’une disposition dans *_Imports. Razor* remplace une disposition spécifiée comme *disposition par défaut*du routeur.</span><span class="sxs-lookup"><span data-stu-id="18d6a-140">Specifying a layout in *_Imports.razor* overrides a layout specified as the router's *default layout*.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="18d6a-141">Dispositions imbriquées</span><span class="sxs-lookup"><span data-stu-id="18d6a-141">Nested layouts</span></span>

<span data-ttu-id="18d6a-142">Les applications peuvent se composer de dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="18d6a-142">Apps can consist of nested layouts.</span></span> <span data-ttu-id="18d6a-143">Un composant peut faire référence à une disposition qui, à son tour, fait référence à une autre disposition.</span><span class="sxs-lookup"><span data-stu-id="18d6a-143">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="18d6a-144">Par exemple, les dispositions d’imbrication sont utilisées pour créer une structure de menus à plusieurs niveaux.</span><span class="sxs-lookup"><span data-stu-id="18d6a-144">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="18d6a-145">L’exemple suivant montre comment utiliser des dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="18d6a-145">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="18d6a-146">Le fichier *EpisodesComponent. Razor* est le composant à afficher.</span><span class="sxs-lookup"><span data-stu-id="18d6a-146">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="18d6a-147">Le composant fait référence à la `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="18d6a-147">The component references the `MasterListLayout`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="18d6a-148">Le fichier *MasterListLayout. Razor* fournit le `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="18d6a-148">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="18d6a-149">La disposition fait référence à une autre disposition, `MasterLayout`, où elle est affichée.</span><span class="sxs-lookup"><span data-stu-id="18d6a-149">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="18d6a-150">`EpisodesComponent` s’affiche où `@Body` apparaît :</span><span class="sxs-lookup"><span data-stu-id="18d6a-150">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="18d6a-151">Enfin, `MasterLayout` dans *MasterLayout. Razor* contient les éléments de disposition de niveau supérieur, tels que l’en-tête, le menu principal et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="18d6a-151">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="18d6a-152">`MasterListLayout` avec le `EpisodesComponent` est rendu là où `@Body` apparaît :</span><span class="sxs-lookup"><span data-stu-id="18d6a-152">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a><span data-ttu-id="18d6a-153">Partager une disposition de Razor Pages avec des composants intégrés</span><span class="sxs-lookup"><span data-stu-id="18d6a-153">Share a Razor Pages layout with integrated components</span></span>

<span data-ttu-id="18d6a-154">Lorsque les composants routables sont intégrés dans une application Razor Pages, la disposition partagée de l’application peut être utilisée avec les composants.</span><span class="sxs-lookup"><span data-stu-id="18d6a-154">When routable components are integrated into a Razor Pages app, the app's shared layout can be used with the components.</span></span> <span data-ttu-id="18d6a-155">Pour plus d’informations, consultez <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="18d6a-155">For more information, see <xref:blazor/hosting-model-configuration#integrate-razor-components-into-razor-pages-and-mvc-apps>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18d6a-156">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="18d6a-156">Additional resources</span></span>

* <xref:mvc/views/layout>
