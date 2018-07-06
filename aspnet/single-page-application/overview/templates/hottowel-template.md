---
uid: single-page-application/overview/templates/hottowel-template
title: Modèle de serviette à chaud | Microsoft Docs
author: madskristensen
description: Modèle de HotTowel
ms.author: aspnetcontent
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: 6145a34286ef113002b92d722e227005657098d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825135"
---
<a name="hot-towel-template"></a>Modèle hot Towel
====================
par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle MVC SERVIETTE à chaud est écrite par John Papa
> 
> Choisir la version à télécharger :
> 
> [Modèle MVC hot Towel pour Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modèle MVC hot Towel pour Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel : Étant donné que vous ne souhaitez pas accéder à l’application SPA sans un !


À générer une application à page unique, mais ne peut pas décider où commencer ? Utilisez Hot Towel et en quelques secondes, vous aurez une application SPA et tous les outils que nécessaires à la création dessus !

Hot Towel crée un excellent point de départ pour la création d’une Application à Page unique (SPA) avec ASP.NET. Prêt à l’emploi vous il fournit une structure modulaire pour votre code navigation de la vue, liaison de données, gestion des données riches et styles simples mais élégantes. Hot Towel fournit tout ce dont vous avez besoin pour générer une application SPA, afin de vous concentrer sur votre application, pas la plomberie.

> En savoir plus sur la création d’une application à page unique à partir de [vidéos, des didacticiels et des cours Pluralsight John Papa](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Structure de l’application

Hot Towel SPA fournit un dossier d’application qui contient les fichiers JavaScript et HTML qui définissent votre application.

Dans le dossier d’application :

- durandal
- services
- ViewModels
- vues

Le dossier de l’application contient une collection de modules. Ces modules encapsulent des fonctionnalités et déclarent des dépendances sur d’autres modules. Le dossier views contient le code HTML de votre application et le dossier viewmodels contient la logique de présentation pour les vues (MVVM courant). Le dossier services est idéal pour héberger tous les services courants tels que la récupération des données HTTP ou d’une interaction de stockage local requis par votre application. Il est courant pour plusieurs ViewModel à réutiliser le code à partir des modules de service.

## <a name="aspnet-mvc-server-side-application-structure"></a>Structure d’Application ASP.NET MVC serveur côté

Hot Towel s’appuie sur la structure ASP.NET MVC familière et puissante.

- Application\_Démarrer
- Contenu
- Contrôleurs
- Modèles
- scripts ;
- Affichages

## <a name="featured-libraries"></a>Bibliothèques par défaut

- ASP.NET MVC
- API web ASP.NET
- L’optimisation Web ASP.NET - regroupement et minimisation
- [Breeze.js](http://Breezejs.com) -gestion des données riche
- [Durandal.js](http://Durandaljs.com) -navigation et la composition de la vue
- [Knockout.js](http://Knockoutjs.com) -des liaisons de données
- [Require.js](http://requirejs.org) -modularité avec AMD et optimisation
- [Toastr.js](http://jpapa.me/c7toastr) -messages contextuels
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) : styles de CSS robuste

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installation via le modèle SPA Hot Towel Visual Studio 2012

Hot Towel peut être installé comme un modèle Visual Studio 2012. Cliquez simplement sur `File`  |  `New Project` et choisissez `ASP.NET MVC 4 Web Application`. Puis sélectionnez le « Hot Towel Single Page Application « modèle et exécution !

## <a name="installing-via-the-nuget-package"></a>Installation via le Package NuGet

Hot Towel est également un package NuGet qui augmente d’un projet ASP.NET MVC vide existant. Installez simplement à l’aide de Nuget, puis exécutez !

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Comment créer SERVIETTE chaud ?

Démarrez simplement l’ajout de code !

1. Ajouter votre propre code côté serveur, de préférence Entity Framework et API Web (qui se distinguent avec Breeze.js)
2. Ajouter des affichages pour le `App/views` dossier
3. Ajouter les viewmodels à la `App/viewmodels` dossier
4. Ajouter des liaisons de données HTML et Knockout à vos nouvelles vues
5. Mettre à jour les itinéraires de la navigation dans `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Procédure pas à pas du code HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml est l’itinéraire et la vue de l’application MVC départ. Il contient toutes les balises meta standard, les liens de css et les références JavaScript que vous pouvez l’imaginer. Le corps contient un seul `<div>` qui est tout le contenu (vos vues) emplacement quand ils sont demandés. Le `@Scripts.Render` utilise Require.js pour exécuter le point d’entrée pour le code de l’application, qui est contenu dans le `main.js` fichier. Un écran de démarrage est fourni pour montrer comment créer un écran de démarrage pendant le chargement de votre application.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

Le `main.js` fichier contient le code qui s’exécutera dès que votre application est chargée. Il s’agit dans lequel vous souhaitez définir vos routes de navigation, vos vues de démarrage et effectuer tout le programme d’installation/amorçage telles que l’amorçage des données de votre application.

Le `main.js` fichier définit plusieurs des modules de durandal pour aider le lancement de l’application. L’instruction de définition vous aide à résoudre les dépendances de modules afin qu’ils soient disponibles pour la fonction. Tout d’abord les messages de débogage sont activées, ce qui envoient des messages sur les événements d’exécution dans la fenêtre de console. Le code app.start indique au framework durandal pour démarrer l’application. Les conventions sont définies afin que durandal connaît toutes les vues et viewmodels sont contenus dans les mêmes dossiers nommés, respectivement. Enfin, le `app.setRoot` intervient charges le `shell` à l’aide de prédéfini `entrance` animation.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Affichages

Vues sont trouvent dans le `App/views` dossier.

### <a name="shellhtml"></a>Shell.HTML

Le `shell.html` contient la disposition principale pour votre code HTML. Toutes vos autres vues se composera quelque part dans le côté de votre `shell` vue. Hot Towel fournit un `shell` avec trois régions de ce type : un en-tête, une zone de contenu et un pied de page. Chacune de ces régions est chargé avec contenu forme d’autres vues lors de la demande.

Le `compose` liaisons pour l’en-tête et le pied de page sont figés dans Hot Towel pour pointer vers le `nav` et `footer` consulte, respectivement. La liaison de compose pour la section `#content` est lié à la `router` élément actif du module. En d’autres termes, lorsque vous cliquez sur un lien de navigation est vue correspondante est chargé dans ce domaine.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

Le `nav.html` contient des liens de navigation pour l’application SPA. Il s’agit où la structure de menu peut être placée, par exemple. Il s’agit souvent les données liées (à l’aide de Knockout) à la `router` module pour afficher le volet de navigation vous avez défini dans le `shell.js`. Knockout recherche la liaison de données des attributs et celles qui lie la `shell` viewmodel pour afficher les itinéraires de navigation et afficher un progressbar (à l’aide de Twitter Bootstrap) si le `router` module est en train de naviguer d’une vue à une autre (voir `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML et details.html

Ces vues contiennent HTML pour des vues personnalisées. Lorsque le `home` lien dans le `nav` clic sur le menu de la vue, le `home` affichage sera placé dans la zone de contenu de la `shell` vue. Ces vues peuvent être augmentées ou remplacés par vos propres affichages personnalisés.

### <a name="footerhtml"></a>Footer.HTML

Le `footer.html` contient du code HTML qui s’affiche dans le pied de page, en bas de la `shell` vue.

## <a name="viewmodels"></a>ViewModels

ViewModels se trouvent dans le `App/viewmodels` dossier.

### <a name="shelljs"></a>Shell.js

Le `shell` viewmodel contient les propriétés et les fonctions qui sont liées à la `shell` vue. Il s’agit souvent où sont trouvent les liaisons de navigation de menu (voir la `router.mapNav` logique).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js et details.js

Ces viewmodels contiennent les propriétés et les fonctions qui sont liées à la `home` vue. Il contient la logique de présentation de la vue et vous est le lien entre les données et la vue.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Services

Services sont trouvent dans le dossier/services d’application. Dans l’idéal, vos futurs services tel qu’un module dataservice, qui est responsable de l’obtention et la validation des données à distance, peut être placées.

### <a name="loggerjs"></a>Logger.js

Hot Towel fournit un `logger` module dans le dossier services. Le `logger` module est idéal pour les messages de journalisation dans la console et à l’utilisateur dans la fenêtre contextuelle toasts.
