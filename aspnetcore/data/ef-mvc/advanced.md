---
title: ASP.NET Core MVC avec EF Core - Rubriques avancées - 10 sur 10
author: rick-anderson
description: Ce tutoriel présente plusieurs rubriques pratiques pour aller au-delà des principes de base du développement d’applications web ASP.NET Core qui utilisent Entity Framework Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/advanced
ms.openlocfilehash: ba3834b29e78972bf914a5cba1a2cae3cc19a315
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "50090782"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a><span data-ttu-id="1d605-103">ASP.NET Core MVC avec EF Core - Rubriques avancées - 10 sur 10</span><span class="sxs-lookup"><span data-stu-id="1d605-103">ASP.NET Core MVC with EF Core - Advanced - 10 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1d605-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1d605-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1d605-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1d605-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="1d605-106">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="1d605-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="1d605-107">Dans le didacticiel précédent, vous avez implémenté l’héritage TPH (table par hiérarchie).</span><span class="sxs-lookup"><span data-stu-id="1d605-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="1d605-108">Ce didacticiel présente plusieurs rubriques qu’il est utile de connaître lorsque vous allez au-delà des principes de base du développement d’applications web ASP.NET Core qui utilisent Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1d605-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="1d605-109">Requêtes SQL brutes</span><span class="sxs-lookup"><span data-stu-id="1d605-109">Raw SQL Queries</span></span>

<span data-ttu-id="1d605-110">L’un des avantages d’utiliser Entity Framework est que cela évite de lier votre code trop étroitement à une méthode particulière de stockage des données.</span><span class="sxs-lookup"><span data-stu-id="1d605-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="1d605-111">Il le fait en générant des requêtes et des commandes SQL pour vous, ce qui vous évite d’avoir à les écrire vous-même.</span><span class="sxs-lookup"><span data-stu-id="1d605-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="1d605-112">Mais, dans certains scénarios exceptionnels, vous devez exécuter des requêtes SQL spécifiques que vous créez manuellement.</span><span class="sxs-lookup"><span data-stu-id="1d605-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="1d605-113">Pour ces scénarios, l’API Entity Framework Code First comprend des méthodes qui vous permettent de transmettre des commandes SQL directement à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1d605-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="1d605-114">Les options suivantes sont disponibles dans EF Core 1.0 :</span><span class="sxs-lookup"><span data-stu-id="1d605-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="1d605-115">Utilisez la méthode `DbSet.FromSql` pour les requêtes qui renvoient des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="1d605-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="1d605-116">Les objets renvoyés doivent être du type attendu par l’objet `DbSet` et ils sont automatiquement suivis par le contexte de base de données, sauf si vous [désactivez le suivi](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="1d605-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="1d605-117">Utilisez `Database.ExecuteSqlCommand` pour les commandes ne se rapportant pas aux requêtes.</span><span class="sxs-lookup"><span data-stu-id="1d605-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="1d605-118">Si vous avez besoin d’exécuter une requête qui renvoie des types qui ne sont pas des entités, vous pouvez utiliser ADO.NET avec la connexion de base de données fournie par EF.</span><span class="sxs-lookup"><span data-stu-id="1d605-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="1d605-119">Les données renvoyées ne font pas l’objet d’un suivi par le contexte de base de données, même si vous utilisez cette méthode pour récupérer des types d’entités.</span><span class="sxs-lookup"><span data-stu-id="1d605-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="1d605-120">Comme c’est toujours le cas lorsque vous exécutez des commandes SQL dans une application web, vous devez prendre des précautions pour protéger votre site contre des attaques par injection de code SQL.</span><span class="sxs-lookup"><span data-stu-id="1d605-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="1d605-121">Une manière de procéder consiste à utiliser des requêtes paramétrables pour vous assurer que les chaînes soumises par une page web ne peuvent pas être interprétées comme des commandes SQL.</span><span class="sxs-lookup"><span data-stu-id="1d605-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="1d605-122">Dans ce didacticiel, vous utiliserez des requêtes paramétrables lors de l’intégration de l’entrée utilisateur dans une requête.</span><span class="sxs-lookup"><span data-stu-id="1d605-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="1d605-123">Appeler une requête qui renvoie des entités</span><span class="sxs-lookup"><span data-stu-id="1d605-123">Call a query that returns entities</span></span>

