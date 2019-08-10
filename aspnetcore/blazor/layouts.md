---
title: ASP.NET Core les dispositions éblouissantes
author: guardrex
description: Découvrez comment créer des composants de disposition réutilisables pour les applications éblouissantes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/layouts
ms.openlocfilehash: 2d652e149381f0a93e3135da978ab5737d47c6f1
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "68948219"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="db6b1-103">ASP.NET Core les dispositions éblouissantes</span><span class="sxs-lookup"><span data-stu-id="db6b1-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="db6b1-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="db6b1-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="db6b1-105">Certains éléments de l’application, tels que les menus, les messages de copyright et les logos de l’entreprise, font généralement partie de la mise en page globale de l’application et sont utilisés par chaque composant de l’application.</span><span class="sxs-lookup"><span data-stu-id="db6b1-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="db6b1-106">La copie du code de ces éléments dans tous les composants d’une application n’est pas une&mdash;approche efficace chaque fois que l’un des éléments requiert une mise à jour, chaque composant doit être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="db6b1-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="db6b1-107">Une telle duplication est difficile à gérer et peut entraîner une incohérence du contenu au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="db6b1-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="db6b1-108">Les *dispositions* résolvent ce problème.</span><span class="sxs-lookup"><span data-stu-id="db6b1-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="db6b1-109">Techniquement, une disposition est simplement un autre composant.</span><span class="sxs-lookup"><span data-stu-id="db6b1-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="db6b1-110">Une disposition est définie dans un modèle Razor ou dans C# du code et peut utiliser la [liaison de données](xref:blazor/components#data-binding), l' [injection](xref:blazor/dependency-injection)de dépendances et d’autres scénarios de composants.</span><span class="sxs-lookup"><span data-stu-id="db6b1-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="db6b1-111">Pour transformer un *composant* en une *disposition*, le composant:</span><span class="sxs-lookup"><span data-stu-id="db6b1-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="db6b1-112">Hérite de `LayoutComponentBase`, qui définit une `Body` propriété pour le contenu rendu à l’intérieur de la disposition.</span><span class="sxs-lookup"><span data-stu-id="db6b1-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="db6b1-113">Utilise la syntaxe Razor `@Body` pour spécifier l’emplacement dans la balise de mise en page où le contenu est affiché.</span><span class="sxs-lookup"><span data-stu-id="db6b1-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="db6b1-114">L’exemple de code suivant montre le modèle Razor d’un composant de disposition, *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="db6b1-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="db6b1-115">La disposition hérite `LayoutComponentBase` de et définit `@Body` la entre la barre de navigation et le pied de page:</span><span class="sxs-lookup"><span data-stu-id="db6b1-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="db6b1-116">Spécifier une disposition dans un composant</span><span class="sxs-lookup"><span data-stu-id="db6b1-116">Specify a layout in a component</span></span>

