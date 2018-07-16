---
title: ASP.NET Core MVC avec EF Core - Tri, filtre, changement de page - 3 sur 10
author: rick-anderson
description: Dans ce didacticiel, vous allez ajouter des fonctionnalités de tri, de filtrage et de changement de page à une page à l’aide d’ASP.NET Core et d’Entity Framework Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 1f80faf0e36332c28e8337ddc331cc8b4c4970d7
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38193947"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="ec28e-103">ASP.NET Core MVC avec EF Core - Tri, filtre, changement de page - 3 sur 10</span><span class="sxs-lookup"><span data-stu-id="ec28e-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ec28e-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec28e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ec28e-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec28e-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="ec28e-106">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="ec28e-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="ec28e-107">Dans le didacticiel précédent, vous avez implémenté un ensemble de pages web pour les opérations CRUD de base pour les entités Student.</span><span class="sxs-lookup"><span data-stu-id="ec28e-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="ec28e-108">Dans ce didacticiel, vous allez ajouter les fonctionnalités de tri, de filtrage et de changement de page à la page d’index des étudiants.</span><span class="sxs-lookup"><span data-stu-id="ec28e-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="ec28e-109">Vous allez également créer une page qui effectue un regroupement simple.</span><span class="sxs-lookup"><span data-stu-id="ec28e-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="ec28e-110">L’illustration suivante montre à quoi ressemblera la page quand vous aurez terminé.</span><span class="sxs-lookup"><span data-stu-id="ec28e-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="ec28e-111">Les en-têtes des colonnes sont des liens sur lesquels l’utilisateur peut cliquer pour trier selon les colonnes.</span><span class="sxs-lookup"><span data-stu-id="ec28e-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="ec28e-112">Cliquer de façon répétée sur un en-tête de colonne permet de changer l’ordre de tri (croissant ou décroissant).</span><span class="sxs-lookup"><span data-stu-id="ec28e-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Page d’index des étudiants](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="ec28e-114">Ajouter des liens de tri de colonne à la page d’index des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="ec28e-115">Pour ajouter le tri à la page d’index des étudiants, vous allez modifier la méthode `Index` du contrôleur Students et ajouter du code à la vue de l’index des étudiants.</span><span class="sxs-lookup"><span data-stu-id="ec28e-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="ec28e-116">Ajouter la fonctionnalité de tri à la méthode Index</span><span class="sxs-lookup"><span data-stu-id="ec28e-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="ec28e-117">Dans *StudentsController.cs*, remplacez la méthode `Index` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec28e-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="ec28e-118">Ce code reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="ec28e-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="ec28e-119">La valeur de chaîne de requête est fournie par ASP.NET Core MVC en tant que paramètre à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="ec28e-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="ec28e-120">Le paramètre sera la chaîne « Name » ou « Date », éventuellement suivie d’un trait de soulignement et de la chaîne « desc » pour spécifier l’ordre décroissant.</span><span class="sxs-lookup"><span data-stu-id="ec28e-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="ec28e-121">L'ordre de tri par défaut est le tri croissant.</span><span class="sxs-lookup"><span data-stu-id="ec28e-121">The default sort order is ascending.</span></span>

