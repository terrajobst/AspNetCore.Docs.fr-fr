---
title: Mise en page dans ASP.NET Core
author: ardalis
description: Apprenez à utiliser des dispositions communes, à partager des directives et à exécuter le code commun avant d’afficher les vues dans une application ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 8e89c8e6cf18c47abb6bf432cdc6bb6b97e8aeb0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="layout"></a>Mise en page

Par [Steve Smith](https://ardalis.com/)

Les vues partagent fréquemment des éléments visuels et de programmation. Dans cet article, vous allez apprendre à utiliser des mises en page courantes, partager des directives et exécuter le code commun avant le rendu des vues dans votre application ASP.NET.

## <a name="what-is-a-layout"></a>Qu’est une mise en page

La plupart des applications web ont une mise en page courante qui offre une expérience cohérente à l’utilisateur lorsqu’il navigue de page en page. En général, la mise en page inclut des éléments d’interface utilisateur courantes comme l’en-tête de l’application, la navigation ou les éléments de menu et un pied de page. 

![Exemple de mise en page](layout/_static/page-layout.png)

Les structures HTML courantes telles que des scripts et des feuilles de style sont fréquemment utilisées par de nombreuses pages au sein d’une application. Tous ces éléments partagés peuvent être définis dans un fichier *layout*, qui peut ensuite être référencé par n’importe quelle vue utilisée dans l’application. Les mises en page réduisent le code dupliqué dans les vues, en aidant à suivre le [principe Don't repeat yourself (DRY)](http://deviq.com/don-t-repeat-yourself/).

Par convention, la mise en page par défaut pour une application ASP.NET est nommée `_Layout.cshtml`. Le modèle de projet Visual Studio ASP.NET Core MVC inclut ce fichier de mise en page dans le dossier `Views/Shared` :

![Dossier Views dans l’Explorateur de solutions](layout/_static/web-project-views.png) 

Cette mise en page définit un modèle de niveau supérieur pour les vues dans l’application. Les applications ne nécessitent pas une mise en page et peuvent définir plusieurs mises en page, avec des vues différentes spécifiant des mises en page différentes. 

Un exemple de `_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Spécification d’une disposition

Les vues Razor ont une propriété `Layout`. Les vues individuelles spécifient une mise en page en définissant la propriété suivante : 

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

La mise en page spécifiée peut utiliser un chemin d’accès complet (exemple : `/Views/Shared/_Layout.cshtml`) ou un nom partiel (exemple : `_Layout`). Lorsqu’un nom partiel est fourni, le moteur d’affichage Razor recherchera le fichier de mise en page à l’aide de son processus de découverte standard. Le dossier associé au contrôleur de la recherche est effectuée en premier lieu, suivi par le dossier `Shared`. Ce processus de découverte est identique à celui utilisé pour découvrir les [vues partielles](partial.md).

Par défaut, chaque mise en page doit appeler `RenderBody`. Chaque fois que l’appel à `RenderBody` est placé, le contenu de la vue est restitué.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sections

Une mise en page peut éventuellement faire référence à une ou plusieurs *sections*, en appelant `RenderSection`. Les sections fournissent un moyen d’organiser où certains éléments de la page doivent être placés. Chaque appel à `RenderSection` peut spécifier si cette section est obligatoire ou facultative. Si une section requise n’est pas trouvée, une exception est levée. Des vues spécifient le contenu à restituer dans une section à l’aide de la syntaxe Razor `@section`. Si une vue définit une section, il doit être rendu (ou une erreur se produit).

Un exemple de définition `@section` dans une vue :

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Dans le code ci-dessus, les scripts de validation sont ajoutés à la section `scripts` sur une vue qui inclut un formulaire. Les autres vues dans la même application ne nécessitent pas d’autres scripts, donc vous n’avez pas besoin de définir une section de scripts. 

Les sections définies dans une vue sont disponibles uniquement dans sa mise en page immédiate. Elles ne peuvent pas être référencées à partir de vues partielles, de composants de vue ou d'autres parties du système de vue. 

### <a name="ignoring-sections"></a>Ignorer des sections

Par défaut, le corps et toutes les sections d'une page de contenu doivent être rendus par la page de disposition. Le moteur d’affichage Razor applique ceci en effectuant un suivi pour savoir si le corps et chaque section ont été rendus. 

Pour indiquer au moteur de vue d’ignorer le corps ou des sections, appelez les méthodes `IgnoreBody` et `IgnoreSection`.

Le corps et chaque section dans une page Razor doivent être soit rendus, soit ignorés.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importation de directives partagées 

Les vues peuvent utiliser des directives Razor pour effectuer diverses opérations, comme l’importation d’espaces de noms ou l'[injection de dépendance](dependency-injection.md). Les directives partagées par plusieurs vues peuvent être spécifiées dans un fichier commun `_ViewImports.cshtml`. Le fichier `_ViewImports` prend en charge les directives suivantes : 

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Le fichier ne prend pas en charge les autres fonctionnalités Razor, telles que les fonctions et les définitions de section.

Un exemple de fichier `_ViewImports.cshtml` :

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

Le fichier `_ViewImports.cshtml` pour une application ASP.NET Core MVC est généralement placé dans le dossier `Views`. Un fichier `_ViewImports.cshtml` peut être placé dans un dossier, dans ce cas il sera uniquement appliqué aux vues dans ce dossier et ses sous-dossiers. Les fichiers `_ViewImports` sont traités en commençant au niveau racine, et ensuite pour chaque dossier jusqu'à l’emplacement de la vue proprement dite, donc les paramètres spécifiés au niveau racine peuvent être remplacés au niveau du dossier.

Par exemple, si un fichier `_ViewImports.cshtml` au niveau racine spécifie `@model` et `@addTagHelper`et un autre fichier `_ViewImports.cshtml` dans le dossier associé au contrôleur de la vue spécifie un autre `@model` et ajoute un autre `@addTagHelper`, la vue aura accès à ces deux tag helpers et utilisera ce dernier `@model`.

Si plusieurs fichiers `_ViewImports.cshtml` sont exécutés pour une vue, le comportement combiné des directives incluses dans les fichiers `ViewImports.cshtml` sera comme suit : 

* `@addTagHelper`, `@removeTagHelper`: tous exécutés dans l’ordre

* `@tagHelperPrefix`: le plus proche de la vue remplace les autres noms

* `@model`: le plus proche de la vue remplace les autres noms

* `@inherits`: le plus proche de la vue remplace les autres noms

* `@using`: tous sont inclus ; les doublons sont ignorés.

* `@inject`: pour chaque propriété, le plus proche de la vue remplace les autres noms portant le même nom de propriété

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Exécution du Code avant chaque vue.

Si vous avez un code que vous devez exécuter avant chaque vue, il doit être placé dans le fichier `_ViewStart.cshtml`. Par convention, le fichier `_ViewStart.cshtml` se trouve dans le dossier `Views`. Les instructions figurant dans `_ViewStart.cshtml` sont exécutées avant chaque vue complète (pas de mise en page et de vues partielles). Comme [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` est hiérarchique. Si un fichier `_ViewStart.cshtml` est défini dans le dossier de la vue associée au contrôleur, il sera exécuté après celui défini dans la racine de le dossier `Views` (le cas échéant).

Un exemple de fichier `_ViewStart.cshtml` :

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Le fichier ci-dessus spécifie que toutes les vues utilisent la mise en page `_Layout.cshtml`.

> [!NOTE]
> Ni `_ViewStart.cshtml` ni `_ViewImports.cshtml` ne sont généralement placés dans le dossier `/Views/Shared`. Les versions au niveau de l’application de ces fichiers doivent être placées directement dans le dossier `/Views`.