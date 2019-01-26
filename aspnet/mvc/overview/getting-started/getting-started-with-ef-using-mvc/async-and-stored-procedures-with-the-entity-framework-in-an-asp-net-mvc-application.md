---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutoriel : Utiliser async et les procédures stockées avec Entity Framework dans une application ASP.NET MVC'
description: Dans ce didacticiel, vous allez apprendre à implémenter le modèle de programmation asynchrone et découvrez comment utiliser des procédures stockées.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9041167af076d80ebf294e054ffe51293d11e888
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889871"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="34bd6-103">Tutoriel : Utiliser async et les procédures stockées avec Entity Framework dans une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="34bd6-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="34bd6-104">Dans les didacticiels précédents, vous avez appris comment lire et mettre à jour des données à l’aide du modèle de programmation synchrone.</span><span class="sxs-lookup"><span data-stu-id="34bd6-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="34bd6-105">Dans ce didacticiel, vous allez apprendre à implémenter le modèle de programmation asynchrone.</span><span class="sxs-lookup"><span data-stu-id="34bd6-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="34bd6-106">Code asynchrone peut vous aider à une application plus performantes, car il fait un meilleur usage des ressources du serveur.</span><span class="sxs-lookup"><span data-stu-id="34bd6-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="34bd6-107">Dans ce didacticiel, vous allez également apprendre à utiliser des procédures stockées pour insert, update et les opérations de suppression sur une entité.</span><span class="sxs-lookup"><span data-stu-id="34bd6-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="34bd6-108">Enfin, vous redéployez l’application dans Azure, ainsi que toutes les modifications de base de données que vous avez implémentées depuis la première fois que vous avez déployé.</span><span class="sxs-lookup"><span data-stu-id="34bd6-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="34bd6-109">Les illustrations suivantes montrent quelques-unes des pages que vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="34bd6-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Page départements](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Créer le service](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="34bd6-112">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="34bd6-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="34bd6-113">En savoir plus sur le code asynchrone</span><span class="sxs-lookup"><span data-stu-id="34bd6-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="34bd6-114">Créer un contrôleur de service</span><span class="sxs-lookup"><span data-stu-id="34bd6-114">Create a Department controller</span></span>
> * <span data-ttu-id="34bd6-115">Utiliser des procédures stockées</span><span class="sxs-lookup"><span data-stu-id="34bd6-115">Use stored procedures</span></span>
> * <span data-ttu-id="34bd6-116">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="34bd6-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34bd6-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="34bd6-117">Prerequisites</span></span>

* [<span data-ttu-id="34bd6-118">Mise à jour de données associées</span><span class="sxs-lookup"><span data-stu-id="34bd6-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="34bd6-119">Pourquoi utiliser le code asynchrone</span><span class="sxs-lookup"><span data-stu-id="34bd6-119">Why use asynchronous code</span></span>

<span data-ttu-id="34bd6-120">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="34bd6-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="34bd6-121">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="34bd6-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="34bd6-122">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="34bd6-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="34bd6-123">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="34bd6-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="34bd6-124">Par conséquent, le code asynchrone permet à des ressources serveur à utiliser plus efficacement, et le serveur est activé pour traiter plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="34bd6-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="34bd6-125">Dans les versions antérieures de .NET, écrire et tester le code asynchrone a été complexe, sujettes aux erreurs et difficile à déboguer.</span><span class="sxs-lookup"><span data-stu-id="34bd6-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="34bd6-126">Dans .NET 4.5, écriture, test et débogage du code asynchrone sont tellement plus facile que vous devez généralement écrire du code asynchrone, sauf si vous avez une raison de ne pas le faire.</span><span class="sxs-lookup"><span data-stu-id="34bd6-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="34bd6-127">Le code asynchrone introduit néanmoins une petite quantité de surcharge, mais pour les situations de faible trafic la baisse de performances est négligeable, tout en cas de trafic élevé, l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="34bd6-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="34bd6-128">Pour plus d’informations sur la programmation asynchrone, consultez [prise en charge d’utiliser .NET 4.5 asynchrone pour éviter de bloquer les appels](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="34bd6-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="34bd6-129">Créer le contrôleur de service</span><span class="sxs-lookup"><span data-stu-id="34bd6-129">Create Department controller</span></span>

<span data-ttu-id="34bd6-130">Créer un contrôleur de département Sélectionnez de la même façon que vous l’avez fait les contrôleurs antérieures, mais cette fois le **utiliser les actions de contrôleur asynchrones** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="34bd6-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="34bd6-131">Les points importants suivants montrent ce qui a été ajouté au code synchrone pour le `Index` méthode pour la rendre asynchrone :</span><span class="sxs-lookup"><span data-stu-id="34bd6-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="34bd6-132">Quatre modifications ont été appliquées pour activer la requête de base de données Entity Framework exécuter de façon asynchrone :</span><span class="sxs-lookup"><span data-stu-id="34bd6-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="34bd6-133">La méthode est marquée avec le `async` mot clé, qui indique au compilateur de générer des rappels pour les parties du corps de méthode et pour créer automatiquement le `Task<ActionResult>` objet qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="34bd6-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="34bd6-134">Le type de retour a été remplacé par `ActionResult` à `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="34bd6-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="34bd6-135">Le `Task<T>` type représente le travail en cours avec un résultat de type `T`.</span><span class="sxs-lookup"><span data-stu-id="34bd6-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="34bd6-136">Le `await` mot clé a été appliqué à l’appel de service web.</span><span class="sxs-lookup"><span data-stu-id="34bd6-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="34bd6-137">Lorsque le compilateur voit ce mot clé, il divise les coulisses la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="34bd6-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="34bd6-138">La première partie se termine par l’opération qui est lancée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="34bd6-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="34bd6-139">La deuxième partie est placée dans une méthode de rappel est appelée lorsque l’opération terminée.</span><span class="sxs-lookup"><span data-stu-id="34bd6-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="34bd6-140">La version asynchrone de la `ToList` méthode d’extension a été appelée.</span><span class="sxs-lookup"><span data-stu-id="34bd6-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="34bd6-141">Pourquoi est la `departments.ToList` instruction a été modifiée, mais pas la `departments = db.Departments` instruction ?</span><span class="sxs-lookup"><span data-stu-id="34bd6-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="34bd6-142">La raison est que seules les instructions qui provoquent des requêtes ou des commandes à envoyer à la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="34bd6-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="34bd6-143">Le `departments = db.Departments` instruction définit une requête, mais la requête n’est pas exécutée avant la `ToList` méthode est appelée.</span><span class="sxs-lookup"><span data-stu-id="34bd6-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="34bd6-144">Par conséquent, seuls les `ToList` méthode est exécutée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="34bd6-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="34bd6-145">Dans le `Details` (méthode) et le `HttpGet` `Edit` et `Delete` méthodes, le `Find` méthode est celle qui provoque une requête à envoyer à la base de données, c’est la méthode qui est exécutée de façon asynchrone :</span><span class="sxs-lookup"><span data-stu-id="34bd6-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="34bd6-146">Dans le `Create`, `HttpPost Edit`, et `DeleteConfirmed` méthodes, il est le `SaveChanges` appel de méthode qui entraîne une commande à exécuter, pas les instructions telles que `db.Departments.Add(department)` qui seulement provoquer des entités en mémoire à modifier.</span><span class="sxs-lookup"><span data-stu-id="34bd6-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="34bd6-147">Ouvrez *Views\Department\Index.cshtml*et remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="34bd6-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="34bd6-148">Ce code modifie le titre de l’Index aux départements, déplace le nom de l’administrateur vers la droite et fournit le nom complet de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="34bd6-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="34bd6-149">Dans la création, suppression, détails et modifier des vues, changez la légende pour le `InstructorID` champ « Administrateur » de la même façon que vous avez modifié le champ de nom de service pour « Department » dans les vues des cours.</span><span class="sxs-lookup"><span data-stu-id="34bd6-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="34bd6-150">Dans le créer et modifier les vues utilisent le code suivant :</span><span class="sxs-lookup"><span data-stu-id="34bd6-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="34bd6-151">Dans les vues Delete et Details, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="34bd6-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="34bd6-152">Exécutez l’application, puis cliquez sur le **départements** onglet.</span><span class="sxs-lookup"><span data-stu-id="34bd6-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="34bd6-153">Tout fonctionne comme dans les autres contrôleurs, mais dans ce contrôleur de toutes les requêtes SQL s’exécutent en mode asynchrone.</span><span class="sxs-lookup"><span data-stu-id="34bd6-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="34bd6-154">Voici quelques éléments à connaître lorsque vous utilisez la programmation asynchrone avec Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="34bd6-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="34bd6-155">Le code asynchrone n’est pas thread-safe.</span><span class="sxs-lookup"><span data-stu-id="34bd6-155">The async code is not thread safe.</span></span> <span data-ttu-id="34bd6-156">En d’autres termes, en d’autres termes, n’essayez pas d’effectuer plusieurs opérations en parallèle en utilisant la même instance de contexte.</span><span class="sxs-lookup"><span data-stu-id="34bd6-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="34bd6-157">Si vous souhaitez tirer profit des meilleures performances du code asynchrone, assurez-vous que tous les packages de bibliothèque que vous utilisez (par exemple pour changer de page) utilisent également du code asynchrone s’ils appellent des méthodes Entity Framework qui provoquent l’envoi des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="34bd6-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="34bd6-158">Utiliser des procédures stockées</span><span class="sxs-lookup"><span data-stu-id="34bd6-158">Use stored procedures</span></span>

<span data-ttu-id="34bd6-159">Certains développeurs et les administrateurs préfèrent utiliser des procédures stockées pour l’accès de base de données.</span><span class="sxs-lookup"><span data-stu-id="34bd6-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="34bd6-160">Dans les versions précédentes d’Entity Framework vous pouvez récupérer des données à l’aide d’une procédure stockée par [l’exécution d’une requête SQL brute](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), mais vous ne pouvez pas demander à EF d’utiliser des procédures stockées pour les opérations de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="34bd6-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="34bd6-161">Dans EF 6, il est facile à configurer Code First pour utiliser des procédures stockées.</span><span class="sxs-lookup"><span data-stu-id="34bd6-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="34bd6-162">Dans *DAL\SchoolContext.cs*, ajoutez le code en surbrillance à la `OnModelCreating` (méthode).</span><span class="sxs-lookup"><span data-stu-id="34bd6-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="34bd6-163">Ce code indique à Entity Framework aux procédures d’utilisation stockée pour insérer, mettre à jour et supprimer des opérations sur le `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="34bd6-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="34bd6-164">Dans la Console de gestion de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="34bd6-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="34bd6-165">Ouvrez *Migrations\&lt ; horodatage&gt;\_DepartmentSP.cs* pour visualiser le code dans le `Up` méthode créé par Insert, Update et Delete de procédures stockées :</span><span class="sxs-lookup"><span data-stu-id="34bd6-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="34bd6-166">Dans la Console de gestion de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="34bd6-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="34bd6-167">Exécutez l’application en mode débogage, cliquez sur le **départements** onglet, puis cliquez sur **créer un nouveau**.</span><span class="sxs-lookup"><span data-stu-id="34bd6-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="34bd6-168">Entrer des données pour un nouveau service, puis cliquez sur **créer**.</span><span class="sxs-lookup"><span data-stu-id="34bd6-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="34bd6-169">Dans Visual Studio, consultez les journaux dans le **sortie** fenêtre pour voir qu’une procédure stockée a été utilisée pour insérer la nouvelle ligne de service.</span><span class="sxs-lookup"><span data-stu-id="34bd6-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Département Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="34bd6-171">Code crée d’abord les noms de procédures stockée de par défaut.</span><span class="sxs-lookup"><span data-stu-id="34bd6-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="34bd6-172">Si vous utilisez une base de données existante, vous devrez peut-être personnaliser les noms des procédures stockées afin d’utiliser des procédures stockées prédéfinies dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="34bd6-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="34bd6-173">Pour savoir comment procéder, consultez [Entity Framework Code première Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="34bd6-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="34bd6-174">Si vous souhaitez personnaliser ce qui généré do de procédures stockées, vous pouvez modifier le code généré automatiquement pour les migrations `Up` méthode qui crée la procédure stockée.</span><span class="sxs-lookup"><span data-stu-id="34bd6-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="34bd6-175">De cette façon vos modifications sont répercutées lorsque que la migration est exécutée et est appliquée à votre base de données de production lors de la migration s’exécute automatiquement en production après le déploiement.</span><span class="sxs-lookup"><span data-stu-id="34bd6-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="34bd6-176">Si vous souhaitez modifier une procédure stockée existante qui a été créée dans une migration précédente, vous pouvez utiliser la commande Add-Migration pour générer une migration vide et ensuite manuellement écrire du code qui appelle le [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (méthode) .</span><span class="sxs-lookup"><span data-stu-id="34bd6-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="34bd6-177">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="34bd6-177">Deploy to Azure</span></span>

<span data-ttu-id="34bd6-178">Cette section vous oblige à terminées facultatif **déploiement de l’application vers Azure** section dans le [Migrations et déploiement](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel de cette série.</span><span class="sxs-lookup"><span data-stu-id="34bd6-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="34bd6-179">Si vous aviez des erreurs de migrations que vous avez résolu en supprimant la base de données dans votre projet local, ignorez cette section.</span><span class="sxs-lookup"><span data-stu-id="34bd6-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="34bd6-180">Dans Visual Studio, cliquez sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="34bd6-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="34bd6-181">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="34bd6-181">Click **Publish**.</span></span>

    <span data-ttu-id="34bd6-182">Visual Studio déploie l’application dans Azure, et l’application s’ouvre dans votre navigateur par défaut, s’exécutant dans Azure.</span><span class="sxs-lookup"><span data-stu-id="34bd6-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="34bd6-183">Tester l’application afin de vérifier qu’il fonctionne.</span><span class="sxs-lookup"><span data-stu-id="34bd6-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="34bd6-184">La première fois que vous exécutez une page qui accède à la base de données, Entity Framework s’exécute toutes les migrations `Up` méthodes requis pour mettre à jour avec le modèle de données actuel de la base de données.</span><span class="sxs-lookup"><span data-stu-id="34bd6-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="34bd6-185">Vous pouvez désormais utiliser toutes les pages web que vous avez ajouté depuis la dernière fois que vous avez déployé, y compris les pages de service que vous avez ajouté dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="34bd6-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="34bd6-186">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="34bd6-186">Get the code</span></span>

[<span data-ttu-id="34bd6-187">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="34bd6-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="34bd6-188">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="34bd6-188">Additional resources</span></span>

<span data-ttu-id="34bd6-189">Vous trouverez des liens vers d’autres ressources Entity Framework dans le [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="34bd6-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="34bd6-190">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="34bd6-190">Next steps</span></span>

<span data-ttu-id="34bd6-191">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="34bd6-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="34bd6-192">Découvert de code asynchrone</span><span class="sxs-lookup"><span data-stu-id="34bd6-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="34bd6-193">Création d’un contrôleur de service</span><span class="sxs-lookup"><span data-stu-id="34bd6-193">Created a Department controller</span></span>
> * <span data-ttu-id="34bd6-194">Utiliser des procédures stockées</span><span class="sxs-lookup"><span data-stu-id="34bd6-194">Used stored procedures</span></span>
> * <span data-ttu-id="34bd6-195">Déployé sur Azure</span><span class="sxs-lookup"><span data-stu-id="34bd6-195">Deployed to Azure</span></span>

<span data-ttu-id="34bd6-196">Passez à l’article suivant pour apprendre à gérer les conflits quand plusieurs utilisateurs mettre à jour la même entité en même temps.</span><span class="sxs-lookup"><span data-stu-id="34bd6-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="34bd6-197">Gestion des accès concurrentiels</span><span class="sxs-lookup"><span data-stu-id="34bd6-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
