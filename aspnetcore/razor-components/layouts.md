---
title: Dispositions de composants de Razor
author: guardrex
description: Découvrez comment créer des composants réutilisables de disposition pour les applications de composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515523"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="24136-103">Dispositions de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="24136-103">Razor Components layouts</span></span>

<span data-ttu-id="24136-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="24136-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="24136-105">En règle générale, les applications contiennent plusieurs composants.</span><span class="sxs-lookup"><span data-stu-id="24136-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="24136-106">Les éléments de disposition, tels que les menus, les messages de droits d’auteur et les logos, doivent être présents sur tous les composants.</span><span class="sxs-lookup"><span data-stu-id="24136-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="24136-107">Copier le code de ces éléments de disposition dans tous les composants d’une application n’est pas une approche efficace.</span><span class="sxs-lookup"><span data-stu-id="24136-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="24136-108">Toute duplication est difficile à entretenir et probablement mène au contenu incohérent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="24136-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="24136-109">*Dispositions* résoudre ce problème.</span><span class="sxs-lookup"><span data-stu-id="24136-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="24136-110">Techniquement, une disposition est simplement un autre composant.</span><span class="sxs-lookup"><span data-stu-id="24136-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="24136-111">Une disposition est définie dans un modèle Razor ou dans C# de code et peut contenir [liaison de données](xref:razor-components/components#data-binding), [l’injection de dépendances](xref:razor-components/dependency-injection)et d’autres fonctionnalités ordinaires des composants.</span><span class="sxs-lookup"><span data-stu-id="24136-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="24136-112">Deux aspects supplémentaires activer un *composant* dans un *mise en page*</span><span class="sxs-lookup"><span data-stu-id="24136-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="24136-113">Le composant de mise en page doit hériter de `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="24136-113">The layout component must inherit from `LayoutComponentBase`.</span></span> `LayoutComponentBase` <span data-ttu-id="24136-114">définit un `Body` propriété qui contient le contenu à restituer à l’intérieur de la disposition.</span><span class="sxs-lookup"><span data-stu-id="24136-114">defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="24136-115">Le composant de mise en page utilise le `Body` propriété pour spécifier où le contenu du corps doit être restitué à l’aide de la syntaxe Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="24136-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="24136-116">Lors de la génération `@Body` est remplacé par le contenu de la mise en page.</span><span class="sxs-lookup"><span data-stu-id="24136-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="24136-117">L’exemple de code suivant montre le modèle d’un composant de mise en page Razor.</span><span class="sxs-lookup"><span data-stu-id="24136-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="24136-118">Notez l’utilisation de `LayoutComponentBase` et `@Body`:</span><span class="sxs-lookup"><span data-stu-id="24136-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="24136-119">Utiliser une disposition dans un composant</span><span class="sxs-lookup"><span data-stu-id="24136-119">Use a layout in a component</span></span>

<span data-ttu-id="24136-120">Utilisez la directive Razor `@layout` pour appliquer une mise en page à un composant.</span><span class="sxs-lookup"><span data-stu-id="24136-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="24136-121">Le compilateur convertit cette directive dans une `LayoutAttribute`, qui est appliqué à la classe de composant.</span><span class="sxs-lookup"><span data-stu-id="24136-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="24136-122">L’exemple de code suivant illustre le concept.</span><span class="sxs-lookup"><span data-stu-id="24136-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="24136-123">Le contenu de ce composant est inséré dans le *MasterLayout* à la position de `@Body`:</span><span class="sxs-lookup"><span data-stu-id="24136-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="24136-124">Sélection de configuration centralisé</span><span class="sxs-lookup"><span data-stu-id="24136-124">Centralized layout selection</span></span>

<span data-ttu-id="24136-125">Tous les dossiers d’une une application peut éventuellement contenir un fichier de modèle nommé *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="24136-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="24136-126">Le compilateur inclut les directives spécifiées dans le fichier d’importations de vue dans tous les modèles Razor dans le même dossier et dans tous ses sous-dossiers de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="24136-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="24136-127">Par conséquent, un *_ViewImports.cshtml* fichier contenant `@layout MainLayout` garantit que tous les composants dans un dossier, utilisez le *MainLayout* mise en page.</span><span class="sxs-lookup"><span data-stu-id="24136-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="24136-128">Il est inutile d’ajouter à plusieurs reprises `@layout` à tous les *.razor* fichiers.</span><span class="sxs-lookup"><span data-stu-id="24136-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="24136-129">Notez que le modèle par défaut utilise le *_ViewImports.cshtml* mécanisme pour la sélection de la mise en page.</span><span class="sxs-lookup"><span data-stu-id="24136-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="24136-130">Une application qui vient d’être créée contient la *_ViewImports.cshtml* de fichiers dans le *composants/Pages* dossier.</span><span class="sxs-lookup"><span data-stu-id="24136-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="24136-131">Dispositions imbriquées</span><span class="sxs-lookup"><span data-stu-id="24136-131">Nested layouts</span></span>

<span data-ttu-id="24136-132">Peut s’agir de dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="24136-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="24136-133">Un composant peut faire référence à une disposition qui à son tour fait référence à une autre disposition.</span><span class="sxs-lookup"><span data-stu-id="24136-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="24136-134">Par exemple, les dispositions d’imbrication peuvent être utilisées afin de refléter une structure de menu à plusieurs niveaux.</span><span class="sxs-lookup"><span data-stu-id="24136-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="24136-135">Exemples de code suivants montrent comment utiliser des dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="24136-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="24136-136">Le *EpisodesComponent.cshtml* fichier est le composant à afficher.</span><span class="sxs-lookup"><span data-stu-id="24136-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="24136-137">Notez que le composant fait référence à la disposition `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="24136-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="24136-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="24136-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="24136-139">Le *MasterListLayout.cshtml* fichier fournit le `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="24136-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="24136-140">La mise en page fait référence à une autre disposition, `MasterLayout`, où elle va être incorporé.</span><span class="sxs-lookup"><span data-stu-id="24136-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="24136-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="24136-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="24136-142">Enfin, `MasterLayout` contient les éléments de disposition de niveau supérieur, tels que l’en-tête, pied de page et menu principal.</span><span class="sxs-lookup"><span data-stu-id="24136-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="24136-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="24136-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
