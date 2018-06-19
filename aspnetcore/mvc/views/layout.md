---
title: Disposition dans ASP.NET Core
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
ms.locfileid: "29904749"
---
# <a name="layout-in-aspnet-core"></a>Disposition dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Les vues ont souvent des objets visuels et des éléments de programme en commun. Dans cet article, vous allez apprendre à utiliser des dispositions communes, à partager des directives et à exécuter le code commun avant d’afficher les vues dans votre application ASP.NET.

## <a name="what-is-a-layout"></a>Qu’est-ce qu’une disposition ?

La plupart des applications web ont une disposition commune pour offrir aux utilisateurs une expérience homogène quand ils naviguent de page en page. En général, la disposition inclut des éléments d’interface utilisateur communs à toute l’application, tels que l’en-tête, des éléments de menu ou de navigation et le pied de page.

![Exemple de disposition de page](layout/_static/page-layout.png)

Les structures HTML communes comme les scripts et les feuilles de style sont aussi fréquemment utilisées par bon nombre de pages dans une application. Tous ces éléments partagés peuvent être définis dans un fichier de *disposition*, qui peut ensuite être référencé par n’importe quelle vue utilisée dans l’application. Les dispositions réduisent la répétition de code dans les vues, selon le [principe Ne vous répétez pas (DRY, Don’t Repeat Yourself)](http://deviq.com/don-t-repeat-yourself/).

Par convention, la disposition par défaut d’une application ASP.NET est nommée `_Layout.cshtml`. Le modèle de projet Visual Studio ASP.NET Core MVC stocke ce fichier de disposition dans le dossier `Views/Shared` :

![Dossier Vues dans l’Explorateur de solutions](layout/_static/web-project-views.png)

Cette disposition définit un modèle général pour les vues dans l’application. Les applications ne nécessitent pas toujours une disposition, mais elles peuvent aussi définir plusieurs dispositions, avec des vues différentes spécifiant des dispositions différentes.

Exemple `_Layout.cshtml` :

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Spécification d’une disposition

Les vues Razor ont une propriété `Layout`. Chaque vue spécifie une disposition en définissant cette propriété :

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

La disposition spécifiée peut utiliser un chemin complet (par exemple, `/Views/Shared/_Layout.cshtml`) ou un nom partiel (par exemple, `_Layout`). Quand un nom partiel est fourni, le moteur de vue Razor recherche le fichier de disposition en suivant son processus de détection habituel. Il recherche d’abord le dossier associé au contrôleur, puis le dossier `Shared`. Ce processus de détection est le même que celui utilisé pour détecter les [vues partielles](partial.md).

Par défaut, chaque disposition doit appeler `RenderBody`. À chaque appel de `RenderBody`, le contenu de la vue est affiché.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sections

Une disposition peut éventuellement faire référence à une ou plusieurs *sections*, en appelant `RenderSection`. Les sections sont un moyen d’organiser certains éléments dans la page. Chaque appel à `RenderSection` peut spécifier si cette section est obligatoire ou facultative. Si une section obligatoire est introuvable, une exception est levée. Chacune des vues spécifient le contenu à afficher dans une section à l’aide de la syntaxe Razor `@section`. Si une vue définit une section, elle doit être affichée (sinon, une erreur se produit).

Exemple de définition `@section` dans une vue :

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

Dans le code ci-dessus, des scripts de validation sont ajoutés à la section `scripts` dans une vue qui contient un formulaire. Il est possible que d’autres vues dans la même application ne nécessitent pas de scripts supplémentaires. Pour ces vues, il n’y a donc pas besoin de définir une section de scripts.

Les sections définies dans une vue sont disponibles uniquement dans sa page de disposition la plus proche. Elles ne peuvent pas être référencées à partir de vues partielles, de composants de vue ou d’autres parties du système de vue.

### <a name="ignoring-sections"></a>Ignorer des sections

Par défaut, le corps et toutes les sections dans une page de contenu doivent intégralement être affichés par la page de disposition. Le moteur de vue Razor s’assure que c’est bien le cas en vérifiant que le corps et chaque section ont été affichés.

Pour indiquer au moteur de vue d’ignorer le corps ou les sections, appelez les méthodes `IgnoreBody` et `IgnoreSection`.

Le corps et toutes les sections dans une page Razor doivent être soit affichés, soit ignorés.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importation de directives partagées

Les vues peuvent utiliser des directives Razor pour diverses opérations, par exemple, importer des espaces de noms ou effectuer une [injection de dépendances](dependency-injection.md). Les directives partagées par plusieurs vues peuvent être spécifiées dans un fichier `_ViewImports.cshtml` commun. Le fichier `_ViewImports` prend en charge les directives suivantes :

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Le fichier ne prend pas en charge les autres fonctionnalités Razor, telles que les fonctions et les définitions de section.

Exemple de fichier `_ViewImports.cshtml` :

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

Le fichier `_ViewImports.cshtml` pour une application ASP.NET Core MVC est généralement créé dans le dossier `Views`. Un fichier `_ViewImports.cshtml` peut être créé dans un autre dossier, auquel cas il s’applique uniquement aux vues contenues dans ce dossier et ses sous-dossiers. Les fichiers `_ViewImports` sont traités d’abord au niveau racine, puis pour chaque dossier jusqu’à l’emplacement de la vue proprement dite. Les paramètres spécifiés au niveau racine peuvent dont être remplacés par les paramètres définis au niveau du dossier.

Par exemple, si le fichier `_ViewImports.cshtml` au niveau racine spécifie `@model` et `@addTagHelper`, mais qu’un autre fichier `_ViewImports.cshtml` dans le dossier associé au contrôleur de la vue spécifie un `@model` différent et un `@addTagHelper` supplémentaire, la vue a accès aux deux Tag Helpers et utilise le dernier `@model`.

Si plusieurs fichiers `_ViewImports.cshtml` sont exécutés pour une vue, le comportement combiné des directives incluses dans les fichiers `ViewImports.cshtml` est le suivant :

* `@addTagHelper`, `@removeTagHelper` : les deux directives sont exécutées, dans l’ordre

* `@tagHelperPrefix` : la directive la plus proche de la vue se substitue aux autres

* `@model` : la directive la plus proche de la vue se substitue aux autres

* `@inherits` : la directive la plus proche de la vue se substitue aux autres

* `@using` : toutes les directives sont incluses ; les doublons sont ignorés

* `@inject` : pour chaque propriété, la plus proche de la vue se substitue aux autres propriétés ayant le même nom

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Exécution du code avant chaque vue

Si vous avez du code à exécuter avant chaque vue, vous devez l’ajouter au fichier `_ViewStart.cshtml`. Par convention, le fichier `_ViewStart.cshtml` se trouve dans le dossier `Views`. Les instructions contenues dans `_ViewStart.cshtml` sont exécutées avant chaque vue complète (donc hors dispositions et vues partielles). Comme [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` a une structure hiérarchique. Si un fichier `_ViewStart.cshtml` est défini dans le dossier de vue associé au contrôleur, il est exécuté après celui qui est défini à la racine du dossier `Views` (le cas échéant).

Exemple de fichier `_ViewStart.cshtml` :

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Le fichier ci-dessus spécifie que toutes les vues doivent utiliser la disposition `_Layout.cshtml`.

> [!NOTE]
> Les fichiers `_ViewStart.cshtml` et `_ViewImports.cshtml` ne sont généralement pas placés dans le dossier `/Views/Shared`. Les versions de ces fichiers qui sont au niveau de l’application doivent être placées directement dans le dossier `/Views`.
