---
title: Création de sites attrayants et réactifs avec Bootstrap
=======
author: ardalis
description: Découvrez comment utiliser des données d’amorçage pour le développement d’applications web réactives avec ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bootstrap
ms.openlocfilehash: a11ed13c709830795ebfd0e658d3f2fd2fd5a458
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Créer des sites attrayants et réactifs avec Bootstrap

<a name="bootstrap-index"></a>

Par [Steve Smith](https://ardalis.com/)

Bootstrap est actuellement le framework web le plus populaire pour le développement d’applications web responsives. Il offre un nombre de fonctionnalités et d'avantages qui peuvent améliorer l’expérience de vos utilisateurs avec votre site web, que vous soyez un utilisateur débutant dans le design et le développement front-end ou un expert. Bootstrap est déployé en tant qu’ensemble de fichiers CSS et JavaScript et est conçu pour évoluer vos application ou un site efficacement des téléphones jusqu’aux tablettes et aux ordinateurs de bureau.

## <a name="getting-started"></a>Bien démarrer

Il existe plusieurs façons de démarrer avec Bootstrap. Si vous démarrez une nouvelle application web dans Visual Studio, vous pouvez choisir le modèle de démarrage par défaut pour ASP.NET Core, dans lequel Bootstrap sera préalablement installé :

![Démarrer dans la vue de solution de modèle de démarrage](bootstrap/_static/bootstrap-in-starter-template.png)

Ajouter Bootstrap à un projet ASP.NET Core consiste simplement à ajouter *bower.json* en tant que dépendance :

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Il s’agit de la méthode recommandée pour ajouter Bootstrap à un projet ASP.NET Core.

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
> La méthode recommandée pour installer des dépendances côté client comme Bootstrap dans ASP.NET Core est via Bower (à l’aide de *bower.json*, comme indiqué ci-dessus). L’utilisation de npm/NuGet est affichée pour illustrer comment Bootstrap peut facilement être ajouté à d’autres types d’applications web, y compris les versions antérieures d’ASP.NET.

Si vous faites référence à vos propres versions locales de Bootstrap, vous devez les référencer dans toutes les pages qui l’utilisent. En production, vous devez référencer Bootstrap à l’aide d’un CDN. Dans le modèle de site ASP.NET par défaut, le fichier *_Layout.cshtml* fait donc comme ceci :

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Si vous souhaitez utiliser des plug-ins de jQuery Bootstrap, vous devez également référencer jQuery.

## <a name="basic-templates-and-features"></a>Fonctionnalités et modèles de base

Le modèle de démarrage plus simple ressemble beaucoup au fichier *_Layout.cshtml* illustré ci-dessus et comprend simplement un menu de base pour la navigation et un emplacement pour restituer le reste de la page. 

### <a name="basic-navigation"></a>Navigation de base

Le modèle par défaut utilise un ensemble d'éléments `<div>` afin d’afficher une barre de navigation supérieure et le corps de la page. Si vous utilisez HTML5, vous pouvez remplacer la première balise `<div>` avec une balise `<nav>` pour obtenir le même effet, mais avec une sémantique plus précise. Dans ce premier `<div>` vous pouvez en voir d’autres. Tout d’abord, un `<div>` avec une classe de "container", puis, dans les deux autres éléments `<div>` : "navbar-header" et "navbar-collapse". L’élément div navbar-header inclut un bouton qui s’affiche lorsque l’écran est inférieure à une certaine largeur minimale, affichant 3 lignes horizontales (dite "icône hamburger"). L’icône est restituée à l’aide de pure HTML et CSS ; Aucune image n’est requise. Voici le code qui affiche l’icône, avec chacun des balises <span> affichant une des barres blanches :

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Il inclut également le nom de l’application qui apparaît dans le coin supérieur gauche. Le menu de navigation principal est restitué par l'élément `<ul>` dans la deuxième div et inclut des liens pour Home, About, et Contact. Des liens supplémentaires pour s'inscrire et se connecter sont ajoutés à _LoginPartial ligne 29. Sous le volet de navigation, le corps principal de chaque page est rendu dans un autre `<div>`, marqués avec les classes "container" et "body-content". Dans le fichier _Layout simple par défaut indiqué ici, le contenu de la page est rendu par l’affichage spécifique associé à la page, puis un simple `<footer>` est ajouté à la fin de l'élément `<div>`. Vous pouvez voir comment la page About s’affiche à l’aide de ce modèle :

![Sur la page](bootstrap/_static/about-page-wide.png)

La barre de navigation réduite, avec un bouton affichant 3 lignes horizontales (appelé style "hamburger") dans le coin supérieur droit, s’affiche quand la fenêtre est inférieure à une certaine largeur : 

![sur la page avec hamburger menu](bootstrap/_static/about-page-hamburger.png)

Le fait de cliquer sur l’icône affiche les éléments de menu dans un développement vertical qui glisse vers le bas à partir du haut de la page : 

![sur la page avec hamburger menu développé](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Typographie et liens 

Bootstrap définit la typographie de base, les couleurs et la mise en forme des liens du site dans son fichier CSS. Ce fichier CSS inclut les styles par défaut pour les tables, les boutons, les éléments de formulaire, les images et plus ([en savoir plus](http://getbootstrap.com/css/)). Une fonctionnalité particulièrement utile est le système de disposition par grille, traité ci-après. 

### <a name="grids"></a>Grilles

Une des fonctionnalités plus populaires de Bootstrap est son système de mise en page par grille. Les applications web modernes devraient évitez d’utiliser la balise `<table>` pour la mise en page, en limitant l’utilisation de cet élément aux données tabulaires réelles. Au lieu de cela, les colonnes et lignes peuvent être présentées à l’aide d’une série d'éléments `<div>` et de classes CSS appropriées. Il existe plusieurs avantages de cette approche, y compris la possibilité d’ajuster la disposition des grilles pour s'afficher verticalement sur les écrans étroits, comme sur les téléphones.

[Le système de disposition par grille de Bootstrap](http://getbootstrap.com/css/#grid) est basé sur douze colonnes. Ce nombre a été choisi, car il peut être divisé uniformément par 1, 2, 3 ou 4 colonnes et les largeurs de colonne peuvent varier de 1/12 de la largeur verticale de l’écran. Pour commencer à utiliser le système de disposition par grille, vous devez commencer par un conteneur `<div>`, puis ajouter une ligne `<div>`, comme illustré ici : 

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Ensuite, ajoutez des éléments `<div>` supplémentaires pour chaque colonne et spécifiez le nombre de colonnes que le `<div>` doit occuper (parmi 12) dans le cadre d’une classe CSS en commençant par "col-md-". Par exemple, si vous souhaitez simplement comporter deux colonnes de taille égale, vous utiliseriez une classe de "col-md-6" pour chacun d’eux. Dans ce cas, "md" est l’abréviation de "medium" et fait référence à des tailles d’affichage de format d'ordinateur de bureau standard. Il existe quatre options différentes, que vous pouvez choisir parmi elles, et chacune sera utilisé pour les largeurs supérieures sauf substitution (de sorte que si vous souhaitez que la mise en page soit fixe, quelle que soit la largeur de l’écran, il vous suffit de spécifier les classes xs).

Préfixe de classe CSS | Niveau de l’appareil | Largeur
:---: | :---: | :---:
col-xs- | Téléphones | < 768px
col-sm - | Tablettes | > = 768px
col-md - | Ordinateurs de bureau | > = 992px
col-lg - | Affiche de bureau plus grande | > = 1200px

Lors de la spécification des deux colonnes avec "col-md-6", la mise en page obtenue sera constituée de deux colonnes avec des résolutions de bureau, mais ces deux colonnes seront empilées verticalement lors du rendu sur les appareils de petite taille (ou une fenêtre de navigateur plus étroite sur un ordinateur de bureau), permettant aux utilisateurs d’afficher facilement le contenu sans avoir besoin de faire défiler horizontalement. 

Bootstrap aura toujours par défaut une disposition de colonne unique, vous devez uniquement spécifier des colonnes lorsque vous souhaitez plusieurs colonnes. Le seul moment où vous ne souhaitez pas spécifier explicitement qu’un `<div>` occupent toutes les 12 colonnes serait pour substituer le comportement d’un plus grand niveau de périphérique. Lorsque vous spécifiez plusieurs classes de niveau de périphérique, vous devrez peut-être réinitialiser le rendu des colonnes à certains points. l'ajout d’un élément div clearfix qui est uniquement visible dans une certaine fenêtre d’affichage permet d'effectuer cette opération, comme indiqué ici :

![grille de la fenêtre d’affichage étroites et étendues](bootstrap/_static/narrow-and-wide-viewport-grid.png)

Dans l’exemple ci-dessus, One et Two partagent une ligne dans la disposition "md", tandis que Two et Three partagent une ligne dans la disposition "xs". Sans le clearfix `<div>`, Two et Three ne sont pas affichés correctement dans la vue "xs" (notez que seul One, Four et Five sont affichés) : 

![Grille sans utiliser de clearfix](bootstrap/_static/grid-without-clearfix.png)

Dans cet exemple, une seule ligne `<div>` a été utilisée, et Bootstrap a le comportement approprié en ce qui concerne la mise en page et l’empilement des colonnes. En règle générale, vous devez spécifier une ligne `<div>` pour chaque ligne horizontale que la mise en page nécessite, et bien entendu, vous pouvez imbriquer des grilles Bootstrap dans und autre. Lorsque vous procédez ainsi, chaque grille imbriquée occupera 100 % de la largeur de l’élément sur lequel il est placé, qui peut ensuite être subdivisée à l’aide des classes de colonne.

### <a name="jumbotron"></a>Jumbotron

Si vous avez utilisé les modèles ASP.NET MVC par défaut dans Visual Studio 2012 ou 2013, vous avez probablement remarqué le Jumbotron en action. Il fait référence à une section d’une page qui prend toute la largeur qui peut être utilisée pour afficher une image d’arrière-plan de grande taille, un call to action, une rotation ou des éléments similaires. Pour ajouter un jumbotron à une page, ajoutez simplement un `<div>` et lui donner une classe de "jumbotron", puis placer un `<div>` container à l’intérieur et ajoutez votre contenu. Nous pouvons facilement ajuster la page About pour utiliser un jumbotron pour les titres principaux qu'elle affiche :

![exemple de JumboTron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Boutons

Les classes de bouton par défaut et leurs couleurs sont affichés dans la figure ci-dessous.

![boutons à thème](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Badges

Les badges font référence aux légendes de petite taille, généralement numériques en regard d’un élément de navigation. Ils peuvent indiquer un nombre de messages ou des notifications en attente ou la présence de mises à jour. Spécifier ces badges est aussi simple que l’ajout d’un <span> contenant le texte, avec une classe de "badge" :

![badges à thème](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertes

Vous devrez peut-être afficher un type de notification, une alerte ou un message d’erreur pour les utilisateurs de votre application. C'est là où les classes d’alerte standard sont utiles. Il existe quatre niveaux de gravité différents avec des jeux de couleurs associés :

![alertes](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menus et barres de navigation

Notre disposition inclut déjà une barre de navigation standard, mais le thème Bootstrap prend en charge les options de style supplémentaires. Nous pouvons également facilement choisir d’afficher la barre de navigation verticalement plutôt que horizontalement si préféré, ainsi que l’ajout de sous éléments de navigation dans les menus déroulants. Les menus de navigation simples comme les onglets reposent sur des éléments <ul>. Vous pouvez créer très simplement en leur permettant uniquement avec les classes CSS "nav" et "nav-tabs" :

![poste à thème](bootstrap/_static/theme-tabstrips.png)

Les barres de navigation sont générées de la même façon, mais sont un peu plus complexes. Elles commencent par `<nav>` ou `<div>` avec une classe "navbar", dans laquelle un élément div container conserve le reste des éléments. Notre page inclut déjà une barre de navigation dans son en-tête, celui illustré ci-dessous se développe simplement, en ajoutant la prise en charge pour un menu déroulant : 

![barres de navigation à thème](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Éléments supplémentaires

Le thème par défaut peut également servir à présenter des tableaux HTML dans un style de mise en forme correcte, y compris la prise en charge pour des vues alternées. Il existe des labels avec des styles qui sont semblables à celles des boutons. Vous pouvez créer des menus déroulants personnalisés qui prennent en charge les options de style supplémentaires au-delà de l'élément HTML standard `<select>`, ainsi que les barres de navigation semblable à celles que notre site de démarrage par défaut utilise déjà. Si vous avez besoin d’une barre de progression, il existe plusieurs styles à sélectionner, ainsi que la liste des groupes et des panneaux qui incluent un titre et un contenu. Explorez les options supplémentaires dans le thème d’amorçage standard ici :

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Plus de thèmes

Vous pouvez étendre le thème Bootstrap standard en remplaçant tout ou partie de ses CSS, régler les couleurs et les styles selon les besoins de votre propre application. Si vous souhaitez démarrer à partir d’un thème prêt à l’emploi, il existe plusieurs galeries de thème disponibles en ligne spécialisées dans les thèmes Bootstrap, telles que WrapBootstrap.com (qui possède un éventail de thèmes commerciales) et Bootswatch.com (qui offre des thèmes libres). Certains des modèles disponibles payants fournissent un large éventail de fonctionnalités par-dessus le thème Bootstrap de base, tels que la prise en charge des menus d’administration et des tableaux de bord avec des jauges et des graphiques enrichis. Un exemple d’un modèle payant populaire est Inspinia, actuellement en vente pour $18, qui inclut un modèle ASP.NET MVC5 en addition des versions AngularJS et HTML statiques. Vous trouverez ci-dessous une capture d’écran de l’exemple.

![Exemple thème inspinia](bootstrap/_static/theme-inspinia.png)

Si vous souhaitez modifier le thème Bootstrap, placez le fichier *bootstrap.css* pour le thème que vous souhaitez dans le dossier **wwwroot/css** et modifiez les références dans *_Layout.cshtml* pour pointer vers le fichier. Modifiez les liens pour tous les environnements : 

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Si vous souhaitez créer votre propre tableau de bord, vous pouvez démarrer à partir de l’exemple libre disponible ici : [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Composants

En plus de ces éléments déjà mentionnés, Bootstrap inclut la prise en charge pour un large éventail de [composants d’interface utilisateur intégrés](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap inclut des jeux d’icônes à partir de Glyphicons ([http://glyphicons.com](http://glyphicons.com)), avec plus de 200 icônes  disponibles gratuitement pour une utilisation dans votre application web compatible Bootstrap. Voici juste un petit échantillon :

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Groupe d'entrée

Les groupes d'entrée permettent le regroupement de texte ou des boutons avec un élément input, en fournissant une expérience plus intuitive à l’utilisateur : 

![groupes d’entrée](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Breadcrumbs

Les Breadcrumbs sont un composant de l’interface utilisateur commun utilisé pour montrer leur historique récent ou la profondeur dans la hiérarchie de navigation d’un site à l’utilisateur. Ajoutez-les facilement en appliquant la classe "breadcrumb" aux éléments de liste `<ol>`. Vous pouvez inclure la prise en charge intégrée pour la pagination à l’aide de la classe "pagination" sur un élément `<ul>` au sein d’un `<nav>`. Ajoutez des diaporamas incorporé responsive et vidéo à l’aide d'éléments `<iframe>`, `<embed>`, `<video>`, ou `<object>` auxquels Bootstrap appliquera un style automatiquement. Spécifiez un format particulier à l’aide des classes spécifiques, comme "embed-responsive-16by9".

## <a name="javascript-support"></a>Prise en charge de JavaScript

La librairie JavaScript de Bootstrap inclut une API de prise en charge pour les composants inclus, ce qui vous permet de contrôler leur comportement par programme au sein de votre application. En outre, *bootstrap.js* inclut plus d’une dizaine plug-ins jQuery personnalisés, en fournissant des fonctionnalités supplémentaires, comme des transitions, des boîtes de dialogue modales, la détection du déroulement (en mettant à jour les styles en fonction où l’utilisateur a défilé dans le document), le comportement de réduction, les carrousels et les menus apposés à la fenêtre de sorte qu’ils ne défilent pas hors de l’écran. Il n’y a pas de suffisamment d’espace pour couvrir tous les modules complémentaires JavaScript intégrés dans Bootstrap – pour en savoir plus, consultez [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Récapitulatif

Bootstrap fournit une infrastructure web qui peut être utilisée pour rapidement et efficacement mettre en page un large éventail d’applications et de sites Web. La typographie et les styles de base fournissent une apparence et convivialité agréable qui peuvent être manipulés aisément par la prise en charge de thème personnalisé qui peut être écrit à la main ou acheté dans le commerce. Il prend en charge un ensemble de composants web qui aurait requis des achats de contrôles onéreux pour mener cela à bien dans le passé, tout en prenant en charge des normes web modernes et ouvertes.