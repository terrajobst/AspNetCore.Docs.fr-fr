---
title: "Création de sites attrayants et réactifs avec Bootstrap et ASP.NET Core"
author: ardalis
description: "Découvrez comment utiliser des données d’amorçage pour le développement d’applications web réactives avec ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: c3dfaa53e9e3277d025d014f65004e4c24a5acc4
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Création de sites attrayants et réactifs avec Bootstrap et ASP.NET Core

<a name="bootstrap-index"></a>

Par [Steve Smith](https://ardalis.com/)

Programme d’amorçage est actuellement l’infrastructure web les plus populaires pour le développement d’applications web réactives. Il offre un nombre de fonctionnalités et avantages qui peuvent améliorer l’expérience de vos utilisateurs avec votre site web, si vous êtes un utilisateur débutant à la conception frontale et de développement ou d’un expert. Programme d’amorçage est déployé en tant qu’ensemble de fichiers CSS et JavaScript et est conçu pour évoluer vos application ou un site efficacement des téléphones jusqu’aux tablettes aux ordinateurs de bureau.

## <a name="get-started"></a>Prise en main

Il existe plusieurs façons de démarrer avec Bootstrap. Si vous démarrez une nouvelle application web dans Visual Studio, vous pouvez choisir le modèle de démarrage par défaut pour ASP.NET Core, dans lequel Bootstrap sera préalablement installé :

![Démarrer dans la vue de solution de modèle de démarrage](bootstrap/_static/bootstrap-in-starter-template.png)

Ajouter Bootstrap à un ASP.NET Core projet consiste simplement à l'ajouter en tant que dépendance dans le fichier *bower.json*  :

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Vous pouvez également installer d’amorçage à l’aide de plusieurs gestionnaires de packages, tels que Bower, npm ou NuGeIl s’agit de la méthode recommandée pour ajouter Bootstrap à un projet ASP.NET Core.

Vous pouvez également installer Bootstrap à l’aide de plusieurs gestionnaires de packages, tels que Bower, npm ou NuGet. Dans chaque cas, le processus est essentiellement le même :

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

Si vous souhaitez faire référence à vos propres versions locales de Bootstrap, vous devez les référencer dans toutes les pages qui l’utilisent. En production, vous devez référencer Bootstrap à l’aide d’un CDN. Dans le modèle de site ASP.NET par défaut, le *_Layout.cshtml*  se présente comme ceci :

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Si vous souhaitez utiliser des plug-ins jQuery de Bootstrap, vous devez également référencer jQuery.

## <a name="basic-templates-and-features"></a>Fonctionnalités et modèles de base

Le modèle de projet Bootstrap le plus simple ressemble beaucoup au fichier *_Layout.cshtml* illustré ci-dessus et comprend simplement un menu de base pour la navigation et un emplacement pour restituer le reste de la page.

### <a name="basic-navigation"></a>Navigation de base

Le modèle par défaut utilise un ensemble de `<div>` éléments afin d’afficher une barre de navigation supérieure et le corps de la page. Si vous utilisez HTML5, vous pouvez remplacer la première `<div>` la balise avec un `<nav>` balise pour obtenir le même effet, mais avec une sémantique plus précise. Dans ce premier `<div>` vous pouvez voir d’autres. Tout d’abord, un `<div>` avec une classe de « conteneur », puis, dans, les deux autres `<div>` éléments : « barre de navigation en-tête » et « réduction de la barre de navigation ». L’élément div d’en-tête de la barre de navigation inclut un bouton qui s’affiche lorsque l’écran est inférieure à une certaine largeur minimale, affichant 3 lignes horizontales (un dite « icône représentant un hamburger »). L’icône est restitué à l’aide de pure HTML et CSS ; Aucune image n’est requise. Voici le code qui affiche l’icône, avec chacun de la <span> une des barres blanches de rendu des balises :

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Il inclut également le nom de l’application qui apparaît dans le coin supérieur gauche. Le menu de navigation principal est restitué par le `<ul>` élément dans la deuxième div et inclut des liens pour chez eux, sur et de Contact. Des liens supplémentaires pour inscrire et de connexion sont ajoutés à la ligne _LoginPartial ligne 29. Sous le volet de navigation, le corps principal de chaque page est rendu dans un autre `<div>`, marqués avec les classes « conteneur » et « contenu du corps ». Dans le fichier de _Layout simple par défaut indiqué ici, le contenu de la page est rendu par l’affichage spécifique associé à la page, puis un simple `<footer>` est ajouté à la fin de la `<div>` élément. Vous pouvez voir comment la fonction intégrée sur la page s’affiche à l’aide ce modèle :

![Sur la page](bootstrap/_static/about-page-wide.png)

La barre de navigation réduite, avec un bouton "hamburger" dans le coin supérieur droit s’affiche quand la fenêtre devient inférieure à une certaine largeur :

![à propos de la page avec le menu hamburger](bootstrap/_static/about-page-hamburger.png)

En cliquant sur l’icône pour afficher les éléments de menu dans un tiroir vertical qui glisse vers le bas à partir du haut de la page :

![à propos de la page avec le menu hamburger développé](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Des liens et la typographie

Bootstrap définit la typographie de base, les couleurs et la mise en forme des liens dans son fichier CSS Ce fichier CSS inclut les styles par défaut pour des tableaux, des boutons, des éléments de formulaire, des images et plus encore ([plus](http://getbootstrap.com/css/)). Une fonctionnalité particulièrement utile est le système de disposition par grille expliqué par la suite.

### <a name="grids"></a>Grilles

Une des fonctionnalités plus connues de Bootstrap est son système de mise en page par grille. Dans les applications web modernes, évitez d’utiliser la balise  `<table>` pour la mise en page. au lieu de limiter l’utilisation de cet élément à des données tabulaires réelles. Au lieu de cela, colonnes et lignes peuvent être présentés à l’aide d’une série de `<div>` éléments et les classes CSS appropriées. Il existe plusieurs avantages avec cette approche, comme la possibilité d’ajuster la disposition des grilles pour afficher du contenu verticalement sur des écrans étroits comme ceux des téléphones.


[Le système de présentation en grille de Bootstrap](http://getbootstrap.com/css/#grid) est basée sur douze colonnes. Ce nombre a été choisi, car il peut être divisé pour produire 1, 2, 3 ou 4 colonnes, et les largeurs de colonne peuvent varier sur 1/12 de la largeur verticale de l’écran. Pour commencer à utiliser le système de disposition Grille, vous devez commencer par un conteneur `<div>` , puis ajouter une ligne `<div>`, comme illustré ici :

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Ensuite, ajoutez des éléments `<div>` supplémentaires pour chaque colonne et spécifiez le nombre de colonnes que chaque`<div>` doit occuper (sur 12) à l'aide d’une classe CSS commençant par  « col - md- ». Par exemple, si vous souhaitez simplement déclarer deux colonnes de taille égale, vous utiliserez une classe « col-md-6 » pour chacune d’entre elles. Dans ce cas, « md » est l’abréviation de "medium" et fait référence à des tailles d’affichage de format standard pour ordinateurs de bureau. Il existe quatre options différentes, parmi lesquelles vous pouvez choisir, et chacune sera utilisée pour les largeurs d'écran supérieures sauf surcharge (si vous souhaitez que la mise en page reste fixe, quelle que soit la largeur de l’écran, vous pouvez spécifier seulement des classes xs).

Préfixe de classe CSS | Niveau de l’appareil | Largeur
:---: | :---: | :---:
col-xs- | Téléphones | < 768px
col-sm - | Tablettes | > = 768px
col-md - | Ordinateurs de bureau | > = 992px
col-lg - | Affiche de bureau plus grande | > = 1200px

Lors de la spécification de deux colonnes avec "col-md-6", la mise en page qui en résulte consiste en deux colonnes avec des résolutions pour poste de travail, mais ces deux colonnes seront empilées verticalement lors de l’affichage sur des appareils plus petits (ou dans une fenêtre de navigateur plus étroite sur un poste de travail), permettant aux utilisateurs d’afficher facilement du contenu sans devoir faire défiler horizontalement.

Bootstrap utilise toujours par défaut une disposition à une seule colonne : vous devez donc spécifier des colonnes seulement quand vous voulez avoir plusieurs colonnes. Le seul cas où vous devez spécifier explicitement qu’un`<div>`occupe les 12 colonnes est pour remplacer le comportement d’un niveau pour un  appareil plus grand. Lorsque vous spécifiez plusieurs classes de niveau de périphérique, vous devrez peut-être réinitialiser le rendu des colonnes à certains points.  L'ajout d’un élément clearfix div visible seulement dans une certaine fenêtre d’affichage permet cela, comme montré ici :

![grille de la fenêtre d’affichage étroites et étendues](bootstrap/_static/narrow-and-wide-viewport-grid.png)

Dans l’exemple ci-dessus, les cellules One et Two partagent une ligne avec la disposition "md", tandis que les cellules Two et Three partagent une ligne avec la disposition "xs". Sans le clearfix `<div>`, les cellules Two et Three ne sont pas affichées correctement dans la vue "xs" (Notez que seul les cellules Four et Five sont affichées) :

![grille sans utiliser de clearfix](bootstrap/_static/grid-without-clearfix.png)

Dans cet exemple, une seule ligne `<div>` a été utilisée, et Bootstrap a fait ce qu’il fallait pour la mise en page et l’empilement des colonnes. En règle générale, vous devez spécifier une ligne `<div>` pour chaque ligne horizontale nécessaire à la mise en page, et vous pouvez bien sûr imbriquer des grilles Bootstrap l’une dans l’autre. Quand vous procédez ainsi, chaque grille imbriquée occupe 100 % de la largeur de l’élément où elle est placée, qui peut ensuite être subdivisé avec des classes de colonne.

### <a name="jumbotron"></a>Jumbotron

Si vous avez utilisé des modèles ASP.NET MVC par défaut dans Visual Studio 2012 ou 2013, vous avez probablement remarqué le Jumbotron en action. Il fait référence à une section de grande taille en pleine largeur d’une page, qui peut être utilisée pour afficher une image d’arrière-plan de grande taille, un appel à une action, un rotateur ou des éléments similaires. Pour ajouter un jumbotron à une page, ajoutez simplement un `<div>` et donnez lui une classe `<div>` puis placez un conteneur Nous pouvons ajuster facilement la page About pour qu'elle utilise un jumbotron pour afficher les titres principaux :

![exemple de JumboTron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Boutons

Les classes de bouton par défaut et leurs couleurs sont affichés dans la figure ci-dessous.

![boutons à thème](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Badges

Badges font référence aux légendes de petite taille, généralement numériques en regard d’un élément de navigation. Ils peuvent indiquer un nombre de messages ou des notifications en attente ou la présence de mises à jour. En spécifiant ces badges est aussi simple que l’ajout d’un <span> contenant le texte, avec une classe de « badge » :

![badges à thème](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertes

Vous devrez peut-être afficher un type de notification, une alerte ou message d’erreur pour les utilisateurs de votre application. C'est-à-dire, où les classes d’alerte standards sont utiles. Il existe quatre niveaux de gravité différents jeux de couleurs associé :

![alertes à thème](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menus et barres de navigation

Notre disposition inclut déjà une barre de navigation standard, mais le thème d’amorçage prend en charge les options de style supplémentaires. Nous pouvons également facilement choisir d’afficher la barre de navigation verticalement plutôt que horizontalement si qui a par défaut, ainsi que l’ajout sous éléments de navigation dans les menus volants. Menus de navigation simple comme onglet bandes, reposent sur des <ul> éléments. Vous pouvez créer très simplement en leur permettant uniquement avec les classes CSS « nav » et « nav onglets » :

![poste à thème](bootstrap/_static/theme-tabstrips.png)

Barres de navigation sont générés de la même façon, mais sont un peu plus complexes. Ils commencent par un `<nav>` ou `<div>` avec une classe de « barre de navigation », dans lequel un élément div conteneur conserve le reste des éléments. Notre page inclut déjà une barre de navigation dans son en-tête, celui illustré ci-dessous se développe simplement à ce sujet, ajout de la prise en charge pour un menu déroulant :

![barres de navigation à thème](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Éléments supplémentaires

Le thème par défaut peut également servir à présenter des tableaux HTML dans un style de mise en forme correcte, y compris la prise en charge pour les vues distribuées. Il existe des étiquettes avec des styles sont semblables à celles des boutons. Vous pouvez créer des menus déroulants personnalisés qui prennent en charge les options de style supplémentaires au-delà de la norme HTML `<select>` élément, ainsi que les barres de navigation semblable à celui de notre site de démarrage par défaut est déjà utilisé. Si vous avez besoin d’une barre de progression, il existe plusieurs styles à sélectionner, ainsi que la liste des groupes et des panneaux d’incluent un titre et le contenu. Explorer les options supplémentaires dans le thème d’amorçage standard ici :

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Plus de thèmes

Vous pouvez étendre le thème d’amorçage standard en remplaçant tout ou partie de ses CSS, régler les couleurs et les styles selon les besoins de votre propre application. Si vous souhaitez démarrer à partir d’un thème prêts à l’emploi, il existe plusieurs galeries de thème disponibles en ligne spécialisés dans les thèmes d’amorçage, telles que WrapBootstrap.com (qui possède un éventail de thèmes commerciales) et Bootswatch.com (qui offre des thèmes libres). Certains des modèles disponibles payants fournissent un large éventail de fonctionnalités par-dessus le thème d’amorçage base, tels que prise en charge des menus d’administration et des tableaux de bord avec des jauges et graphiques enrichis. Est un exemple d’un modèle payant populaires Inspinia, actuellement vente pour $18, qui inclut un modèle MVC5 ASP.NET outre AngularJS et les versions HTML statiques. Vous trouverez ci-dessous une capture d’écran de l’exemple.

![Exemple thème inspinia](bootstrap/_static/theme-inspinia.png)

Si vous souhaitez modifier le thème d’amorçage, placez le *bootstrap.css* fichier pour le thème que vous souhaitez dans le **wwwroot/css** dossier et modifiez les références dans *_Layout.cshtml* afin qu’il pointe. Modifier les liens pour tous les environnements :

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Si vous souhaitez créer votre propre tableau de bord, vous pouvez démarrer à partir de l’exemple libre disponible ici : [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Composants

En plus de ces éléments déjà mentionnés, amorçage inclut la prise en charge pour un large éventail de [composants d’interface utilisateur intégrées](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Amorçage inclut les jeux d’icônes à partir de Glyphicons ([http://glyphicons.com](http://glyphicons.com)), avec des icônes plus de 200 disponibles gratuitement pour une utilisation dans votre application web compatible d’amorçage. Voici juste un petit échantillon :

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>groupes d’entrée

Groupes d’entrée autoriser le regroupement de texte ou des boutons à un élément d’entrée, en fournissant une expérience plus intuitive à l’utilisateur :

![groupes d’entrée](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Vues miniatures

Vues miniatures sont un composant de l’interface utilisateur commun utilisé pour montrer leur historique récent ou la profondeur dans la hiérarchie de navigation d’un site à l’utilisateur. Ajoutez-les facilement en appliquant la classe « fil d’Ariane » aux `<ol>` élément de liste. Inclure la prise en charge intégrée pour la pagination à l’aide de la classe « pagination » sur un `<ul>` élément au sein d’un `<nav>`. Ajouter des diaporamas incorporé réactive et vidéo à l’aide de `<iframe>`, `<embed>`, `<video>`, ou `<object>` éléments dont le programme d’amorçage sera style automatiquement. Spécifiez un format particulier à l’aide des classes spécifiques, comme « incorporer-réactive-16by9 ».

## <a name="javascript-support"></a>Prise en charge de JavaScript

Bibliothèque de JavaScript du programme d’amorçage inclut la prise en charge API pour les composants inclus, ce qui vous permet de contrôler leur comportement par programme au sein de votre application. En outre, *bootstrap.js* inclut plus d’une dizaine plug-ins de jQuery personnalisées, en fournissant des fonctionnalités supplémentaires, comme des transitions, boîtes de dialogue modales, faites défiler la détection (où l’utilisateur a défilé dans le document en fonction des styles de mise à jour), comportement de réduction, tapis roulants et menus apposition dans la fenêtre de sorte qu’ils ne défilent pas hors de l’écran. Il n’existe pas de suffisamment d’espace pour couvrir tous les modules complémentaires JavaScript intégrés Bootstrap – pour en savoir plus, consultez [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Récapitulatif

Programme d’amorçage fournit une infrastructure web qui peut être utilisée pour rapidement et efficacement mise en page et le style d’un large éventail d’applications et sites Web. Typographie de base et des styles fournissent une apparence et convivialité agréable qui peuvent être manipulée aisément prise en charge du thème personnalisé qui peut être écrit à la main ou acheté dans le commerce. Il prend en charge un ordinateur hôte des composants web qui serait avez requise coûteuse des contrôles tiers pour mener à bien, lors de la prise en charge des normes web modernes et ouvertes dans le passé.
