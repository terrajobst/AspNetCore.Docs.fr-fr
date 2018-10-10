---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Créer une API REST avec le routage par attributs dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 3b66f02c9be6441ff7d6c38d6180cc8556c6b8b9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912889"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="1c298-102">Créer une API REST avec routage d’attributs dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1c298-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1c298-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1c298-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1c298-104">Web API 2 prend en charge un nouveau type de routage, appelé *routage par attributs*.</span><span class="sxs-lookup"><span data-stu-id="1c298-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="1c298-105">Pour obtenir une vue d’ensemble du routage par attributs, consultez [routage par attributs dans Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="1c298-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="1c298-106">Dans ce didacticiel, vous allez utiliser le routage par attributs pour créer une API REST pour un ensemble de livres.</span><span class="sxs-lookup"><span data-stu-id="1c298-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="1c298-107">L’API prendra en charge les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="1c298-107">The API will support the following actions:</span></span>

| <span data-ttu-id="1c298-108">Action</span><span class="sxs-lookup"><span data-stu-id="1c298-108">Action</span></span> | <span data-ttu-id="1c298-109">Exemple d’URI</span><span class="sxs-lookup"><span data-stu-id="1c298-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="1c298-110">Obtenir une liste de tous les livres.</span><span class="sxs-lookup"><span data-stu-id="1c298-110">Get a list of all books.</span></span> | <span data-ttu-id="1c298-111">documentation/api /</span><span class="sxs-lookup"><span data-stu-id="1c298-111">/api/books</span></span> |
| <span data-ttu-id="1c298-112">Obtenir un livre par ID.</span><span class="sxs-lookup"><span data-stu-id="1c298-112">Get a book by ID.</span></span> | <span data-ttu-id="1c298-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="1c298-113">/api/books/1</span></span> |
| <span data-ttu-id="1c298-114">Obtenir les détails d’un livre.</span><span class="sxs-lookup"><span data-stu-id="1c298-114">Get the details of a book.</span></span> | <span data-ttu-id="1c298-115">/API/Books/1/Details</span><span class="sxs-lookup"><span data-stu-id="1c298-115">/api/books/1/details</span></span> |
| <span data-ttu-id="1c298-116">Obtenir une liste de livres par genre.</span><span class="sxs-lookup"><span data-stu-id="1c298-116">Get a list of books by genre.</span></span> | <span data-ttu-id="1c298-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="1c298-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="1c298-118">Obtenir une liste de livres par date de publication.</span><span class="sxs-lookup"><span data-stu-id="1c298-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="1c298-119">/API/Books/date/2013-02-16 /api/books/date/2013/02/16 (autre forme)</span><span class="sxs-lookup"><span data-stu-id="1c298-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="1c298-120">Obtenir une liste de livres publiés par un auteur spécifique.</span><span class="sxs-lookup"><span data-stu-id="1c298-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="1c298-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="1c298-121">/api/authors/1/books</span></span> |

