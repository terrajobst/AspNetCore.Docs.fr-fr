---
uid: single-page-application/overview/templates/emberjs-template
title: Modèle EmberJS | Microsoft Docs
author: xqiu
description: Modèle EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: fbc3b1d299ace27d38d895e42b8e3bb3b51b36f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831313"
---
<a name="emberjs-template"></a><span data-ttu-id="74611-103">Modèle EmberJS</span><span class="sxs-lookup"><span data-stu-id="74611-103">EmberJS template</span></span>
====================
<span data-ttu-id="74611-104">par [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="74611-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="74611-105">Le modèle MVC EmberJS est rédigé par Nathan Totten, Thiago Santos et Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="74611-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="74611-106">Télécharger le modèle EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="74611-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="74611-107">Le modèle EmberJS SPA est conçu pour vous aider à créer rapidement des applications web côté client interactives à l’aide de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="74611-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="74611-108">« Single-page application » (SPA) est le terme général qui désigne une application web qui charge une seule page HTML et puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages.</span><span class="sxs-lookup"><span data-stu-id="74611-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="74611-109">Après le chargement de page initial, le SPA s’entretient avec le serveur via les demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="74611-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="74611-110">AJAX n’est pas nouveau, mais aujourd'hui, il existe des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée.</span><span class="sxs-lookup"><span data-stu-id="74611-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="74611-111">En outre, HTML5 et CSS3 sont qu’il soit plus facile de créer des interfaces utilisateur riches.</span><span class="sxs-lookup"><span data-stu-id="74611-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="74611-112">Le modèle de SPA EmberJS utilise le [Ember](http://emberjs.com/) bibliothèque JavaScript pour gérer les mises à jour de la page à partir de demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="74611-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="74611-113">Ember.js utilise la liaison de données pour synchroniser la page avec les dernières données.</span><span class="sxs-lookup"><span data-stu-id="74611-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="74611-114">De cette façon, il est inutile d’écrire du code qui vous guide à travers les données JSON et met à jour le modèle DOM.</span><span class="sxs-lookup"><span data-stu-id="74611-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="74611-115">Au lieu de cela, vous placez des attributs déclaratifs dans le code HTML qui indiquent Ember.js comment présenter les données.</span><span class="sxs-lookup"><span data-stu-id="74611-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="74611-116">Sur le côté serveur, le modèle EmberJS est presque identique à la [les modèle KnockoutJS SPA](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="74611-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="74611-117">Il utilise ASP.NET MVC pour traiter les documents HTML et ASP.NET Web API pour gérer les demandes AJAX à partir du client.</span><span class="sxs-lookup"><span data-stu-id="74611-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="74611-118">Pour plus d’informations sur ces aspects du modèle, reportez-vous à la [modèle KnockoutJS](../introduction/knockoutjs-template.md) documentation.</span><span class="sxs-lookup"><span data-stu-id="74611-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="74611-119">Cette rubrique se concentre sur les différences entre le modèle Knockout et le modèle EmberJS.</span><span class="sxs-lookup"><span data-stu-id="74611-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="74611-120">Créer un projet de modèle EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="74611-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="74611-121">Téléchargez et installez le modèle en cliquant sur le bouton Télécharger ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="74611-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="74611-122">Vous devrez peut-être redémarrer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74611-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="74611-123">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="74611-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="74611-124">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="74611-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="74611-125">Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="74611-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="74611-126">Nommez le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="74611-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="74611-127">Dans le **nouveau projet** Assistant, sélectionnez **Ember.js SPA projet**.</span><span class="sxs-lookup"><span data-stu-id="74611-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="74611-128">Vue d’ensemble du modèle EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="74611-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="74611-129">Le modèle EmberJS utilise une combinaison de jQuery, Ember.js, Handlebars.js pour créer une interface utilisateur interactive sans heurts.</span><span class="sxs-lookup"><span data-stu-id="74611-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="74611-130">Ember.js est une bibliothèque JavaScript qui utilise un modèle MVC côté client.</span><span class="sxs-lookup"><span data-stu-id="74611-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="74611-131">Un *modèle*, écrit dans le langage de création de modèles Handlebars, décrit l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="74611-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="74611-132">En mode de mise en production, le [compilateur de Handlebars](https://github.com/Myslik/csharp-ember-handlebars) sert à regrouper et compiler le modèle handlebars.</span><span class="sxs-lookup"><span data-stu-id="74611-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="74611-133">Un *modèle* stocke les données d’application qu’il obtient à partir du serveur (listes de tâches et éléments de tâche).</span><span class="sxs-lookup"><span data-stu-id="74611-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="74611-134">Un *contrôleur* stocke l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="74611-134">A *controller* stores application state.</span></span> <span data-ttu-id="74611-135">Contrôleurs présentent souvent des données de modèle pour les modèles correspondants.</span><span class="sxs-lookup"><span data-stu-id="74611-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="74611-136">Un *vue* se traduit par des événements primitifs à partir de l’application et transmet ces au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="74611-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="74611-137">Un *routeur* gère l’état de l’application, synchronisation des modèles et des URL.</span><span class="sxs-lookup"><span data-stu-id="74611-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="74611-138">En outre, la bibliothèque Ember données peut être utilisée pour synchroniser des objets JSON (obtenus à partir du serveur via une API RESTful) et les modèles de client.</span><span class="sxs-lookup"><span data-stu-id="74611-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="74611-139">Le modèle EmberJS SPA organise les scripts en huit couches :</span><span class="sxs-lookup"><span data-stu-id="74611-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="74611-140">WebAPI\_adapter.js, webapi\_serializer.js : étend la bibliothèque de données de Ember pour travailler avec l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="74611-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="74611-141">Scripts/Helpers.js : Définit les nouveaux programmes d’assistance Ember Handlebars.</span><span class="sxs-lookup"><span data-stu-id="74611-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="74611-142">Scripts/App.js : Crée l’application et configure l’adaptateur et le sérialiseur.</span><span class="sxs-lookup"><span data-stu-id="74611-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="74611-143">Modèles/application/scripts/\*.js : définit les modèles.</span><span class="sxs-lookup"><span data-stu-id="74611-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="74611-144">Scripts/application/vues/\*.js : définit les affichages.</span><span class="sxs-lookup"><span data-stu-id="74611-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="74611-145">Scripts/application/contrôleurs/\*.js : définit les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="74611-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="74611-146">Scripts/application/itinéraires, Scripts/app/router.js : Définit les itinéraires.</span><span class="sxs-lookup"><span data-stu-id="74611-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="74611-147">Modèles /\*.hbs : définit les modèles handlebars.</span><span class="sxs-lookup"><span data-stu-id="74611-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="74611-148">Examinons quelques-unes de ces scripts plus en détail.</span><span class="sxs-lookup"><span data-stu-id="74611-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="74611-149">Modèles</span><span class="sxs-lookup"><span data-stu-id="74611-149">Models</span></span>

<span data-ttu-id="74611-150">Les modèles sont définis dans le dossier Scripts / / modèles d’application.</span><span class="sxs-lookup"><span data-stu-id="74611-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="74611-151">Il existe deux fichiers de modèle : todoItem.js et todoList.js.</span><span class="sxs-lookup"><span data-stu-id="74611-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="74611-152">**TODO.Model.js** définit les modèles côté client (navigateur) pour les listes de tâches.</span><span class="sxs-lookup"><span data-stu-id="74611-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="74611-153">Il existe deux classes de modèle : todoItem et todoList.</span><span class="sxs-lookup"><span data-stu-id="74611-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="74611-154">Dans Ember, les modèles sont des sous-classes de DS. Modèle.</span><span class="sxs-lookup"><span data-stu-id="74611-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="74611-155">Un modèle peut avoir des propriétés avec des attributs :</span><span class="sxs-lookup"><span data-stu-id="74611-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="74611-156">Les modèles peuvent définir des relations à d’autres modèles :</span><span class="sxs-lookup"><span data-stu-id="74611-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="74611-157">Les modèles peuvent les propriétés qui lient à d’autres propriétés calculées :</span><span class="sxs-lookup"><span data-stu-id="74611-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="74611-158">Les modèles peuvent avoir des fonctions de l’Observateur, qui sont appelées lorsqu’une propriété observée change :</span><span class="sxs-lookup"><span data-stu-id="74611-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="74611-159">Affichages</span><span class="sxs-lookup"><span data-stu-id="74611-159">Views</span></span>

<span data-ttu-id="74611-160">Les vues sont définies dans le dossier Scripts/application/vues.</span><span class="sxs-lookup"><span data-stu-id="74611-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="74611-161">Une vue se traduit par des événements de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="74611-161">A view translates events from the application UI.</span></span> <span data-ttu-id="74611-162">Un gestionnaire d’événements peut rappeler aux fonctions du contrôleur, ou simplement appeler directement le contexte de données.</span><span class="sxs-lookup"><span data-stu-id="74611-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="74611-163">Par exemple, le code suivant provient de views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="74611-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="74611-164">Il définit la gestion des événements pour un champ de texte d’entrée.</span><span class="sxs-lookup"><span data-stu-id="74611-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="74611-165">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="74611-165">Controller</span></span>

<span data-ttu-id="74611-166">Les contrôleurs sont définis dans le dossier Scripts/application/contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="74611-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="74611-167">Pour représenter un modèle unique, étendre `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="74611-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="74611-168">Un contrôleur peut également représenter une collection de modèles en étendant `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="74611-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="74611-169">Par exemple, le TodoListController représente un tableau de `todoList` objets.</span><span class="sxs-lookup"><span data-stu-id="74611-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="74611-170">Le contrôleur de trie par ID de la liste des tâches, dans l’ordre décroissant :</span><span class="sxs-lookup"><span data-stu-id="74611-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="74611-171">Le contrôleur définit une fonction nommée `addTodoList`, qui crée une nouvelle liste des tâches et l’ajoute dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="74611-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="74611-172">Pour voir comment cette fonction est appelée, ouvrez le fichier de modèle nommé todoListTemplate.html, dans le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="74611-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="74611-173">Le code de modèle suivant lie un bouton à la `addTodoList` (fonction) :</span><span class="sxs-lookup"><span data-stu-id="74611-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="74611-174">Le contrôleur contient également un `error` propriété, qui contient un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="74611-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="74611-175">Voici le code du modèle pour afficher le message d’erreur (également dans todoListTemplate.html) :</span><span class="sxs-lookup"><span data-stu-id="74611-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="74611-176">Routes</span><span class="sxs-lookup"><span data-stu-id="74611-176">Routes</span></span>

<span data-ttu-id="74611-177">Router.js définit les itinéraires et le modèle par défaut à afficher, les jeux de l’état de l’application et correspond à des URL pour les itinéraires :</span><span class="sxs-lookup"><span data-stu-id="74611-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="74611-178">TodoListRoute.js charge les données pour le TodoListRoute en substituant la fonction setupController :</span><span class="sxs-lookup"><span data-stu-id="74611-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="74611-179">Ember utilise les conventions d’affectation de noms pour faire correspondre les URL, les noms de routes, contrôleurs et modèles.</span><span class="sxs-lookup"><span data-stu-id="74611-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="74611-180">Pour plus d’informations, consultez [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) à la documentation EmberJS.</span><span class="sxs-lookup"><span data-stu-id="74611-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="74611-181">Modèles</span><span class="sxs-lookup"><span data-stu-id="74611-181">Templates</span></span>

<span data-ttu-id="74611-182">Le dossier de modèles contient quatre modèles :</span><span class="sxs-lookup"><span data-stu-id="74611-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="74611-183">application.HBS : le modèle par défaut qui est affiché lorsque l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="74611-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="74611-184">About.HBS : le modèle pour l’itinéraire « / environ ».</span><span class="sxs-lookup"><span data-stu-id="74611-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="74611-185">index.HBS : le modèle pour la racine « / » itinéraire.</span><span class="sxs-lookup"><span data-stu-id="74611-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="74611-186">todoList.hbs : le modèle pour le « / todo « itinéraire.</span><span class="sxs-lookup"><span data-stu-id="74611-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="74611-187">\_navbar.HBS : le modèle définit le menu de navigation.</span><span class="sxs-lookup"><span data-stu-id="74611-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="74611-188">Le modèle d’application agit comme une page maître.</span><span class="sxs-lookup"><span data-stu-id="74611-188">The application template acts like a master page.</span></span> <span data-ttu-id="74611-189">Il contient un en-tête, un pied de page et un « {{outlet}} » pour insérer d’autres modèles dans en fonction de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="74611-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="74611-190">Pour plus d’informations sur les modèles d’application dans Ember, consultez [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="74611-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="74611-191">Le « / todoList « modèle contient deux expressions de boucle.</span><span class="sxs-lookup"><span data-stu-id="74611-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="74611-192">La boucle extérieure est `{{#each controller}}`et la boucle est `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="74611-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="74611-193">Le code suivant montre un intégré `Ember.Checkbox` afficher un texte personnalisé `App.TodoItemEditView`, ainsi qu’un lien avec un `deleteTodo` action.</span><span class="sxs-lookup"><span data-stu-id="74611-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="74611-194">Le `HtmlHelperExtensions` (classe), définie dans Controllers/HtmlHelperExensions.cs, définit une assistance afin de mettre en cache et insérer le modèle de fichiers quand **déboguer** a la valeur **true** dans le fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="74611-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="74611-195">Cette fonction est appelée à partir du fichier de vue ASP.NET MVC défini dans Views/Home/App.cshtml :</span><span class="sxs-lookup"><span data-stu-id="74611-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="74611-196">Appelée sans argument, la fonction affiche tous les fichiers de modèle dans le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="74611-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="74611-197">Vous pouvez également spécifier un sous-dossier ou un fichier de modèle spécifique.</span><span class="sxs-lookup"><span data-stu-id="74611-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="74611-198">Lorsque **déboguer** est **false** dans le fichier Web.config, l’application inclut l’élément de lot « ~/bundles/templates ».</span><span class="sxs-lookup"><span data-stu-id="74611-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="74611-199">Cet élément de regroupement est ajouté dans BundleConfig.cs, à l’aide de la bibliothèque de compilateur Handlebars :</span><span class="sxs-lookup"><span data-stu-id="74611-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
