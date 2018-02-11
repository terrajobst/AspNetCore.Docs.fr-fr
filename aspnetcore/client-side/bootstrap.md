---
title: "Création de sites web attrayants et adaptatifs avec Bootstrap"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: dfed5c7a8e103973048295b7607008ecc7e90eeb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Création de sites web attrayants et adaptatifs avec Bootstrap

<a name="bootstrap-index"></a>

Par [Steve Smith](https://ardalis.com/)

Bootstrap est actuellement le framework le plus populaire pour le développement d’applications web adaptatives. Il offre un nombre de fonctionnalités et avantages qui peuvent améliorer l’expérience de vos utilisateurs avec votre site web, que vous soyez un débutant dans la conception frontale ou le développement ou un expert. Bootstrap est déployé en tant qu’ensemble de fichiers CSS et JavaScript et est conçu pour permettre à vos applications ou sites web de s'adapter efficacement sur les téléphones jusqu’aux tablettes aux ordinateurs de bureau.

## <a name="getting-started"></a>Bien démarrer

Il existe plusieurs façons de prise en main Bootstrap. Si vous démarrez une nouvelle application web dans Visual Studio, vous pouvez choisir le modèle de démarrage par défaut pour ASP.NET Core, dans lequel Bootstrap sera préalablement installé :

![Démarrer dans la vue de solution de modèle de démarrage](bootstrap/_static/bootstrap-in-starter-template.png)

Ajouter Bootstrap à un ASP.NET Core projet consiste simplement à l'ajouter en tant que dépendance dans le fichier *bower.json* :

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Il s’agit de la méthode recommandée pour ajouter Bootstrap à un projet ASP.NET Core.

Vous pouvez également installer Bootstrap à l’aide de plusieurs gestionnaires de packages, tels que Bower, npm ou NuGet. Dans chaque cas, le processus est essentiellement le même :

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> La méthode recommandée pour installer des dépendances côté client comme Bootstrap dans ASP.NET Core est celle via Bower (à l’aide de *bower.json*, comme indiqué ci-dessus). L’utilisation de npm/NuGet sont affichés pour illustrer comment Bootstrap peut être ajouté facilemet à d’autres types d’applications web, y compris les versions antérieures d’ASP.NET.

Si vous souhaitez faire référence à vos propres versions locales de Bootstrap, vous devez les référencer dans toutes les pages qui l’utilisent. En production, vous devez référencer Bootstrap à l’aide d’un CDN. Dans le modèle de site ASP.NET par défaut, le fichier *_Layout.cshtml* se présente comme ceci :

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Si vous souhaitez utiliser des plug-ins jQuery de Bootstrap, vous devez également référencer jQuery.

## <a name="basic-templates-and-features"></a>Fonctionnalités et modèles de base

Le modèle de projet Bootstrap le plus simple ressemble beaucoup au fichier *_Layout.cshtml* illustré ci-dessus et comprend simplement un menu de base pour la navigation et un emplacement pour restituer le reste de la page.

### <a name="basic-navigation"></a>Navigation de base

Le modèle par défaut utilise un ensemble d'éléments `<div>`  afin d’afficher une barre de navigation supérieure et le corps de la page. Si vous utilisez HTML5, vous pouvez remplacer la première balise `<div>` avec une balise `<nav>` pour obtenir le même effet, mais avec une sémantique plus précise. Dans le premier élément `<div>` vous pouvez en voir d’autres. Tout d’abord, un `<div>` avec une classe "container", puis deux autres éléments `<div>` à l'intérieur avec les classes respectives "navbar-header" et "navbar-collapse". L’élément div d’en-tête de la barre de navigation inclut un bouton affichant 3 lignes horizontales (habituellement appelé icône "hamburger") qui apparaît lorsque la taille d'écran ou de fenêtre est inférieure à une certaine largeur minimale. L’icône hamburger est restitué à l’aide de pure HTML et CSS : aucune image n’est requise. Voici le code qui permet d'afficher l’icône hamburger avec chaque balise <span> responsable du rendu d'une des barres blanches :

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

La barre de navigation inclut également le nom de l’application qui apparaît dans le coin supérieur gauche. Le menu de navigation principal est restitué par l'élément `<ul>` dans la deuxième div et inclut des liens vers les pages Home, About et Contact. Des liens supplémentaires vers les pages Register et Login sont ajoutés grâce la ligne _LoginPartial à la ligne 29. Sous la navigation, le corps principal de chaque page est rendu dans un autre `<div>`, marqués avec les classes "container" et "body-content". Dans le fichier par défaut _Layout montré ici, le contenu de la page est rendu par une vue spécifique associé à la page, puis un simple `<footer>` est ajouté à la fin de l'élément `<div>`. Vous pouvez voir ici comment la page About s’affiche à l’aide ce modèle :

![Sur la page](bootstrap/_static/about-page-wide.png)

La barre de navigation réduite, avec un bouton "hamburger" dans le coin supérieur droit s’affiche quand la fenêtre devient inférieure à une certaine largeur :

![sur la page avec le menu hamburger](bootstrap/_static/about-page-hamburger.png)

Cliquer sur l’icône pour afficher les éléments de menu dans une direction verticale qui glisse du haut de la page vers le bas :

![sur la page avec hamburger menu développé](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Des liens et la typographie

Bootstrap définit la typographie de base, les couleurs et la mise en forme des liens dans son fichier CSS. Ce fichier CSS inclut les styles par défaut pour des tableaux, des boutons, des éléments de formulaire, des images et plus encore ([plus](http://getbootstrap.com/css/)). Une fonctionnalité particulièrement utile est le système de disposition par grille expliqué par la suite.

### <a name="grids"></a>Grilles

Une des fonctionnalités plus populaires d’amorçage est son système de mise en page par grille. Dans les applications web modernes, évitez d’utiliser la balise `<table>` pour la mise en page. Limitez l’utilisation de cet élément aux données tabulaires réelles. Au lieu de cela, colonnes et lignes peuvent être présentés à l’aide d’une série d'éléments `<div>` et les classes CSS appropriées. Il existe plusieurs avantages avec cette approche comme la possibilité d’ajuster la disposition des grilles pour afficher du contenu verticalement sur les écrans étroits comme ceux des téléphones.

[Le système de grille de Bootstrap](http://getbootstrap.com/css/#grid) est basée sur douze colonnes. Ce nombre a été choisi, car il peut être divisée uniformément en 1, 2, 3 ou 4 colonnes et les largeurs de colonne peuvent varier sur 1/12 de la largeur verticale de l’écran. Pour commencer à utiliser le système de grille, vous devez commencer par un conteneur `<div>` , puis ajoutez une ligne `<div>`, comme illustré ici :

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Ensuite, ajoutez des éléments `<div>` supplémentaires pour chaque colonne et spécifiez le nombre de colonnes que chaque `<div>` doit occuper (hors 12) à l'aide d’une classe CSS commençant par "col- md-". Par exemple, si vous souhaitez simplement déclarer deux colonnes de taille égale, vous utiliserez une classe "col-md-6" pour chacun d’entre elles. Dans ce cas, "md" est l’abréviation de "medium" et fait référence à des tailles d’affichage de format standard pour ordinateurs de bureau. Il existe quatre options différentes, parmi lesquelles vous pouvez choisir, et chacune sera utilisé pour les largeurs d'écran supérieures sauf surcharge (si vous souhaitez que la mise en page reste fixe, quelle que soit la largeur de l’écran, il vous suffit d'utiliser uniquement que les classes xs).

Préfixe de classe CSS | Niveau de l’appareil | Largeur
:---: | :---: | :---:
col-xs- | Téléphones | < 768px
col-sm - | Tablettes | > = 768px
col-md - | Ordinateurs de bureau | > = 992px
col-lg - | Affiche de bureau plus grande | > = 1200px

Ainsi lors de la spécification de deux colonnes avec "col-md-6", la mise en page qui en résulte consiste en deux colonnes avec des résolutions de bureau, mais ces deux colonnes seront empilées verticalement lors du rendu sur les périphériques de petite taille (ou une fenêtre de navigateur plus étroite sur un ordinateur de bureau), permettant aux utilisateurs d’afficher facilement contenu sans avoir besoin de faire défiler horizontalement.

Bootstrap utilise toujours par défaut à une disposition à colonne unique, vous devez uniquement spécifier des colonnes lorsque vous souhaitez avoir plusieurs colonnes. Le seul cas où vous devez spécifier explicitement qu’un `<div>` occupent les 12 colonnes est utilisé pour substituer le comportement d’un périphérique à écran large. Lorsque vous spécifiez plusieurs classes de niveau de périphérique, vous devrez peut-être réinitialiser le rendu des colonnes à certains points. L'ajout d’un élément div clearfix qui est uniquement visible dans une certaine fenêtre d’affichage permet d'effectuer cette opération, comme indiqué ici :

![grille de la fenêtre d’affichage étroites et étendues](bootstrap/_static/narrow-and-wide-viewport-grid.png)

Dans l’exemple ci-dessus, les cellules 1 et 2 partagent une ligne avec la disposition "md", tandis que les cellules 2 et 3 partagent une ligne avec la disposition "xs". Sans le clearfix `<div>`, les cellules 2 et 3 ne sont pas affichées correctement dans la vue "xs" (Notez que seul les cellules 4 et 5 sont affichées) :

![grille sans utiliser de clearfix](bootstrap/_static/grid-without-clearfix.png)

Dans cet exemple, une seule ligne `<div>` a été utilisée, et Bootstrap se charge d'effectuer les opérations concernant la mise en page et l’empilement des colonnes. En règle générale, vous devez spécifier une ligne `<div>` pour chaque ligne horizontale nécessaire à la mise en page, et bien entendu, vous pouvez imbriquer des grilles Bootstrap à l'intérieur d'une autre grille. Lorsque vous procédez ainsi, chaque grille imbriquée occupera 100 % de la largeur de l’élément dans lequel elle est placée, qui peut ensuite être subdivisée à l’aide des classes de colonne.

### <a name="jumbotron"></a>Jumbotron

Si vous avez utilisé les modèles ASP.NET MVC par défaut dans Visual Studio 2012 ou 2013, vous avez probablement remarqué le Jumbotron en action. Il fait référence à une section pleine et volumineuse d’une page qui peut être utilisée pour afficher une image d’arrière-plan de grande taille, une invitation à une action spécifique, un rotateur ou des éléments similaires. Pour ajouter un jumbotron à une page, ajoutez simplement un `<div>` et donnez lui une classe de "jumbotron", puis placez un conteneur `<div>` à l’intérieur et ajoutez votre contenu. Nous pouvons ajuster facilement la page About pour qu'elle utilise un jumbotron pour afficher les titres principaux :

![exemple de JumboTron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Boutons

Les classes de bouton par défaut et leurs couleurs sont affichése dans la figure ci-dessous.

![boutons à thème](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Badges

Las badges font référence aux légendes de petite taille, généralement numériques en regard d’un élément de navigation. Ils peuvent indiquer un nombre de messages ou des notifications en attente ou la présence de mises à jour. Spécifier un badges est aussi simple que l’ajout d’un <span> contenant le texte, avec une classe de "badge" :

![badges à thème](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertes

Vous devrez peut-être afficher un type de notification, une alerte ou message d’erreur pour les utilisateurs de votre application. Dans de tels cas, les classes d’alerte standards sont utiles. Il existe quatre niveaux de gravité différents avec jeux de couleurs associé :

![alertes à thème](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menus et barres de navigation

Notre disposition inclut déjà une barre de navigation standard, mais le thème Bootstrap prend en charge des options de style supplémentaires. Nous pouvons également facilement choisir d’afficher la barre de navigation verticalement plutôt que horizontalement, ainsi qu'ajouter des sous-éléments de navigation dans les menus glissants. Les menus de navigation simple comme les barres d'onglets, reposent sur des éléments <ul>. Vous pouvez créer ces derniers très simplement en leur affectant uniquement les classes CSS "nav" et "nav-tabs" :

![poste à thème](bootstrap/_static/theme-tabstrips.png)

Les barres de navigation sont générées de la même façon, mais sont un peu plus complexes. Elles commencent par un élément `<nav>` ou `<div>` avec une classe de "nav-bar", dans lequel un élément div avec la classe "container" conserve le reste des éléments. Notre page inclut déjà une barre de navigation dans son en-tête, celui illustré ci-dessous ajoute simplement la prise en charge d'un menu déroulant :

![barres de navigation à thème](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Éléments supplémentaires

Le thème par défaut peut également servir à présenter des tableaux HTML dans un style de mise en forme correcte, y compris la prise en charge pour les vues distribuées. Il existe des étiquettes avec des styles qui sont semblables à celles des boutons. Vous pouvez créer des menus déroulants personnalisés qui prennent en charge les options de style supplémentaires au-delà de la norme HTML de l'élément `<select>`, ainsi que des barres de navigation semblables à celui de notre site de démarrage utilisé par défaut ici. Si vous avez besoin d’une barre de progression, il existe plusieurs styles à sélectionner, ainsi que pour des listes de groupe et des panneaux incluant un titre et un contenu. Pour explorer les options supplémentaires dans le thème Bootstrap standard, consultez ici :

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Plus de thèmes

Vous pouvez étendre le thème Bootstrap standard en remplaçant tout ou partie de ses styles CSS, régler les couleurs et les styles selon les besoins de votre application. Si vous souhaitez démarrer à partir d’un thème prêt à l’emploi, il existe plusieurs galeries de thème disponibles en ligne spécialisés dans les thèmes Bootstrap, telles que WrapBootstrap.com (qui possède un éventail de thèmes commercials) et Bootswatch.com (qui offre des thèmes libres). Certains des modèles disponibles payants fournissent un large éventail de fonctionnalités par-dessus le thème Bootstrap base, tels que prise en charge des menus d’administration et des tableaux de bord avec des jauges et des graphiques enrichis. Inspinia est un exemple de modèle payant populaire, actuellement en vente pour $18, qui inclut un modèle MVC5 ASP.NET avec AngularJS et les versions HTML statiques. Vous trouverez ci-dessous une capture d’écran de l’exemple.

![Exemple thème inspinia](bootstrap/_static/theme-inspinia.png)

Si vous souhaitez modifier le thème d’amorçage, placez le fichier *bootstrap.css* pour le thème que vous souhaitez dans le dossier **wwwroot/css** et modifiez les références dans le fichier *_Layout.cshtml* afin qu’il pointe dessus. Modifiez les liens pour tous les environnements :

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Si vous souhaitez créer votre propre tableau de bord, vous pouvez démarrer à partir de l’exemple libre disponible ici : [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Composants

En plus de ces éléments déjà mentionnés, Bootstrap inclut la prise en charge d'un large éventail de [composants d’interface utilisateur intégrés](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap inclut les jeux d’icônes à partir de Glyphicons ([http://glyphicons.com](http://glyphicons.com)) avec plus de 200 icônes disponibles gratuitement pour une utilisation dans votre application web compatible Bootstrap. Voici juste un petit échantillon :

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Groupes d'entrée

Les groupes d'entrée autorisent le regroupement de texte ou de boutons avec un élément d'entrée, en fournissant une expérience plus intuitive à l’utilisateur :

![groupes d’entrée](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Le fil d'Ariane

Le fil d'Ariane est un composant commun de l’interface utilisateur utilisé pour montrer à l’utilisateur l'historique de navigation récent ou la profondeur dans la hiérarchie de navigation d’un site. Vous pouvez ajouter ce composant facilement en appliquant la classe "breadcrumb" à un élément de liste `<ol>`. Vous pouvez également inclure la prise en charge intégrée pour la pagination à l’aide de la classe "pagination" sur un élément `<ul>` au sein d’un élément `<nav>`. Enfin vous pouvez incorporer des diaporamas ou des vidéos adaptatifs à l’aide des éléments `<iframe>`, `<embed>`, `<video>`, ou `<object>` dont Bootstrap assurera le style automatiquement. Vous pouvez spécifiez un format particulier à l’aide de classes spécifiques, comme "embed-responsive-16by9".

## <a name="javascript-support"></a>Prise en charge de JavaScript

Les bibliothèques JavaScript de Bootstrap incluent la prise en charge de l'API pour les composants inclus, ce qui vous permet de contrôler leur comportement par programme au sein de votre application. En outre, *bootstrap.js* inclut plus d’une dizaine plug-ins de jQuery personnalisés, en fournissant des fonctionnalités supplémentaires, comme des transitions, des boîtes de dialogue modales, la détection du scroll (où l’utilisateur a défilé dans le document), des fonctions de réduction, des composants de caroussels et des menus adaptables à la fenêtre de sorte qu’ils ne défilent pas hors de l’écran. Pour découvrir tous les modules complémentaires JavaScript intégrés à Bootstrap, consultez [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Récapitulatif

Bootstrap fournit un framework de développement web qui peut être utilisée pour définir rapidement et efficacement la mise en page et le style de vos applications et sites Web. La typographie de base et les styles fournissent une apparence agréable et peuvent être manipulée aisément pour la prise en charge d'un thème personnalisé qui peut être développé manuellement ou acheté dans le commerce. Bootstrap prend en charge des composants web avancés qui étaient autrefois coûteux à acquérir auprès d'un vendeur tiers tout prenant en charge des normes web modernes et ouvertes.
