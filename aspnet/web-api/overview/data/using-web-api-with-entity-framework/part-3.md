---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Utilisez Migrations Code First pour amorcer la base de données | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ca627397f0f100d13388f9afc27ff481886e098
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="1e5d6-102">Utilisez Migrations Code First pour amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="1e5d6-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="1e5d6-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1e5d6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1e5d6-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="1e5d6-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1e5d6-105">Dans cette section, vous allez utiliser [Migrations Code First](https://msdn.microsoft.com/data/jj591621) dans EF pour amorcer la base de données de test.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="1e5d6-106">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1e5d6-107">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1e5d6-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="1e5d6-108">Cette commande ajoute un dossier nommé Migrations à votre projet, ainsi qu’un fichier de code nommé Configuration.cs dans le dossier Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="1e5d6-109">Ouvrez le fichier Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="1e5d6-110">Ajoutez le code suivant **à l’aide de** instruction.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="1e5d6-111">Puis ajoutez le code suivant à la **Configuration.Seed** méthode :</span><span class="sxs-lookup"><span data-stu-id="1e5d6-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="1e5d6-112">Dans la fenêtre de Console du Gestionnaire de Package, tapez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="1e5d6-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="1e5d6-113">La première commande génère du code qui crée la base de données, et la deuxième commande exécute ce code.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="1e5d6-114">La base de données est créé localement, à l’aide de [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e5d6-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="1e5d6-115">Explorer l’API (facultatif)</span><span class="sxs-lookup"><span data-stu-id="1e5d6-115">Explore the API (Optional)</span></span>

<span data-ttu-id="1e5d6-116">Appuyez sur F5 pour exécuter l’application en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="1e5d6-117">Visual Studio démarre IIS Express et s’exécute votre application web.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="1e5d6-118">Ensuite, Visual Studio lance un navigateur et ouvre la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="1e5d6-119">Lorsque Visual Studio est exécuté un projet web, elle affecte un numéro de port.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="1e5d6-120">Dans l’image ci-dessous, le numéro de port est 50524.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="1e5d6-121">Lorsque vous exécutez l’application, vous verrez un numéro de port différent.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="1e5d6-122">La page d’accueil est implémentée à l’aide d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="1e5d6-123">En haut de la page, il existe un lien indiquant que « API ».</span><span class="sxs-lookup"><span data-stu-id="1e5d6-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="1e5d6-124">Ce lien affiche une page d’aide générée automatiquement pour l’API web.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="1e5d6-125">(Pour savoir comment cette page d’aide est générée, et comment vous pouvez ajouter votre propre documentation à la page, consultez [création de Pages aide pour l’API Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Vous pouvez cliquer sur l’aide de liens de la page pour afficher les détails sur l’API, y compris le format de demande et de réponse.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="1e5d6-126">L’API permet les opérations CRUD sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="1e5d6-127">Voici un résumé de l’API.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-127">The following summarizes the API.</span></span>

| <span data-ttu-id="1e5d6-128">Auteurs</span><span class="sxs-lookup"><span data-stu-id="1e5d6-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="1e5d6-129">OBTENIR les api/auteurs</span><span class="sxs-lookup"><span data-stu-id="1e5d6-129">GET api/authors</span></span> | <span data-ttu-id="1e5d6-130">Obtenir tous les auteurs.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-130">Get all authors.</span></span> |
| <span data-ttu-id="1e5d6-131">GET api/auteurs / {id}</span><span class="sxs-lookup"><span data-stu-id="1e5d6-131">GET api/authors/{id}</span></span> | <span data-ttu-id="1e5d6-132">Obtenir un auteur par ID.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-132">Get an author by ID.</span></span> |
| <span data-ttu-id="1e5d6-133">Auteurs/api/POST</span><span class="sxs-lookup"><span data-stu-id="1e5d6-133">POST /api/authors</span></span> | <span data-ttu-id="1e5d6-134">Créer un nouvel auteur.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-134">Create a new author.</span></span> |
| <span data-ttu-id="1e5d6-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="1e5d6-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="1e5d6-136">Mettre à jour un auteur existant.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-136">Update an existing author.</span></span> |
| <span data-ttu-id="1e5d6-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="1e5d6-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="1e5d6-138">Supprimer un auteur.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-138">Delete an author.</span></span> |

| <span data-ttu-id="1e5d6-139">Documentation</span><span class="sxs-lookup"><span data-stu-id="1e5d6-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="1e5d6-140">OBTENIR /api/books</span><span class="sxs-lookup"><span data-stu-id="1e5d6-140">GET /api/books</span></span> | <span data-ttu-id="1e5d6-141">Obtenir tous les livres.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-141">Get all books.</span></span> |
| <span data-ttu-id="1e5d6-142">OBTENIR/API/books / {id}</span><span class="sxs-lookup"><span data-stu-id="1e5d6-142">GET /api/books/{id}</span></span> | <span data-ttu-id="1e5d6-143">Obtenir un livre par ID.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-143">Get a book by ID.</span></span> |
| <span data-ttu-id="1e5d6-144">VALIDER la documentation/api /</span><span class="sxs-lookup"><span data-stu-id="1e5d6-144">POST /api/books</span></span> | <span data-ttu-id="1e5d6-145">Créer un nouveau.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-145">Create a new book.</span></span> |
| <span data-ttu-id="1e5d6-146">PUT/API/books / {id}</span><span class="sxs-lookup"><span data-stu-id="1e5d6-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="1e5d6-147">Mettre à jour un livre existant.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-147">Update an existing book.</span></span> |
| <span data-ttu-id="1e5d6-148">Supprimer/API/books / {id}</span><span class="sxs-lookup"><span data-stu-id="1e5d6-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="1e5d6-149">Supprimer un livre.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="1e5d6-150">Afficher la base de données (facultatif)</span><span class="sxs-lookup"><span data-stu-id="1e5d6-150">View the Database (Optional)</span></span>

<span data-ttu-id="1e5d6-151">Lorsque vous avez exécuté la commande Update-Database, EF créé la base de données et appelé le `Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1e5d6-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="1e5d6-152">Lorsque vous exécutez l’application localement, EF utilise [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e5d6-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="1e5d6-153">Vous pouvez afficher la base de données dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="1e5d6-154">À partir de la **vue** menu, sélectionnez **l’Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="1e5d6-155">Dans le **se connecter au serveur** boîte de dialogue, dans le **nom du serveur** zone d’édition, tapez « (localdb) \v11.0 ».</span><span class="sxs-lookup"><span data-stu-id="1e5d6-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="1e5d6-156">Laissez le **authentification** option « Authentification Windows ».</span><span class="sxs-lookup"><span data-stu-id="1e5d6-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="1e5d6-157">Cliquez sur **Connexion**.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="1e5d6-158">Visual Studio se connecte à la base de données locale et affiche les bases de données existantes dans la fenêtre Explorateur d’objets SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="1e5d6-159">Vous pouvez développer les nœuds pour afficher les tables EF créé.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="1e5d6-160">Pour afficher les données, cliquez sur une table, puis sélectionnez **des données d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="1e5d6-161">La capture d’écran suivante montre les résultats de la table de la documentation.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="1e5d6-162">Notez que EF rempli avec les données de valeur initiale, la base de données, et la table contient la clé étrangère à la table Authors.</span><span class="sxs-lookup"><span data-stu-id="1e5d6-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
<span data-ttu-id="1e5d6-163">[Précédent](part-2.md)
[Suivant](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="1e5d6-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