<span data-ttu-id="db6b1-117">Utilisez la directive `@layout` Razor pour appliquer une disposition à un composant.</span><span class="sxs-lookup"><span data-stu-id="db6b1-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="db6b1-118">Le compilateur convertit `@layout` `LayoutAttribute`en, qui est appliqué à la classe de composant.</span><span class="sxs-lookup"><span data-stu-id="db6b1-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="db6b1-119">Le contenu du composant suivant, *MasterList. Razor*, est inséré dans le `MainLayout` à la position de: `@Body`</span><span class="sxs-lookup"><span data-stu-id="db6b1-119">The content of the following component, *MasterList.razor*, is inserted into the `MainLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="db6b1-120">Sélection de la disposition centralisée</span><span class="sxs-lookup"><span data-stu-id="db6b1-120">Centralized layout selection</span></span>

<span data-ttu-id="db6b1-121">Chaque dossier d’une application peut éventuellement contenir un fichier de modèle nommé *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="db6b1-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="db6b1-122">Le compilateur comprend les directives spécifiées dans le fichier Imports de tous les modèles Razor dans le même dossier et de manière récursive dans tous ses sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="db6b1-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="db6b1-123">Par conséquent, un fichier *_Imports. Razor* contenant `@layout MainLayout` s’assure que tous les composants d’un dossier `MainLayout`utilisent.</span><span class="sxs-lookup"><span data-stu-id="db6b1-123">Therefore, an *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use `MainLayout`.</span></span> <span data-ttu-id="db6b1-124">Il n’est pas nécessaire d’ajouter `@layout MainLayout` à plusieurs reprises à tous les fichiers *. Razor* dans le dossier et les sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="db6b1-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="db6b1-125">`@using`les directives sont également appliquées aux composants de la même façon.</span><span class="sxs-lookup"><span data-stu-id="db6b1-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="db6b1-126">Le fichier *_Imports. Razor* suivant importe:</span><span class="sxs-lookup"><span data-stu-id="db6b1-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="db6b1-127">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="db6b1-127">`MainLayout`.</span></span>
* <span data-ttu-id="db6b1-128">Tous les composants Razor dans le même dossier et dans tous les sous-dossiers.</span><span class="sxs-lookup"><span data-stu-id="db6b1-128">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="db6b1-129">Espace de noms `BlazorApp1.Data` .</span><span class="sxs-lookup"><span data-stu-id="db6b1-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="db6b1-130">Le fichier *_Imports. Razor* est semblable au [fichier _ViewImports. cshtml pour les vues et les pages Razor,](xref:mvc/views/layout#importing-shared-directives) mais appliqué spécifiquement aux fichiers du composant Razor.</span><span class="sxs-lookup"><span data-stu-id="db6b1-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="db6b1-131">Les modèles éblouissant utilisent des fichiers *_Imports. Razor* pour la sélection de la disposition.</span><span class="sxs-lookup"><span data-stu-id="db6b1-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="db6b1-132">Une application créée à partir d’un modèle éblouissant contient le fichier *_Imports. Razor* à la racine du projet et dans le dossier *pages* .</span><span class="sxs-lookup"><span data-stu-id="db6b1-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="db6b1-133">Dispositions imbriquées</span><span class="sxs-lookup"><span data-stu-id="db6b1-133">Nested layouts</span></span>

<span data-ttu-id="db6b1-134">Les applications peuvent se composer de dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="db6b1-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="db6b1-135">Un composant peut faire référence à une disposition qui, à son tour, fait référence à une autre disposition.</span><span class="sxs-lookup"><span data-stu-id="db6b1-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="db6b1-136">Par exemple, les dispositions d’imbrication sont utilisées pour créer une structure de menus à plusieurs niveaux.</span><span class="sxs-lookup"><span data-stu-id="db6b1-136">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="db6b1-137">L’exemple suivant montre comment utiliser des dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="db6b1-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="db6b1-138">Le fichier *EpisodesComponent. Razor* est le composant à afficher.</span><span class="sxs-lookup"><span data-stu-id="db6b1-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="db6b1-139">Le composant fait référence `MasterListLayout`à:</span><span class="sxs-lookup"><span data-stu-id="db6b1-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="db6b1-140">Le fichier *MasterListLayout. Razor* fournit le `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="db6b1-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="db6b1-141">La disposition fait référence à une `MasterLayout`autre disposition,, où elle est rendue.</span><span class="sxs-lookup"><span data-stu-id="db6b1-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="db6b1-142">`EpisodesComponent`l’emplacement `@Body` est affiché:</span><span class="sxs-lookup"><span data-stu-id="db6b1-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="db6b1-143">Enfin, `MasterLayout` dans *MasterLayout. Razor* contient les éléments de disposition de niveau supérieur, tels que l’en-tête, le menu principal et le pied de page.</span><span class="sxs-lookup"><span data-stu-id="db6b1-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="db6b1-144">`MasterListLayout`avec `EpisodesComponent` sont affichés, `@Body` où s’affiche:</span><span class="sxs-lookup"><span data-stu-id="db6b1-144">`MasterListLayout` with `EpisodesComponent` are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="db6b1-145">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="db6b1-145">Additional resources</span></span>

* <xref:mvc/views/layout>