<span data-ttu-id="1d605-124">La classe `DbSet<TEntity>` fournit une méthode que vous pouvez utiliser pour exécuter une requête qui renvoie une entité de type `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="1d605-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="1d605-125">Pour voir comment cela fonctionne vous allez modifier le code dans la méthode `Details` du contrôleur Department.</span><span class="sxs-lookup"><span data-stu-id="1d605-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="1d605-126">Dans *DepartmentsController.cs*, dans la méthode `Details`, remplacez le code qui récupère un service par un appel de méthode `FromSql`, comme indiqué dans le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="1d605-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="1d605-127">Pour vérifier que le nouveau code fonctionne correctement, sélectionnez l’onglet **Departments**, puis **Details** pour l’un des services.</span><span class="sxs-lookup"><span data-stu-id="1d605-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Détails du service](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="1d605-129">Appeler une requête qui renvoie d’autres types</span><span class="sxs-lookup"><span data-stu-id="1d605-129">Call a query that returns other types</span></span>

<span data-ttu-id="1d605-130">Précédemment, vous avez créé une grille de statistiques des étudiants pour la page About, qui montrait le nombre d’étudiants pour chaque date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="1d605-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="1d605-131">Vous avez obtenu les données à partir du jeu d’entités Students (`_context.Students`) et utilisé LINQ pour projeter les résultats dans une liste d’objets de modèle de vue `EnrollmentDateGroup`.</span><span class="sxs-lookup"><span data-stu-id="1d605-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="1d605-132">Supposons que vous voulez écrire le code SQL lui-même plutôt qu’utiliser LINQ.</span><span class="sxs-lookup"><span data-stu-id="1d605-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="1d605-133">Pour ce faire, vous avez besoin d’exécuter une requête SQL qui renvoie autre chose que des objets d’entité.</span><span class="sxs-lookup"><span data-stu-id="1d605-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="1d605-134">Dans EF Core 1.0, une manière de procéder consiste à écrire du code ADO.NET et à obtenir la connexion de base de données à partir d’EF.</span><span class="sxs-lookup"><span data-stu-id="1d605-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="1d605-135">Dans *HomeController.cs*, remplacez la méthode `About` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1d605-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="1d605-136">Ajoutez une instruction using :</span><span class="sxs-lookup"><span data-stu-id="1d605-136">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="1d605-137">Exécutez l’application et accédez à la page About.</span><span class="sxs-lookup"><span data-stu-id="1d605-137">Run the app and go to the About page.</span></span> <span data-ttu-id="1d605-138">Elle affiche les mêmes données qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="1d605-138">It displays the same data it did before.</span></span>

![Page About](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="1d605-140">Appeler une requête de mise à jour</span><span class="sxs-lookup"><span data-stu-id="1d605-140">Call an update query</span></span>

<span data-ttu-id="1d605-141">Supposons que les administrateurs de Contoso University veuillent effectuer des modifications globales dans la base de données, comme par exemple modifier le nombre de crédits pour chaque cours.</span><span class="sxs-lookup"><span data-stu-id="1d605-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="1d605-142">Si l’université a un grand nombre de cours, il serait inefficace de les récupérer tous sous forme d’entités et de les modifier individuellement.</span><span class="sxs-lookup"><span data-stu-id="1d605-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="1d605-143">Dans cette section, vous allez implémenter une page web permettant à l’utilisateur de spécifier un facteur selon lequel il convient de modifier le nombre de crédits pour tous les cours, et vous effectuerez la modification en exécutant une instruction SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="1d605-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="1d605-144">La page web ressemblera à l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="1d605-144">The web page will look like the following illustration:</span></span>

![Page de mise à jour des crédits de cours](advanced/_static/update-credits.png)

<span data-ttu-id="1d605-146">Dans *CoursesContoller.cs*, ajoutez les méthodes UpdateCourseCredits pour HttpGet et HttpPost :</span><span class="sxs-lookup"><span data-stu-id="1d605-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="1d605-147">Lorsque le contrôleur traite une demande HttpGet, rien n’est renvoyé dans `ViewData["RowsAffected"]` et la vue affiche une zone de texte vide et un bouton d’envoi, comme indiqué dans l’illustration précédente.</span><span class="sxs-lookup"><span data-stu-id="1d605-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="1d605-148">Lorsque vous cliquez sur le bouton **Update**, la méthode HttpPost est appelée et le multiplicateur a la valeur entrée dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="1d605-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="1d605-149">Le code exécute alors l’instruction SQL qui met à jour les cours et renvoie le nombre de lignes affectées à la vue dans `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="1d605-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="1d605-150">Lorsque la vue obtient une valeur `RowsAffected`, elle affiche le nombre de lignes mis à jour.</span><span class="sxs-lookup"><span data-stu-id="1d605-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="1d605-151">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Views/Courses*, puis cliquez sur **Ajouter > Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="1d605-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="1d605-152">Dans la boîte de dialogue **Ajouter un nouvel élément**, cliquez sur **ASP.NET** sous **Installé** dans le volet gauche, cliquez sur **Page de vue MVC** et nommez la nouvelle vue *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1d605-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="1d605-153">Dans *Views/Courses/UpdateCourseCredits.cshtml*, remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1d605-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="1d605-154">Exécutez la méthode `UpdateCourseCredits` en sélectionnant l’onglet **Courses**, puis en ajoutant « /UpdateCourseCredits » à la fin de l’URL dans la barre d’adresse du navigateur (par exemple : `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="1d605-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="1d605-155">Entrez un nombre dans la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="1d605-155">Enter a number in the text box:</span></span>

