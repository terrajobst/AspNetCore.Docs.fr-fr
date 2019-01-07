---
title: Créer des API web avec ASP.NET Core et MongoDB
author: prkhandelwal
description: Ce didacticiel montre comment créer une API web ASP.NET Core à l’aide d’une base de données NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: bd9a36c5eb06542c820e71e937b8da10f735a0f8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577836"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="46fe7-103">Créer une API web avec ASP.NET Core et MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="46fe7-104">Par [Pratik Khandelwal](https://twitter.com/K2Prk) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="46fe7-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="46fe7-105">Ce didacticiel crée une API web qui effectue des opérations de création, lecture, mise à jour et suppression (CRUD) sur une base de données NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="46fe7-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="46fe7-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="46fe7-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="46fe7-107">Configurer MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-107">Configure MongoDB</span></span>
> * <span data-ttu-id="46fe7-108">Créer une base de données MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="46fe7-109">Définir une collection et un schéma MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="46fe7-110">Effectuer des opérations CRUD MongoDB à partir d’une API web</span><span class="sxs-lookup"><span data-stu-id="46fe7-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="46fe7-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="46fe7-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46fe7-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="46fe7-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46fe7-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46fe7-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="46fe7-114">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="46fe7-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="46fe7-115">[Visual Studio 2017 version 15.9 ou ultérieure](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="46fe7-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="46fe7-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="46fe7-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="46fe7-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="46fe7-118">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="46fe7-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="46fe7-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="46fe7-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="46fe7-120">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="46fe7-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="46fe7-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="46fe7-122">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="46fe7-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="46fe7-123">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="46fe7-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="46fe7-124">Visual Studio pour Mac version 7.7 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="46fe7-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="46fe7-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="46fe7-126">Configurer MongoDB</span><span class="sxs-lookup"><span data-stu-id="46fe7-126">Configure MongoDB</span></span>