<span data-ttu-id="1c298-122">Toutes les méthodes sont en lecture seule (requêtes HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="1c298-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="1c298-123">Pour la couche données, nous allons utiliser Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c298-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="1c298-124">Enregistrements de livre aura les champs suivants :</span><span class="sxs-lookup"><span data-stu-id="1c298-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="1c298-125">Id</span><span class="sxs-lookup"><span data-stu-id="1c298-125">ID</span></span>
- <span data-ttu-id="1c298-126">Titre</span><span class="sxs-lookup"><span data-stu-id="1c298-126">Title</span></span>
- <span data-ttu-id="1c298-127">Genre</span><span class="sxs-lookup"><span data-stu-id="1c298-127">Genre</span></span>
- <span data-ttu-id="1c298-128">Date de publication</span><span class="sxs-lookup"><span data-stu-id="1c298-128">Publication date</span></span>
- <span data-ttu-id="1c298-129">Prix</span><span class="sxs-lookup"><span data-stu-id="1c298-129">Price</span></span>
- <span data-ttu-id="1c298-130">Description</span><span class="sxs-lookup"><span data-stu-id="1c298-130">Description</span></span>
- <span data-ttu-id="1c298-131">AuthorID (clé étrangère à une table Authors)</span><span class="sxs-lookup"><span data-stu-id="1c298-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="1c298-132">Toutefois, pour la plupart des requêtes, l’API retourne un sous-ensemble de ces données (titre, auteur et genre).</span><span class="sxs-lookup"><span data-stu-id="1c298-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="1c298-133">Pour obtenir des demandes de l’enregistrement complet, le client `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="1c298-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c298-134">Prérequis</span><span class="sxs-lookup"><span data-stu-id="1c298-134">Prerequisites</span></span>

<span data-ttu-id="1c298-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="1c298-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1c298-136">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c298-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="1c298-137">Commencez par exécuter Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c298-137">Start by running Visual Studio.</span></span> <span data-ttu-id="1c298-138">À partir de la **fichier** menu, sélectionnez **New** , puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="1c298-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="1c298-139">Développez le **installé** > **Visual C#** catégorie.</span><span class="sxs-lookup"><span data-stu-id="1c298-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="1c298-140">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="1c298-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1c298-141">Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="1c298-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1c298-142">Nommez le projet &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c298-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="1c298-143">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle.</span><span class="sxs-lookup"><span data-stu-id="1c298-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="1c298-144">Sous « Ajouter des dossiers et les références principales pour », sélectionnez le **API Web** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="1c298-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="1c298-145">Cliquez sur **créer le projet**.</span><span class="sxs-lookup"><span data-stu-id="1c298-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="1c298-146">Cette opération crée une structure de projet qui est configuré pour la fonctionnalité de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="1c298-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="1c298-147">Modèles de domaine</span><span class="sxs-lookup"><span data-stu-id="1c298-147">Domain Models</span></span>

<span data-ttu-id="1c298-148">Ensuite, ajoutez des classes pour les modèles de domaine.</span><span class="sxs-lookup"><span data-stu-id="1c298-148">Next, add classes for domain models.</span></span> <span data-ttu-id="1c298-149">Dans l’Explorateur de solutions, cliquez sur le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="1c298-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="1c298-150">Sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="1c298-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="1c298-151">Nommez la classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="1c298-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="1c298-152">Remplacez le code dans Author.cs avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="1c298-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="1c298-153">Ajoutez maintenant une autre classe nommée `Book`.</span><span class="sxs-lookup"><span data-stu-id="1c298-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="1c298-154">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="1c298-154">Add a Web API Controller</span></span>

<span data-ttu-id="1c298-155">Dans cette étape, nous allons ajouter un contrôleur d’API Web qui utilise Entity Framework en tant que la couche données.</span><span class="sxs-lookup"><span data-stu-id="1c298-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="1c298-156">Appuyez sur CTRL+MAJ+B pour générer le projet.</span><span class="sxs-lookup"><span data-stu-id="1c298-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="1c298-157">Entity Framework utilise la réflexion pour découvrir les propriétés des modèles, de sorte qu’elle nécessite un assembly compilé créer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="1c298-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="1c298-158">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="1c298-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1c298-159">Sélectionnez **ajouter**, puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="1c298-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="1c298-160">Dans le **ajouter une structure** boîte de dialogue, sélectionnez « Web API 2 contrôleur avec actions de lecture/écriture, à l’aide d’Entity Framework. »</span><span class="sxs-lookup"><span data-stu-id="1c298-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="1c298-161">Dans le **ajouter un contrôleur** boîte de dialogue, pour **nom du contrôleur**, entrez &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c298-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="1c298-162">Sélectionnez le &quot;utiliser les actions de contrôleur asynchrones&quot; case à cocher.</span><span class="sxs-lookup"><span data-stu-id="1c298-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="1c298-163">Pour **classe de modèle**, sélectionnez &quot;livre&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c298-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="1c298-164">(Si vous ne voyez pas la `Book` classe répertorié dans la liste déroulante, assurez-vous que vous avez créé le projet.) Cliquez ensuite sur le bouton « + ».</span><span class="sxs-lookup"><span data-stu-id="1c298-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="1c298-165">Cliquez sur **ajouter** dans le **nouveau contexte de données** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="1c298-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="1c298-166">Cliquez sur **ajouter** dans le **ajouter un contrôleur** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="1c298-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="1c298-167">La génération de modèles automatique ajoute une classe nommée `BooksController` qui définit le contrôleur d’API.</span><span class="sxs-lookup"><span data-stu-id="1c298-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="1c298-168">Il ajoute également une classe nommée `BooksAPIContext` dans le dossier Modèles, qui définit le contexte de données pour Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c298-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="1c298-169">Valeur initiale de la base de données</span><span class="sxs-lookup"><span data-stu-id="1c298-169">Seed the Database</span></span>

<span data-ttu-id="1c298-170">Dans le menu Outils, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="1c298-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="1c298-171">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1c298-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="1c298-172">Cette commande crée un dossier Migrations et ajoute un nouveau fichier de code nommé Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="1c298-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="1c298-173">Ouvrez ce fichier et ajoutez le code suivant à la `Configuration.Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1c298-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="1c298-174">Dans la fenêtre de Console du Gestionnaire de Package, tapez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="1c298-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="1c298-175">Ces commandes créent une base de données locale et appeler la méthode Seed pour remplir la base de données.</span><span class="sxs-lookup"><span data-stu-id="1c298-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="1c298-176">Ajouter des Classes DTO</span><span class="sxs-lookup"><span data-stu-id="1c298-176">Add DTO Classes</span></span>

<span data-ttu-id="1c298-177">Si vous exécutez maintenant l’application et envoyez une demande GET à /api/books/1, la réponse ressemble à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="1c298-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="1c298-178">(J’ai ajouté la mise en retrait pour une meilleure lisibilité).</span><span class="sxs-lookup"><span data-stu-id="1c298-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="1c298-179">Au lieu de cela, je veux cette requête pour retourner un sous-ensemble des champs.</span><span class="sxs-lookup"><span data-stu-id="1c298-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="1c298-180">En outre, je veux qu’il retourne le nom de l’auteur, plutôt que l’ID de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="1c298-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="1c298-181">Pour ce faire, nous allons modifier les méthodes de contrôleur pour retourner un *objet de transfert de données* (DTO) au lieu du modèle Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c298-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="1c298-182">Un objet DTO est un objet qui est conçu uniquement pour transporter des données.</span><span class="sxs-lookup"><span data-stu-id="1c298-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="1c298-183">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** | **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="1c298-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="1c298-184">Nommez le dossier &quot;DTO&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c298-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="1c298-185">Ajoutez une classe nommée `BookDto` dans le dossier DTO, avec la définition suivante :</span><span class="sxs-lookup"><span data-stu-id="1c298-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="1c298-186">Ajouter une autre classe nommée `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="1c298-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="1c298-187">Ensuite, mettez à jour le `BooksController` classe pour retourner `BookDto` instances.</span><span class="sxs-lookup"><span data-stu-id="1c298-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="1c298-188">Nous allons utiliser le [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) méthode au projet `Book` instances à `BookDto` instances.</span><span class="sxs-lookup"><span data-stu-id="1c298-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="1c298-189">Voici le code mis à jour pour la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1c298-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="1c298-190">J’ai supprimé le `PutBook`, `PostBook`, et `DeleteBook` méthodes, car ils ne sont pas nécessaires pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="1c298-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="1c298-191">Maintenant si vous exécutez l’application et que vous demandez /api/books/1, le corps de réponse doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="1c298-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="1c298-192">Ajouter des attributs de Route</span><span class="sxs-lookup"><span data-stu-id="1c298-192">Add Route Attributes</span></span>

<span data-ttu-id="1c298-193">Ensuite, nous allons convertir le contrôleur pour utiliser le routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="1c298-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="1c298-194">Tout d’abord, ajoutez un **RoutePrefix** attribut au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1c298-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="1c298-195">Cet attribut définit les segments d’URI initiales pour toutes les méthodes sur ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1c298-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="1c298-196">Ajoutez ensuite **[Route]** des attributs pour les actions de contrôleur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="1c298-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="1c298-197">Le modèle d’itinéraire pour chaque méthode de contrôleur est le préfixe ainsi que la chaîne spécifiée dans le **itinéraire** attribut.</span><span class="sxs-lookup"><span data-stu-id="1c298-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="1c298-198">Pour le `GetBook` (méthode), le modèle d’itinéraire inclut la chaîne paramétrable &quot;{id : int}&quot;, qui met en correspondance si le segment d’URI contient une valeur entière.</span><span class="sxs-lookup"><span data-stu-id="1c298-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="1c298-199">Méthode</span><span class="sxs-lookup"><span data-stu-id="1c298-199">Method</span></span> | <span data-ttu-id="1c298-200">Modèle de routage</span><span class="sxs-lookup"><span data-stu-id="1c298-200">Route Template</span></span> | <span data-ttu-id="1c298-201">Exemple d’URI</span><span class="sxs-lookup"><span data-stu-id="1c298-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="1c298-202">« api/books »</span><span class="sxs-lookup"><span data-stu-id="1c298-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="1c298-203">« api/la documentation / {id : int} »</span><span class="sxs-lookup"><span data-stu-id="1c298-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="1c298-204">Obtenir les détails des livres</span><span class="sxs-lookup"><span data-stu-id="1c298-204">Get Book Details</span></span>

<span data-ttu-id="1c298-205">Pour obtenir les détails des livres, le client envoie une demande GET à `/api/books/{id}/details`, où *{id}* est l’ID du livre.</span><span class="sxs-lookup"><span data-stu-id="1c298-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="1c298-206">Ajoutez la méthode suivante à la classe `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="1c298-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="1c298-207">Si vous demandez `/api/books/1/details`, la réponse ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1c298-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="1c298-208">Obtenir la documentation par Genre</span><span class="sxs-lookup"><span data-stu-id="1c298-208">Get Books By Genre</span></span>

<span data-ttu-id="1c298-209">Pour obtenir une liste de livres d’un genre spécifique, le client envoie une demande GET à `/api/books/genre`, où *genre* est le nom du genre.</span><span class="sxs-lookup"><span data-stu-id="1c298-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="1c298-210">(Par exemple, `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="1c298-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="1c298-211">Ajoutez la méthode suivante à `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1c298-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="1c298-212">Ici nous allons la définir un itinéraire qui contient un paramètre de {genre} dans le modèle d’URI.</span><span class="sxs-lookup"><span data-stu-id="1c298-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="1c298-213">Notez que les API Web est en mesure de distinguer ces deux URI et de les acheminer vers les différentes méthodes :</span><span class="sxs-lookup"><span data-stu-id="1c298-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="1c298-214">C’est parce que le `GetBook` méthode inclut une contrainte que le segment « id » doit être une valeur entière :</span><span class="sxs-lookup"><span data-stu-id="1c298-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="1c298-215">Si vous demandez /api/books/fantasy, la réponse ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1c298-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="1c298-216">Obtenir la documentation par auteur</span><span class="sxs-lookup"><span data-stu-id="1c298-216">Get Books By Author</span></span>

<span data-ttu-id="1c298-217">Pour obtenir une liste d’une documentation pour un auteur particulier, le client envoie une demande GET à `/api/authors/id/books`, où *id* est l’ID de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="1c298-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="1c298-218">Ajoutez la méthode suivante à `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1c298-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="1c298-219">Cet exemple est intéressant car &quot;la documentation&quot; est traité une ressource enfant de &quot;auteurs&quot;.</span><span class="sxs-lookup"><span data-stu-id="1c298-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="1c298-220">Ce modèle est assez courant dans les API RESTful.</span><span class="sxs-lookup"><span data-stu-id="1c298-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="1c298-221">Le tilde (~) dans le modèle d’itinéraire remplace le préfixe d’itinéraire dans le **RoutePrefix** attribut.</span><span class="sxs-lookup"><span data-stu-id="1c298-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="1c298-222">Obtenir la documentation par Date de Publication</span><span class="sxs-lookup"><span data-stu-id="1c298-222">Get Books By Publication Date</span></span>

<span data-ttu-id="1c298-223">Pour obtenir une liste de livres par date de publication, le client envoie une demande GET à `/api/books/date/yyyy-mm-dd`, où *aaaa-mm-jj* correspond à la date.</span><span class="sxs-lookup"><span data-stu-id="1c298-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="1c298-224">Voici une façon de procéder :</span><span class="sxs-lookup"><span data-stu-id="1c298-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="1c298-225">Le `{pubdate:datetime}` paramètre est contraint pour faire correspondre un **DateTime** valeur.</span><span class="sxs-lookup"><span data-stu-id="1c298-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="1c298-226">Cela fonctionne, mais elle est en fait plus permissive que nous voudrions.</span><span class="sxs-lookup"><span data-stu-id="1c298-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="1c298-227">Par exemple, ces URI correspondent également l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="1c298-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="1c298-228">Il n’existe pas de mal à autoriser ces URI.</span><span class="sxs-lookup"><span data-stu-id="1c298-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="1c298-229">Toutefois, vous pouvez restreindre l’itinéraire dans un format particulier en ajoutant une contrainte d’expression régulière pour le modèle de routage :</span><span class="sxs-lookup"><span data-stu-id="1c298-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="1c298-230">Désormais uniquement les dates sous la forme &quot;aaaa-mm-jj&quot; correspondra.</span><span class="sxs-lookup"><span data-stu-id="1c298-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="1c298-231">Notez que nous n’utilisons l’expression régulière pour valider que nous avons une date réelle.</span><span class="sxs-lookup"><span data-stu-id="1c298-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="1c298-232">Qui est gérée lors de l’API Web tente de convertir le segment d’URI dans un **DateTime** instance.</span><span class="sxs-lookup"><span data-stu-id="1c298-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="1c298-233">Une date non valide comme « 2012-47-99 » ne pourra pas être converti, et le client reçoit une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="1c298-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="1c298-234">Vous pouvez prennent également en charge un séparateur de barre oblique (`/api/books/date/yyyy/mm/dd`) en ajoutant un autre **[Route]** attribut avec un autre regex.</span><span class="sxs-lookup"><span data-stu-id="1c298-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="1c298-235">Il existe un détail subtil mais important ici.</span><span class="sxs-lookup"><span data-stu-id="1c298-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="1c298-236">Le second modèle d’itinéraire comporte un caractère générique (\*) au début du paramètre {pubdate} :</span><span class="sxs-lookup"><span data-stu-id="1c298-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="1c298-237">Cela indique au moteur de routage que pubdate {} doit correspondre à celui de l’URI.</span><span class="sxs-lookup"><span data-stu-id="1c298-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="1c298-238">Par défaut, un paramètre de modèle correspond à un seul segment d’URI.</span><span class="sxs-lookup"><span data-stu-id="1c298-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="1c298-239">Dans ce cas, nous souhaitons {pubdate} s’étendent sur plusieurs segments d’URI :</span><span class="sxs-lookup"><span data-stu-id="1c298-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="1c298-240">Code du contrôleur</span><span class="sxs-lookup"><span data-stu-id="1c298-240">Controller Code</span></span>

<span data-ttu-id="1c298-241">Voici le code complet pour la classe BooksController.</span><span class="sxs-lookup"><span data-stu-id="1c298-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="1c298-242">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="1c298-242">Summary</span></span>

<span data-ttu-id="1c298-243">Routage par attributs vous donne davantage de contrôle et une plus grande flexibilité lors de la conception de l’URI pour votre API.</span><span class="sxs-lookup"><span data-stu-id="1c298-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
