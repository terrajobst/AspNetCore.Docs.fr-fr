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
# <a name="razor-components-layouts"></a>Dispositions de composants de Razor

Par [Rainer Stropek](https://www.timecockpit.com)

En règle générale, les applications contiennent plusieurs composants. Les éléments de disposition, tels que les menus, les messages de droits d’auteur et les logos, doivent être présents sur tous les composants. Copier le code de ces éléments de disposition dans tous les composants d’une application n’est pas une approche efficace. Toute duplication est difficile à entretenir et probablement mène au contenu incohérent au fil du temps. *Dispositions* résoudre ce problème.

Techniquement, une disposition est simplement un autre composant. Une disposition est définie dans un modèle Razor ou dans C# de code et peut contenir [liaison de données](xref:razor-components/components#data-binding), [l’injection de dépendances](xref:razor-components/dependency-injection)et d’autres fonctionnalités ordinaires des composants.

Deux aspects supplémentaires activer un *composant* dans un *mise en page*

* Le composant de mise en page doit hériter de `LayoutComponentBase`. `LayoutComponentBase` définit un `Body` propriété qui contient le contenu à restituer à l’intérieur de la disposition.
* Le composant de mise en page utilise le `Body` propriété pour spécifier où le contenu du corps doit être restitué à l’aide de la syntaxe Razor `@Body`. Lors de la génération `@Body` est remplacé par le contenu de la mise en page.

L’exemple de code suivant montre le modèle d’un composant de mise en page Razor. Notez l’utilisation de `LayoutComponentBase` et `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a>Utiliser une disposition dans un composant

Utilisez la directive Razor `@layout` pour appliquer une mise en page à un composant. Le compilateur convertit cette directive dans une `LayoutAttribute`, qui est appliqué à la classe de composant.

L’exemple de code suivant illustre le concept. Le contenu de ce composant est inséré dans le *MasterLayout* à la position de `@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>Sélection de configuration centralisé

Tous les dossiers d’une une application peut éventuellement contenir un fichier de modèle nommé *_ViewImports.cshtml*. Le compilateur inclut les directives spécifiées dans le fichier d’importations de vue dans tous les modèles Razor dans le même dossier et dans tous ses sous-dossiers de manière récursive. Par conséquent, un *_ViewImports.cshtml* fichier contenant `@layout MainLayout` garantit que tous les composants dans un dossier, utilisez le *MainLayout* mise en page. Il est inutile d’ajouter à plusieurs reprises `@layout` à tous les *.razor* fichiers.

Notez que le modèle par défaut utilise le *_ViewImports.cshtml* mécanisme pour la sélection de la mise en page. Une application qui vient d’être créée contient la *_ViewImports.cshtml* de fichiers dans le *composants/Pages* dossier.

## <a name="nested-layouts"></a>Dispositions imbriquées

Peut s’agir de dispositions imbriquées. Un composant peut faire référence à une disposition qui à son tour fait référence à une autre disposition. Par exemple, les dispositions d’imbrication peuvent être utilisées afin de refléter une structure de menu à plusieurs niveaux.

Exemples de code suivants montrent comment utiliser des dispositions imbriquées. Le *EpisodesComponent.cshtml* fichier est le composant à afficher. Notez que le composant fait référence à la disposition `MasterListLayout`.

*EpisodesComponent.cshtml*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

Le *MasterListLayout.cshtml* fichier fournit le `MasterListLayout`. La mise en page fait référence à une autre disposition, `MasterLayout`, où elle va être incorporé.

*MasterListLayout.cshtml*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

Enfin, `MasterLayout` contient les éléments de disposition de niveau supérieur, tels que l’en-tête, pied de page et menu principal.

*MasterLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
