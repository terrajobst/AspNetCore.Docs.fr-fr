---
title: Créer une API web avec ASP.NET Core et Visual Studio pour Mac
author: rick-anderson
description: Créer une API web avec ASP.NET Core MVC et Visual Studio pour Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Créer une API web avec ASP.NET Core et Visual Studio pour Mac

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)

Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches. L’interface utilisateur n’est pas construite.

Il existe trois versions de ce didacticiel :

* macOS : API web avec Visual Studio pour Mac (ce didacticiel)
* Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Pour obtenir un exemple qui utilise une base de données persistante, consultez [Présentation d’ASP.NET Core MVC sur macOS ou Linux](xref:tutorials/first-mvc-app-xplat/index).

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Créer le projet

Dans Visual Studio, sélectionnez **Fichier** > **Nouvelle solution**.

![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

Sélectionnez **Application .NET Core** > **API web ASP.NET Core** > **Suivant**.

![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)

Entrez *TodoApi* comme **Nom de projet**, puis cliquez sur **Créer**.

![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Lancer l’application

Dans Visual Studio, sélectionnez **Exécuter** > **Démarrer avec débogage** pour lancer l’application. Visual Studio lance un navigateur et accède à `http://localhost:5000`. Vous obtenez une erreur HTTP 404 (introuvable). Remplacez l’URL par `http://localhost:<port>/api/values`. Les données `ValuesController` s’affichent :

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Ajouter la prise en charge d’Entity Framework Core

Installez le fournisseur de base de données [Entity Framework Core InMemory](/ef/core/providers/in-memory/). Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.

* Dans le menu **Projet**, choisissez **Ajouter des packages NuGet**.

  * Vous pouvez aussi cliquer avec le bouton droit sur **Dépendances**, puis sélectionner **Ajouter des packages**.

* Entrez `EntityFrameworkCore.InMemory` dans la zone de recherche.
* Sélectionnez `Microsoft.EntityFrameworkCore.InMemory`, puis **Ajouter un package**.

### <a name="add-a-model-class"></a>Ajouter une classe de modèle

Un modèle est un objet qui représente les données de votre application. Dans le cas présent, le seul modèle est une tâche.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet. Sélectionnez **Ajouter** > **Nouveau dossier**. Nommez le dossier *Models*.

![nouveau dossier](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.

Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**. Nommez la classe *TodoItem* et cliquez sur **Nouveau**.

Remplacez le code généré par ce qui suit :

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

La base de données génère la valeur `Id` quand un `TodoItem` est créé.

### <a name="create-the-database-context"></a>Créer le contexte de base de données

Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié. Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.

Ajoutez une classe `TodoContext` au dossier *Models*.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Ajouter un contrôleur

Dans l’Explorateur de solutions, dans le dossier *Contrôleurs*, ajoutez la classe `TodoController`.

Remplacez le code généré par ce qui suit :

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Lancer l’application

Dans Visual Studio, sélectionnez **Exécuter** > **Démarrer avec débogage** pour lancer l’application. Visual Studio lance un navigateur et accède à `http://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire. Vous obtenez une erreur HTTP 404 (introuvable). Remplacez l’URL par `http://localhost:<port>/api/values`. Les données `ValuesController` s’affichent :

```json
["value1","value2"]
```

Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`. Le code JSON suivant est retourné :

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implémenter les autres opérations CRUD

Nous allons ajouter les méthodes `Create`, `Update` et `Delete` au contrôleur. Comme ces méthodes sont des variations sur un même thème, je vais simplement montrer le code et mettre en évidence les principales différences. Générez le projet après avoir ajouté ou modifié du code.

### <a name="create"></a>Créer

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

La méthode précédente répond à HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). L’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indique à MVC qu’il faut obtenir la valeur de la tâche dans le corps de la requête HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

La méthode précédente répond à HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC obtient la valeur de la tâche dans le corps de la requête HTTP.
::: moniker-end

La méthode `CreatedAtRoute` retourne une réponse 201. Il s’agit de la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur. `CreatedAtRoute` ajoute également un en-tête Location à la réponse. L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé. Consultez [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Utiliser Postman pour envoyer une requête Create

* Démarrez l’application (**Exécuter** > **Démarrer avec débogage**).
* Ouvrez Postman.

![Console Postman](first-web-api/_static/pmc.png)

* Mettez à jour le numéro de port dans l’URL localhost.
* Définissez la méthode HTTP sur *POST*.
* Cliquez sur l’onglet **Body** (Corps).
* Sélectionnez la case d’option **raw** (données brutes).
* Définissez le type sur *JSON (application/json)*.
* Entrez un corps de requête avec une tâche similaire au code JSON suivant :

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Cliquez sur le bouton **Send** (Envoyer).

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Si vous ne recevez pas de réponse après avoir cliqué sur **Send** (Envoyer), désactivez l’option **SSL certification verification** (Vérification de la certification SSL). Celle-ci se trouve sous **File** (Fichier) > **Settings** (Paramètres). Cliquez à nouveau sur le bouton **Send** (Envoyer) une fois le paramètre désactivé.
::: moniker-end

Cliquez sur l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse), puis copiez la valeur de l’en-tête **Location** (Emplacement) :

![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

Vous pouvez utiliser l’URI d’en-tête Location pour accéder à la ressource que vous avez créée. La méthode `Create` retourne [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Le premier paramètre passé à `CreatedAtRoute` représente la route nommée à utiliser pour générer l’URL. Rappelez la méthode `GetById` qui a créé la route nommée `"GetTodo"` :

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Mise à jour

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` est similaire à `Create`, mais utilise HTTP PUT. La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les différences. Pour prendre en charge les mises à jour partielles, utilisez HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Supprimer

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
