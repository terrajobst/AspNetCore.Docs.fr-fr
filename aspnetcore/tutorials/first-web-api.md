---
title: 'Didacticiel : créer une API Web avec ASP.NET Core'
author: rick-anderson
description: Apprendre à créer une API web avec ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541853"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Didacticiel : créer une API Web avec ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)

Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.

Dans ce didacticiel, vous apprendrez à :

> [!div class="checklist"]
> * Créer un projet d’API web.
> * Ajouter une classe de modèle et un contexte de base de données.
> * Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.
> * Configurer le routage, les chemins d’URL et les valeurs de retour.
> * Appeler l’API web avec Postman

À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.

## <a name="overview"></a>Vue d'ensemble

Ce didacticiel crée une API Web contenant les points de terminaison suivants :

|Point de terminaison                  |Description            |Corps de la requête|Corps de réponse       |
|--------------------------|-----------------------|------------|--------------------|
|GET /api/TodoItems        |Obtenir toutes les tâches    |aucune.        |Tableau de tâches|
|GET /api/TodoItems/{id}   |Obtenir un élément par ID      |aucune.        |Tâche          |
|POST /api/TodoItems       |Ajouter un nouvel élément         |Tâche  |Tâche          |
|PUT /api/TodoItems/{id}   |Mettre à jour un élément existant|Tâche  |aucune.                |
|SUPPRIMER/api/TodoItems/{id}|Supprimer un élément         |aucune.        |aucune.                |

Le diagramme suivant illustre la conception de l’application.

![Le client est représenté par une zone située à gauche. Il envoie une demande et reçoit une réponse de l’application, représentée par une zone dessinée à droite. Dans la zone de l’application, trois zones représentent le contrôleur, le modèle et la couche d’accès aux données. La requête provient du contrôleur de l’application, et les opérations de lecture/écriture se produisent entre le contrôleur et la couche d’accès aux données. Le modèle est sérialisé et retourné au client dans la réponse.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Configuration requise

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>Créer un projet web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
1. Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.
1. Nommez le projet *TodoApi* et cliquez sur **Créer**.
1. Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés. Sélectionnez le modèle **API** et cliquez sur **Créer**.

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).
1. Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.
1. Exécutez les commandes suivantes :

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.

  Les commandes précédentes :

  * Créez un projet d’API Web et ouvrez-le dans Visual Studio Code.
  * Ajoutez les packages NuGet qui sont requis dans la section suivante.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

1. Sélectionnez **Fichier** > **Nouvelle solution**.

    ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

1. Sélectionnez **.NET Core** > **Application** > **API** > **Suivant**.

    ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
1. Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, sélectionnez *.NET Core 3.0* comme **Framework cible**.
1. Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

Ouvrez un terminal de commande dans le dossier de projet, puis exécutez les commandes suivantes :

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>Tester l’API

Le modèle de projet crée une API `WeatherForecast`. Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Appuyez sur <kbd>CTRL + F5</kbd> pour exécuter l’application. Visual Studio lance un navigateur et accède à `https://localhost:<port>/WeatherForecast`, où `<port>` est un numéro de port choisi de manière aléatoire.

Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**. Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Appuyez sur <kbd>CTRL + F5</kbd> pour exécuter l’application. Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application. Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire. Une erreur HTTP 404 (introuvable) est retournée. Ajoutez `/WeatherForecast` à l’URL (définissez-la sur `https://localhost:<port>/WeatherForecast`).

---

Un code JSON similaire au suivant est retourné :

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a>Ajouter une classe de modèle

Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application. Le modèle pour cette application est une classe `TodoItem` unique.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet. Sélectionnez **Ajouter** > **Nouveau dossier**. Nommez le dossier *Models*.
1. Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**. Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.
1. Remplacez le code du modèle par le code suivant :

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Ajoutez un dossier nommé *Models*.
1. Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

1. Cliquez avec le bouton droit sur le projet. Sélectionnez **Ajouter** > **Nouveau dossier**. Nommez le dossier *Models*.

    ![nouveau dossier](first-web-api-mac/_static/folder.png)

1. Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.
1. Nommez la classe *TodoItem* et cliquez sur **Nouveau**.
1. Remplacez le code du modèle par le code suivant :

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.

Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.

## <a name="add-a-database-context"></a>Ajouter un contexte de base de données

Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données. Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Ajouter Microsoft.EntityFrameworkCore.SqlServer

1. Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.
1. Sélectionnez l’onglet **Parcourir**, puis entrez **Microsoft.EntityFrameworkCore.SqlServer** dans la zone de recherche.
1. Dans le volet gauche, sélectionnez **Microsoft. EntityFrameworkCore. SqlServer** .
1. Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.
1. Utilisez les instructions précédentes pour ajouter le package NuGet `Microsoft.EntityFrameworkCore.InMemory`.

