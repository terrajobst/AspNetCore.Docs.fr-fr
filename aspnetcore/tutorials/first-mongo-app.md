---
title: Créer des API web avec ASP.NET Core et MongoDB
author: prkhandelwal
description: Ce didacticiel montre comment créer une API web ASP.NET Core à l’aide d’une base de données NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 5e146261fdc8354fc9f4295a8af317e5cc36332f
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667334"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Créer une API web avec ASP.NET Core et MongoDB

Par [Pratik Khandelwal](https://twitter.com/K2Prk) et [Scott Addie](https://twitter.com/Scott_Addie)

Ce didacticiel crée une API web qui effectue des opérations de création, lecture, mise à jour et suppression (CRUD) sur une base de données NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).

Dans ce didacticiel, vous apprendrez à :

> [!div class="checklist"]
> * Configurer MongoDB
> * Créer une base de données MongoDB
> * Définir une collection et un schéma MongoDB
> * Effectuer des opérations CRUD MongoDB à partir d’une API web

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Kit SDK .NET Core 2.2 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017 version 15.9 ou ultérieure](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) avec la charge de travail **ASP.NET et développement web**
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Kit SDK .NET Core 2.2 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

* [Kit SDK .NET Core 2.2 ou version ultérieure](https://www.microsoft.com/net/download/all)
* [Visual Studio pour Mac version 7.7 ou ultérieure](https://www.visualstudio.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Configurer MongoDB

Si vous utilisez Windows, MongoDB est installé par défaut dans *C:\\Program Files\\MongoDB*. Ajoutez *C:\\Program Files\\MongoDB\\Server\\\<numéro_version>\\bin* à la variable d’environnement `Path`. Cette modification permet d’accéder à MongoDB depuis n’importe quel emplacement sur votre ordinateur de développement.

Utilisez l’interpréteur de commandes mongo dans les étapes suivantes pour créer une base de données, des collections, et stocker des documents. Pour plus d’informations sur les commandes de l’interpréteur mongo, consultez [Utilisation de l’interpréteur de commandes mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Choisissez sur votre ordinateur de développement un répertoire pour le stockage des données. Par exemple, *C:\\BooksData* sous Windows. Le cas échéant, créez le répertoire. L’interpréteur de commandes mongo ne crée pas nouveaux répertoires.
1. Ouvrez un interpréteur de commandes. Exécutez la commande suivante pour vous connecter à MongoDB sur le port par défaut 27017. N’oubliez pas de remplacer `<data_directory_path>` par le répertoire que vous avez choisi à l’étape précédente.

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. Ouvrez une autre instance de l’interpréteur de commandes. Connectez-vous à la base de données de test par défaut en exécutant la commande suivante :

    ```console
    mongo
    ```

1. Utilisez la ligne suivante dans l’interpréteur de commandes :

    ```console
    use BookstoreDb
    ```

    Le cas échéant, une base de données nommée *BookstoreDb* est créée. Si la base de données existe déjà, sa connexion est ouverte pour les transactions.

1. Créez une collection `Books` à l’aide de la commande suivante :

    ```console
    db.createCollection('Books')
    ```

    Le résultat suivant s’affiche :

    ```console
    { "ok" : 1 }
    ```

1. Définissez un schéma pour la collection `Books` et insérez deux documents à l’aide de la commande suivante :

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    Le résultat suivant s’affiche :

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. Affichez les documents de la base de données à l’aide de la commande suivante :

    ```console
    db.Books.find({}).pretty()
    ```

    Le résultat suivant s’affiche :

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    Le schéma ajoute une propriété `_id` automatiquement générée de type `ObjectId` pour chaque document.

La base de données est en lecture seule. Vous pouvez commencer à créer l’API web ASP.NET Core.

## <a name="create-the-aspnet-core-web-api-project"></a>Créer le projet d’API web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Sélectionnez **Fichier** > **Nouveau** > **Projet**.
1. Sélectionnez **Application web ASP.NET Core**, nommez le projet *BooksApi*, puis cliquez sur **OK**.
1. Sélectionnez le framework cible **.NET Core** et **ASP.NET Core 2.1**. Sélectionnez le modèle de projet **API**, puis cliquez sur **OK** :
1. Consultez la galerie [NuGet : MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) pour déterminer la dernière version stable du pilote .NET pour MongoDB. Dans la fenêtre **Console du Gestionnaire de Package**, accédez à la racine du projet. Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Exécutez les commandes suivantes dans l’interpréteur de commandes :

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    Un nouveau projet API web ASP.NET Core ciblant .NET Core est généré et ouvert dans Visual Studio Code.

1. Cliquez sur **Oui** lorsque la notification *Les composants nécessaires à la build et au débogage sont manquants dans 'BooksApi'. Faut-il les ajouter ?* s’affiche.
1. Consultez la galerie [NuGet : MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) pour déterminer la dernière version stable du pilote .NET pour MongoDB. Ouvrez **Terminal intégré** et accédez à la racine du projet. Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

1. Sélectionnez **Fichier** > **Nouvelle solution** > **.NET Core** > **App**.
1. Sélectionnez le modèle de projet C# **API Web ASP.NET Core**, puis cliquez sur **Suivant**.
1. Sélectionnez **.NET Core 2.2** dans la liste déroulante **Framework cible**, puis cliquez sur **Suivant**.
1. Entrez *BooksApi* comme **Nom de projet**, puis cliquez sur **Créer**.
1. Dans le panneau **Solution**, cliquez avec le bouton droit sur le nœud **Dépendances** du projet, puis sélectionnez **Ajouter des packages**.
1. Entrez *MongoDB.Driver* dans la zone de recherche, sélectionnez le package *MongoDB.Driver*, puis cliquez sur **Ajouter un package**.
1. Cliquez sur le bouton **Accepter** dans la boîte de dialogue **Acceptation de la licence**.

---

## <a name="add-a-model"></a>Ajouter un modèle

1. Ajoutez un répertoire *Modèles* à la racine du projet.
1. Ajoutez une classe `Book` au répertoire *Modèles* avec le code suivant :

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

Dans la classe précédente, la propriété `Id` :

* Est requise pour mapper l’objet Common Language Runtime (CLR) à la collection MongoDB.
* Est annotée avec `[BsonId]` pour désigner cette propriété comme clé primaire du document.
* Est annotée avec `[BsonRepresentation(BsonType.ObjectId)]` pour autoriser la transmission du paramètre en tant que type `string` au lieu de `ObjectId`. Mongo gère la conversion de `string` à `ObjectId`.

Les autres propriétés de la classe sont annotées avec l’attribut `[BsonElement]`. La valeur de l’attribut représente le nom de la propriété dans la collection MongoDB.

## <a name="add-a-crud-operations-class"></a>Ajouter une classe d’opérations CRUD

1. Ajoutez un répertoire *Services* à la racine du projet.
1. Ajoutez une classe `BookService` au répertoire *Services* avec le code suivant :

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. Ajoutez une chaîne de connexion MongoDB au fichier *appsettings.json* :

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    La propriété `BookstoreDb` précédente est accessible via le constructeur de classe `BookService`.

1. Dans `Startup.ConfigureServices`, inscrivez la classe `BookService` dans le système d’injection de dépendances :

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    L’inscription du service qui précède est nécessaire pour prendre en charge l’injection de constructeurs dans les classes utilisées.

La classe `BookService` utilise les membres `MongoDB.Driver` suivants pour effectuer des opérations CRUD dans la base de données :

* `MongoClient` &ndash; Lit l’instance de serveur pour effectuer des opérations dans la base de données. Le constructeur de cette classe reçoit la chaîne de connexion MongoDB :

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; Représente la base de données Mongo pour effectuer des opérations. Ce didacticiel utilise la méthode générique `GetCollection<T>(collection)` sur l’interface pour accéder aux données d’une collection spécifique. Des opérations CRUD peuvent être exécutées sur la collection, une fois cette méthode appelée. Dans l’appel à la méthode `GetCollection<T>(collection)` :
  * `collection` représente le nom de la collection.
  * `T` représente le type d’objet CLR stocké dans la collection.

`GetCollection<T>(collection)` retourne un objet `MongoCollection` représentant la collection. Dans ce didacticiel, les méthodes suivantes sont appelées sur la collection :

* `Find<T>` &ndash; Retourne tous les documents dans la collection qui correspondent aux critères de recherche spécifiés.
* `InsertOne` &ndash; Insère l’objet fourni en tant que nouveau document dans la collection.
* `ReplaceOne` &ndash; Remplace le document unique correspondant aux critères de recherche spécifiés par l’objet fourni.
* `DeleteOne` &ndash; Supprime un document unique correspondant aux critères de recherche spécifiés.

## <a name="add-a-controller"></a>Ajouter un contrôleur

1. Ajoutez une classe `BooksController` au répertoire *Contrôleurs* avec le code suivant :

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    Le contrôleur d’API web précédent :

    * Utilise la classe `BookService` pour effectuer des opérations CRUD.
    * Contient des méthodes d’action pour prendre en charge les requêtes HTTP GET, POST, PUT et DELETE.
1. Générez et exécutez l’application.
1. Accédez à `http://localhost:<port>/api/books` dans votre navigateur. La réponse JSON suivante apparaît :

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la création d’API web ASP.NET Core, consultez les ressources suivantes :

* <xref:web-api/index>
* <xref:web-api/action-return-types>
