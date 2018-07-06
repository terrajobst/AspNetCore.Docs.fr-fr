---
uid: single-page-application/overview/introduction/other-libraries
title: Connaître les bibliothèques autres que Knockout ? | Microsoft Docs
author: madskristensen
description: Connaître les bibliothèques autres que Knockout ?
ms.author: aspnetcontent
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 69006df6a5de3290ac294a309756eb0a53baa975
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834592"
---
<a name="know-a-library-other-than-knockout"></a>Connaître les bibliothèques autres que Knockout ?
====================
par [Mads Kristensen](https://github.com/madskristensen)

Le [modèle d’Application à Page unique (SPA)](knockoutjs-template.md) est un excellent moyen pour commencer à écrire des applications à page unique. Le modèle utilise [KnockoutJS](http://knockoutjs.com/) pour lier des données d’application aux éléments DOM.

Mais, Knockout n’est pas la seule bibliothèque JavaScript pour créer des applications client riche. Autres bibliothèques de relever des défis similaires de différentes façons. Vous préférerez peut-être une une bibliothèque plutôt qu’un autre, donc nous avons apporté plusieurs modèles créés par la Communauté disponible au téléchargement. Chacun de ces modèles utilise une combinaison différente des bibliothèques JavaScript client.

Pour installer un modèle créés par la Communauté, visitez l’un du modèle de pages répertoriées ci-dessous et cliquez sur le bouton Télécharger. Les modèles sont fournis en tant que fichiers VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modèle SPA backbone.js](../templates/backbonejs-template.md). Ce modèle fournit un squelette initial pour le développement d’un [Backbone.js](http://backbonejs.org/) application dans ASP.NET MVC. Prêt à l’emploi, il fournit les fonctionnalités de connexion utilisateur de base, y compris la réinitialisation de mot de passe d’inscription, la connexion, l’utilisateur et la confirmation de l’utilisateur avec des modèles de messagerie de base.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) est une bibliothèque open source pour la gestion des données riches dans un client JavaScript. Breeze gère l’interrogation, la mise en cache, le suivi des modifications, validation et bien plus encore. Deux modèles dotés d’un jeu d’enfant :

- Le [Breeze/Knockout](../templates/breezeknockout-template.md) modèle étend le modèle SPA de Knockout, montrant comment facilement, vous pouvez créer une application à page unique avec Breeze pour la gestion des données et KnockoutJS pour la liaison de données.
- Le [Breeze/Angular](../templates/breezeangular-template.md) modèle permet également d’étendre le modèle SPA de Knockout Breeze, mais en utilisant le [AngularJS](http://angularjs.org) bibliothèque pour la liaison de données, l’injection de dépendances et la gestion de l’écran.

En outre, le [les modèle Hot Towel SPA](../templates/hottowel-template.md) utilise BreezeJS.

## <a name="emberjs"></a>EmberJS

[Modèle de EmberJS SPA](../templates/emberjs-template.md). Ce modèle utilise [Ember](http://emberjs.com/), une bibliothèque JavaScript de MVC puissante qui permet de résoudre un large éventail de défis pour les applications clientes riches.

Le modèle de Ember SPA est une nouvelle implémentation du modèle Knockout SPA, à l’aide de la création de modèles EmberJS et Handlebars.

## <a name="hot-towel"></a>Hot Towel

[Modèle de serviette SPA à chaud](../templates/hottowel-template.md). Ce modèle permet d’utiliser plusieurs bibliothèques JavaScript, notamment Breeze et Knockout, RequireJS Twitter Bootstrap.

Comparaison avec les autres modèles répertoriés ici, le teample Hot Towel fournit une application plus complète à partir de laquelle vous pouvez créer vos propres. Il existe plusieurs concepts à connaître, mais une fois que vous comprenez les, ce modèle peut être simplement ce que vous recherchez. Si vous souhaitez générer un projet SPA mais que vous ne pouvez pas décider de l’emplacement Démarrer, utilisez Hot Towel et en quelques secondes, vous aurez une application SPA et tous les outils que vous deviez générer sur celui-ci.

## <a name="feature-table"></a>Tableau des fonctionnalités

Voici les fonctionnalités fournies par chaque modèle SPA :


|                        | Application à page unique ASP.NET | Réseau principal | Breeze/Angular | Breeze/KO |  Ember   | Hot Towel |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Exemple ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Modèle de récupération      |             | &#10003; |                |           |          | &#10003;  |
| Navigation et historique |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Suivies        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Réseau principal     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