<span data-ttu-id="ec28e-122">La première fois que la page d’index est demandée, il n’y a pas de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="ec28e-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="ec28e-123">Les étudiants sont affichés dans l’ordre croissant par leur nom, ce qui correspond au paramétrage par défaut de l’instruction `switch`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="ec28e-124">Quand l’utilisateur clique sur un lien hypertexte d’en-tête de colonne, la valeur `sortOrder` appropriée est fournie dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="ec28e-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="ec28e-125">Les deux éléments `ViewData` (NameSortParm et DateSortParm) sont utilisés par la vue pour configurer les liens hypertexte d’en-tête de colonne avec les valeurs de chaîne de requête appropriées.</span><span class="sxs-lookup"><span data-stu-id="ec28e-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="ec28e-126">Il s’agit d’instructions ternaires.</span><span class="sxs-lookup"><span data-stu-id="ec28e-126">These are ternary statements.</span></span> <span data-ttu-id="ec28e-127">La première spécifie que si le paramètre `sortOrder` est null ou vide, NameSortParm doit être défini sur « name_desc » ; sinon, il doit être défini sur une chaîne vide.</span><span class="sxs-lookup"><span data-stu-id="ec28e-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="ec28e-128">Ces deux instructions permettent à la vue de définir les liens hypertexte d’en-tête de colonne comme suit :</span><span class="sxs-lookup"><span data-stu-id="ec28e-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="ec28e-129">Ordre de tri actuel</span><span class="sxs-lookup"><span data-stu-id="ec28e-129">Current sort order</span></span>  | <span data-ttu-id="ec28e-130">Lien hypertexte Nom de famille</span><span class="sxs-lookup"><span data-stu-id="ec28e-130">Last Name Hyperlink</span></span> | <span data-ttu-id="ec28e-131">Lien hypertexte Date</span><span class="sxs-lookup"><span data-stu-id="ec28e-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="ec28e-132">Nom de famille croissant</span><span class="sxs-lookup"><span data-stu-id="ec28e-132">Last Name ascending</span></span>  | <span data-ttu-id="ec28e-133">descending</span><span class="sxs-lookup"><span data-stu-id="ec28e-133">descending</span></span>          | <span data-ttu-id="ec28e-134">ascending</span><span class="sxs-lookup"><span data-stu-id="ec28e-134">ascending</span></span>      |
| <span data-ttu-id="ec28e-135">Nom de famille décroissant</span><span class="sxs-lookup"><span data-stu-id="ec28e-135">Last Name descending</span></span> | <span data-ttu-id="ec28e-136">ascending</span><span class="sxs-lookup"><span data-stu-id="ec28e-136">ascending</span></span>           | <span data-ttu-id="ec28e-137">ascending</span><span class="sxs-lookup"><span data-stu-id="ec28e-137">ascending</span></span>      |
| <span data-ttu-id="ec28e-138">Date croissante</span><span class="sxs-lookup"><span data-stu-id="ec28e-138">Date ascending</span></span>       | <span data-ttu-id="ec28e-139">ascending</span><span class="sxs-lookup"><span data-stu-id="ec28e-139">ascending</span></span>           | <span data-ttu-id="ec28e-140">descending</span><span class="sxs-lookup"><span data-stu-id="ec28e-140">descending</span></span>     |
| <span data-ttu-id="ec28e-141">Date décroissante</span><span class="sxs-lookup"><span data-stu-id="ec28e-141">Date descending</span></span>      | <span data-ttu-id="ec28e-142">ascending</span><span class="sxs-lookup"><span data-stu-id="ec28e-142">ascending</span></span>           | <span data-ttu-id="ec28e-143">ascending</span><span class="sxs-lookup"><span data-stu-id="ec28e-143">ascending</span></span>      |

