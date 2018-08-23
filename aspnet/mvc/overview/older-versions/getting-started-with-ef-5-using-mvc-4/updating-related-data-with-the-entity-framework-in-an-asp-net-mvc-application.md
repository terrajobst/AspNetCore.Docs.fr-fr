---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Mise à jour des données associées avec Entity Framework dans une Application ASP.NET MVC (6 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ceb311f16575f7aedf0fd789a28e7313daeeaceb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827488"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Mise à jour des données associées avec Entity Framework dans une Application ASP.NET MVC (6 sur 10)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels à partir du début ou [télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et commencez ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [télécharger le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire votre problème. Vous trouverez généralement la solution au problème en comparant votre code pour le code complet. Pour certaines erreurs courantes et comment les résoudre, consultez [erreurs et des solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Dans le didacticiel précédent, vous avez affiché les données associées ; Dans ce didacticiel, vous allez mettre à jour les données associées. Pour la plupart des relations, cela est possible en mettant à jour les champs de clé étrangère appropriées. Pour les relations plusieurs-à-plusieurs, Entity Framework n’expose pas la table de jointure directement, donc vous devez explicitement ajouter et supprimer des entités vers et depuis les propriétés de navigation.

Les illustrations suivantes montrent les pages que vous allez utiliser.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personnaliser les pages Create et Edit pour les cours

Quand une entité Course est créée, elle doit avoir une relation avec un département existant. Pour faciliter cela, le code du modèle généré automatiquement inclut des méthodes de contrôleur, et des vues Create et Edit qui incluent une liste déroulante pour sélectionner le département. Les jeux de liste déroulante la `Course.DepartmentID` une propriété de clé étrangère, et c’est tout Entity Framework a besoin pour charger le `Department` propriété de navigation avec le bon `Department` entité. Vous utilisez le code du modèle généré automatiquement, mais que vous modifiez un peu pour ajouter la gestion des erreurs et trier la liste déroulante.

Dans *CourseController.cs*, supprimez les quatre `Edit` et `Create` méthodes et les remplacer par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Le `PopulateDepartmentsDropDownList` méthode obtient une liste de tous les départements triés par nom, crée un `SelectList` collection pour obtenir la liste déroulante et passe la collection à la vue dans un `ViewBag` propriété. La méthode accepte le paramètre facultatif `selectedDepartment` qui permet au code appelant de spécifier l’élément sélectionné lors de l’affichage de la liste déroulante. La vue passe le nom `DepartmentID` à [le `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), et l’application d’assistance peut alors pour rechercher le `ViewBag` de l’objet pour un `SelectList` nommé `DepartmentID`.

Le `HttpGet` `Create` les appels de méthode le `PopulateDepartmentsDropDownList` méthode sans définir l’élément sélectionné, car pour un nouveau cours le département n’est pas établi encore :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Le `HttpGet` `Edit` méthode définit l’élément sélectionné, selon l’ID du service qui est déjà affecté au cours en cours de modification :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Le `HttpPost` méthodes pour les deux `Create` et `Edit` également inclure du code qui définit l’élément sélectionné quand elles réaffichent la page après une erreur :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ce code garantit que lorsque la page est réaffichée pour montrer le message d’erreur, le département qui a été sélectionné le reste.

Dans *Views\Course\Create.cshtml*, ajoutez le code en surbrillance pour créer un nouveau champ de numéro de cours avant la **titre** champ. Comme expliqué dans un tutoriel précédent, les champs clés primaires ne sont pas généré automatiquement par défaut, mais cette clé primaire est significative, donc vous souhaitez que l’utilisateur à entrer la valeur de clé.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

Dans *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, et *Views\Course\Details.cshtml*, ajoutez un champ de numéro de cours avant la **titre**  champ. Car il s’agit de la clé primaire, il est affiché, mais il ne peut pas être modifié.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Exécutez le **créer** page (afficher la page d’Index des cours et cliquez sur **créer un nouveau**) et entrez les données d’un nouveau cours :

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Cliquez sur **Créer**. La page Index des cours s’affiche avec le nouveau cours ajouté à la liste. Le nom du département dans la liste de la page Index provient de la propriété de navigation, ce qui montre que la relation a été établie correctement.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Exécutez le **modifier** page (afficher la page d’Index des cours et cliquez sur **modifier** sur un cours).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Modifiez les données dans la page et cliquez sur **Save**. La page Index des cours s’affiche avec les données de cours mis à jour.

## <a name="adding-an-edit-page-for-instructors"></a>Ajout d’une Page Edit pour les formateurs

Quand vous modifiez un enregistrement de formateur, vous voulez avoir la possibilité de mettre à jour l’attribution du bureau du formateur. Le `Instructor` entité a une relation un-à-zéro-ou-un avec le `OfficeAssignment` entité, ce qui signifie que vous devez gérer les situations suivantes :

- Si l’utilisateur efface l’attribution de bureau et qu’il possédait initialement une valeur, vous devez supprimer et le `OfficeAssignment` entité.
- Si l’utilisateur entre une valeur d’affectation office et qu’elle était initialement vide, vous devez créer un nouveau `OfficeAssignment` entité.
- Si l’utilisateur modifie la valeur d’une affectation de bureau, vous devez modifier la valeur dans une existante `OfficeAssignment` entité.

Ouvrez *InstructorController.cs* et examinez le `HttpGet` `Edit` méthode :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Le code structuré ici n’est pas ce que vous voulez. Il configure des données pour une liste déroulante, mais vous ce dont vous avez besoin est une zone de texte. Remplacez cette méthode par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ce code supprime le `ViewBag` instruction et ajoute un chargement hâtif pour associé `OfficeAssignment` entité. Vous ne pouvez effectuer un chargement hâtif avec la `Find` (méthode), donc le `Where` et `Single` méthodes sont utilisées à la place pour sélectionner le formateur.

Remplacez le `HttpPost` `Edit` méthode avec le code suivant. qui gère les mises à jour de l’attribution d’office :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Le code effectue les actions suivantes :

- Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`. Il est identique à ce que vous l’avez fait le `HttpGet` `Edit` (méthode).
- Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles. Le [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) vous permet de surcharge utilisée *liste verte* les propriétés que vous souhaitez inclure. Cela empêche la survalidation, comme expliqué dans [le deuxième didacticiel](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Si l’emplacement du bureau est vide, définit le `Instructor.OfficeAssignment` propriété sur null afin que la ligne correspondante dans la `OfficeAssignment` table est supprimée.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Il enregistre les modifications dans la base de données.

Dans *Views\Instructor\Edit.cshtml*, après le `div` éléments pour le **Date d’embauche** champ, ajoutez un nouveau champ pour la modification de l’emplacement du bureau :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Exécution de la page (sélectionnez le **formateurs** onglet, puis cliquez sur **modifier** pour un formateur). Modifiez **Office Location** et cliquez sur **Save**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Ajout des affectations de cours à l’instructeur modifier la Page

Les instructeurs peuvent enseigner dans n’importe quel nombre de cours. Maintenant, vous allez améliorer la page de modification des formateurs en ajoutant la possibilité de modifier les affectations de cours avec un groupe de cases à cocher, comme le montre la capture d’écran suivante :

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relation entre la `Course` et `Instructor` entités est plusieurs-à-plusieurs, ce qui signifie que vous n’avez pas un accès direct à la table de jointure. Au lieu de cela, vous ajouter et supprimer des entités vers et depuis le `Instructor.Courses` propriété de navigation.

L’interface utilisateur qui vous permet de changer les cours auxquels un formateur est affecté est un groupe de cases à cocher. Une case à cocher est affichée pour chaque cours de la base de données, et ceux auxquels le formateur est actuellement affecté sont sélectionnés. L’utilisateur peut cocher ou décocher les cases pour changer les affectations de cours. Si le nombre de cours était beaucoup plus important, vous souhaiterez probablement utiliser une autre méthode de présentation des données dans la vue, mais vous utiliseriez la même méthode de manipulation des propriétés de navigation pour pouvoir créer ou supprimer des relations.

Pour fournir des données à la vue pour la liste de cases à cocher, vous utilisez une classe de modèle de vue. Créer *AssignedCourseData.cs* dans le *ViewModels* dossier et remplacez le code existant par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Dans *InstructorController.cs*, remplacez le `HttpGet` `Edit` méthode avec le code suivant. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Le code ajoute un chargement hâtif pour la propriété de navigation `Courses` et appelle la nouvelle méthode `PopulateAssignedCourseData` pour fournir des informations pour le tableau de cases à cocher avec la classe de modèle de vue `AssignedCourseData`.

Le code dans le `PopulateAssignedCourseData` méthode lit toutes les `Course` entités pour charger une liste de cours à l’aide de la vue de classe de modèle. Pour chaque cours, le code vérifie s’il existe dans la propriété de navigation `Courses` du formateur. Pour créer une recherche efficace lors de la vérification si un cours est affecté au formateur, les cours affectés au formateur sont placés dans un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection. Le `Assigned` propriété est définie sur `true` pour les cours le formateur est affecté. La vue utilise cette propriété pour déterminer quelles cases doivent être affichées cochées. Enfin, la liste est passée à la vue dans un `ViewBag` propriété.

Ensuite, ajoutez le code qui est exécuté quand l’utilisateur clique sur **Save**. Remplacez le `HttpPost` `Edit` méthode avec le code suivant, qui appelle une méthode qui met à jour le `Courses` propriété de navigation de la `Instructor` entité. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Étant donné que la vue n’a pas une collection de `Course` entités, le binder de modèle ne peut pas mettre à jour automatiquement le `Courses` propriété de navigation. Au lieu d’utiliser le binder de modèle pour mettre à jour la propriété de navigation de cours, vous pouvez utiliser dans le nouveau `UpdateInstructorCourses` (méthode). Par conséquent, vous devez exclure la propriété `Courses` de la liaison de modèle. Cela ne nécessite aucune modification au code qui appelle [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , car vous utilisez le *mise en liste verte* surcharge et `Courses` n’est pas dans la liste d’inclusion.

Si aucune case a été cochée, le code dans `UpdateInstructorCourses` initialise le `Courses` propriété de navigation avec une collection vide :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Le code boucle ensuite à travers tous les cours dans la base de données, et vérifie chaque cours par rapport à ceux actuellement affectés au formateur relativement à ceux qui ont été sélectionnés dans la vue. Pour faciliter des recherches efficaces, les deux dernières collections sont stockées dans des objets `HashSet`.

Si la case pour un cours a été cochée mais que le cours n’est pas dans la propriété de navigation `Instructor.Courses`, le cours est ajouté à la collection dans la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Si la case pour un cours a été cochée mais que le cours est dans la propriété de navigation `Instructor.Courses`, le cours est supprimé de la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Dans *Views\Instructor\Edit.cshtml*, ajouter un **cours** champ avec un tableau de cases à cocher en ajoutant le code suivant mis en surbrillance de code immédiatement après le `div` éléments pour le `OfficeAssignment` champ :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Ce code crée un tableau HTML qui a trois colonnes. Dans chaque colonne se trouve une case à cocher, suivie d’une légende qui est constituée du numéro et du titre du cours. Toutes les cases à cocher ont le même nom (« selectedCourses »), qui informe le binder de modèle, ils doivent être traités en tant que groupe. Le `value` attribut de chaque case à cocher est défini sur la valeur de `CourseID.` lorsque la page est publiée, le classeur de modèles passe un tableau au contrôleur qui se compose de la `CourseID` valeurs pour seulement les cases à cocher qui sont sélectionnés.

Quand les cases à cocher sont affichées au départ, celles qui correspondent à des cours affectés au formateur ont `checked` attributs, qui les sélectionnent (ils les affichent cochées).

Après avoir modifié les affectations de cours, vous souhaitez être en mesure de vérifier les modifications lorsque le site revient au `Index` page. Par conséquent, vous devez ajouter une colonne à la table dans cette page. Dans ce cas vous n’avez pas besoin d’utiliser le `ViewBag` de l’objet, car les informations que vous souhaitez afficher se trouve déjà dans le `Courses` propriété de navigation de la `Instructor` entité que vous transmettez à la page que le modèle.

Dans *Views\Instructor\Index.cshtml*, ajoutez un **cours** titre qui suit immédiatement la **Office** titre, comme indiqué dans l’exemple suivant :

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Ajoutez ensuite une nouvelle cellule de détail qui suit immédiatement la cellule de détail d’emplacement office :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Exécutez le **Index des formateurs** page pour afficher les cours affectés à chaque formateur :

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Cliquez sur **modifier** sur un formateur pour voir la page de modification.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Changez certaines affectations de cours et cliquez sur **enregistrer**. Les modifications que vous apportez sont reflétées dans la page Index.

 Remarque : L’approche adoptée pour modifier les données de formation de formateurs fonctionne bien quand il y a un nombre limité de cours. Pour les collections qui sont beaucoup plus volumineuses, une autre interface utilisateur et une autre méthode de mise à jour seraient nécessaires.  
 

## <a name="update-the-delete-method"></a>Mettre à jour de la méthode Delete

Modifier le code dans la méthode HttpPost Delete afin de l’enregistrement d’affectation d’office (le cas échéant) est supprimé lorsque l’instructeur est supprimé :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Si vous essayez de supprimer un formateur qui est affecté à un service en tant qu’administrateur, vous obtiendrez une erreur d’intégrité référentielle. Consultez [la version actuelle de ce didacticiel](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) pour du code supplémentaire qui supprime automatiquement le formateur à partir de n’importe quel service dans lequel le formateur est affecté en tant qu’administrateur.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant terminé cette introduction à l’utilisation des données liées. Jusqu'à présent dans ces didacticiels que vous avez effectué une plage complète des opérations, mais vous n’avez pas traité les problèmes d’accès concurrentiel. Le didacticiel suivant introduire la rubrique de l’accès concurrentiel, expliquent les options pour la traiter et ajouter d’accès concurrentiel pour le code CRUD que vous avez déjà écrit pour un type d’entité de gestion des.

Vous trouverez des liens vers d’autres ressources Entity Framework, à la fin de [le dernier didacticiel de cette série](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Précédent](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
