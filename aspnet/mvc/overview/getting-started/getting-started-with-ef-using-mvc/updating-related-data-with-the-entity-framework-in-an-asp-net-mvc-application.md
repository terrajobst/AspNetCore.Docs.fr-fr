---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Mise à jour des données associées avec Entity Framework dans une Application ASP.NET MVC | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio...
ms.author: aspnetcontent
ms.date: 05/01/2015
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 171314835781ba8cc1edbb0a02db0ac0592cf920
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819680"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Mise à jour des données associées avec Entity Framework dans une Application ASP.NET MVC
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans le didacticiel précédent, vous avez affiché les données associées ; Dans ce didacticiel, vous allez mettre à jour les données associées. Pour la plupart des relations, cela est possible en mettant à jour les champs de clé étrangère ou de propriétés de navigation. Pour les relations plusieurs-à-plusieurs, Entity Framework n’expose pas la table de jointure directement, afin que vous ajoutez et supprimez des entités à partir des propriétés de navigation.

Les illustrations suivantes montrent quelques-unes des pages que vous allez utiliser.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Modification de formateur avec les cours](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personnaliser les pages Create et Edit pour les cours

Quand une entité Course est créée, elle doit avoir une relation avec un département existant. Pour faciliter cela, le code du modèle généré automatiquement inclut des méthodes de contrôleur, et des vues Create et Edit qui incluent une liste déroulante pour sélectionner le département. Les jeux de liste déroulante la `Course.DepartmentID` une propriété de clé étrangère, et c’est tout Entity Framework a besoin pour charger le `Department` propriété de navigation avec le bon `Department` entité. Vous utilisez le code du modèle généré automatiquement, mais que vous modifiez un peu pour ajouter la gestion des erreurs et trier la liste déroulante.

Dans *CourseController.cs*, supprimez les quatre `Create` et `Edit` méthodes et les remplacer par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Ajoutez le code suivant `using` instruction au début du fichier :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Le `PopulateDepartmentsDropDownList` méthode obtient une liste de tous les départements triés par nom, crée un `SelectList` collection pour obtenir la liste déroulante et passe la collection à la vue dans un `ViewBag` propriété. La méthode accepte le paramètre facultatif `selectedDepartment` qui permet au code appelant de spécifier l’élément sélectionné lors de l’affichage de la liste déroulante. La vue passe le nom `DepartmentID` à la [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) d’assistance et le helper peut alors pour rechercher le `ViewBag` de l’objet pour un `SelectList` nommé `DepartmentID`.

Le `HttpGet` `Create` les appels de méthode le `PopulateDepartmentsDropDownList` méthode sans définir l’élément sélectionné, car pour un nouveau cours le département n’est pas établi encore :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Le `HttpGet` `Edit` méthode définit l’élément sélectionné, selon l’ID du service qui est déjà affecté au cours en cours de modification :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Le `HttpPost` méthodes pour les deux `Create` et `Edit` également inclure du code qui définit l’élément sélectionné quand elles réaffichent la page après une erreur :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Ce code garantit que lorsque la page est réaffichée pour montrer le message d’erreur, le département qui a été sélectionné le reste.

Les vues des cours sont déjà structurées avec les listes déroulantes pour le champ department, mais vous ne voulez pas pour ce champ, la légende de DepartmentID. par conséquent, assurez-vous que la mise en surbrillance les éléments suivants à la *Views\Course\Create.cshtml* de fichiers à modifier la légende.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Apporter la même modification dans *Views\Course\Edit.cshtml*.

Normalement, le Générateur de modèles automatique ne structure une clé primaire, car la valeur de clé est générée par la base de données et ne peut pas être modifiée et n’est pas une valeur significative à afficher aux utilisateurs. Pour les entités Course, le Générateur de modèles automatique inclut une zone de texte pour le `CourseID` champ, car il comprend que la `DatabaseGeneratedOption.None` attribut signifie que l’utilisateur doit être en mesure de saisir la valeur de clé primaire. Mais il ne comprend pas, car le nombre est significatif voulez-vous voir dans les autres vues, vous devez l’ajouter manuellement.

Dans *Views\Course\Edit.cshtml*, ajoutez un champ de numéro de cours avant la **titre** champ. Car il s’agit de la clé primaire, il est affiché, mais il ne peut pas être modifié.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Il existe déjà un champ masqué (`Html.HiddenFor` helper) pour le numéro de cours dans la vue Edit. Ajout d’un *Html.LabelFor* helper n’élimine la nécessité pour le champ masqué, car elle n’entraîne pas le numéro de cours à inclure dans les données publiées lorsque l’utilisateur clique sur **enregistrer** sur la page de modification.

Dans *Views\Course\Delete.cshtml* et *Views\Course\Details.cshtml*, modifier la légende de nom de service à partir de « Name » à « Department » et ajoutez un champ de numéro de cours avant la **titre**  champ.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Exécutez le **créer** page (afficher la page d’Index des cours et cliquez sur **créer un nouveau**) et entrez les données d’un nouveau cours :

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Cliquez sur **Créer**. La page Index des cours s’affiche avec le nouveau cours ajouté à la liste. Le nom du département dans la liste de la page Index provient de la propriété de navigation, ce qui montre que la relation a été établie correctement.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Exécutez le **modifier** page (afficher la page d’Index des cours et cliquez sur **modifier** sur un cours).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modifiez les données dans la page et cliquez sur **Save**. La page Index des cours s’affiche avec les données de cours mis à jour.

## <a name="adding-an-edit-page-for-instructors"></a>Ajout d’une Page Edit pour les formateurs

Quand vous modifiez un enregistrement de formateur, vous voulez avoir la possibilité de mettre à jour l’attribution du bureau du formateur. Le `Instructor` entité a une relation un-à-zéro-ou-un avec le `OfficeAssignment` entité, ce qui signifie que vous devez gérer les situations suivantes :

- Si l’utilisateur efface l’attribution de bureau et qu’il possédait initialement une valeur, vous devez supprimer et le `OfficeAssignment` entité.
- Si l’utilisateur entre une valeur d’affectation office et qu’elle était initialement vide, vous devez créer un nouveau `OfficeAssignment` entité.
- Si l’utilisateur modifie la valeur d’une affectation de bureau, vous devez modifier la valeur dans une existante `OfficeAssignment` entité.

Ouvrez *InstructorController.cs* et examinez le `HttpGet` `Edit` méthode :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Le code structuré ici n’est pas ce que vous voulez. Il configure des données pour une liste déroulante, mais vous ce dont vous avez besoin est une zone de texte. Remplacez cette méthode par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Ce code supprime le `ViewBag` instruction et ajoute un chargement hâtif pour associé `OfficeAssignment` entité. Vous ne pouvez effectuer un chargement hâtif avec la `Find` (méthode), donc le `Where` et `Single` méthodes sont utilisées à la place pour sélectionner le formateur.

Remplacez le `HttpPost` `Edit` méthode avec le code suivant. qui gère les mises à jour de l’attribution d’office :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

La référence à `RetryLimitExceededException` nécessite un `using` instruction ; ajouter, avec le bouton droit `RetryLimitExceededException`, puis cliquez sur **résoudre** - **à l’aide de System.Data.Entity.Infrastructure**.

![Résoudre l’exception de nouvelle tentative](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Le code effectue les actions suivantes :

- Modifie le nom de méthode à `EditPost` , car la signature est maintenant le même que le `HttpGet` (méthode) (le `ActionName` attribut spécifie que l’URL /Edit/ est toujours utilisée).
- Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`. Il est identique à ce que vous l’avez fait le `HttpGet` `Edit` (méthode).
- Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles. Le [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) vous permet de surcharge utilisée *liste verte* les propriétés que vous souhaitez inclure. Cela empêche la survalidation, comme expliqué dans [le deuxième didacticiel](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Si l’emplacement du bureau est vide, définit le `Instructor.OfficeAssignment` propriété sur null afin que la ligne correspondante dans la `OfficeAssignment` table est supprimée.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Il enregistre les modifications dans la base de données.

Dans *Views\Instructor\Edit.cshtml*, après le `div` éléments pour le **Date d’embauche** champ, ajoutez un nouveau champ pour la modification de l’emplacement du bureau :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Exécution de la page (sélectionnez le **formateurs** onglet, puis cliquez sur **modifier** pour un formateur). Modifiez **Office Location** et cliquez sur **Save**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Ajout des affectations de cours à l’instructeur modifier la Page

Les instructeurs peuvent enseigner dans n’importe quel nombre de cours. Maintenant, vous allez améliorer la page de modification des formateurs en ajoutant la possibilité de modifier les affectations de cours avec un groupe de cases à cocher, comme le montre la capture d’écran suivante :

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

La relation entre la `Course` et `Instructor` entités est plusieurs-à-plusieurs, ce qui signifie que vous n’avez pas accès direct aux propriétés de clé étrangère qui se trouvent dans la table de jointure. Au lieu de cela, vous ajoutez et supprimez des entités vers et depuis le `Instructor.Courses` propriété de navigation.

L’interface utilisateur qui vous permet de changer les cours auxquels un formateur est affecté est un groupe de cases à cocher. Une case à cocher est affichée pour chaque cours de la base de données, et ceux auxquels le formateur est actuellement affecté sont sélectionnés. L’utilisateur peut cocher ou décocher les cases pour changer les affectations de cours. Si le nombre de cours était beaucoup plus important, vous souhaiterez probablement utiliser une autre méthode de présentation des données dans la vue, mais vous utiliseriez la même méthode de manipulation des propriétés de navigation pour pouvoir créer ou supprimer des relations.

Pour fournir des données à la vue pour la liste de cases à cocher, vous utilisez une classe de modèle de vue. Créer *AssignedCourseData.cs* dans le *ViewModels* dossier et remplacez le code existant par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Dans *InstructorController.cs*, remplacez le `HttpGet` `Edit` méthode avec le code suivant. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Le code ajoute un chargement hâtif pour la propriété de navigation `Courses` et appelle la nouvelle méthode `PopulateAssignedCourseData` pour fournir des informations pour le tableau de cases à cocher avec la classe de modèle de vue `AssignedCourseData`.

Le code dans le `PopulateAssignedCourseData` méthode lit toutes les `Course` entités pour charger une liste de cours à l’aide de la vue de classe de modèle. Pour chaque cours, le code vérifie s’il existe dans la propriété de navigation `Courses` du formateur. Pour créer une recherche efficace lors de la vérification si un cours est affecté au formateur, les cours affectés au formateur sont placés dans un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection. Le `Assigned` propriété est définie sur `true` pour les cours le formateur est affecté. La vue utilise cette propriété pour déterminer quelles cases doivent être affichées cochées. Enfin, la liste est passée à la vue dans un `ViewBag` propriété.

Ensuite, ajoutez le code qui est exécuté quand l’utilisateur clique sur **Save**. Remplacez le `EditPost` méthode avec le code suivant, qui appelle une méthode qui met à jour le `Courses` propriété de navigation de la `Instructor` entité. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La signature de méthode diffère maintenant le `HttpGet` `Edit` méthode, de sorte que si le nom de la méthode change de `EditPost` à `Edit`.

Étant donné que la vue n’a pas une collection de `Course` entités, le binder de modèle ne peut pas mettre à jour automatiquement le `Courses` propriété de navigation. Au lieu d’utiliser le binder de modèle pour mettre à jour le `Courses` propriété de navigation, vous pouvez utiliser dans le nouveau `UpdateInstructorCourses` (méthode). Par conséquent, vous devez exclure la propriété `Courses` de la liaison de modèle. Cela ne nécessite aucune modification au code qui appelle [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , car vous utilisez le *mise en liste verte* surcharge et `Courses` n’est pas dans la liste d’inclusion.

Si aucune case a été cochée, le code dans `UpdateInstructorCourses` initialise le `Courses` propriété de navigation avec une collection vide :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Le code boucle ensuite à travers tous les cours dans la base de données, et vérifie chaque cours par rapport à ceux actuellement affectés au formateur relativement à ceux qui ont été sélectionnés dans la vue. Pour faciliter des recherches efficaces, les deux dernières collections sont stockées dans des objets `HashSet`.

Si la case pour un cours a été cochée mais que le cours n’est pas dans la propriété de navigation `Instructor.Courses`, le cours est ajouté à la collection dans la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Si la case pour un cours a été cochée mais que le cours est dans la propriété de navigation `Instructor.Courses`, le cours est supprimé de la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Dans *Views\Instructor\Edit.cshtml*, ajouter un **cours** champ avec un tableau de cases à cocher en ajoutant le code immédiatement après le `div` éléments pour le `OfficeAssignment` champ et avant du `div` élément pour le **enregistrer** bouton :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Une fois que vous collez le code, si les sauts de ligne et la mise en retrait ne s’affichent pas comme ils le font ici, tout corriger manuellement afin qu’il ressemble à ce que vous voyez ici. L’indentation ne doit pas nécessairement être parfaite, mais les lignes `@</tr><tr>`, `@:<td>`, `@:</td>` et `@</tr>` doivent chacune tenir sur une seule ligne comme dans l’illustration, sinon vous recevrez une erreur d’exécution.

Ce code crée un tableau HTML qui a trois colonnes. Dans chaque colonne se trouve une case à cocher, suivie d’une légende qui est constituée du numéro et du titre du cours. Toutes les cases à cocher ont le même nom (« selectedCourses »), qui informe le binder de modèle, ils doivent être traités en tant que groupe. Le `value` attribut de chaque case à cocher est défini sur la valeur de `CourseID.` lorsque la page est publiée, le classeur de modèles passe un tableau au contrôleur qui se compose de la `CourseID` valeurs pour seulement les cases à cocher qui sont sélectionnés.

Quand les cases à cocher sont affichées au départ, celles qui correspondent à des cours affectés au formateur ont `checked` attributs, qui les sélectionnent (ils les affichent cochées).

Après avoir modifié les affectations de cours, vous souhaitez être en mesure de vérifier les modifications lorsque le site revient au `Index` page. Par conséquent, vous devez ajouter une colonne à la table dans cette page. Dans ce cas vous n’avez pas besoin d’utiliser le `ViewBag` de l’objet, car les informations que vous souhaitez afficher se trouve déjà dans le `Courses` propriété de navigation de la `Instructor` entité que vous transmettez à la page que le modèle.

Dans *Views\Instructor\Index.cshtml*, ajoutez un **cours** titre qui suit immédiatement la **Office** titre, comme indiqué dans l’exemple suivant :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Ajoutez ensuite une nouvelle cellule de détail qui suit immédiatement la cellule de détail d’emplacement office :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Exécutez le **Index des formateurs** page pour afficher les cours affectés à chaque formateur :

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Cliquez sur **modifier** sur un formateur pour voir la page de modification.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Changez certaines affectations de cours et cliquez sur **enregistrer**. Les modifications que vous apportez sont reflétées dans la page Index.

 Remarque : L’approche adoptée ici pour modifier les données de formation de formateurs fonctionne bien quand il y a un nombre limité de cours. Pour les collections qui sont beaucoup plus volumineuses, une autre interface utilisateur et une autre méthode de mise à jour seraient nécessaires.  
 

## <a name="update-the-deleteconfirmed-method"></a>Mettre à jour de la méthode DeleteConfirmed

Dans *InstructorController.cs*, supprimez le `DeleteConfirmed` méthode et insérer le code suivant à la place.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Ce code apporte les modifications suivantes :

- Si le formateur est affecté en tant qu’administrateur de n’importe quel service, supprime l’affectation de l’instructeur de ce département. Sans ce code, vous obtiendrez une erreur d’intégrité référentielle si vous avez tenté de supprimer un formateur qui a été affecté en tant qu’administrateur pour un service.

Ce code ne gère pas le scénario d’un formateur affecté en tant qu’administrateur pour plusieurs départements. Dans le dernier didacticiel vous ajouterez du code qui empêche ce scénario ne se produise.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Ajouter des emplacements de bureau et des cours à la page Create

Dans *InstructorController.cs*, supprimez le `HttpGet` et `HttpPost` `Create` méthodes, puis ajoutez le code suivant à leur place :


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ce code est similaire à ce que vous avez vu pour les méthodes de modification, à ceci près qu’aucun cours ne sont sélectionnées par défaut. Le `HttpGet` `Create` les appels de méthode le `PopulateAssignedCourseData` méthode pas, car il peut y avoir des cours sélectionnés, mais pour pouvoir fournir une collection vide pour le `foreach` boucle dans la vue (sinon le code de la vue lèverait une exception de référence null ).

La méthode HttpPost création ajoute chaque cours sélectionné à la propriété de navigation de cours avant le code du modèle qui recherche les erreurs de validation et ajoute le nouveau formateur à la base de données. Les cours sont ajoutés même s’il existe des erreurs de modèle afin que lorsqu’il existe des erreurs de modèle (par exemple, l’utilisateur tapé une date non valide) afin que lorsque la page est réaffichée avec un message d’erreur, les sélections de cours qui ont été apportées sont automatiquement restaurées.

Notez que pour pouvoir ajouter des cours à la propriété de navigation `Courses`, vous devez initialiser la propriété en tant que collection vide :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Comme alternative à cette opération dans le code du contrôleur, vous pouvez l’effectuer dans le modèle Instructor en modifiant le getter de propriété pour créer automatiquement la collection si elle n’existe pas, comme le montre l’exemple suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Si vous modifiez la propriété `Courses` de cette façon, vous pouvez supprimer le code d’initialisation explicite de la propriété dans le contrôleur.

Dans *Views\Instructor\Create.cshtml*, ajoutez une zone de texte emplacement office et les cases à cocher du cours une fois que le champ de date de la location et avant le **envoyer** bouton.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Une fois que vous collez le code, résoudre des sauts de ligne et de mise en retrait comme vous l’avez fait précédemment pour la page de modification.

Exécutez la page Créer et ajouter un formateur.

![Formateur créer avec les cours](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Gestion des transactions

Comme expliqué dans la [didacticiel de base fonctionnalité CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), par défaut Entity Framework implémente implicitement les transactions. Pour les scénarios où vous avez besoin de plus de contrôle, par exemple, si vous souhaitez inclure des opérations effectuées en dehors d’Entity Framework dans une transaction, consultez [utilisation de Transactions](https://msdn.microsoft.com/data/dn456843) sur MSDN.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant terminé cette introduction à l’utilisation des données liées. Jusqu'à présent dans ces didacticiels, vous avez travaillé avec un code qui effectue des e/s synchrones. Vous pouvez rendre l’application d’utiliser plus efficacement les ressources du serveur web en implémentant le code asynchrone, et c’est ce que vous allez faire dans le didacticiel suivant.

Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vous trouverez des liens vers d’autres ressources Entity Framework dans [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
