---
title: "Pages Razor avec EF Core - Lecture des données associées - 6 sur 8"
author: rick-anderson
description: "Dans ce didacticiel vous lisez et affichez les données associées, autrement dit, les données qu'Entity Framework charge dans les propriétés de navigation."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Lecture des données associées - EF Core avec les Pages Razor (6 sur 8)

Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Dans ce didacticiel, les données associées sont lues et affichées. Les données associées sont des données qu'EF Core charge dans les propriétés de navigation.

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez l'[application terminée pour cette étape](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

Les illustrations suivantes montrent les pages terminées pour ce didacticiel :

![Page d’Index de cours](read-related-data/_static/courses-index.png)

![Page d’Index instructeurs](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Chargement hâtif, explicit et différé de données connexes

Il existe plusieurs façons que EF Core chargez des données connexes dans les propriétés de navigation d’une entité :

* [Chargement hâtif (eager loading)](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Le chargement hâtif se produit lorsqu’une requête pour un type d’entité charge également des entités associées. Lors de la lecture de l’entité, ses données associées sont récupérées. Cela entraîne généralement une requête de jointure unique qui extrait toutes les données nécessaires. EF Core émet plusieurs requêtes pour certains types de chargement hâtif. L'émission de requêtes multiples peut être plus efficace que ça ne l'était pour certaines requêtes de EF6 où il avait une seule requête. Le chargement hâtif est spécifié avec les méthodes `Include` et `ThenInclude`.

 ![Exemple de chargement hâtif](read-related-data/_static/eager-loading.png)
 
 Le chargement hâtif envoie plusieurs requêtes lorsqu’une collection de nvavigation est incluse :

 * Une requête pour la requête principale 
 * Une requête pour chaque collection "en marge" (edge) dans l’arborescence de l'arborescence de chargement.

* Séparer les requêtes avec `Load`: les données peuvent être récupérées dans des requêtes distinctes et EF Core "corrige" les propriétés de navigation. "corrige" signifie que EF Core remplit automatiquement les propriétés de navigation. Séparer les requêtes avec `Load` est un chargement plus explicite que le chargement hâtif.

 ![Exemple de requêtes distinctes](read-related-data/_static/separate-queries.png)

 Remarque : EF Core résout automatiquement les propriétés de navigation pour toutes les autres entités qui ont été précédemment chargées dans l’instance de contexte. Même si les données pour une propriété de navigation ne seront *pas* explicitement inclus, la propriété peut toujours être remplie si certaines ou toutes les entités connexes ont été précédemment chargées.

* [Chargement explicite](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Lorsque l’entité est lue la première fois, les données associées ne sont pas récupérées. Le code doit être écrit pour récupérer les données associées lorsque cela est nécessaire. Le chargement explicite avec des requêtes distinctes entraîne que plusieurs requêtes sont envoyées à la base de données. Avec le chargement explicite, le code spécifie que les propriétés de navigation doivent être chargées. Utilisez la méthode `Load` pour effectuer le chargement explicite. Exemple :

 ![Exemple de chargement explicite](read-related-data/_static/explicit-loading.png)

* [Chargement différé](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [EF Core ne prend pas actuellement en charge le chargement différé](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Lorsque l’entité est lue la première fois, les données associées ne sont pas récupérées. La première fois qu’une propriété de navigation est accessible, les données requises pour cette propriété de navigation sont automatiquement récupérées. Une requête est envoyée à la base de données chaque fois qu’une propriété de navigation est accessible pour la première fois.

* L'opérateur `Select` charge uniquement les données connexes nécessaires.

## <a name="create-a-courses-page-that-displays-department-name"></a>Créer une page de cours qui affiche le nom du service

L’entité de cours inclut une propriété de navigation qui contient l'entité `Department`. L'entité `Department` contient le service auquel appartient le cours.

Pour afficher le nom du service affecté dans une liste de cours :

* Obtenez la propriété `Name` à partir de la `Department` entité.
* Le `Department` provient de la propriété de navigation `Course.Department` de l’entité.

![ourse. Service](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Echafaudage (scaffolding) du modèle de cours

* Quittez Visual Studio.
* Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).
* Exécutez la commande suivante :

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

Les structures de commande précédent le modèle `Course`. Ouvrez le projet dans Visual Studio.

Générez le projet. La build génère des erreurs comme suit :

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Modifiez globalement `_context.Course` en `_context.Courses` (autrement dit, ajoutez un « s » à `Course`). 7 occurrences sont trouvées et mises à jour.

Ouvrez *Pages/Courses/Index.cshtml.cs* et examinez la méthode `OnGetAsync`. Le moteur de génération de modèles automatique a spécifié un chargement hâtif pour la propriété de navigation `Department`. La méthode `Include` spécifie un chargement hâtif.

Exécutez l’application et sélectionnez le lien **Courses**. La colonne Service affiche le `DepartmentID`, ce qui n’est pas utile.

Mettez à jour la méthode `OnGetAsync` avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Le code précédent ajoute `AsNoTracking`. `AsNoTracking` améliore les performances, car les entités retournées ne sont pas suivies. Les entités ne sont pas suivies, car ils ne sont pas mises à jour dans le contexte actuel.

Mettez à jour *Views/Courses/Index.cshtml* avec le balisage mis en surbrillance suivant :

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Les modifications suivantes ont été apportées au code de modèle généré automatiquement :

* Modifiez le titre d’Index en Courses.
* Ajoutez une colonne **Number** qui affiche la valeur de propriété `CourseID`. Par défaut, les clés primaires ne sont pas générées, car elles n'ont normalement pas de sens pour les utilisateurs finaux. Toutefois, dans ce cas la clé primaire est significative.
* Modifié la colonne **Department** pour afficher le nom du service. Le code affiche la propriété`Name` de l'entité `Department` qui est chargée dans la propriété de navigation `Department` :

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Exécutez l’application et sélectionnez l'onglet **Courses** pour afficher la liste avec les noms de service.

![Page d’Index de cours](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Chargement de données associées avec Select

La méthode `OnGetAsync` charge les données associées avec la méthode `Include` :

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

L'opérateur `Select` charge uniquement les données connexes nécessaires. Pour les éléments uniques, comme le `Department.Name` qui utilise une jointure interne SQL. Pour les collectionns, il utilise un autre accès en base de données, mais le opérateur `Include` sur des collections fait de même.

Le code suivant charge les données associées avec la méthode `Select` :

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

Le `CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Consultez [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) et [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) pour obtenir un exemple complet.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Créer une page d'instructeurs qui affiche des cours et des inscriptions

Dans cette section, la page Instructeos est créée.

<a name="IP"></a>
![Page d’Index instructeurs](read-related-data/_static/instructors-index.png)

Cette page lit et affiche les données connexes comme suit :

* La liste des instructeurs affiche les données connexes à partir de l'entité `OfficeAssignment` (Office dans l’image précédente). `Instructor` et `OfficeAssignment` sont des entités dans une relation un-à-zéro-ou-un. Le chargement hâtif est utilisé pour les entités `OfficeAssignment`. Le chargement hâtif est généralement plus efficace lorsque les données connexes doivent être affichée. Dans ce cas, les affectations de bureau pour les formateurs sont affichées.
* Lorsque l’utilisateur sélectionne un formateur (Harui dans l’image précédente), les entités `Course` associées sont affichées. `Instructor` et `Course` sont des entités dans une relation plusieurs-à-plusieurs. Le chargement hâtif est utilisé pour leurs entités `Course` associées et les entités `Department`. Dans ce cas, les requêtes distinctes peuvent être plus efficaces, car seuls les cours pour l’enseignant sélectionné sont nécessaires. Cet exemple montre comment utiliser un chargement hâtif pour les propriétés de navigation dans les entités qui se trouvent dans les propriétés de navigation.
* Lorsque l’utilisateur sélectionne un cours (chimie dans l’image précédente), les données associées à partir de l'entité `Enrollments` sont affichées. Dans l’image précédente, le niveau  et étudiant s’affichent. Le `Course` et `Enrollment` sont des entités dans une relation un-à-plusieurs.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Créer un modèle de vue pour la vue d’Index Instructor

La page de formateurs affiche les données de trois tables différentes. Un modèle de vue est créé qui inclut les trois entités représentant les trois tables.

Dans le dossier *SchoolViewModels*, créez *InstructorIndexData.cs* avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Structure du modèle de formateur

* Quittez Visual Studio.
* Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).
* Exécutez la commande suivante :

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

Les structures de commande précédent le modèle `Instructor`. Ouvrez le projet dans Visual Studio.

Générez le projet. La build génère des erreurs.

Modifier globalement `_context.Instructor` en `_context.Instructors` (autrement dit, ajoutez un « s » à `Instructor`). 7 occurrences sont trouvées et mises à jour.

Exécuter l’application et accédez à la page des instructeurs.

Remplacez *Pages/Instructors/Index.cshtml.cs* avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

La méthode `OnGetAsync` accepte les données de routing facultatif pour l’ID de l’enseignant sélectionné.

Examinez la requête sur la page *Pages/Instructors/Index.cshtml* :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

La requête comporte deux includes :

* `OfficeAssignment`: Affichée dans la [vue instructeurs](#IP).
* `CourseAssignments`: Ce qui permet de bénéficier des cours dispensés.


### <a name="update-the-instructors-index-page"></a>Mise à jour de la page d’Index instructeurs

Mettez à jour *Pages/Instructors/Index.cshtml* par le balisage suivant :

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Le balisage précédent apporte les modifications suivantes :

* Met à jour la directive `page` de `@page` en `@page "{id:int?}"`. `"{id:int?}"`est un modèle de routing. Le modèle de routing modifie les chaînes de requête entière dans l’URL pour les données de routing. Par exemple, en cliquant sur le lien **Select** d'un instructeur lorsque la directive de page génère une URL comme suit :

    `http://localhost:1234/Instructors?id=2`

    Lorsque la directive de page est `@page "{id:int?}"`, l’URL précédente est :

    `http://localhost:1234/Instructors/2`

* Le titre de la page est **Instructors**.
* Ajoute une colonne **Office** qui affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas null. Il s’agit d’une relation un-à-zéro-ou-un, il peut ne pas y avoir d'entité OfficeAssignment associée.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Ajouter une colonne **Course** qui affiche les cours dispensés par chaque instructeur. Consultez [Transition de ligne explicite avec `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) pour plus d’informations sur cette syntaxe razor.

* Ajouter le code qui ajoute dynamiquement `class="success"` à l'élément `tr` de l’enseignant sélectionné. Cela définit la couleur d’arrière-plan de la ligne sélectionnée à l’aide d’une classe Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Ajouter un nouveau lien hypertexte avec le label **Select**. Ce lien envoie l’ID de l'instructeur sélectionné à la méthode `Index` et définit une couleur d’arrière-plan.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Exécutez l’application et sélectionnez l'onglet **Instructors**. La page affiche la `Location` de l'entité `OfficeAssignment` associée. Si OfficeAssignment' est null, une cellule de table vide s’affiche.

![Page d’Index instructeurs qu'aucun élément sélectionné](read-related-data/_static/instructors-index-no-selection.png)

Cliquez sur le lien **Select**. Le style des lignes changent.

### <a name="add-courses-taught-by-selected-instructor"></a>Ajouter les cours dispensés par un insgtructeur sélectionné

Mettez à jour la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

Examinez la requête de mise à jour :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

La requête précédente ajoute les entités `Department`.

Le code suivant s’exécute lorsqu’un instructeur est sélectionné (`id != null`). Le formateur sélectionné est récupéré à partir de la liste des formateurs dans le view model. La propriété `Courses` du view model est chargée avec les entités `Course` à partir de la propriété de navigation `CourseAssignments` de cet instructeur.

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

La méthode `Where` retourne une collection. Dans la précédente méthode `Where`, un seul `Instructor` est retourné. La méthode `Single` convertit la collection en une seule entité `Instructor`. L'entité `Instructor` fournit l’accès à la propriété `CourseAssignments`. `CourseAssignments` fournit l’accès aux entités `Course`.

![Formateur à cours m:M](complex-data-model/_static/courseassignment.png)

La méthode `Single` est utilisée sur une collection lorsque la collection comporte un seul élément. La méthode `Single` lève une exception si la collection est vide ou s’il existe plusieurs éléments. Une alternative est `SingleOrDefault`, qui retourne une valeur par défaut (null dans ce cas) si la collection est vide. En utilisant `SingleOrDefault` sur une collection vide :

* Une exception se produit (tente de trouver une propriété `Courses` sur une référence null).
* Le message d’exception indiquerait moins clairement la cause du problème.

Le code suivant remplit la propriété du modèle d’affichage `Enrollments` lorsqu’un cours est sélectionné :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Ajoutez le balisage suivant à la fin de la Page Razor *Pages/Courses/Index.cshtml* :

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

La balise précédente affiche une liste de cours associées à un instructeur lorsqu’un instructeur est sélectionné.

Tester l’application. Cliquez sur un lien **Select** dans la page des instructeur.

![Formateur de page d’Index instructeurs sélectionné](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Afficher les données de l’étudiant

Dans cette section, l’application est mise à jour pour afficher les données de l’étudiant pour le cours sélectionné.

Mettre à jour de la requête dans la méthode `OnGetAsync` dans *Pages/Instructors/Index.cshtml.cs* avec le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Mettez à jour *Pages/Instructors/Index.cshtml*. Ajoutez le balisage suivant à la fin du fichier :

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

La balise précédente affiche une liste des étudiants qui sont inscrits dans le cours sélectionné.

Actualisez la page et sélectionner un formateur. Sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs catégories.

![Formateur de page d’Index instructeurs et cours sélectionné](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>À l’aide d’unique

La méthode `Single` peut passer la condition `Where` au lieu d’appeler la méthode `Where séparément :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

L’approche précédente `Single` ne présente aucun avantage principal à utiliser `Where`. Certains développeurs préfèrent le style d'approche `Single`.

## <a name="explicit-loading"></a>Chargement explicite

Le code actuel spécifie un chargement hâtif pour `Enrollments` et `Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Supposons que les utilisateurs souhaitent rarement voir les inscriptions dans un cours. Dans ce cas, une optimisation serait de charger uniquement les données d’inscription si elle est demandée. Dans cette section, le `OnGetAsync` est mis à jour pour utiliser le chargement explicite de `Enrollments` et `Students`.

Mettez à jour le `OnGetAsync` par le code suivant :

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Le code précédent supprime les appels de méthode *ThenInclude* pour les données d’inscription et les étudiants. Si un cours est sélectionné, le code en surbrillance récupère :

* Les entités`Enrollment` pour le cours sélectionné.
* Les entités `Student` de chaque `Enrollment`.

Notez que le code précédent décommente `.AsNoTracking()`. Les propriétés de navigation peuvent uniquement être chargées explicitement pour les entités de suivi.

Tester l’application. Du point de vue des utilisateurs, l’application se comporte comme la version précédente.

Le didacticiel suivant montre comment mettre à jour les données associées.

>[!div class="step-by-step"]
>[Précédent](xref:data/ef-rp/complex-data-model)
>[Suivant](xref:data/ef-rp/update-related-data)
