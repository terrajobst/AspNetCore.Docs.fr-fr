---
uid: single-page-application/overview/templates/breezeangular-template
title: Modèle Breeze/Angular | Microsoft Docs
author: madskristensen
description: Modèle d’Application à Page unique Breeze/Angular
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 541d1a71b58a0d55651d823dc7425b7d1ce73cdb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378058"
---
<a name="breezeangular-template"></a>Modèle Breeze/Angular
====================
par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle MVC Breeze/Angular a été écrit par Ward Bell
> 
> [Télécharger le modèle MVC Breeze/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) est une bibliothèque open source à partir de Google pour la création d’Applications à Page unique (SPA). Il offre la liaison de données, l’injection de dépendances et la gestion de l’écran. Combiner avec [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), une autre bibliothèque open source pour la modélisation des données et gestion des données et que vous avez les ingrédients essentiels d’une application de client HTML/JavaScript exceptionnelle.

Le modèle Breeze/Angular SPA est une variante sur le [les modèle KnockoutJS SPA](../introduction/knockoutjs-template.md) inclus dans ASP.NET et Web Tools 2012.2 à jour. Si vous avez Visual Studio, vous aurez un exemple SPA opérationnel en moins de 60 secondes.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Vers l’extérieur, l’application le très similaire au modèle KnockoutJS SPA. Mais il est tout à fait différent sous le capot. Le modèle KnockoutJS utilise Knockout pour la liaison de données et AJAX brute pour accéder aux données. Le modèle Breeze/Angular utilise Angular pour la liaison de données et de Breeze pour accéder aux données. Ces bibliothèques activent des fonctionnalités supplémentaires, y compris l’historique et la navigation entre les pages.

Voici la page About de l’application :

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Cette page affiche un journal en cours d’exécution des événements pendant la session utilisateur en cours, y compris :

- Pagination. Notez la création du contrôleur Todo à #2 et #7.
- Requêtes distantes (3) et les requêtes de cache local (#7).
- Enregistrement de nouveau (5, #6) et modifié les entités (4).
- Modifications validées sur le client (#9), afin de l’utilisateur peut corriger les erreurs avant de valider les modifications apportées à la base de données.

Il est bien plus encore à découvrir dans ce modèle, y compris :

- Chargement dynamique des modèles de vue HTML.
- Liaison de données personnalisées via Angular « directives ».
- Injection de dépendance et de modularité.
- Filtres de requête trie, la pagination, projections et inclusion d’entités associées.
- Partager des données entre plusieurs écrans.
- Enregistrement des modifications de plusieurs comme une transaction unique.
- Règles de validation propagées automatiquement à partir du serveur au client JavaScript.

Allons-y.

## <a name="create-a-breezeangular-template-project"></a>Créer un projet de modèle Breeze/Angular

Téléchargez et installez le modèle en cliquant sur le bouton Télécharger ci-dessus. Le modèle est empaqueté comme un fichier d’Extension Visual Studio (VSIX). Vous devrez peut-être redémarrer Visual Studio.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet et cliquez sur **OK**.

Dans le **nouveau projet** Assistant, sélectionnez **SPA Angular Breeze**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Appuyez sur Ctrl + F5 pour générer et exécuter l’application sans débogage, ou appuyez sur F5 pour exécuter avec débogage.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Cliquez sur le lien « About » dans le coin supérieur droit pour afficher un journal de ces activités.

La logique de validation est effectuée côté client par Breeze. Attributs de validation sur les classes de modèle de serveur sont propagées vers le client et exécutées automatiquement avant que le client contacte le serveur.

Passez en revue le trafic réseau. Notez qu’il n’y avait aucun appel au serveur lors de Breeze a détecté une erreur. Chaque modification valide a entraîné une demande POST en « / api/Todo/SaveChanges ». Breeze regroupe les modifications et les envoie ensemble en une seule requête pour le contrôleur d’API Web `SaveChanges` (méthode). Qui diffère du modèle KockoutJS SPA, ce qui rend PUT, POST et DELETE de requêtes pour chaque élément individuellement.

En outre, ne notez aucun trafic réseau lorsque vous basculez entre la liste des tâches et sur les pages. C’est parce que la requête a été limitée dans le cache local de Breeze.

## <a name="peek-inside"></a>Aperçu à l’intérieur

Cette application a un côté client et un côté serveur. La pile de côté client se compose d’une petite HTML et une combinaison de modules d’application JavaScript (dans le dossier « application ») ainsi que des bibliothèques de JavaScript par des tiers (dans le dossier « Scripts »).

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

L’architecture de l’interface utilisateur sépare les widgets HTML des vues à partir du code de présentation de prise en charge dans les contrôleurs. Le système de liaison de données Angular coordonne les vues et contrôleurs afin que chacun puisse effectuer son travail sans une connaissance approfondie de l’autre.

Le contrôleur demande le contexte de données pour acquérir et enregistrer les entités du modèle. Le contexte de données délègue la plupart du travail à Breeze, qui construit des objets de modèle de suivi automatique des résultats de requête JSON.

La pile côté serveur se compose d’un code de développeur et trois bibliothèques .NET de principe : Breeze.NET API Web et Entity Framework :

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L’architecture de base est le même que le modèle KockoutJS SPA. Toutefois, l’implémentation est beaucoup plus simple : le DTO ont été supprimés, et la plupart des détails de l’Entity Framework ont été déléguée Breeze.NET.

## <a name="next-steps"></a>Étapes suivantes

Nous vous suggérons d’Explorer le code, guidé par les [description détaillée](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) du client et les piles de serveur sur le site Web de Breeze.

Vous pouvez essayer de s’amuser avec les requêtes de Breeze côté client ; ajouter des filtres et les tris. Vous pouvez ajouter plus de propriétés de modèle et des entités supplémentaires pour obtenir une meilleure idée pour le développement de SPA de bout en bout. Lorsque vous êtes certain de la conception, vous pouvez supprimer les fonctionnalités Todo et remplacez-les par les vôtres.

Bon développement !
