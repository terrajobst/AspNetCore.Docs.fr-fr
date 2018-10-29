---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Application à Page unique : Modèle de KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Modèle Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 60bc8bf95cace722244ffc87ff4c00126a0ed2a0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207444"
---
<a name="single-page-application-knockoutjs-template"></a>Application à Page unique : Modèle de KnockoutJS
====================
par [Mike Wasson](https://github.com/MikeWasson)

> Le modèle MVC Knockout fait partie d’ASP.NET et Web Tools 2012.2
> 
> [Téléchargez ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


La mise à jour ASP.NET et Web Tools 2012.2 inclut un modèle d’Application à Page unique (SPA) pour ASP.NET MVC 4. Ce modèle est conçu pour vous aider à générer rapidement des applications web côté client interactives.

« Single-page application » (SPA) est le terme général qui désigne une application web qui charge une seule page HTML et puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages. Après le chargement de page initial, le SPA s’entretient avec le serveur via les demandes AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX n’est pas nouveau, mais aujourd'hui, il existe des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée. En outre, HTML5 et CSS3 sont qu’il soit plus facile de créer des interfaces utilisateur riches.

Pour vous aider à démarrer, le modèle SPA crée un exemple d’application « Liste des tâches ». Dans ce didacticiel, nous allons suivez une visite guidée du modèle. Tout d’abord, nous allons examiner l’application de liste de tâches elle-même, puis examiner les éléments de technologie qui le font fonctionner.

## <a name="create-a-new-spa-template-project"></a>Créer un nouveau projet de modèle SPA

Configuration requise :

- Visual Studio 2012 ou Visual Studio Express 2012 pour le Web
- ASP.NET Web Tools 2012.2 mettre à jour. Vous pouvez installer la mise à jour [ici](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la page de démarrage. Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet et cliquez sur **OK**.

![](knockoutjs-template/_static/image2.png)

Dans le **nouveau projet** Assistant, sélectionnez **Single Page Application**.

![](knockoutjs-template/_static/image3.png)

Appuyez sur F5 pour générer et exécuter l'application. Lorsque l’application s’exécute tout d’abord, il affiche un écran de connexion.

![](knockoutjs-template/_static/image4.png)

Cliquez sur le &quot;Inscrivez-vous&quot; lier et créer un nouvel utilisateur.

![](knockoutjs-template/_static/image5.png)

Une fois que vous êtes connecté, l’application crée une liste de tâches par défaut avec deux éléments. Vous pouvez cliquer sur « Ajouter une liste de tâches » pour ajouter une nouvelle liste.

![](knockoutjs-template/_static/image6.png)

Renommer la liste, ajouter des éléments à la liste et les marquer comme. Vous pouvez également supprimer des éléments ou supprimer une liste entière. Les modifications sont automatiquement conservées dans une base de données sur le serveur (LocalDB réellement à ce stade, étant donné que vous exécutez l’application localement).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architecture du modèle SPA

Ce diagramme illustre les principaux blocs de construction de l’application.

![](knockoutjs-template/_static/image8.png)

Sur le côté serveur, ASP.NET MVC sert le code HTML et gère également l’authentification basée sur les formulaires.

API Web ASP.NET gère toutes les demandes qui se rapportent à la ToDoLists et ToDoItems, y compris l’obtention, création, la mise à jour et suppression. Le client échange des données avec l’API Web au format JSON.

Entity Framework (EF) est la couche de O/RM. Il sert d’intermédiaire entre le monde orientée objet, d’ASP.NET et de la base de données sous-jacente. La base de données utilise la base de données locale, mais vous pouvez le modifier dans le fichier Web.config. En général vous utiliser LocalDB pour le développement local et déployer une base de données SQL sur le serveur, à l’aide de la migration de code first EF.

Du côté client, la bibliothèque Knockout.js gère les mises à jour de la page à partir de demandes AJAX. Knockout utilise la liaison de données pour synchroniser la page avec les dernières données. De cette façon, il est inutile d’écrire du code qui vous guide à travers les données JSON et met à jour le modèle DOM. Au lieu de cela, vous placez des attributs déclaratifs dans le code HTML qui indiquent comment présenter les données à Knockout.

L’avantage de cette architecture est qu’il sépare la couche de présentation de la logique d’application. Vous pouvez créer la partie de l’API Web sans connaître quoi que ce soit sur l’apparence de votre page web. Du côté client, vous créez un « modèle de vue » pour représenter ces données, et le modèle de vue utilise Knockout pour lier le code HTML. Qui vous permet de modifier facilement le code HTML sans modifier le modèle de vue. (Nous examinerons Knockout un peu plus loin.)

## <a name="models"></a>Modèles

Dans le projet Visual Studio, le dossier Models contienne les modèles qui sont utilisés sur le côté serveur. (Il existe également des modèles côté client ; nous le verrons ceux).

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Voici les modèles de base de données pour Entity Framework Code First. Notez que ces modèles ont des propriétés qui pointent vers les uns des autres. `ToDoList` contient une collection de ToDoItems et chaque `ToDoItem` a une référence à son parent ToDoList. Ces propriétés sont appelées propriétés de navigation, et elles représentent la relation un-à-plusieurs, une liste de tâches et ses éléments de tâche.

Le `ToDoItem` classe également utilise le **[ForeignKey]** attribut pour spécifier que `ToDoListId` est une clé étrangère dans la `ToDoList` table. Ce code indique à Entity Framework pour ajouter une contrainte foreign key à la base de données.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Ces classes définissent les données qui seront envoyées au client. « DTO » est l’acronyme « objet de transfert de données. » Le DTO définit comment les entités seront sérialisées en JSON. En règle générale, il existe plusieurs raisons d’utiliser des DTO :

- Pour contrôler quelles propriétés sont sérialisées. Le DTO peut contenir un sous-ensemble des propriétés du modèle de domaine. Vous pouvez procéder ainsi pour des raisons de sécurité (masquer les données sensibles) ou simplement pour réduire la quantité de données que vous envoyez.
- Pour modifier la forme des données – par exemple, pour aplatir une structure de données plus complexe.
- Pour conserver toute logique métier en dehors de l’objet DTO (séparation des préoccupations).
- Si vos modèles de domaine ne peut pas être sérialisées pour une raison quelconque. Par exemple, les références circulaires peuvent entraîner des problèmes lorsque vous sérialisez un objet sont moyens de gérer ce problème dans l’API Web (voir [gestion des références d’objet circulaires](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)) ; mais à l’aide d’un objet DTO simplement permet d’éviter le problème complètement.

Dans le modèle SPA, les objets DTO contient les mêmes données que les modèles de domaine. Toutefois, ils sont toujours utiles car ils évitent les références circulaires à partir des propriétés de navigation, et ils présentent le modèle DTO général.

**AccountModels.cs**

Ce fichier contient des modèles pour l’appartenance de site. Le `UserProfile` classe définit le schéma pour les profils utilisateur dans la base de données d’appartenance. (Dans ce cas, la seule information est l’ID d’utilisateur et le nom d’utilisateur.) Les autres classes de modèle dans ce fichier sont utilisés pour créer des formulaires d’inscription et connexion de l’utilisateur.

## <a name="entity-framework"></a>Entity Framework

Le modèle SPA utilise EF Code First. Dans le développement Code First, vous définissez tout d’abord les modèles dans le code, et EF utilise ensuite le modèle pour créer la base de données. Vous pouvez également utiliser Entity Framework avec une base de données existante ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

Le `TodoItemContext` dérive de la classe dans le dossier Models **DbContext**. Cette classe fournit le « glue » entre les modèles et d’Entity Framework. Le `TodoItemContext` contient un `ToDoItem` collection et un `TodoList` collection. Pour interroger la base de données, vous permet simplement d’écrire une requête LINQ sur ces collections. Par exemple, voici comment vous pouvez sélectionner toutes les listes de tâches pour l’utilisateur « Alice » :

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Vous pouvez également ajouter de nouveaux éléments à la collection, éléments, de mettre à jour ou supprimer des éléments de la collection et conserver les modifications apportées à la base de données.

## <a name="aspnet-web-api-controllers"></a>Contrôleurs d’API Web ASP.NET

Dans l’API Web ASP.NET, les contrôleurs sont des objets qui gèrent les demandes HTTP. Comme mentionné, le modèle SPA utilise des API Web pour activer les opérations CRUD sur `ToDoList` et `ToDoItem` instances. Les contrôleurs sont situés dans le dossier contrôleurs, de la solution.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Gère les requêtes HTTP pour les éléments de tâche
- `TodoListController`: Gère les requêtes HTTP pour les listes de tâches.

Ces noms sont importants, car le chemin d’accès de l’URI pour le nom du contrôleur correspond à l’API Web. (Pour savoir comment les API Web achemine les requêtes HTTP aux contrôleurs, consultez [routage dans ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Examinons la `ToDoListController` classe. Il contient un membre de données unique :

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

Le `TodoItemContext` est utilisé pour communiquer avec Entity Framework, comme décrit précédemment. Les méthodes sur le contrôleur implémentent les opérations CRUD. API Web mappe les requêtes HTTP à partir du client aux méthodes de contrôleur, comme suit :

| Requête HTTP | Méthode de contrôleur | Description |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtient une collection de listes de tâches. |
| GET/API/todo/*id* | `GetTodoList` | Obtient une liste de tâches par ID |
| PUT/API/todo/*id* | `PutTodoList` | Met à jour une liste de tâches. |
| POST /api/todo | `PostTodoList` | Crée une liste de tâches. |
| DELETE/API/todo/*id* | `DeleteTodoList` | Supprime une liste de tâches. |

Notez que les URI pour certaines opérations contiennent des espaces réservés pour la valeur d’ID. Par exemple, pour supprimer une liste à dont l’ID est 42, l’URI est `/api/todo/42`.

Pour en savoir plus sur l’utilisation des API Web pour les opérations CRUD, consultez [d’une API Web qui prend en charge les opérations CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Le code pour ce contrôleur est assez simple. Voici quelques points intéressants :

- Le `GetTodoLists` méthode utilise une requête LINQ pour filtrer les résultats par l’ID de l’utilisateur connecté. De cette façon, un utilisateur voit uniquement les données qui appartiennent à lui. En outre, notez qu’une instruction Select est utilisée pour convertir le `ToDoList` instances dans `TodoListDto` instances.
- Les méthodes PUT et POST vérifient l’état du modèle avant de modifier la base de données. Si **ModelState.IsValid** a la valeur false, ces méthodes retournent HTTP 400 demande incorrecte. En savoir plus sur la validation du modèle dans l’API Web à [Validation du modèle](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La classe de contrôleur est également décorée avec le **[Authorize]** attribut. Cet attribut vérifie si la requête HTTP est authentifiée. Si la demande n’est pas authentifiée, le client reçoit HTTP 401, non autorisé. En savoir plus sur l’authentification au niveau [authentification et autorisation dans ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

Le `TodoController` classe est très similaire à `TodoListController`. La plus grande différence est qu’il ne définit pas toutes les méthodes GET, car le client reçoit les tâches à effectuer, ainsi que de chaque liste de tâches.

## <a name="mvc-controllers-and-views"></a>Vues et contrôleurs MVC

Les contrôleurs MVC se trouvent également dans le dossier contrôleurs, de la solution. `HomeController` restitue le code HTML principal pour l’application. La vue pour le contrôleur Home est définie dans Views/Home/Index.cshtml. La vue d’accueil restitue un contenu différent selon que l’utilisateur est connecté :

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Lorsque les utilisateurs sont connectés, ils voient l’interface utilisateur principale. Sinon, ils voient le panneau de connexion. Notez que ce rendu conditionnel se produit côté serveur. N’essaient jamais de masquer le contenu sensible sur le côté client&#8212;tout ce que vous envoyez dans une réponse HTTP est visible à quelqu'un qui surveille les messages HTTP bruts.

## <a name="client-side-javascript-and-knockoutjs"></a>Knockout.js et JavaScript côté client

Maintenant nous allons activer à partir du serveur de l’application au client. Le modèle SPA utilise une combinaison de jQuery et Knockout.js pour créer une interface utilisateur interactive sans heurts. Knockout.js est une bibliothèque JavaScript qui facilite la liaison HTML aux données. Knockout.js utilise un modèle appelé « Model-View-ViewModel ».

- Le modèle est les données de domaine (listes de tâches et éléments de tâche).
- La vue est le document HTML.
- Le modèle de vue est un objet JavaScript qui contient les données de modèle. Le modèle de vue est une abstraction de code de l’interface utilisateur. Elle n’a aucune connaissance de la représentation HTML. Au lieu de cela, il représente les fonctionnalités abstraites de la vue, telles que « il s’agit d’une liste des éléments de tâche ».

La vue est lié aux données au modèle de vue. Mises à jour le modèle de vue sont automatiquement reflétées dans la vue. Liaisons fonctionnent à l’autre direction. Événements dans le DOM (par exemple, clique sur) sont liés aux données aux fonctions sur le modèle de vue, qui déclenchent des appels AJAX.

Le modèle SPA organise le code JavaScript côté client en trois couches :

- TODO.DataContext.js : envoie des demandes AJAX.
- TODO.Model.js : définit les modèles.
- TODO.ViewModel.js : définit le modèle de vue.

![](knockoutjs-template/_static/image11.png)

Ces fichiers de script se trouvent dans le dossier Scripts/application de la solution.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** gère tous les appels AJAX pour les contrôleurs d’API Web. (Les appels AJAX pour la connexion sont définis ailleurs, dans ajaxlogin.js.)

**TODO.Model.js** définit les modèles côté client (navigateur) pour les listes de tâches. Il existe deux classes de modèle : todoItem et todoList.

La plupart des propriétés dans les classes de modèle sont de type « ko.observable ». Sont des observables Knockout en quoi sa magie. À partir de la [documentation de Knockout](http://knockoutjs.com/documentation/introduction.html): un observable est un « objet JavaScript qui peut avertir les abonnés sur les modifications apportées. » Lorsque la valeur de l’observable change, Knockout met à jour tous les éléments HTML qui sont liés à ces observables. Par exemple, todoItem a observables pour les propriétés titre et l’opération est effectuée :

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Vous pouvez également vous abonner à un observable dans le code. Par exemple, la classe todoItem s’abonne à des modifications dans les propriétés « opération est effectuée » et « title » :

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modèle de vue**

Le modèle de vue est défini dans todo.viewmodel.js. Le modèle de vue est le point central, où l’application lie les éléments de page HTML pour les données de domaine. Dans le modèle SPA, le modèle de vue contient un tableau observable de todoLists. Le code suivant dans le modèle de vue indique à Knockout pour appliquer les liaisons :

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML et la liaison de données

Le HTML de la page principale est défini dans Views/Home/Index.cshtml. Étant donné que nous utilisons la liaison de données, le code HTML est uniquement un modèle pour ce qui est réellement rendu. Utilise Knockout *déclarative* liaisons. Vous liez des éléments de page à des données en ajoutant un attribut de « data-bind » à l’élément. Voici un exemple très simple, tirée de la documentation de Knockout :

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Dans cet exemple, Knockout met à jour le contenu de la **&lt;span&gt;** élément avec la valeur de `myItems.count()`. Chaque fois que cette valeur change, Knockout met à jour le document.

Knockout fournit différents types de liaison différents. Voici quelques-unes des liaisons utilisées dans le modèle SPA :

- **foreach**: vous permet d’effectuer une itération dans une boucle et appliquer le même balisage à chaque élément dans la liste. Cela est utilisé pour restituer les listes de tâches et les éléments de tâche. Dans le **foreach**, les liaisons sont appliquées aux éléments de la liste.
- **visible**: utilisé pour afficher ou masquer. Masquer le balisage lorsqu’une collection est vide ou que le message d’erreur visible.
- **valeur**: permet de remplir les valeurs de formulaire.
- **Cliquez sur**: lie un événement de clic à une fonction sur le modèle de vue.

## <a name="anti-csrf-protection"></a>Protection de l’anti-CSRF

Falsification de demande intersites (CSRF) est une attaque où un site malveillant envoie une demande à un site vulnérable, où l’utilisateur est actuellement connecté. Pour aider à empêcher les attaques CSRF, ASP.NET MVC utilise *des jetons anti-contrefaçon*, également appelé demande de jetons de vérification. L’idée est que le serveur place un jeton généré de manière aléatoire dans une page web. Lorsque le client envoie des données au serveur, il doit inclure cette valeur dans le message de demande.

Les jetons anti-contrefaçon fonctionnent, car la page malveillante ne peut pas lire des jetons de l’utilisateur, en raison des stratégies de même origine. (Les stratégies de même origine empêchent documents hébergés sur deux sites différents à partir de l’accès au contenu entre eux.)

ASP.NET MVC fournit la prise en charge intégrée pour les jetons anti-contrefaçon, via le [anti-contrefaçon](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) classe et le [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) attribut. Actuellement, cette fonctionnalité n’est pas intégrée dans l’API Web. Toutefois, le modèle SPA inclut une implémentation personnalisée pour l’API Web. Ce code est défini dans le `ValidateHttpAntiForgeryTokenAttribute` (classe), qui se trouve dans le dossier de filtres de la solution. Pour en savoir plus sur anti-CSRF dans l’API Web, consultez [empêchant Cross-Site demande falsification attaques](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusion

Le modèle SPA est conçu pour vous aider à commencer à écrire rapidement des applications web modernes et interactifs. Il utilise la bibliothèque Knockout.js pour séparer la présentation (balisage HTML) à partir de la logique de données et des applications. Mais, Knockout n’est pas la bibliothèque JavaScript seule, que vous pouvez utiliser pour créer une application SPA. Si vous souhaitez Explorer d’autres options, jetez un coup de œil à la [modèles créés par la Communauté](../templates/index.md).