![Page de mise à jour des crédits de cours](advanced/_static/update-credits.png)

<span data-ttu-id="1d605-157">Cliquez sur **Mettre à jour**.</span><span class="sxs-lookup"><span data-stu-id="1d605-157">Click **Update**.</span></span> <span data-ttu-id="1d605-158">Vous voyez le nombre de lignes affectées :</span><span class="sxs-lookup"><span data-stu-id="1d605-158">You see the number of rows affected:</span></span>

![Page de mise à jour des crédits de cours – lignes affectées](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="1d605-160">Cliquez sur **Revenir à la liste** pour afficher la liste des cours avec le nombre révisé de crédits.</span><span class="sxs-lookup"><span data-stu-id="1d605-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="1d605-161">Notez que le code de production garantit que les mises à jour fourniront toujours des données valides.</span><span class="sxs-lookup"><span data-stu-id="1d605-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="1d605-162">Le code simplifié indiqué ici peut multiplier le nombre de crédits suffisamment pour générer des nombres supérieurs à 5.</span><span class="sxs-lookup"><span data-stu-id="1d605-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="1d605-163">(La propriété `Credits` a un attribut `[Range(0, 5)]`.) La requête de mise à jour fonctionne, mais des données non valides peuvent provoquer des résultats inattendus dans d’autres parties du système qui supposent que le nombre de crédits est inférieur ou égal à 5.</span><span class="sxs-lookup"><span data-stu-id="1d605-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="1d605-164">Pour plus d’informations sur les requêtes SQL brutes, consultez [Requêtes SQL brutes](/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="1d605-164">For more information about raw SQL queries, see [Raw SQL Queries](/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="1d605-165">Examiner les requêtes SQL envoyées à la base de données</span><span class="sxs-lookup"><span data-stu-id="1d605-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="1d605-166">Il est parfois utile de pouvoir voir les requêtes SQL réelles qui sont envoyées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1d605-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="1d605-167">Les fonctionnalité de journalisation intégrées pour ASP.NET Core sont utilisées automatiquement par EF Core pour écrire des journaux qui contiennent le code SQL pour les requêtes et les mises à jour.</span><span class="sxs-lookup"><span data-stu-id="1d605-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="1d605-168">Dans cette section, vous verrez des exemples de journalisation SQL.</span><span class="sxs-lookup"><span data-stu-id="1d605-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="1d605-169">Ouvrez *StudentsController.cs* et dans la méthode `Details`, définissez un point d’arrêt sur l’instruction `if (student == null)`.</span><span class="sxs-lookup"><span data-stu-id="1d605-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="1d605-170">Exécutez l’application en mode débogage et accédez à la page Details d’un étudiant.</span><span class="sxs-lookup"><span data-stu-id="1d605-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="1d605-171">Accédez à la fenêtre **Output** qui indique la sortie de débogage. Vous voyez la requête :</span><span class="sxs-lookup"><span data-stu-id="1d605-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="1d605-172">Vous pouvez remarquer ici quelque chose susceptible de vous surprendre : l’instruction SQL sélectionne jusqu’à 2 lignes (`TOP(2)`) à partir de la table Person.</span><span class="sxs-lookup"><span data-stu-id="1d605-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="1d605-173">La méthode `SingleOrDefaultAsync` ne se résout pas à 1 ligne sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1d605-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="1d605-174">En voici les raisons :</span><span class="sxs-lookup"><span data-stu-id="1d605-174">Here's why:</span></span>

* <span data-ttu-id="1d605-175">Si la requête retourne plusieurs lignes, la méthode retourne la valeur Null.</span><span class="sxs-lookup"><span data-stu-id="1d605-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="1d605-176">Pour déterminer si la requête retourne plusieurs lignes, EF doit vérifier s’il retourne au moins 2.</span><span class="sxs-lookup"><span data-stu-id="1d605-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="1d605-177">Notez que vous n’êtes pas tenu d’utiliser le mode débogage et de vous arrêter à un point d’arrêt pour obtenir la sortie de journalisation dans la fenêtre **Output**.</span><span class="sxs-lookup"><span data-stu-id="1d605-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="1d605-178">C’est simplement un moyen pratique d’arrêter la journalisation au stade où vous voulez examiner la sortie.</span><span class="sxs-lookup"><span data-stu-id="1d605-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="1d605-179">Si vous ne le faites pas, la journalisation se poursuit et vous devez revenir en arrière pour rechercher les parties qui vous intéressent.</span><span class="sxs-lookup"><span data-stu-id="1d605-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="1d605-180">Modèles d’unité de travail et de référentiel</span><span class="sxs-lookup"><span data-stu-id="1d605-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="1d605-181">De nombreux développeurs écrivent du code pour implémenter les modèles d’unité de travail et de référentiel comme un wrapper autour du code qui fonctionne avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1d605-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="1d605-182">Ces modèles sont destinés à créer une couche d’abstraction entre la couche d’accès aux données et la couche de logique métier d’une application.</span><span class="sxs-lookup"><span data-stu-id="1d605-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="1d605-183">L’implémentation de ces modèles peut favoriser l’isolation de votre application face à des modifications dans le magasin de données et peut faciliter le test unitaire automatisé ou le développement piloté par les tests (TDD).</span><span class="sxs-lookup"><span data-stu-id="1d605-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="1d605-184">Toutefois, l’écriture de code supplémentaire pour implémenter ces modèles n’est pas toujours le meilleur choix pour les applications qui utilisent EF, et ce pour plusieurs raisons :</span><span class="sxs-lookup"><span data-stu-id="1d605-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="1d605-185">La classe de contexte EF elle-même isole votre code face au code spécifique de magasin de données.</span><span class="sxs-lookup"><span data-stu-id="1d605-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="1d605-186">La classe de contexte EF peut agir comme une classe d’unité de travail pour les mises à jour de base de données que vous effectuez à l’aide d’EF.</span><span class="sxs-lookup"><span data-stu-id="1d605-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="1d605-187">EF inclut des fonctionnalités pour implémenter TDD sans écrire de code de référentiel.</span><span class="sxs-lookup"><span data-stu-id="1d605-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="1d605-188">Pour plus d’informations sur la façon d’implémenter les modèles d’unité de travail et de référentiel, consultez [la version Entity Framework 5 de cette série de didacticiels](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="1d605-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="1d605-189">Entity Framework Core implémente un fournisseur de base de données en mémoire qui peut être utilisé pour les tests.</span><span class="sxs-lookup"><span data-stu-id="1d605-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="1d605-190">Pour plus d’informations, consultez [Tester avec InMemory](/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="1d605-190">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="1d605-191">Détection automatique des modifications</span><span class="sxs-lookup"><span data-stu-id="1d605-191">Automatic change detection</span></span>

<span data-ttu-id="1d605-192">Entity Framework détermine la manière dont une entité a changé (et par conséquent les mises à jour qui doivent être envoyées à la base de données) en comparant les valeurs en cours d’une entité avec les valeurs d’origine.</span><span class="sxs-lookup"><span data-stu-id="1d605-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="1d605-193">Les valeurs d’origine sont stockées lorsque l’entité fait l’objet d’une requête ou d’une jointure.</span><span class="sxs-lookup"><span data-stu-id="1d605-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="1d605-194">Certaines des méthodes qui provoquent la détection automatique des modifications sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="1d605-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="1d605-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="1d605-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="1d605-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="1d605-196">DbContext.Entry</span></span>

* <span data-ttu-id="1d605-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="1d605-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="1d605-198">Si vous effectuez le suivi d’un grand nombre d’entités et que vous appelez l’une de ces méthodes de nombreuses fois dans une boucle, vous pouvez obtenir des améliorations significatives des performances en désactivant temporairement la détection automatique des modifications à l’aide de la propriété `ChangeTracker.AutoDetectChangesEnabled`.</span><span class="sxs-lookup"><span data-stu-id="1d605-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="1d605-199">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="1d605-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="1d605-200">Code source et plans de développement d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1d605-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="1d605-201">La source d’Entity Framework Core se trouve à l’adresse [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="1d605-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="1d605-202">Le dépôt EF Core contient les builds nocturnes, le suivi des problèmes, les spécifications des fonctionnalités, les notes des réunions de conception et [la feuille de route de développement futur](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="1d605-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="1d605-203">Vous pouvez signaler ou rechercher des bogues, et apporter votre contribution.</span><span class="sxs-lookup"><span data-stu-id="1d605-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="1d605-204">Bien que le code source soit ouvert, Entity Framework Core est entièrement pris en charge comme produit Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1d605-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="1d605-205">L’équipe Microsoft Entity Framework garde le contrôle sur le choix des contributions qui sont acceptées et teste toutes les modifications du code pour garantir la qualité de chaque version.</span><span class="sxs-lookup"><span data-stu-id="1d605-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="1d605-206">Ingénierie à rebours à partir de la base de données existante</span><span class="sxs-lookup"><span data-stu-id="1d605-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="1d605-207">Pour rétroconcevoir un modèle de données comprenant des classes d’entité issues d’une base de données existante, utilisez la commande [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="1d605-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="1d605-208">Consultez le [didacticiel de prise en main](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="1d605-208">See the [getting-started tutorial](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="1d605-209">Utiliser du code dynamique LINQ pour simplifier le code de sélection de tri</span><span class="sxs-lookup"><span data-stu-id="1d605-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="1d605-210">Le [troisième didacticiel de cette série](sort-filter-page.md) montre comment écrire du code LINQ en codant en dur les noms des colonnes dans une instruction `switch`.</span><span class="sxs-lookup"><span data-stu-id="1d605-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="1d605-211">Avec deux colonnes sélectionnables, cela fonctionne correctement, mais si vous avez de nombreuses colonnes, le code peut devenir très détaillé.</span><span class="sxs-lookup"><span data-stu-id="1d605-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="1d605-212">Pour résoudre ce problème, vous pouvez utiliser la méthode `EF.Property` pour spécifier le nom de la propriété sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="1d605-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="1d605-213">Pour tester cette approche, remplacez la méthode `Index` dans `StudentsController` par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="1d605-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="1d605-214">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="1d605-214">Next steps</span></span>

<span data-ttu-id="1d605-215">Cette étape termine cette série de tutoriels sur l’utilisation d’Entity Framework Core dans une application ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="1d605-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET Core MVC application.</span></span>

<span data-ttu-id="1d605-216">Pour plus d’informations sur EF Core, consultez la [documentation sur Entity Framework Core](/ef/core).</span><span class="sxs-lookup"><span data-stu-id="1d605-216">For more information about EF Core, see the [Entity Framework Core documentation](/ef/core).</span></span> <span data-ttu-id="1d605-217">Un ouvrage est également disponible : [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="1d605-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="1d605-218">Pour plus d’informations sur le déploiement d’une application web, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="1d605-218">For information on how to deploy a web app, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="1d605-219">Pour plus d’informations sur les autres rubriques associées à ASP.NET Core MVC, par exemple l’authentification et l’autorisation, consultez <xref:index>.</span><span class="sxs-lookup"><span data-stu-id="1d605-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see <xref:index>.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="1d605-220">Remerciements</span><span class="sxs-lookup"><span data-stu-id="1d605-220">Acknowledgments</span></span>

<span data-ttu-id="1d605-221">Tom Dykstra et Rick Anderson (twitter @RickAndMSFT) ont rédigé ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="1d605-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="1d605-222">Rowan Miller, Diego Vega et d’autres membres de l’équipe Entity Framework ont participé à la revue du code et ont aidé à résoudre les problèmes qui se sont posés lorsque nous avons écrit le code pour ces didacticiels.</span><span class="sxs-lookup"><span data-stu-id="1d605-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="1d605-223">Erreurs courantes</span><span class="sxs-lookup"><span data-stu-id="1d605-223">Common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="1d605-224">ContosoUniversity.dll est utilisé par un autre processus</span><span class="sxs-lookup"><span data-stu-id="1d605-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="1d605-225">Message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="1d605-225">Error message:</span></span>

> <span data-ttu-id="1d605-226">Impossible d’ouvrir ’...bin\Debug\netcoreapp1.0\ContosoUniversity.dll’ en écriture -- ’Le processus ne peut pas accéder au fichier ’...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll’, car il est en cours d’utilisation par un autre processus.</span><span class="sxs-lookup"><span data-stu-id="1d605-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="1d605-227">Solution :</span><span class="sxs-lookup"><span data-stu-id="1d605-227">Solution:</span></span>

<span data-ttu-id="1d605-228">Arrêtez le site dans IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1d605-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="1d605-229">Accédez à la barre d’état système de Windows, recherchez IIS Express, cliquez avec le bouton droit sur son icône, sélectionnez le site Contoso University, puis cliquez sur **Arrêter le site**.</span><span class="sxs-lookup"><span data-stu-id="1d605-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="1d605-230">Migration structurée sans code dans les méthodes Up et Down</span><span class="sxs-lookup"><span data-stu-id="1d605-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="1d605-231">Cause possible :</span><span class="sxs-lookup"><span data-stu-id="1d605-231">Possible cause:</span></span>

<span data-ttu-id="1d605-232">Les commandes CLI d’EF ne ferment et n’enregistrent pas automatiquement des fichiers de code.</span><span class="sxs-lookup"><span data-stu-id="1d605-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="1d605-233">Si vous avez des modifications non enregistrées lorsque vous exécutez la commande `migrations add`, EF ne trouve pas vos modifications.</span><span class="sxs-lookup"><span data-stu-id="1d605-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="1d605-234">Solution :</span><span class="sxs-lookup"><span data-stu-id="1d605-234">Solution:</span></span>

<span data-ttu-id="1d605-235">Exécutez la commande `migrations remove`, enregistrez vos modifications de code et réexécutez la commande `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="1d605-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="1d605-236">Erreurs lors de l’exécution de la mise à jour de base de données</span><span class="sxs-lookup"><span data-stu-id="1d605-236">Errors while running database update</span></span>

<span data-ttu-id="1d605-237">Vous pouvez obtenir d’autres erreurs en apportant des modifications au schéma dans une base de données qui comporte déjà des données.</span><span class="sxs-lookup"><span data-stu-id="1d605-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="1d605-238">Si vous obtenez des erreurs de migration que vous ne pouvez pas résoudre, vous pouvez changer le nom de la base de données dans la chaîne de connexion ou supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="1d605-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="1d605-239">Avec une nouvelle base de données, il n’y a pas de données à migrer et la commande de mise à jour de base de données a beaucoup plus de chances de s’exécuter sans erreur.</span><span class="sxs-lookup"><span data-stu-id="1d605-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="1d605-240">L’approche la plus simple consiste à renommer la base de données en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1d605-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="1d605-241">La prochaine fois que vous exécuterez `database update`, une nouvelle base de données sera créée.</span><span class="sxs-lookup"><span data-stu-id="1d605-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="1d605-242">Pour supprimer une base de données dans SSOX, cliquez avec le bouton droit sur la base de données, cliquez sur **Supprimer**, puis, dans la boîte de dialogue **Supprimer la base de données**, sélectionnez **Fermer les connexions existantes** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d605-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="1d605-243">Pour supprimer une base de données à l’aide de l’interface CLI, exécutez la commande CLI `database drop` :</span><span class="sxs-lookup"><span data-stu-id="1d605-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="1d605-244">Erreur lors de la localisation de l’instance SQL Server</span><span class="sxs-lookup"><span data-stu-id="1d605-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="1d605-245">Message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="1d605-245">Error Message:</span></span>

> <span data-ttu-id="1d605-246">Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1d605-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="1d605-247">Le serveur est introuvable ou n’est pas accessible.</span><span class="sxs-lookup"><span data-stu-id="1d605-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="1d605-248">Vérifiez que le nom de l’instance est correct et que SQL Server est configuré pour autoriser les connexions distantes.</span><span class="sxs-lookup"><span data-stu-id="1d605-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="1d605-249">(fournisseur : Interfaces réseau SQL, erreur : 26 - Erreur lors de la localisation du serveur/de l’instance spécifiés)</span><span class="sxs-lookup"><span data-stu-id="1d605-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="1d605-250">Solution :</span><span class="sxs-lookup"><span data-stu-id="1d605-250">Solution:</span></span>

<span data-ttu-id="1d605-251">Vérifiez la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="1d605-251">Check the connection string.</span></span> <span data-ttu-id="1d605-252">Si vous avez supprimé manuellement le fichier de base de données, modifiez le nom de la base de données dans la chaîne de construction pour recommencer avec une nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="1d605-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="1d605-253">Précédent</span><span class="sxs-lookup"><span data-stu-id="1d605-253">Previous</span></span>](inheritance.md)
