---
title: Créer une API web avec ASP.NET Core et MongoDB
author: prkhandelwal
description: Ce tutoriel montre comment créer une API web ASP.NET Core à l’aide d’une base de données NoSQL MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/04/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 6a8c5d75f562b38015101e039a2f5d96a5491595
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692549"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="1aae5-103">Créer une API web avec ASP.NET Core et MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="1aae5-104">Par [Pratik Khandelwal](https://twitter.com/K2Prk) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1aae5-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1aae5-105">Ce didacticiel crée une API web qui effectue des opérations de création, lecture, mise à jour et suppression (CRUD) sur une base de données NoSQL [MongoDB](https://www.mongodb.com/what-is-mongodb).</span><span class="sxs-lookup"><span data-stu-id="1aae5-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="1aae5-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="1aae5-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1aae5-107">Configurer MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-107">Configure MongoDB</span></span>
> * <span data-ttu-id="1aae5-108">Créer une base de données MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="1aae5-109">Définir une collection et un schéma MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="1aae5-110">Effectuer des opérations CRUD MongoDB à partir d’une API web</span><span class="sxs-lookup"><span data-stu-id="1aae5-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="1aae5-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1aae5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aae5-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="1aae5-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1aae5-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1aae5-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="1aae5-114">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="1aae5-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="1aae5-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="1aae5-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="1aae5-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1aae5-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1aae5-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="1aae5-118">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="1aae5-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1aae5-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1aae5-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="1aae5-120">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1aae5-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="1aae5-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1aae5-122">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="1aae5-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="1aae5-123">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="1aae5-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="1aae5-124">Visual Studio pour Mac version 7.7 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="1aae5-124">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="1aae5-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="1aae5-126">Configurer MongoDB</span><span class="sxs-lookup"><span data-stu-id="1aae5-126">Configure MongoDB</span></span>

<span data-ttu-id="1aae5-127">Si vous utilisez Windows, MongoDB est installé par défaut dans *C:\\Program Files\\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="1aae5-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="1aae5-128">Ajoutez *C:\\Program Files\\MongoDB\\Server\\\<numéro_version>\\bin* à la variable d’environnement `Path`.</span><span class="sxs-lookup"><span data-stu-id="1aae5-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="1aae5-129">Cette modification permet d’accéder à MongoDB depuis n’importe quel emplacement sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="1aae5-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="1aae5-130">Utilisez l’interpréteur de commandes mongo dans les étapes suivantes pour créer une base de données, des collections, et stocker des documents.</span><span class="sxs-lookup"><span data-stu-id="1aae5-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="1aae5-131">Pour plus d’informations sur les commandes de l’interpréteur mongo, consultez [Utilisation de l’interpréteur de commandes mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="1aae5-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="1aae5-132">Choisissez sur votre ordinateur de développement un répertoire pour le stockage des données.</span><span class="sxs-lookup"><span data-stu-id="1aae5-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="1aae5-133">Par exemple, *C:\\BooksData* sous Windows.</span><span class="sxs-lookup"><span data-stu-id="1aae5-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="1aae5-134">Le cas échéant, créez le répertoire.</span><span class="sxs-lookup"><span data-stu-id="1aae5-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="1aae5-135">L’interpréteur de commandes mongo ne crée pas nouveaux répertoires.</span><span class="sxs-lookup"><span data-stu-id="1aae5-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="1aae5-136">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="1aae5-136">Open a command shell.</span></span> <span data-ttu-id="1aae5-137">Exécutez la commande suivante pour vous connecter à MongoDB sur le port par défaut 27017.</span><span class="sxs-lookup"><span data-stu-id="1aae5-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="1aae5-138">N’oubliez pas de remplacer `<data_directory_path>` par le répertoire que vous avez choisi à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="1aae5-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="1aae5-139">Ouvrez une autre instance de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="1aae5-139">Open another command shell instance.</span></span> <span data-ttu-id="1aae5-140">Connectez-vous à la base de données de test par défaut en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1aae5-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="1aae5-141">Utilisez la ligne suivante dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="1aae5-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="1aae5-142">Le cas échéant, une base de données nommée *BookstoreDb* est créée.</span><span class="sxs-lookup"><span data-stu-id="1aae5-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="1aae5-143">Si la base de données existe déjà, sa connexion est ouverte pour les transactions.</span><span class="sxs-lookup"><span data-stu-id="1aae5-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="1aae5-144">Créez une collection `Books` à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1aae5-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="1aae5-145">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="1aae5-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="1aae5-146">Définissez un schéma pour la collection `Books` et insérez deux documents à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1aae5-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="1aae5-147">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="1aae5-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="1aae5-148">Affichez les documents de la base de données à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1aae5-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="1aae5-149">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="1aae5-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="1aae5-150">Le schéma ajoute une propriété `_id` automatiquement générée de type `ObjectId` pour chaque document.</span><span class="sxs-lookup"><span data-stu-id="1aae5-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="1aae5-151">La base de données est en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="1aae5-151">The database is ready.</span></span> <span data-ttu-id="1aae5-152">Vous pouvez commencer à créer l’API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1aae5-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="1aae5-153">Créer le projet d’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1aae5-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1aae5-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1aae5-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1aae5-155">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="1aae5-156">Sélectionnez le type de projet **Application web ASP.NET Core**, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-156">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="1aae5-157">Nommez le projet *BooksApi*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-157">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="1aae5-158">Sélectionnez la version cible de .NET Framework **.NET Core** et **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-158">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="1aae5-159">Sélectionnez le modèle de projet **API**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-159">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="1aae5-160">Consultez la galerie [NuGet : MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) pour déterminer la dernière version stable du pilote .NET pour MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1aae5-160">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="1aae5-161">Dans la fenêtre **Console du Gestionnaire de Package**, accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="1aae5-161">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="1aae5-162">Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :</span><span class="sxs-lookup"><span data-stu-id="1aae5-162">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1aae5-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1aae5-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="1aae5-164">Exécutez les commandes suivantes dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="1aae5-164">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="1aae5-165">Un nouveau projet API web ASP.NET Core ciblant .NET Core est généré et ouvert dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1aae5-165">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="1aae5-166">Une fois que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les composants nécessaires à la build et au débogage sont manquants dans « BooksApi ». Faut-il les ajouter ?** .</span><span class="sxs-lookup"><span data-stu-id="1aae5-166">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="1aae5-167">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-167">Select **Yes**.</span></span>
1. <span data-ttu-id="1aae5-168">Consultez la galerie [NuGet : MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) pour déterminer la dernière version stable du pilote .NET pour MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1aae5-168">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="1aae5-169">Ouvrez **Terminal intégré** et accédez à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="1aae5-169">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="1aae5-170">Exécutez la commande suivante afin d’installer le pilote .NET pour MongoDB :</span><span class="sxs-lookup"><span data-stu-id="1aae5-170">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1aae5-171">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="1aae5-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="1aae5-172">Sélectionnez **Fichier** > **Nouvelle solution** >  **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-172">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="1aae5-173">Sélectionnez le modèle de projet C# **API web ASP.NET Core**, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-173">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="1aae5-174">Sélectionnez **.NET Core 2.2** dans la liste déroulante **Framework cible**, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-174">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="1aae5-175">Entrez *BooksApi* pour **Nom de projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-175">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="1aae5-176">Dans le panneau **Solution**, cliquez avec le bouton droit sur le nœud **Dépendances** du projet, puis sélectionnez **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-176">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="1aae5-177">Entrez *MongoDB.Driver* dans la zone de recherche, sélectionnez le package *MongoDB.Driver*, puis sélectionnez **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-177">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="1aae5-178">Sélectionnez le bouton **Accepter** dans la boîte de dialogue **Acceptation de la licence**.</span><span class="sxs-lookup"><span data-stu-id="1aae5-178">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="1aae5-179">Ajouter un modèle d’entité</span><span class="sxs-lookup"><span data-stu-id="1aae5-179">Add an entity model</span></span>

1. <span data-ttu-id="1aae5-180">Ajoutez un répertoire *Models* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="1aae5-180">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="1aae5-181">Ajoutez une classe `Book` au répertoire *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1aae5-181">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

    <span data-ttu-id="1aae5-182">Dans la classe précédente, la propriété `Id` :</span><span class="sxs-lookup"><span data-stu-id="1aae5-182">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="1aae5-183">Est requise pour mapper l’objet Common Language Runtime (CLR) à la collection MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1aae5-183">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="1aae5-184">Est annotée avec [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) pour désigner cette propriété en tant que clé primaire du document.</span><span class="sxs-lookup"><span data-stu-id="1aae5-184">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="1aae5-185">Est annotée avec [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) pour autoriser le passage du paramètre en tant que type `string` à la place d’une structure [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm).</span><span class="sxs-lookup"><span data-stu-id="1aae5-185">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="1aae5-186">Mongo gère la conversion de `string` en `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="1aae5-186">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="1aae5-187">Les autres propriétés de la classe sont annotées avec l’attribut [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm).</span><span class="sxs-lookup"><span data-stu-id="1aae5-187">Other properties in the class are annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="1aae5-188">La valeur de l’attribut représente le nom de la propriété dans la collection MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1aae5-188">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="1aae5-189">Ajouter un modèle de configuration</span><span class="sxs-lookup"><span data-stu-id="1aae5-189">Add a configuration model</span></span>

1. <span data-ttu-id="1aae5-190">Ajoutez les valeurs de configuration de base de données suivantes à *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="1aae5-190">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="1aae5-191">Ajoutez un fichier *BookstoreDatabaseSettings.cs* au répertoire *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1aae5-191">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="1aae5-192">La classe `BookstoreDatabaseSettings` précédente est utilisée pour stocker les valeurs de propriété `BookstoreDatabaseSettings` du fichier  *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1aae5-192">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="1aae5-193">Les noms de propriétés JSON et C# sont nommés de manière identique pour faciliter le processus de mappage.</span><span class="sxs-lookup"><span data-stu-id="1aae5-193">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="1aae5-194">Ajoutez le code suivant à `Startup.ConfigureServices` avant l’appel à `AddMvc` :</span><span class="sxs-lookup"><span data-stu-id="1aae5-194">Add the following code to `Startup.ConfigureServices`, before the call to `AddMvc`:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureDatabaseSettings)]

    <span data-ttu-id="1aae5-195">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="1aae5-195">In the preceding code:</span></span>

    * <span data-ttu-id="1aae5-196">L’instance de configuration à laquelle la section `BookstoreDatabaseSettings` du fichier *appsettings.json* est liée est inscrite dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="1aae5-196">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="1aae5-197">Par exemple, la propriété `ConnectionString` d’un objet `BookstoreDatabaseSettings` est peuplée avec la propriété `BookstoreDatabaseSettings:ConnectionString` dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1aae5-197">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="1aae5-198">L’interface `IBookstoreDatabaseSettings` est inscrite auprès de l’injection de dépendances avec une [durée de vie de service](xref:fundamentals/dependency-injection#service-lifetimes) de singleton.</span><span class="sxs-lookup"><span data-stu-id="1aae5-198">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="1aae5-199">Une fois injectée, l’instance d’interface est résolue en objet `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="1aae5-199">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="1aae5-200">Ajoutez le code suivant en haut de *Startup.cs* pour résoudre les références à `BookstoreDatabaseSettings` et `IBookstoreDatabaseSettings` :</span><span class="sxs-lookup"><span data-stu-id="1aae5-200">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="1aae5-201">Ajouter un service d’opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="1aae5-201">Add a CRUD operations service</span></span>

1. <span data-ttu-id="1aae5-202">Ajoutez un répertoire *Services* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="1aae5-202">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="1aae5-203">Ajoutez une classe `BookService` au répertoire *Services* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1aae5-203">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="1aae5-204">Dans le code précédent, une instance de `IBookstoreDatabaseSettings` est récupérée à partir de l’injection de dépendances via l’injection de constructeur.</span><span class="sxs-lookup"><span data-stu-id="1aae5-204">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="1aae5-205">Cette technique permet d’accéder aux valeurs de configuration d’*appsettings.json*, qui ont été ajoutées à la section [Ajouter un modèle de configuration](#add-a-configuration-model).</span><span class="sxs-lookup"><span data-stu-id="1aae5-205">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="1aae5-206">Dans `Startup.ConfigureServices`, inscrivez la classe `BookService` auprès de l’injection de dépendances :</span><span class="sxs-lookup"><span data-stu-id="1aae5-206">In `Startup.ConfigureServices`, register the `BookService` class with DI:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=9)]

    <span data-ttu-id="1aae5-207">Dans le code précédent, la classe `BookService` est inscrite auprès de l’injection de dépendances pour permettre la prise en charge de l’injection de constructeur dans les classes consommatrices.</span><span class="sxs-lookup"><span data-stu-id="1aae5-207">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="1aae5-208">La durée de vie de service de singleton est la plus appropriée, car `BookService` a une dépendance directe sur `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="1aae5-208">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="1aae5-209">Selon les [recommandations officielles de réutilisation de Mongo Client](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` doit être inscrit auprès de l’injection de dépendances avec une durée de vie de service de singleton.</span><span class="sxs-lookup"><span data-stu-id="1aae5-209">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="1aae5-210">Ajoutez le code suivant en haut de *Startup.cs* pour résoudre la référence à `BookService` :</span><span class="sxs-lookup"><span data-stu-id="1aae5-210">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="1aae5-211">La classe `BookService` utilise les membres `MongoDB.Driver` suivants pour effectuer des opérations CRUD dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="1aae5-211">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="1aae5-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Lit l’instance de serveur qui permet d’effectuer des opérations de base de données.</span><span class="sxs-lookup"><span data-stu-id="1aae5-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="1aae5-213">Le constructeur de cette classe reçoit la chaîne de connexion MongoDB :</span><span class="sxs-lookup"><span data-stu-id="1aae5-213">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="1aae5-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Représente la base de données Mongo qui permet d’effectuer des opérations.</span><span class="sxs-lookup"><span data-stu-id="1aae5-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="1aae5-215">Ce tutoriel utilise la méthode générique [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) sur l’interface pour accéder aux données d’une collection spécifique.</span><span class="sxs-lookup"><span data-stu-id="1aae5-215">This tutorial uses the generic [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="1aae5-216">Effectuez des opérations CRUD sur la collection, après l’appel de cette méthode.</span><span class="sxs-lookup"><span data-stu-id="1aae5-216">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="1aae5-217">Dans l’appel à la méthode `GetCollection<TDocument>(collection)` :</span><span class="sxs-lookup"><span data-stu-id="1aae5-217">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="1aae5-218">`collection` représente le nom de la collection.</span><span class="sxs-lookup"><span data-stu-id="1aae5-218">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="1aae5-219">`TDocument` représente le type d’objet CLR stocké dans la collection.</span><span class="sxs-lookup"><span data-stu-id="1aae5-219">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="1aae5-220">`GetCollection<TDocument>(collection)` retourne un objet [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) représentant la collection.</span><span class="sxs-lookup"><span data-stu-id="1aae5-220">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="1aae5-221">Dans ce didacticiel, les méthodes suivantes sont appelées sur la collection :</span><span class="sxs-lookup"><span data-stu-id="1aae5-221">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="1aae5-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Supprime un seul document correspondant aux critères de recherche fournis.</span><span class="sxs-lookup"><span data-stu-id="1aae5-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="1aae5-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Retourne tous les documents de la collection correspondant aux critères de recherche fournis.</span><span class="sxs-lookup"><span data-stu-id="1aae5-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="1aae5-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Insère l’objet fourni en tant que nouveau document dans la collection.</span><span class="sxs-lookup"><span data-stu-id="1aae5-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="1aae5-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Remplace le seul document correspondant aux critères de recherche fournis par l’objet indiqué.</span><span class="sxs-lookup"><span data-stu-id="1aae5-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="1aae5-226">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="1aae5-226">Add a controller</span></span>

<span data-ttu-id="1aae5-227">Ajoutez une classe `BooksController` au répertoire *Controllers* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1aae5-227">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="1aae5-228">Le contrôleur d’API web précédent :</span><span class="sxs-lookup"><span data-stu-id="1aae5-228">The preceding web API controller:</span></span>

* <span data-ttu-id="1aae5-229">Utilise la classe `BookService` pour effectuer des opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="1aae5-229">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="1aae5-230">Contient des méthodes d’action pour prendre en charge les requêtes HTTP GET, POST, PUT et DELETE.</span><span class="sxs-lookup"><span data-stu-id="1aae5-230">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="1aae5-231">Appelle <xref:System.Web.Http.ApiController.CreatedAtRoute*> dans la méthode d’action `Create` pour retourner une réponse [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="1aae5-231">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="1aae5-232">Le code d’état 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1aae5-232">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="1aae5-233">`CreatedAtRoute` ajoute également un en-tête `Location` à la réponse.</span><span class="sxs-lookup"><span data-stu-id="1aae5-233">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="1aae5-234">L’en-tête `Location` spécifie l’URI du livre qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="1aae5-234">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="1aae5-235">Tester l’API web</span><span class="sxs-lookup"><span data-stu-id="1aae5-235">Test the web API</span></span>

1. <span data-ttu-id="1aae5-236">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="1aae5-236">Build and run the app.</span></span>

1. <span data-ttu-id="1aae5-237">Accédez à `http://localhost:<port>/api/books` pour tester la méthode d’action `Get` sans paramètre du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1aae5-237">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="1aae5-238">La réponse JSON suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="1aae5-238">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="1aae5-239">Accédez à `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` pour tester la méthode d’action `Get` surchargée du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1aae5-239">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="1aae5-240">La réponse JSON suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="1aae5-240">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="1aae5-241">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="1aae5-241">Next steps</span></span>

<span data-ttu-id="1aae5-242">Pour plus d’informations sur la création d’API web ASP.NET Core, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="1aae5-242">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="1aae5-243">Version YouTube de cet article</span><span class="sxs-lookup"><span data-stu-id="1aae5-243">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
