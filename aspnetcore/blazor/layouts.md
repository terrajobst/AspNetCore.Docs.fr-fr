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
# <a name="aspnet-core-blazor-layouts"></a>ASP.NET Core les dispositions éblouissantes

Par [Rainer Stropek](https://www.timecockpit.com)

Certains éléments de l’application, tels que les menus, les messages de copyright et les logos de l’entreprise, font généralement partie de la mise en page globale de l’application et sont utilisés par chaque composant de l’application. La copie du code de ces éléments dans tous les composants d’une application n’est pas une&mdash;approche efficace chaque fois que l’un des éléments requiert une mise à jour, chaque composant doit être mis à jour. Une telle duplication est difficile à gérer et peut entraîner une incohérence du contenu au fil du temps. Les *dispositions* résolvent ce problème.

Techniquement, une disposition est simplement un autre composant. Une disposition est définie dans un modèle Razor ou dans C# du code et peut utiliser la [liaison de données](xref:blazor/components#data-binding), l' [injection](xref:blazor/dependency-injection)de dépendances et d’autres scénarios de composants.

Pour transformer un *composant* en une *disposition*, le composant:

* Hérite de `LayoutComponentBase`, qui définit une `Body` propriété pour le contenu rendu à l’intérieur de la disposition.
* Utilise la syntaxe Razor `@Body` pour spécifier l’emplacement dans la balise de mise en page où le contenu est affiché.

L’exemple de code suivant montre le modèle Razor d’un composant de disposition, *MainLayout. Razor*. La disposition hérite `LayoutComponentBase` de et définit `@Body` la entre la barre de navigation et le pied de page:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a>Spécifier une disposition dans un composant

Utilisez la directive `@layout` Razor pour appliquer une disposition à un composant. Le compilateur convertit `@layout` `LayoutAttribute`en, qui est appliqué à la classe de composant.

Le contenu du composant suivant, *MasterList. Razor*, est inséré dans le `MainLayout` à la position de: `@Body`

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a>Sélection de la disposition centralisée

Chaque dossier d’une application peut éventuellement contenir un fichier de modèle nommé *_Imports. Razor*. Le compilateur comprend les directives spécifiées dans le fichier Imports de tous les modèles Razor dans le même dossier et de manière récursive dans tous ses sous-dossiers. Par conséquent, un fichier *_Imports. Razor* contenant `@layout MainLayout` s’assure que tous les composants d’un dossier `MainLayout`utilisent. Il n’est pas nécessaire d’ajouter `@layout MainLayout` à plusieurs reprises à tous les fichiers *. Razor* dans le dossier et les sous-dossiers. `@using`les directives sont également appliquées aux composants de la même façon.

Le fichier *_Imports. Razor* suivant importe:

* `MainLayout`.
* Tous les composants Razor dans le même dossier et dans tous les sous-dossiers.
* Espace de noms `BlazorApp1.Data` .
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

Le fichier *_Imports. Razor* est semblable au [fichier _ViewImports. cshtml pour les vues et les pages Razor,](xref:mvc/views/layout#importing-shared-directives) mais appliqué spécifiquement aux fichiers du composant Razor.

Les modèles éblouissant utilisent des fichiers *_Imports. Razor* pour la sélection de la disposition. Une application créée à partir d’un modèle éblouissant contient le fichier *_Imports. Razor* à la racine du projet et dans le dossier *pages* .

## <a name="nested-layouts"></a>Dispositions imbriquées

Les applications peuvent se composer de dispositions imbriquées. Un composant peut faire référence à une disposition qui, à son tour, fait référence à une autre disposition. Par exemple, les dispositions d’imbrication sont utilisées pour créer une structure de menus à plusieurs niveaux.

L’exemple suivant montre comment utiliser des dispositions imbriquées. Le fichier *EpisodesComponent. Razor* est le composant à afficher. Le composant fait référence `MasterListLayout`à:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

Le fichier *MasterListLayout. Razor* fournit le `MasterListLayout`. La disposition fait référence à une `MasterLayout`autre disposition,, où elle est rendue. `EpisodesComponent`l’emplacement `@Body` est affiché:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Enfin, `MasterLayout` dans *MasterLayout. Razor* contient les éléments de disposition de niveau supérieur, tels que l’en-tête, le menu principal et le pied de page. `MasterListLayout`avec `EpisodesComponent` sont affichés, `@Body` où s’affiche:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/layout>
