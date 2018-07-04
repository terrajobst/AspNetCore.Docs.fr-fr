---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modèle Breeze/Knockout | Microsoft Docs
author: madskristensen
description: Modèle d’Application à Page unique Breeze/Knockout
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 48ee0463fe950c28832523986a2242417411c96a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376862"
---
<a name="breezeknockout-template"></a>Modèle Breeze/Knockout
====================
par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle MVC Breeze/Knockout a été écrit par Ward Bell
> 
> [Télécharger le modèle MVC de Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Vous avez entendu parler de « application à page unique » (SPA) et de vous demander ce qu’il est. Pendant que vous pouvez lire à ce sujet, vous serez plutôt l’expérience pour vous-même. Mais qui a le temps pour télécharger un exemple ? Si vous avez Visual Studio, vous aurez un exemple SPA et en cours d’exécution en moins de 60 secondes avec ASP.NET MVC 4 modèle « Application à Page unique Breeze/Knockout ».

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Quel est le modèle de SPA Breeze/Knockout ?

La plupart des modèles de projet génèrent un squelette d’application. Vous placez des précisions sur ces segments en ajoutant votre code et que vous distribuez finalement une application opérationnelle. Le modèle Breeze/Knockout SPA est différent. Il génère un exemple d’application pour pouvoir étudier. Il montre une conception d’application SPA et la plupart des techniques pour la création d’une application SPA.

Modèle Breeze/Knockout est une variante sur le [les modèle KnockoutJS SPA](../introduction/knockoutjs-template.md) inclus dans ASP.NET et Web Tools 2012.2 à jour. Le modèle de Breeze SPA génère une application avec la même expérience utilisateur, mais il a une implémentation différente, à l’aide de Breeze pour la gestion des données.

Le modèle KnockoutJS SPA adresse des demandes de service avec brutes jQuery AJAX, qui est approprié pour une application simple. Mais des applications plus sophistiquées ont des exigences de gestion de données les plus exigeantes. Par exemple, la plupart des applications :

- Interroger et réinterroge le serveur pendant une session de l’étendue de l’utilisateur.
- Ajouter des filtres de requête, le tri et la pagination.
- Partager les mêmes données sur plusieurs écrans.
- Accumuler les modifications à de nombreux objets, puis les enregistrer sous la forme d’une transaction unique.
- Valide les modifications sur le client, l’utilisateur peut corriger les erreurs avant de valider les modifications à la base de données.

La bibliothèque BreezeJS gère ces tâches ingrates pour vous, vous permettant de développer l’application logique et l’expérience utilisateur qui vous intéressent le plus.

[**Breeze** ](http://www.breezejs.com/?utm_source=ms-spa) est une bibliothèque open source pour créer des applications de données riche en JavaScript et HTML, les types d’applications qui ont été remises par le passé en tant qu’applications de bureau autonomes.

Modèle Breeze/Knockout vous permet d’utiliser cette première étape essentielle vers une infrastructure de gestion des données plus robuste. Il génère un exemple d’application Todo qui l’aspect est identique au modèle KnockoutJS SPA. À l’intérieur, il remplace la couche de données AJAX avec Breeze, vous pouvez alors comparer les deux approches côte à côte. Bien sûr, il touche à peine le potentiel d’une application de Breeze. Mais vous verrez comment Breeze fonctionne et comment peu est requis pour effectuer cette transition.

Allons-y.

## <a name="create-a-breezeknockout-template-project"></a>Créer un projet de modèle Breeze/Knockout

Téléchargez et installez le modèle en cliquant sur le bouton Télécharger ci-dessus. Le modèle est empaqueté comme un fichier d’Extension Visual Studio (VSIX). Vous devrez peut-être redémarrer Visual Studio.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet et cliquez sur **OK**.

Dans le **nouveau projet** Assistant, sélectionnez **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Appuyez sur Ctrl + F5 pour générer et exécuter l’application sans débogage, ou appuyez sur F5 pour exécuter avec débogage.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Lorsque l’application s’exécute tout d’abord, il affiche un écran de connexion. Cliquez sur le lien « Inscrivez-vous » et une nouvelle page glides dans une vue, dans lequel vous pouvez entrer un nom d’utilisateur et le mot de passe. (Les pages de connexion et d’inscription sont générés à l’aide d’ASP.NET MVC.) Lorsque vous envoyez le formulaire d’inscription, le serveur génère une liste des tâches avec deux éléments de votre compte. Puis il les présente sur une note jaune.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Vous êtes maintenant dans le sol de SPA. Tout ce que vous consultez et rencontrez lors de la manipulation des éléments TODO est rendu et géré sur le client à l’aide de Knockout et Breeze. Explorer l’application en tant qu’utilisateur... mais avec les yeux du développeur. Utiliser les outils de développement dans votre navigateur pour capturer le trafic réseau. (Dans Internet Explorer : appuyez sur F12, sélectionnez le **réseau** onglet, puis cliquez sur **commencer à capturer**.) Maintenant, essayez ce qui suit :

- Ajouter un nouvel élément Todo.
- Cliquez sur l’étiquette et de modifier le titre de l’élément Todo
- Activez une case à cocher pour marquer l’élément terminé. Notez que la zone de texte est désactivé, donc le titre n’est plus modifiable.
- Cliquez sur le « x » à droite de l’étiquette. L’élément disparaît et est supprimé de la base de données.
- Choisir un autre élément et effacer son titre. Vous obtiendrez une erreur de validation que le titre est obligatoire. Après une courte pause, le titre précédent est restauré.
- Tapez un titre ridiculement long. Vous obtiendrez une erreur de validation autre que le titre est trop long.
- Cliquez sur le bouton « Ajouter Todo List ». Une nouvelle liste s’affiche à gauche de la liste précédente.
- Jouez avec le titre TodoList, déclencher son requis et des validations de longueur.
- Cliquez dans la zone de texte de titre pour effacer le message d’erreur.
- Cliquez sur le « x » dans le cercle dans le coin supérieur droit de supprimer la liste des tâches et ses éléments TODO.

La logique de validation est effectuée côté client par Breeze. Attributs de validation sur les classes de modèle de serveur sont propagées vers le client et exécutées automatiquement avant que le client contacte le serveur.

Passez en revue le trafic réseau. Notez qu’il n’y avait aucun appel au serveur lors de Breeze a détecté une erreur. Chaque modification valide a entraîné une demande POST en « / api/Todo/SaveChanges ». Breeze regroupe les modifications et les envoie ensemble en une seule requête pour le contrôleur d’API Web `SaveChanges` (méthode). Qui diffère du modèle KockoutJS SPA, ce qui rend PUT, POST et DELETE de requêtes pour chaque élément individuellement.

## <a name="peek-inside"></a>Aperçu à l’intérieur

Cette application a un côté client et un côté serveur. La pile de côté client se compose d’une petite HTML et une combinaison de modules d’application JavaScript (dans le dossier « application ») ainsi que des bibliothèques de JavaScript par des tiers (dans le dossier « Scripts »).

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Si vous avez examiné le modèle KnockoutJS SPA, cela semblera très familier. Concentrez-vous sur les cases bleues. L’architecture de l’interface utilisateur est Model-View-ViewModel (MVVM), dans laquelle les widgets HTML de la vue sont nettement séparées de la prise en charge code de présentation dans le modèle de vue. Un système de liaison de données (Knockout dans ce cas) coordonne la vue et le modèle de vue afin que chacun puisse effectuer son travail sans une connaissance approfondie de l’autre.

Le modèle encapsule les données de tâches. Entités dans le modèle sont construites par Breeze avec des propriétés observables Knockout, ils peuvent être liés directement à des widgets dans la vue. Le modèle de vue vous demande le contexte de données pour acquérir et enregistrer les entités du modèle. Le contexte de données délègue la plupart du travail de Breeze.

La pile côté serveur se compose d’un code de développeur et trois bibliothèques .NET de principe : Breeze.NET API Web et Entity Framework :

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L’architecture de base est le même que le modèle KockoutJS SPA. Toutefois, l’implémentation est beaucoup plus simple : le DTO ont été supprimés, et la plupart des détails de l’Entity Framework ont été déléguée Breeze.NET.

## <a name="next-steps"></a>Étapes suivantes

Nous vous suggérons d’Explorer le code, guidé par les [description détaillée](http://www.breezejs.com/spa-template?utm_source=ms-spa) du client et les piles de serveur sur le site Web de Breeze.

Vous pouvez essayer de s’amuser avec les requêtes de Breeze côté client ; ajouter des filtres et les tris. Vous pouvez ajouter plus de propriétés de modèle et des entités supplémentaires pour obtenir une meilleure idée pour le développement de SPA de bout en bout. Lorsque vous êtes certain de la conception, vous pouvez supprimer les fonctionnalités Todo et remplacez-les par les vôtres.

Vous serez bientôt prêt pour l’étape suivante : ajout d’écrans du côté client et la navigation entre eux. Vous laisse ce modèle SPA et activer pour une pile SPA plus complet, tel que [SERVIETTE à chaud de John Papa](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), qui ajoute Durandal à la combinaison de Breeze et Knockout.
