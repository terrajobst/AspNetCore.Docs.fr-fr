---
title: 'Tutoriel : Appeler une API web ASP.NET Core avec JavaScript'
author: rick-anderson
description: Découvrez comment appeler une API web ASP.NET Core avec JavaScript.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/web-api-javascript
ms.openlocfilehash: 0070816149d64fc1d71d453eb0f135050c78597a
ms.sourcegitcommit: de17150e5ec7507d7114dde0e5dbc2e45a66ef53
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70116638"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-javascript"></a>Tutoriel : Appeler une API web ASP.NET Core avec JavaScript

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce tutoriel montre comment appeler une API web ASP.NET Core avec JavaScript à l’aide de l’[API Fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API).

::: moniker range="< aspnetcore-3.0"

Pour ASP.NET Core 2.2, consultez la version 2.2 de [Appeler l’API web avec JavaScript](xref:tutorials/first-web-api#call-the-web-api-with-javascript).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Prérequis

* Suivre le [Tutoriel : Créer une API web](xref:tutorials/first-web-api)
* Connaissance de CSS, HTML et JavaScript

## <a name="call-the-web-api-with-javascript"></a>Appelez l’API web avec JavaScript

Dans cette section, vous allez ajouter une page HTML contenant des formulaires permettant de créer et de gérer des éléments de tâche. Des gestionnaires d’événements sont joints aux éléments de la page. Les gestionnaires d’événements génèrent des requêtes HTTP pour les méthodes d’action de l’API web. La fonction `fetch` de l’API Fetch lance chaque requête HTTP.

La fonction `fetch` retourne un objet `Promise` qui contient une réponse HTTP représentée sous la forme d’un objet `Response`. Un modèle courant consiste à extraire le corps de réponse JSON en appelant la fonction `json` sur l'objet `Response`. JavaScript met à jour la page avec les détails de la réponse de l’API Web.

L'appel `fetch` le plus simple accepte un seul paramètre représentant l’itinéraire. Un deuxième paramètre, connu sous le nom d’objet `init`, est facultatif. `init` est utilisé pour configurer la requête HTTP.

1. Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage du fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_). Le code en surbrillance suivant est nécessaire dans la méthode `Configure` de *Startup.cs* :

    [!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJavaScript.cs?highlight=8-9&name=snippet_configure)]

1. Créez un répertoire *wwwroot* à la racine du projet.

1. Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*. Remplacez son contenu par le balisage suivant :

    [!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

1. Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*. Remplacez son contenu par le code suivant :

    [!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_SiteJs)]

Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :

1. Ouvrez *Properties\launchSettings.json*.
1. Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.

Cet exemple appelle toutes les méthodes CRUD de l’API web. Les explications suivantes traitent des demandes de l’API web.

### <a name="get-a-list-of-to-do-items"></a>Obtenir une liste de tâches

Dans le code suivant, une requête HTTP GET est envoyée à l'itinéraire *api/TodoItems* :

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_GetItems)]

Quand l’API web retourne un code d’état de réussite, la fonction `_displayItems` est appelée. Chaque élément de tâche du paramètre de tableau accepté par `_displayItems` est ajouté à une table avec les boutons **Modifier** et **Supprimer**. Si la demande de l’API Web échoue, une erreur est consignée dans la console du navigateur.

### <a name="add-a-to-do-item"></a>Ajouter une tâche

Dans le code suivant :

* Une variable `item` est déclarée pour construire une représentation littérale d’objet de l’élément de tâche.
* Une requête Fetch est configurée avec les options suivantes :
    * `method`&mdash; spécifie le verbe d’action POST HTTP.
    * `body`&mdash; spécifie la représentation JSON du corps de la demande. Le JSON est généré en passant le littéral d’objet stocké dans `item` à la fonction [JSON. stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).
    * `headers`&mdash; spécifie les en-têtes de requête HTTP `Accept` et `Content-Type`. Les deux en-têtes sont définies sur `application/json` pour spécifier le type de média respectivement reçu et envoyé.
* Une requête HTTP POST est envoyée à l’itinéraire *api/TodoItems*.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_AddItem)]

Quand l’API web retourne un code d’état de réussite, la fonction `getItems` est appelée pour mettre à jour la table HTML. Si la demande de l’API Web échoue, une erreur est consignée dans la console du navigateur.

### <a name="update-a-to-do-item"></a>Mettre à jour une tâche

La mise à jour d’un élément de tâche est semblable à l’ajout d’un élément. Toutefois, il y a deux différences importantes :

* L’itinéraire est suivi de l’identificateur unique de l’élément à mettre à jour. Par exemple, *api/TodoItems/1*.
* Le verbe d’action HTTP est PUT, comme indiqué par l’option `method`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_UpdateItem)]

### <a name="delete-a-to-do-item"></a>Supprimer une tâche

Pour supprimer un élément de tâche, définissez l’option de la demande `method` sur `DELETE` et spécifiez l’identificateur unique de l’élément dans l’URL.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/js/site.js?name=snippet_DeleteItem)]

Passez au tutoriel suivant pour apprendre à générer des pages d’aide d’API web :

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>

::: moniker-end
