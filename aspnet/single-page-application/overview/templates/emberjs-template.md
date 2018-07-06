---
uid: single-page-application/overview/templates/emberjs-template
title: Modèle EmberJS | Microsoft Docs
author: xqiu
description: Modèle EmberJS
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823775"
---
<a name="emberjs-template"></a>Modèle EmberJS
====================
par [Xinyang Qiu](https://github.com/xqiu)

> Le modèle MVC EmberJS est rédigé par Nathan Totten, Thiago Santos et Xinyang Qiu.
> 
> [Télécharger le modèle EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


Le modèle EmberJS SPA est conçu pour vous aider à créer rapidement des applications web côté client interactives à l’aide de EmberJS.

« Single-page application » (SPA) est le terme général qui désigne une application web qui charge une seule page HTML et puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages. Après le chargement de page initial, le SPA s’entretient avec le serveur via les demandes AJAX.

![](emberjs-template/_static/image1.png)

AJAX n’est pas nouveau, mais aujourd'hui, il existe des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée. En outre, HTML5 et CSS3 sont qu’il soit plus facile de créer des interfaces utilisateur riches.

Le modèle de SPA EmberJS utilise le [Ember](http://emberjs.com/) bibliothèque JavaScript pour gérer les mises à jour de la page à partir de demandes AJAX. Ember.js utilise la liaison de données pour synchroniser la page avec les dernières données. De cette façon, il est inutile d’écrire du code qui vous guide à travers les données JSON et met à jour le modèle DOM. Au lieu de cela, vous placez des attributs déclaratifs dans le code HTML qui indiquent Ember.js comment présenter les données.

Sur le côté serveur, le modèle EmberJS est presque identique à la [les modèle KnockoutJS SPA](../introduction/knockoutjs-template.md). Il utilise ASP.NET MVC pour traiter les documents HTML et ASP.NET Web API pour gérer les demandes AJAX à partir du client. Pour plus d’informations sur ces aspects du modèle, reportez-vous à la [modèle KnockoutJS](../introduction/knockoutjs-template.md) documentation. Cette rubrique se concentre sur les différences entre le modèle Knockout et le modèle EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Créer un projet de modèle EmberJS SPA

Téléchargez et installez le modèle en cliquant sur le bouton Télécharger ci-dessus. Vous devrez peut-être redémarrer Visual Studio.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet et cliquez sur **OK**.

![](emberjs-template/_static/image2.png)

Dans le **nouveau projet** Assistant, sélectionnez **Ember.js SPA projet**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Vue d’ensemble du modèle EmberJS SPA

Le modèle EmberJS utilise une combinaison de jQuery, Ember.js, Handlebars.js pour créer une interface utilisateur interactive sans heurts.

Ember.js est une bibliothèque JavaScript qui utilise un modèle MVC côté client.

- Un *modèle*, écrit dans le langage de création de modèles Handlebars, décrit l’interface utilisateur. En mode de mise en production, le [compilateur de Handlebars](https://github.com/Myslik/csharp-ember-handlebars) sert à regrouper et compiler le modèle handlebars.
- Un *modèle* stocke les données d’application qu’il obtient à partir du serveur (listes de tâches et éléments de tâche).
- Un *contrôleur* stocke l’état de l’application. Contrôleurs présentent souvent des données de modèle pour les modèles correspondants.
- Un *vue* se traduit par des événements primitifs à partir de l’application et transmet ces au contrôleur.
- Un *routeur* gère l’état de l’application, synchronisation des modèles et des URL.

En outre, la bibliothèque Ember données peut être utilisée pour synchroniser des objets JSON (obtenus à partir du serveur via une API RESTful) et les modèles de client.

Le modèle EmberJS SPA organise les scripts en huit couches :

- WebAPI\_adapter.js, webapi\_serializer.js : étend la bibliothèque de données de Ember pour travailler avec l’API Web ASP.NET.
- Scripts/Helpers.js : Définit les nouveaux programmes d’assistance Ember Handlebars.
- Scripts/App.js : Crée l’application et configure l’adaptateur et le sérialiseur.
- Modèles/application/scripts/\*.js : définit les modèles.
- Scripts/application/vues/\*.js : définit les affichages.
- Scripts/application/contrôleurs/\*.js : définit les contrôleurs.
- Scripts/application/itinéraires, Scripts/app/router.js : Définit les itinéraires.
- Modèles /\*.hbs : définit les modèles handlebars.

Examinons quelques-unes de ces scripts plus en détail.

## <a name="models"></a>Modèles

Les modèles sont définis dans le dossier Scripts / / modèles d’application. Il existe deux fichiers de modèle : todoItem.js et todoList.js.

**TODO.Model.js** définit les modèles côté client (navigateur) pour les listes de tâches. Il existe deux classes de modèle : todoItem et todoList. Dans Ember, les modèles sont des sous-classes de DS. Modèle. Un modèle peut avoir des propriétés avec des attributs :

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Les modèles peuvent définir des relations à d’autres modèles :

[!code-css[Main](emberjs-template/samples/sample2.css)]

Les modèles peuvent les propriétés qui lient à d’autres propriétés calculées :

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Les modèles peuvent avoir des fonctions de l’Observateur, qui sont appelées lorsqu’une propriété observée change :

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Affichages

Les vues sont définies dans le dossier Scripts/application/vues. Une vue se traduit par des événements de l’interface utilisateur de l’application. Un gestionnaire d’événements peut rappeler aux fonctions du contrôleur, ou simplement appeler directement le contexte de données.

Par exemple, le code suivant provient de views/TodoItemEditView.js. Il définit la gestion des événements pour un champ de texte d’entrée.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Contrôleur

Les contrôleurs sont définis dans le dossier Scripts/application/contrôleurs. Pour représenter un modèle unique, étendre `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un contrôleur peut également représenter une collection de modèles en étendant `Ember.ArrayController`. Par exemple, le TodoListController représente un tableau de `todoList` objets. Le contrôleur de trie par ID de la liste des tâches, dans l’ordre décroissant :

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Le contrôleur définit une fonction nommée `addTodoList`, qui crée une nouvelle liste des tâches et l’ajoute dans le tableau. Pour voir comment cette fonction est appelée, ouvrez le fichier de modèle nommé todoListTemplate.html, dans le dossier de modèles. Le code de modèle suivant lie un bouton à la `addTodoList` (fonction) :

[!code-html[Main](emberjs-template/samples/sample8.html)]

Le contrôleur contient également un `error` propriété, qui contient un message d’erreur. Voici le code du modèle pour afficher le message d’erreur (également dans todoListTemplate.html) :

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Routes

Router.js définit les itinéraires et le modèle par défaut à afficher, les jeux de l’état de l’application et correspond à des URL pour les itinéraires :

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js charge les données pour le TodoListRoute en substituant la fonction setupController :

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember utilise les conventions d’affectation de noms pour faire correspondre les URL, les noms de routes, contrôleurs et modèles. Pour plus d’informations, consultez [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) à la documentation EmberJS.

## <a name="templates"></a>Modèles

Le dossier de modèles contient quatre modèles :

- application.HBS : le modèle par défaut qui est affiché lorsque l’application est démarrée.
- About.HBS : le modèle pour l’itinéraire « / environ ».
- index.HBS : le modèle pour la racine « / » itinéraire.
- todoList.hbs : le modèle pour le « / todo « itinéraire.
- \_navbar.HBS : le modèle définit le menu de navigation.

Le modèle d’application agit comme une page maître. Il contient un en-tête, un pied de page et un « {{outlet}} » pour insérer d’autres modèles dans en fonction de l’itinéraire. Pour plus d’informations sur les modèles d’application dans Ember, consultez [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Le « / todoList « modèle contient deux expressions de boucle. La boucle extérieure est `{{#each controller}}`et la boucle est `{{#each todos}}`. Le code suivant montre un intégré `Ember.Checkbox` afficher un texte personnalisé `App.TodoItemEditView`, ainsi qu’un lien avec un `deleteTodo` action.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Le `HtmlHelperExtensions` (classe), définie dans Controllers/HtmlHelperExensions.cs, définit une assistance afin de mettre en cache et insérer le modèle de fichiers quand **déboguer** a la valeur **true** dans le fichier Web.config. Cette fonction est appelée à partir du fichier de vue ASP.NET MVC défini dans Views/Home/App.cshtml :

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Appelée sans argument, la fonction affiche tous les fichiers de modèle dans le dossier de modèles. Vous pouvez également spécifier un sous-dossier ou un fichier de modèle spécifique.

Lorsque **déboguer** est **false** dans le fichier Web.config, l’application inclut l’élément de lot « ~/bundles/templates ». Cet élément de regroupement est ajouté dans BundleConfig.cs, à l’aide de la bibliothèque de compilateur Handlebars :

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