<span data-ttu-id="ec28e-144">La méthode utilise LINQ to Entities pour spécifier la colonne d’après laquelle effectuer le tri.</span><span class="sxs-lookup"><span data-stu-id="ec28e-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="ec28e-145">Le code crée une variable `IQueryable` avant l’instruction switch, la modifie dans l’instruction switch et appelle la méthode `ToListAsync` après l’instruction `switch`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="ec28e-146">Lorsque vous créez et modifiez des variables `IQueryable`, aucune requête n’est envoyée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="ec28e-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="ec28e-147">La requête n’est pas exécutée tant que vous ne convertissez pas l’objet `IQueryable` en collection en appelant une méthode telle que `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="ec28e-148">Par conséquent, ce code génère une requête unique qui n’est pas exécutée avant l’instruction `return View`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="ec28e-149">Ce code peut devenir très détaillé avec un grand nombre de colonnes.</span><span class="sxs-lookup"><span data-stu-id="ec28e-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="ec28e-150">[Le dernier didacticiel de cette série](advanced.md#dynamic-linq) montre comment écrire du code qui vous permet de transmettre le nom de la colonne `OrderBy` dans une variable chaîne.</span><span class="sxs-lookup"><span data-stu-id="ec28e-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="ec28e-151">Ajouter des liens hypertexte d’en-tête de colonne dans la vue de l’index des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="ec28e-152">Remplacez le code dans *Views/Students/Index.cshtml* par le code suivant pour ajouter des liens hypertexte d’en-tête de colonne.</span><span class="sxs-lookup"><span data-stu-id="ec28e-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="ec28e-153">Les lignes modifiées apparaissent en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="ec28e-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="ec28e-154">Ce code utilise les informations figurant dans les propriétés `ViewData` pour configurer des liens hypertexte avec les valeurs de chaîne de requête appropriées.</span><span class="sxs-lookup"><span data-stu-id="ec28e-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="ec28e-155">Exécutez l’application, sélectionnez l’onglet **Students**, puis cliquez sur les en-têtes des colonnes **Last Name** et **Enrollment Date** pour vérifier que le tri fonctionne.</span><span class="sxs-lookup"><span data-stu-id="ec28e-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Page d’index des étudiants dans l’ordre de leurs noms](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="ec28e-157">Ajouter une zone de recherche à la page d’index des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="ec28e-158">Pour ajouter le filtrage à la page d’index des étudiants, vous allez ajouter une zone de texte et un bouton d’envoi à la vue et apporter les modifications correspondantes dans la méthode `Index`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="ec28e-159">La zone de texte vous permet d’entrer une chaîne à rechercher dans les champs de prénom et de nom.</span><span class="sxs-lookup"><span data-stu-id="ec28e-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="ec28e-160">Ajouter la fonctionnalité de filtrage à la méthode Index</span><span class="sxs-lookup"><span data-stu-id="ec28e-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="ec28e-161">Dans *StudentsController.cs*, remplacez la méthode `Index` par le code suivant (les modifications apparaissent en surbrillance).</span><span class="sxs-lookup"><span data-stu-id="ec28e-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="ec28e-162">Vous avez ajouté un paramètre `searchString` à la méthode `Index`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="ec28e-163">La valeur de chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue Index.</span><span class="sxs-lookup"><span data-stu-id="ec28e-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="ec28e-164">Vous avez également ajouté à l’instruction LINQ une clause where qui sélectionne uniquement les étudiants dont le prénom ou le nom contient la chaîne de recherche.</span><span class="sxs-lookup"><span data-stu-id="ec28e-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="ec28e-165">L’instruction qui ajoute la clause where est exécutée uniquement s’il existe une valeur à rechercher.</span><span class="sxs-lookup"><span data-stu-id="ec28e-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="ec28e-166">Ici, vous appelez la méthode `Where` sur un objet `IQueryable`, et le filtre sera traité sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec28e-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="ec28e-167">Dans certains scénarios, vous pouvez appeler la méthode `Where` en tant que méthode d’extension sur une collection en mémoire.</span><span class="sxs-lookup"><span data-stu-id="ec28e-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="ec28e-168">(Par exemple, supposons que vous changez la référence à `_context.Students` de sorte qu’à la place d’un `DbSet` EF, elle fasse référence à une méthode de référentiel qui renvoie une collection `IEnumerable`.) Le résultat serait normalement le même, mais pourrait être différent dans certains cas.</span><span class="sxs-lookup"><span data-stu-id="ec28e-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="ec28e-169">Par exemple, l’implémentation par le .NET Framework de la méthode `Contains` effectue une comparaison respectant la casse par défaut, mais dans SQL Server, cela est déterminé par le paramètre de classement de l’instance SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ec28e-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="ec28e-170">Ce paramètre définit par défaut le non-respect de la casse.</span><span class="sxs-lookup"><span data-stu-id="ec28e-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="ec28e-171">Vous pouvez appeler la méthode `ToUpper` pour rendre explicitement le test sensible à la casse :  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="ec28e-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="ec28e-172">Cela garantit que les résultats resteront les mêmes si vous modifiez le code ultérieurement pour utiliser un référentiel qui renverra une collection `IEnumerable` à la place d’un objet `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="ec28e-173">(Lorsque vous appelez la méthode `Contains` sur une collection `IEnumerable`, vous obtenez l’implémentation du .NET Framework ; lorsque vous l’appelez sur un objet `IQueryable`, vous obtenez l’implémentation du fournisseur de base de données.) Toutefois, cette solution présente un coût en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="ec28e-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="ec28e-174">Le code `ToUpper` place une fonction dans la clause WHERE de l’instruction TSQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="ec28e-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="ec28e-175">Elle empêche l’optimiseur d’utiliser un index.</span><span class="sxs-lookup"><span data-stu-id="ec28e-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="ec28e-176">Étant donné que SQL est généralement installé comme non sensible à la casse, il est préférable d’éviter le code `ToUpper` jusqu’à ce que vous ayez migré vers un magasin de données qui respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="ec28e-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="ec28e-177">Ajouter une zone de recherche à la vue de l’index des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="ec28e-178">Dans *Views/Student/Index.cshtml*, ajoutez le code en surbrillance immédiatement avant la balise d’ouverture de table afin de créer une légende, une zone de texte et un bouton de **recherche**.</span><span class="sxs-lookup"><span data-stu-id="ec28e-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="ec28e-179">Ce code utilise le [Tag Helper](xref:mvc/views/tag-helpers/intro) `<form>` pour ajouter le bouton et la zone de texte de recherche.</span><span class="sxs-lookup"><span data-stu-id="ec28e-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="ec28e-180">Par défaut, le Tag Helper `<form>` envoie les données de formulaire avec un POST, ce qui signifie que les paramètres sont transmis dans le corps du message HTTP et non pas dans l’URL sous forme de chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="ec28e-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="ec28e-181">Lorsque vous spécifiez HTTP GET, les données de formulaire sont transmises dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs d’ajouter l’URL aux favoris.</span><span class="sxs-lookup"><span data-stu-id="ec28e-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="ec28e-182">Les recommandations du W3C stipulent que vous devez utiliser GET quand l’action ne produit pas de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="ec28e-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="ec28e-183">Exécutez l’application, sélectionnez l’onglet **Students**, entrez une chaîne de recherche, puis cliquez sur Rechercher pour vérifier que le filtrage fonctionne.</span><span class="sxs-lookup"><span data-stu-id="ec28e-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Page d’index des étudiants avec filtrage](sort-filter-page/_static/filtering.png)