<span data-ttu-id="46fe7-127">Si vous utilisez Windows, MongoDB est installé par défaut dans *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="46fe7-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="46fe7-128">Ajoutez *C:\Program Files\MongoDB\Server\<numéro_version>\bin* à la variable d’environnement `Path`.</span><span class="sxs-lookup"><span data-stu-id="46fe7-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="46fe7-129">Cette modification permet d’accéder à MongoDB depuis n’importe quel emplacement sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="46fe7-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="46fe7-130">Utilisez l’interpréteur de commandes mongo dans les étapes suivantes pour créer une base de données, des collections, et stocker des documents.</span><span class="sxs-lookup"><span data-stu-id="46fe7-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="46fe7-131">Pour plus d’informations sur les commandes de l’interpréteur mongo, consultez [Utilisation de l’interpréteur de commandes mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="46fe7-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="46fe7-132">Choisissez sur votre ordinateur de développement un répertoire pour le stockage des données.</span><span class="sxs-lookup"><span data-stu-id="46fe7-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="46fe7-133">Par exemple, *C:\BooksData* sous Windows.</span><span class="sxs-lookup"><span data-stu-id="46fe7-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="46fe7-134">Le cas échéant, créez le répertoire.</span><span class="sxs-lookup"><span data-stu-id="46fe7-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="46fe7-135">L’interpréteur de commandes mongo ne crée pas nouveaux répertoires.</span><span class="sxs-lookup"><span data-stu-id="46fe7-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="46fe7-136">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="46fe7-136">Open a command shell.</span></span> <span data-ttu-id="46fe7-137">Exécutez la commande suivante pour vous connecter à MongoDB sur le port par défaut 27017.</span><span class="sxs-lookup"><span data-stu-id="46fe7-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="46fe7-138">N’oubliez pas de remplacer `<data_directory_path>` par le répertoire que vous avez choisi à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="46fe7-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="46fe7-139">Ouvrez une autre instance de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="46fe7-139">Open another command shell instance.</span></span> <span data-ttu-id="46fe7-140">Connectez-vous à la base de données de test par défaut en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="46fe7-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="46fe7-141">Utilisez la ligne suivante dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="46fe7-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="46fe7-142">Le cas échéant, une base de données nommée *BookstoreDb* est créée.</span><span class="sxs-lookup"><span data-stu-id="46fe7-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="46fe7-143">Si la base de données existe déjà, sa connexion est ouverte pour les transactions.</span><span class="sxs-lookup"><span data-stu-id="46fe7-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="46fe7-144">Créez une collection `Books` à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="46fe7-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="46fe7-145">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="46fe7-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="46fe7-146">Définissez un schéma pour la collection `Books` et insérez deux documents à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="46fe7-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="46fe7-147">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="46fe7-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="46fe7-148">Affichez les documents de la base de données à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="46fe7-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="46fe7-149">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="46fe7-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="46fe7-150">Le schéma ajoute une propriété `_id` automatiquement générée de type `ObjectId` pour chaque document.</span><span class="sxs-lookup"><span data-stu-id="46fe7-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="46fe7-151">La base de données est en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="46fe7-151">The database is ready.</span></span> <span data-ttu-id="46fe7-152">Vous pouvez commencer à créer l’API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="46fe7-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="46fe7-153">Créer le projet d’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="46fe7-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="46fe7-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46fe7-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="46fe7-155">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="46fe7-156">Sélectionnez **Application web ASP.NET Core**, nommez le projet *BooksApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="46fe7-157">Sélectionnez le framework cible **.NET Core** et **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="46fe7-158">Sélectionnez le modèle de projet **API**, puis cliquez sur **OK** :</span><span class="sxs-lookup"><span data-stu-id="46fe7-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="46fe7-159">Dans la fenêtre **Console du Gestionnaire de Package**, accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="46fe7-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="46fe7-160">Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :</span><span class="sxs-lookup"><span data-stu-id="46fe7-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="46fe7-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="46fe7-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="46fe7-162">Exécutez les commandes suivantes dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="46fe7-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="46fe7-163">Un nouveau projet API web ASP.NET Core ciblant .NET Core est généré et ouvert dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="46fe7-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="46fe7-164">Cliquez sur **Oui** lorsque la notification *Les composants nécessaires à la build et au débogage sont manquants dans 'BooksApi'. Faut-il les ajouter ?* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="46fe7-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="46fe7-165">Ouvrez **Terminal intégré** et accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="46fe7-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="46fe7-166">Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :</span><span class="sxs-lookup"><span data-stu-id="46fe7-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="46fe7-167">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="46fe7-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="46fe7-168">Sélectionnez **Fichier** > **Nouvelle solution** > **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="46fe7-169">Sélectionnez le modèle de projet C# **API Web ASP.NET Core**, puis cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="46fe7-170">Sélectionnez **.NET Core 2.2** dans la liste déroulante **Framework cible**, puis cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="46fe7-171">Entrez *BooksApi* comme **Nom de projet**, puis cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="46fe7-172">Dans le panneau **Solution**, cliquez avec le bouton droit sur le nœud **Dépendances** du projet, puis sélectionnez **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="46fe7-173">Entrez *MongoDB.Driver* dans la zone de recherche, sélectionnez le package *MongoDB.Driver*, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="46fe7-174">Cliquez sur le bouton **Accepter** dans la boîte de dialogue **Acceptation de la licence**.</span><span class="sxs-lookup"><span data-stu-id="46fe7-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="46fe7-175">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="46fe7-175">Add a model</span></span>

1. <span data-ttu-id="46fe7-176">Ajoutez un répertoire *Modèles* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="46fe7-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="46fe7-177">Ajoutez une classe `Book` au répertoire *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="46fe7-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="46fe7-178">Dans la classe précédente, la propriété `Id` est requise pour mapper l’objet Common Language Runtime (CLR) à la collection MongoDB.</span><span class="sxs-lookup"><span data-stu-id="46fe7-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="46fe7-179">Les autres propriétés de la classe reçoivent l’attribut `[BsonElement]`.</span><span class="sxs-lookup"><span data-stu-id="46fe7-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="46fe7-180">La valeur de l’attribut représente le nom de la propriété dans la collection MongoDB.</span><span class="sxs-lookup"><span data-stu-id="46fe7-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="46fe7-181">Ajouter une classe d’opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="46fe7-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="46fe7-182">Ajoutez un répertoire *Services* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="46fe7-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="46fe7-183">Ajoutez une classe `BookService` au répertoire *Services* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="46fe7-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="46fe7-184">Ajoutez une chaîne de connexion MongoDB au fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="46fe7-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="46fe7-185">La propriété `BookstoreDb` précédente est accessible via le constructeur de classe `BookService`.</span><span class="sxs-lookup"><span data-stu-id="46fe7-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="46fe7-186">Dans `Startup.ConfigureServices`, inscrivez la classe `BookService` dans le système d’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="46fe7-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="46fe7-187">L’inscription du service qui précède est nécessaire pour prendre en charge l’injection de constructeurs dans les classes utilisées.</span><span class="sxs-lookup"><span data-stu-id="46fe7-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="46fe7-188">La classe `BookService` utilise les membres `MongoDB.Driver` suivants pour effectuer des opérations CRUD dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="46fe7-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="46fe7-189">`MongoClient` &ndash; Lit l’instance de serveur pour effectuer des opérations dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="46fe7-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="46fe7-190">Le constructeur de cette classe reçoit la chaîne de connexion MongoDB :</span><span class="sxs-lookup"><span data-stu-id="46fe7-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="46fe7-191">`IMongoDatabase` &ndash; Représente la base de données Mongo pour effectuer des opérations.</span><span class="sxs-lookup"><span data-stu-id="46fe7-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="46fe7-192">Ce didacticiel utilise la méthode générique `GetCollection<T>(collection)` sur l’interface pour accéder aux données d’une collection spécifique.</span><span class="sxs-lookup"><span data-stu-id="46fe7-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="46fe7-193">Des opérations CRUD peuvent être exécutées sur la collection, une fois cette méthode appelée.</span><span class="sxs-lookup"><span data-stu-id="46fe7-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="46fe7-194">Dans l’appel à la méthode `GetCollection<T>(collection)` :</span><span class="sxs-lookup"><span data-stu-id="46fe7-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="46fe7-195">`collection` représente le nom de la collection.</span><span class="sxs-lookup"><span data-stu-id="46fe7-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="46fe7-196">`T` représente le type d’objet CLR stocké dans la collection.</span><span class="sxs-lookup"><span data-stu-id="46fe7-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="46fe7-197">`GetCollection<T>(collection)` retourne un objet `MongoCollection` représentant la collection.</span><span class="sxs-lookup"><span data-stu-id="46fe7-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="46fe7-198">Dans ce didacticiel, les méthodes suivantes sont appelées sur la collection :</span><span class="sxs-lookup"><span data-stu-id="46fe7-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="46fe7-199">`Find<T>` &ndash; Retourne tous les documents dans la collection qui correspondent aux critères de recherche spécifiés.</span><span class="sxs-lookup"><span data-stu-id="46fe7-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="46fe7-200">`InsertOne` &ndash; Insère l’objet fourni en tant que nouveau document dans la collection.</span><span class="sxs-lookup"><span data-stu-id="46fe7-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="46fe7-201">`ReplaceOne` &ndash; Remplace le document unique correspondant aux critères de recherche spécifiés par l’objet fourni.</span><span class="sxs-lookup"><span data-stu-id="46fe7-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="46fe7-202">`DeleteOne` &ndash; Supprime un document unique correspondant aux critères de recherche spécifiés.</span><span class="sxs-lookup"><span data-stu-id="46fe7-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="46fe7-203">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="46fe7-203">Add a controller</span></span>

1. <span data-ttu-id="46fe7-204">Ajoutez une classe `BooksController` au répertoire *Contrôleurs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="46fe7-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="46fe7-205">Le contrôleur d’API web précédent :</span><span class="sxs-lookup"><span data-stu-id="46fe7-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="46fe7-206">Utilise la classe `BookService` pour effectuer des opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="46fe7-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="46fe7-207">Contient des méthodes d’action pour prendre en charge les requêtes HTTP GET, POST, PUT et DELETE.</span><span class="sxs-lookup"><span data-stu-id="46fe7-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="46fe7-208">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="46fe7-208">Build and run the app.</span></span>
1. <span data-ttu-id="46fe7-209">Accédez à `http://localhost:<port>/api/books` dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="46fe7-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="46fe7-210">La réponse JSON suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="46fe7-210">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="46fe7-211">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="46fe7-211">Next steps</span></span>

<span data-ttu-id="46fe7-212">Pour plus d’informations sur la création d’API web ASP.NET Core, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="46fe7-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
