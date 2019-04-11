---
title: Créer des API web avec ASP.NET Core et MongoDB
author: prkhandelwal
description: Ce didacticiel montre comment créer une API web ASP.NET Core à l’aide d’une base de données NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 5b8a0c963940d65545579b7120edac3571e4ad2a
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750690"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="cacbf-103">Créer une API web avec ASP.NET Core et MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="cacbf-104">Par [Pratik Khandelwal](https://twitter.com/K2Prk) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="cacbf-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="cacbf-105">Ce didacticiel crée une API web qui effectue des opérations de création, lecture, mise à jour et suppression (CRUD) sur une base de données NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="cacbf-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="cacbf-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="cacbf-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cacbf-107">Configurer MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-107">Configure MongoDB</span></span>
> * <span data-ttu-id="cacbf-108">Créer une base de données MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="cacbf-109">Définir une collection et un schéma MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="cacbf-110">Effectuer des opérations CRUD MongoDB à partir d’une API web</span><span class="sxs-lookup"><span data-stu-id="cacbf-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="cacbf-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cacbf-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cacbf-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="cacbf-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cacbf-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cacbf-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="cacbf-114">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cacbf-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="cacbf-115">[Visual Studio 2017 version 15.9 ou ultérieure](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="cacbf-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="cacbf-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cacbf-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cacbf-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="cacbf-118">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cacbf-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="cacbf-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cacbf-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="cacbf-120">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cacbf-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="cacbf-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cacbf-122">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="cacbf-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="cacbf-123">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cacbf-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="cacbf-124">Visual Studio pour Mac version 7.7 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="cacbf-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="cacbf-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="cacbf-126">Configurer MongoDB</span><span class="sxs-lookup"><span data-stu-id="cacbf-126">Configure MongoDB</span></span>