<span data-ttu-id="ec28e-185">Notez que l’URL contient la chaîne de recherche.</span><span class="sxs-lookup"><span data-stu-id="ec28e-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="ec28e-186">Si vous marquez cette page d’un signet, vous obtenez la liste filtrée lorsque vous utilisez le signet.</span><span class="sxs-lookup"><span data-stu-id="ec28e-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="ec28e-187">L’ajout de `method="get"` dans la balise `form` est ce qui a provoqué la génération de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="ec28e-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="ec28e-188">À ce stade, si vous cliquez sur un lien de tri d’en-tête de colonne, vous perdez la valeur de filtre que vous avez entrée dans la zone **Rechercher**.</span><span class="sxs-lookup"><span data-stu-id="ec28e-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="ec28e-189">Vous corrigerez cela dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="ec28e-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="ec28e-190">Ajouter la fonctionnalité de changement de page à la page d’index des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="ec28e-191">Pour ajouter le changement de page à la page d’index des étudiants, vous allez créer une classe `PaginatedList` qui utilise les instructions `Skip` et `Take` pour filtrer les données sur le serveur au lieu de toujours récupérer toutes les lignes de la table.</span><span class="sxs-lookup"><span data-stu-id="ec28e-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="ec28e-192">Ensuite, vous apporterez des modifications supplémentaires dans la méthode `Index` et ajouterez des boutons de changement de page dans la vue `Index`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="ec28e-193">L’illustration suivante montre les boutons de changement de page.</span><span class="sxs-lookup"><span data-stu-id="ec28e-193">The following illustration shows the paging buttons.</span></span>

![Page d’index des étudiants avec liens de changement de page](sort-filter-page/_static/paging.png)