![NuGet Package Manager](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a>Ajouter le contexte de base de données TodoContext

* Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**. Nommez la classe *TodoContext* et cliquez sur **Ajouter**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

* Ajoutez une classe `TodoContext` au dossier *Models*.

---

* Entrez le code suivant :

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Inscrire le contexte de base de données

Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du conteneur d' [injection de dépendances (di)](xref:fundamentals/dependency-injection) . Le conteneur fournit le service aux contrôleurs.

Mettez à jour *Startup.cs* avec le code en surbrillance suivant :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Le code précédent :

* Supprime les déclarations `using` inutilisées.
* Ajoute le contexte de base de données au conteneur d’injection de dépendances.
* Spécifie que le contexte de base de données utilise une base de données en mémoire.

## <a name="scaffold-a-controller"></a>Générer automatiquement des modèles pour un contrôleur

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Cliquez avec le bouton droit sur le dossier *Contrôleurs*.
1. Sélectionnez **ajouter**  > **nouvel élément de génération de modèles**automatique.
1. Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework**, puis **Ajouter**.
1. Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework** :
    * Sélectionnez **TodoItem (TodoApi. Models)** dans la **classe de modèle**.
    * Sélectionnez **TodoContext (TodoApi. Models)** dans la **classe de contexte de données**.
    * Sélectionnez **Ajouter** .

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

Exécutez les commandes suivantes :

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

Les commandes précédentes :

* Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.
* Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).
* Génèrent automatiquement des modèles pour `TodoItemsController`.

---

Le code généré :

* Définit une classe de contrôleur d’API sans méthodes.
* Décore la classe avec l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute). Cet attribut indique que le contrôleur répond aux requêtes de l’API web. Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.
* Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur. Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.

## <a name="examine-the-posttodoitem-create-method"></a>Examiner la méthode de création de PostTodoItem

Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute). La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.

La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :

* Retourne un code d’état HTTP 201 en cas de réussite. HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.
* Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse. L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé. Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête. Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.

### <a name="install-postman"></a>Installer Postman

Ce tutoriel utilise Postman pour tester l’API web.

1. Installez [Postman](https://www.getpostman.com/downloads/).
1. Démarrez l’application web.
1. Démarrez Postman.
1. Désactivez la **vérification du certificat SSL**.
1. À partir de**paramètres** de  >  de **fichier** (onglet**général** ), désactivez la **vérification de certificat SSL**.

    > [!WARNING]
    > Réactivez la vérification du certificat SSL après avoir testé le contrôleur.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Tester PostTodoItem avec Postman

1. Créez une requête.
1. Affectez `POST` à la méthode HTTP.
1. Sélectionnez l’onglet **Body** (Corps).
1. Sélectionnez la case d’option **raw** (données brutes).
1. Définissez le type sur **JSON (application/json)** .
1. Dans le corps de la demande, entrez JSON pour un élément de tâche :

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. Sélectionnez **Send** (Envoyer).

  ![Postman avec requête de création](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a>Tester l’URI de l’en-tête d’emplacement

1. Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).
1. Copiez la valeur d’en-tête **Location** (Emplacement) :

    ![Onglet Headers de la console Postman](first-web-api/_static/create.png)

1. Définissez la méthode sur GET.
1. Collez l’URI (par exemple, `https://localhost:5001/api/TodoItems/1`).
1. Sélectionnez **Send** (Envoyer).

## <a name="examine-the-get-methods"></a>Examiner les méthodes GET

Ces méthodes implémentent deux points de terminaison GET :

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman. Exemple :

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a>Tester Get avec Postman

1. Créez une requête.
1. Définissez la méthode HTTP sur **GET**.
1. Définissez l’URL de la requête sur `https://localhost:<port>/api/TodoItems`. Par exemple, `https://localhost:5001/api/TodoItems`.
1. Définissez l’**affichage à deux volets** dans Postman.
1. Sélectionnez **Send** (Envoyer).

Cette application utilise une base de données en mémoire. Si l’application est arrêtée et démarrée, la requête d’extraction précédente ne retourne pas de données. Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.

## <a name="routing-and-url-paths"></a>Routage et chemins d’URL

L’attribut [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) désigne une méthode qui répond à une requête http. Le chemin d’URL pour chaque méthode est construit comme suit :

* Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ». Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems**Controller, le nom du contrôleur est « TodoItems ». Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.
* Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin. Cet exemple n’utilise pas de modèle. Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche. Lorsque `GetTodoItem` est appelé, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Valeurs de retour

Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse. Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée. Les exceptions non gérées sont converties en erreurs 5xx.

Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP. Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :

* Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound>.
* Sinon, la méthode retourne 200 avec un corps de réponse JSON. Le retour de `item` entraîne une réponse HTTP 200.

## <a name="the-puttodoitem-method"></a>Méthode PutTodoItem

Examinez la méthode `PutTodoItem` :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT. La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements. Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.

### <a name="test-the-puttodoitem-method"></a>Tester la méthode PutTodoItem

Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée. La base de données doit contenir un élément avant que vous ne passiez un appel PUT. Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.

Mettez à jour l’élément de tâche dont l’ID est 1. Définissez son nom sur « flux de poisson » :

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

L’image suivante montre la mise à jour Postman :

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a>Méthode DeleteTodoItem

Examinez la méthode `DeleteTodoItem` :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Tester la méthode DeleteTodoItem

Utilisez Postman pour supprimer une tâche :

1. Définissez la méthode sur `DELETE`.
1. Définissez l’URI de l’objet à supprimer (par exemple, `https://localhost:5001/api/TodoItems/1`).
1. Sélectionnez **Send** (Envoyer).

## <a name="call-the-web-api-with-javascript"></a>Appelez l’API web avec JavaScript

Consultez [Didacticiel : appeler une API web ASP.net core avec JavaScript](xref:tutorials/web-api-javascript).

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>Ajouter la prise en charge de l’authentification à une API Web

Consultez le didacticiel [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .

## <a name="additional-resources"></a>Ressources supplémentaires

[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).

Pour plus d'informations, reportez-vous aux ressources suivantes :

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Version YouTube de ce tutoriel](https://www.youtube.com/watch?v=TTkhEyGBfAk)