<span data-ttu-id="cacbf-127">Si vous utilisez Windows, MongoDB est installé par défaut dans *C:\\Program Files\\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="cacbf-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="cacbf-128">Ajoutez *C:\\Program Files\\MongoDB\\Server\\\<numéro_version>\\bin* à la variable d’environnement `Path`.</span><span class="sxs-lookup"><span data-stu-id="cacbf-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="cacbf-129">Cette modification permet d’accéder à MongoDB depuis n’importe quel emplacement sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="cacbf-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="cacbf-130">Utilisez l’interpréteur de commandes mongo dans les étapes suivantes pour créer une base de données, des collections, et stocker des documents.</span><span class="sxs-lookup"><span data-stu-id="cacbf-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="cacbf-131">Pour plus d’informations sur les commandes de l’interpréteur mongo, consultez [Utilisation de l’interpréteur de commandes mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="cacbf-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="cacbf-132">Choisissez sur votre ordinateur de développement un répertoire pour le stockage des données.</span><span class="sxs-lookup"><span data-stu-id="cacbf-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="cacbf-133">Par exemple, *C:\\BooksData* sous Windows.</span><span class="sxs-lookup"><span data-stu-id="cacbf-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="cacbf-134">Le cas échéant, créez le répertoire.</span><span class="sxs-lookup"><span data-stu-id="cacbf-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="cacbf-135">L’interpréteur de commandes mongo ne crée pas nouveaux répertoires.</span><span class="sxs-lookup"><span data-stu-id="cacbf-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="cacbf-136">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="cacbf-136">Open a command shell.</span></span> <span data-ttu-id="cacbf-137">Exécutez la commande suivante pour vous connecter à MongoDB sur le port par défaut 27017.</span><span class="sxs-lookup"><span data-stu-id="cacbf-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="cacbf-138">N’oubliez pas de remplacer `<data_directory_path>` par le répertoire que vous avez choisi à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="cacbf-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="cacbf-139">Ouvrez une autre instance de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="cacbf-139">Open another command shell instance.</span></span> <span data-ttu-id="cacbf-140">Connectez-vous à la base de données de test par défaut en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cacbf-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="cacbf-141">Utilisez la ligne suivante dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="cacbf-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="cacbf-142">Le cas échéant, une base de données nommée *BookstoreDb* est créée.</span><span class="sxs-lookup"><span data-stu-id="cacbf-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="cacbf-143">Si la base de données existe déjà, sa connexion est ouverte pour les transactions.</span><span class="sxs-lookup"><span data-stu-id="cacbf-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="cacbf-144">Créez une collection `Books` à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cacbf-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="cacbf-145">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="cacbf-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="cacbf-146">Définissez un schéma pour la collection `Books` et insérez deux documents à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cacbf-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="cacbf-147">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="cacbf-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="cacbf-148">Affichez les documents de la base de données à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cacbf-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="cacbf-149">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="cacbf-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="cacbf-150">Le schéma ajoute une propriété `_id` automatiquement générée de type `ObjectId` pour chaque document.</span><span class="sxs-lookup"><span data-stu-id="cacbf-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="cacbf-151">La base de données est en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="cacbf-151">The database is ready.</span></span> <span data-ttu-id="cacbf-152">Vous pouvez commencer à créer l’API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cacbf-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="cacbf-153">Créer le projet d’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cacbf-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cacbf-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cacbf-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="cacbf-155">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="cacbf-156">Sélectionnez **Application web ASP.NET Core**, nommez le projet *BooksApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="cacbf-157">Sélectionnez la version cible de .NET Framework **.NET Core** et **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-157">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="cacbf-158">Sélectionnez le modèle de projet **API**, puis cliquez sur **OK** :</span><span class="sxs-lookup"><span data-stu-id="cacbf-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="cacbf-159">Consultez la galerie [NuGet : MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) pour déterminer la dernière version stable du pilote .NET pour MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cacbf-159">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="cacbf-160">Dans la fenêtre **Console du Gestionnaire de Package**, accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="cacbf-160">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="cacbf-161">Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :</span><span class="sxs-lookup"><span data-stu-id="cacbf-161">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cacbf-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cacbf-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="cacbf-163">Exécutez les commandes suivantes dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="cacbf-163">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="cacbf-164">Un nouveau projet API web ASP.NET Core ciblant .NET Core est généré et ouvert dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="cacbf-164">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="cacbf-165">Cliquez sur **Oui** lorsque la notification *Les composants nécessaires à la build et au débogage sont manquants dans 'BooksApi'. Faut-il les ajouter ?* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cacbf-165">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="cacbf-166">Consultez la galerie [NuGet : MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) pour déterminer la dernière version stable du pilote .NET pour MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cacbf-166">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="cacbf-167">Ouvrez **Terminal intégré** et accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="cacbf-167">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="cacbf-168">Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :</span><span class="sxs-lookup"><span data-stu-id="cacbf-168">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cacbf-169">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="cacbf-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="cacbf-170">Sélectionnez **Fichier** > **Nouvelle solution** > **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-170">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="cacbf-171">Sélectionnez le modèle de projet C# **API Web ASP.NET Core**, puis cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-171">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="cacbf-172">Sélectionnez **.NET Core 2.2** dans la liste déroulante **Framework cible**, puis cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-172">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="cacbf-173">Entrez *BooksApi* comme **Nom de projet**, puis cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-173">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="cacbf-174">Dans le panneau **Solution**, cliquez avec le bouton droit sur le nœud **Dépendances** du projet, puis sélectionnez **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-174">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="cacbf-175">Entrez *MongoDB.Driver* dans la zone de recherche, sélectionnez le package *MongoDB.Driver*, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-175">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="cacbf-176">Cliquez sur le bouton **Accepter** dans la boîte de dialogue **Acceptation de la licence**.</span><span class="sxs-lookup"><span data-stu-id="cacbf-176">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="cacbf-177">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="cacbf-177">Add a model</span></span>

1. <span data-ttu-id="cacbf-178">Ajoutez un répertoire *Models* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="cacbf-178">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="cacbf-179">Ajoutez une classe `Book` au répertoire *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cacbf-179">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="cacbf-180">Dans la classe précédente, la propriété `Id` :</span><span class="sxs-lookup"><span data-stu-id="cacbf-180">In the preceding class, the `Id` property:</span></span>

* <span data-ttu-id="cacbf-181">Est requise pour mapper l’objet Common Language Runtime (CLR) à la collection MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cacbf-181">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
* <span data-ttu-id="cacbf-182">Est annotée avec `[BsonId]` pour désigner cette propriété comme clé primaire du document.</span><span class="sxs-lookup"><span data-stu-id="cacbf-182">Is annotated with `[BsonId]` to designate this property as the document's primary key.</span></span>
* <span data-ttu-id="cacbf-183">Est annotée avec `[BsonRepresentation(BsonType.ObjectId)]` pour autoriser la transmission du paramètre en tant que type `string` au lieu de `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="cacbf-183">Is annotated with `[BsonRepresentation(BsonType.ObjectId)]` to allow passing the parameter as type `string` instead of `ObjectId`.</span></span> <span data-ttu-id="cacbf-184">Mongo gère la conversion de `string` en `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="cacbf-184">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

<span data-ttu-id="cacbf-185">Les autres propriétés de la classe sont annotées avec l’attribut `[BsonElement]`.</span><span class="sxs-lookup"><span data-stu-id="cacbf-185">Other properties in the class are annotated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="cacbf-186">La valeur de l’attribut représente le nom de la propriété dans la collection MongoDB.</span><span class="sxs-lookup"><span data-stu-id="cacbf-186">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="cacbf-187">Ajouter une classe d’opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="cacbf-187">Add a CRUD operations class</span></span>

1. <span data-ttu-id="cacbf-188">Ajoutez un répertoire *Services* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="cacbf-188">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="cacbf-189">Ajoutez une classe `BookService` au répertoire *Services* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cacbf-189">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="cacbf-190">Ajoutez une chaîne de connexion MongoDB au fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="cacbf-190">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="cacbf-191">La propriété `BookstoreDb` précédente est accessible via le constructeur de classe `BookService`.</span><span class="sxs-lookup"><span data-stu-id="cacbf-191">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="cacbf-192">Dans `Startup.ConfigureServices`, inscrivez la classe `BookService` dans le système d’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="cacbf-192">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="cacbf-193">L’inscription du service qui précède est nécessaire pour prendre en charge l’injection de constructeurs dans les classes utilisées.</span><span class="sxs-lookup"><span data-stu-id="cacbf-193">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="cacbf-194">La classe `BookService` utilise les membres `MongoDB.Driver` suivants pour effectuer des opérations CRUD dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="cacbf-194">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="cacbf-195">`MongoClient` &ndash; Lit l’instance de serveur pour effectuer des opérations dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cacbf-195">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="cacbf-196">Le constructeur de cette classe reçoit la chaîne de connexion MongoDB :</span><span class="sxs-lookup"><span data-stu-id="cacbf-196">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="cacbf-197">`IMongoDatabase` &ndash; Représente la base de données Mongo pour effectuer des opérations.</span><span class="sxs-lookup"><span data-stu-id="cacbf-197">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="cacbf-198">Ce didacticiel utilise la méthode générique `GetCollection<T>(collection)` sur l’interface pour accéder aux données d’une collection spécifique.</span><span class="sxs-lookup"><span data-stu-id="cacbf-198">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="cacbf-199">Des opérations CRUD peuvent être exécutées sur la collection, une fois cette méthode appelée.</span><span class="sxs-lookup"><span data-stu-id="cacbf-199">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="cacbf-200">Dans l’appel à la méthode `GetCollection<T>(collection)` :</span><span class="sxs-lookup"><span data-stu-id="cacbf-200">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="cacbf-201">`collection` représente le nom de la collection.</span><span class="sxs-lookup"><span data-stu-id="cacbf-201">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="cacbf-202">`T` représente le type d’objet CLR stocké dans la collection.</span><span class="sxs-lookup"><span data-stu-id="cacbf-202">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="cacbf-203">`GetCollection<T>(collection)` retourne un objet `MongoCollection` représentant la collection.</span><span class="sxs-lookup"><span data-stu-id="cacbf-203">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="cacbf-204">Dans ce didacticiel, les méthodes suivantes sont appelées sur la collection :</span><span class="sxs-lookup"><span data-stu-id="cacbf-204">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="cacbf-205">`Find<T>` &ndash; Retourne tous les documents dans la collection qui correspondent aux critères de recherche spécifiés.</span><span class="sxs-lookup"><span data-stu-id="cacbf-205">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="cacbf-206">`InsertOne` &ndash; Insère l’objet fourni en tant que nouveau document dans la collection.</span><span class="sxs-lookup"><span data-stu-id="cacbf-206">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="cacbf-207">`ReplaceOne` &ndash; Remplace le document unique correspondant aux critères de recherche spécifiés par l’objet fourni.</span><span class="sxs-lookup"><span data-stu-id="cacbf-207">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="cacbf-208">`DeleteOne` &ndash; Supprime un document unique correspondant aux critères de recherche spécifiés.</span><span class="sxs-lookup"><span data-stu-id="cacbf-208">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="cacbf-209">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="cacbf-209">Add a controller</span></span>

1. <span data-ttu-id="cacbf-210">Ajoutez une classe `BooksController` au répertoire *Controllers* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cacbf-210">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="cacbf-211">Le contrôleur d’API web précédent :</span><span class="sxs-lookup"><span data-stu-id="cacbf-211">The preceding web API controller:</span></span>

    * <span data-ttu-id="cacbf-212">Utilise la classe `BookService` pour effectuer des opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="cacbf-212">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="cacbf-213">Contient des méthodes d’action pour prendre en charge les requêtes HTTP GET, POST, PUT et DELETE.</span><span class="sxs-lookup"><span data-stu-id="cacbf-213">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
    * <span data-ttu-id="cacbf-214">La méthode <xref:System.Web.Http.ApiController.CreatedAtRoute*> retourne une réponse 201, qui constitue la réponse standard pour une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="cacbf-214">The <xref:System.Web.Http.ApiController.CreatedAtRoute*> method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="cacbf-215">`CreatedAtRoute` ajoute également un en-tête Location à la réponse.</span><span class="sxs-lookup"><span data-stu-id="cacbf-215">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="cacbf-216">L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="cacbf-216">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="cacbf-217">Consultez [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="cacbf-217">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
1. <span data-ttu-id="cacbf-218">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="cacbf-218">Build and run the app.</span></span>
1. <span data-ttu-id="cacbf-219">Accédez à `http://localhost:<port>/api/books` dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="cacbf-219">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="cacbf-220">La réponse JSON suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="cacbf-220">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="cacbf-221">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cacbf-221">Next steps</span></span>

<span data-ttu-id="cacbf-222">Pour plus d’informations sur la création d’API web ASP.NET Core, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="cacbf-222">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