<span data-ttu-id="ec28e-195">Dans le dossier du projet, créez `PaginatedList.cs`, puis remplacez le code du modèle par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="ec28e-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="ec28e-196">La méthode `CreateAsync` de ce code accepte la taille de page et le numéro de page, et applique les instructions `Skip` et `Take` appropriées à `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="ec28e-197">Quand la méthode `ToListAsync` est appelée sur `IQueryable`, elle renvoie une liste contenant uniquement la page demandée.</span><span class="sxs-lookup"><span data-stu-id="ec28e-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="ec28e-198">Les propriétés `HasPreviousPage` et `HasNextPage` peuvent être utilisées pour activer ou désactiver les boutons de changement de page **Précédent** et **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ec28e-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="ec28e-199">Une méthode `CreateAsync` est utilisée à la place d’un constructeur pour créer l’objet `PaginatedList<T>`, car les constructeurs ne peuvent pas exécuter de code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ec28e-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="ec28e-200">Ajouter la fonctionnalité de changement de page à la méthode Index</span><span class="sxs-lookup"><span data-stu-id="ec28e-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="ec28e-201">Dans *StudentsController.cs*, remplacez la méthode `Index` par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="ec28e-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="ec28e-202">Ce code ajoute un paramètre de numéro de page, un paramètre d’ordre de tri actuel et un paramètre de filtre actuel à la signature de la méthode.</span><span class="sxs-lookup"><span data-stu-id="ec28e-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="ec28e-203">La première fois que la page s’affiche, ou si l’utilisateur n’a pas cliqué sur un lien de changement de page ni de tri, tous les paramètres sont Null.</span><span class="sxs-lookup"><span data-stu-id="ec28e-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="ec28e-204">Si l’utilisateur clique sur un lien de changement de page, la variable de page contient le numéro de page à afficher.</span><span class="sxs-lookup"><span data-stu-id="ec28e-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="ec28e-205">L’élément `ViewData` nommé CurrentSort fournit à l’affichage l’ordre de tri actuel, car il doit être inclus dans les liens de changement de page pour que l’ordre de tri soit conservé lors du changement de page.</span><span class="sxs-lookup"><span data-stu-id="ec28e-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="ec28e-206">L’élément `ViewData` nommé CurrentFilter fournit à la vue la chaîne de filtre actuelle.</span><span class="sxs-lookup"><span data-stu-id="ec28e-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="ec28e-207">Cette valeur doit être incluse dans les liens de changement de page pour que les paramètres de filtre soient conservés lors du changement de page, et elle doit être restaurée dans la zone de texte lorsque la page est réaffichée.</span><span class="sxs-lookup"><span data-stu-id="ec28e-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="ec28e-208">Si la chaîne de recherche est modifiée au cours du changement de page, la page doit être réinitialisée à 1, car le nouveau filtre peut entraîner l’affichage de données différentes.</span><span class="sxs-lookup"><span data-stu-id="ec28e-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="ec28e-209">La chaîne de recherche est modifiée quand une valeur est entrée dans la zone de texte et que le bouton d’envoi est enfoncé.</span><span class="sxs-lookup"><span data-stu-id="ec28e-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="ec28e-210">Dans ce cas, le paramètre `searchString` n’est pas Null.</span><span class="sxs-lookup"><span data-stu-id="ec28e-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="ec28e-211">À la fin de la méthode `Index`, la méthode `PaginatedList.CreateAsync` convertit la requête d’étudiant en une page individuelle d’étudiants dans un type de collection qui prend en charge le changement de page.</span><span class="sxs-lookup"><span data-stu-id="ec28e-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="ec28e-212">Cette page individuelle d’étudiants est alors transmise à la vue.</span><span class="sxs-lookup"><span data-stu-id="ec28e-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="ec28e-213">La méthode `PaginatedList.CreateAsync` accepte un numéro de page.</span><span class="sxs-lookup"><span data-stu-id="ec28e-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="ec28e-214">Les deux points d’interrogation représentent l’opérateur de fusion Null.</span><span class="sxs-lookup"><span data-stu-id="ec28e-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="ec28e-215">L’opérateur de fusion Null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` indique de renvoyer la valeur de `page` si elle a une valeur, ou de renvoyer 1 si `page` a la valeur Null.</span><span class="sxs-lookup"><span data-stu-id="ec28e-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="ec28e-216">Ajouter des liens de changement de page à la vue de l’index des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="ec28e-217">Dans *Views/Instructors/Index.cshtml*, remplacez le code existant par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="ec28e-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="ec28e-218">Les modifications apparaissent en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="ec28e-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="ec28e-219">L’instruction `@model` en haut de la page spécifie que la vue obtient désormais un objet `PaginatedList<T>` à la place d’un objet `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="ec28e-220">Les liens d’en-tête de colonne utilisent la chaîne de requête pour transmettre la chaîne de recherche actuelle au contrôleur afin que l’utilisateur puisse trier les résultats de filtrage :</span><span class="sxs-lookup"><span data-stu-id="ec28e-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="ec28e-221">Les boutons de changement de page sont affichés par des Tag Helpers :</span><span class="sxs-lookup"><span data-stu-id="ec28e-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="ec28e-222">Exécutez l’application et accédez à la page des étudiants.</span><span class="sxs-lookup"><span data-stu-id="ec28e-222">Run the app and go to the Students page.</span></span>

![Page d’index des étudiants avec liens de changement de page](sort-filter-page/_static/paging.png)

<span data-ttu-id="ec28e-224">Cliquez sur les liens de changement de page dans différents ordres de tri pour vérifier que le changement de page fonctionne.</span><span class="sxs-lookup"><span data-stu-id="ec28e-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="ec28e-225">Ensuite, entrez une chaîne de recherche et essayez de changer de page à nouveau pour vérifier que le changement de page fonctionne correctement avec le tri et le filtrage.</span><span class="sxs-lookup"><span data-stu-id="ec28e-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="ec28e-226">Créer une page About qui affiche les statistiques des étudiants</span><span class="sxs-lookup"><span data-stu-id="ec28e-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="ec28e-227">Pour la page **About** du site web de Contoso University, vous afficherez le nombre d’étudiants inscrits pour chaque date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="ec28e-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="ec28e-228">Cela nécessite un regroupement et des calculs simples sur les groupes.</span><span class="sxs-lookup"><span data-stu-id="ec28e-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="ec28e-229">Pour ce faire, vous devez effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec28e-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="ec28e-230">Créez une classe de modèle de vue pour les données que vous devez transmettre à la vue.</span><span class="sxs-lookup"><span data-stu-id="ec28e-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="ec28e-231">Modifiez la méthode About dans le contrôleur Home.</span><span class="sxs-lookup"><span data-stu-id="ec28e-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="ec28e-232">Modifiez la vue About.</span><span class="sxs-lookup"><span data-stu-id="ec28e-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="ec28e-233">Créer le modèle de vue</span><span class="sxs-lookup"><span data-stu-id="ec28e-233">Create the view model</span></span>

<span data-ttu-id="ec28e-234">Créez un dossier *SchoolViewModels* dans le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="ec28e-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="ec28e-235">Dans le nouveau dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec28e-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="ec28e-236">Modifier le contrôleur Home</span><span class="sxs-lookup"><span data-stu-id="ec28e-236">Modify the Home Controller</span></span>

<span data-ttu-id="ec28e-237">Dans *HomeController.cs*, ajoutez les instructions using suivantes en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="ec28e-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="ec28e-238">Ajoutez une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe et obtenez une instance du contexte à partir d’ASP.NET Core DI :</span><span class="sxs-lookup"><span data-stu-id="ec28e-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="ec28e-239">Remplacez la méthode `About` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec28e-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="ec28e-240">L’instruction LINQ regroupe les entités Student par date d’inscription, calcule le nombre d’entités dans chaque groupe et stocke les résultats dans une collection d’objets de modèle de vue `EnrollmentDateGroup`.</span><span class="sxs-lookup"><span data-stu-id="ec28e-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE]
> <span data-ttu-id="ec28e-241">Dans la version 1.0 d’Entity Framework Core, le jeu de résultats entier est renvoyé au client et le regroupement est effectué sur le client.</span><span class="sxs-lookup"><span data-stu-id="ec28e-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="ec28e-242">Dans certains scénarios, cela peut générer des problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="ec28e-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="ec28e-243">Veillez à tester les performances avec des volumes de données de production et, si nécessaire, utilisez des requêtes SQL brutes pour effectuer le regroupement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec28e-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="ec28e-244">Pour obtenir des informations sur la façon d’utiliser des requêtes SQL brutes, consultez [le dernier didacticiel de cette série](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="ec28e-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="ec28e-245">Modifier la vue About</span><span class="sxs-lookup"><span data-stu-id="ec28e-245">Modify the About View</span></span>

<span data-ttu-id="ec28e-246">Remplacez le code du fichier *Views/Home/About.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec28e-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="ec28e-247">Exécutez l’application et accédez à la page About.</span><span class="sxs-lookup"><span data-stu-id="ec28e-247">Run the app and go to the About page.</span></span> <span data-ttu-id="ec28e-248">Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.</span><span class="sxs-lookup"><span data-stu-id="ec28e-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Page About](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="ec28e-250">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="ec28e-250">Summary</span></span>

<span data-ttu-id="ec28e-251">Dans ce didacticiel, vous avez vu comment effectuer un tri, un filtrage, un changement de page et un regroupement.</span><span class="sxs-lookup"><span data-stu-id="ec28e-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="ec28e-252">Dans le prochain didacticiel, vous allez apprendre à gérer les modifications du modèle de données à l’aide de migrations.</span><span class="sxs-lookup"><span data-stu-id="ec28e-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="ec28e-253">[Précédent](crud.md)
> [Suivant](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="ec28e-253">[Previous](crud.md)
[Next](migrations.md)</span></span>
