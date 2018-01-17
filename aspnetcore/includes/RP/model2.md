<span data-ttu-id="8e749-101">Ajoutez les propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="8e749-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="8e749-102">Le champ `ID` est requis par la base de données pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="8e749-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="8e749-103">Ajouter une classe de contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="8e749-103">Add a database context class</span></span>

<span data-ttu-id="8e749-104">Ajoutez la classe dérivée `DbContext` suivante nommée *MovieContext.cs* au dossier *Models* : [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="8e749-104">Add the following `DbContext` derived class named *MovieContext.cs* to the *Models* folder: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="8e749-105">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="8e749-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="8e749-106">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="8e749-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
