---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lecture des données connexes avec Entity Framework dans une Application ASP.NET MVC | Microsoft Docs
author: tdykstra
description: /AJAX/Tutorials/Using-AJAX-Control-Toolkit-Controls-and-Control-Extenders-VB
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4147feda2b78eefa2f5e280e587759585da738b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839137"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Lecture de données associées avec Entity Framework dans une Application ASP.NET MVC
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans le didacticiel précédent vous terminé le modèle de données School. Dans ce didacticiel, vous allez lire et afficher les données associées, autrement dit, les données qu’Entity Framework charge dans les propriétés de navigation.

Les illustrations suivantes montrent les pages que vous allez utiliser.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Chargement différé et hâtif, Explicit des données associées

Il existe plusieurs façons que Entity Framework peut charger des données associées dans les propriétés de navigation d’une entité :

- *Chargement différé*. Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées. Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement. Il en résulte plusieurs requêtes sont envoyées à la base de données : une pour l’entité elle-même et chaque fois que les données pour l’entité associées doivent être récupérées. Le `DbContext` classe permet le chargement différé par défaut. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Chargement hâtif*. Quand l’entité est lue, ses données associées sont également récupérées. Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires. Vous spécifiez un chargement hâtif à l’aide de la `Include` (méthode).

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Chargement explicite*. Cela revient à chargement différé, à ceci près que vous récupériez les données associées dans le code. Il ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Vous chargez manuellement les données associées en obtenant l’entrée de gestionnaire d’état objet pour une entité et l’appel la [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) méthode pour les collections ou les [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) méthode pour les propriétés qui contiennent un entité unique. (Dans l’exemple suivant, si vous souhaitez charger la propriété de navigation administrateur, vous remplacez `Collection(x => x.Courses)` avec `Reference(x => x.Administrator)`.) En règle générale, vous utiliseriez le chargement explicite uniquement lorsque vous avez activé le chargement désactivé différé.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Car ils ne récupèrent pas immédiatement les valeurs de propriété, le chargement différé et le chargement explicite sont également tous deux appelés *le chargement différé*.

### <a name="performance-considerations"></a>Considérations sur les performances

Si vous savez que vous avez besoin des données associées pour toutes les entités récupérées, le chargement hâtif souvent offre des performances optimales, car une seule requête envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée. Par exemple, dans les exemples ci-dessus, supposons que chaque département a dix cours associés. L’exemple de chargement hâtif entraînerait une requête unique (jointure) et un seul aller-retour à la base de données. Le chargement différé et des exemples de chargement explicite à la fois entraînerait onze requêtes et les onze allers-retours à la base de données. Les allers-retours supplémentaires à la base de données sont particulièrement nuisibles pour les performances lorsque la latence est élevée.

En revanche, dans certains scénarios de chargement différé est plus efficace. Le chargement hâtif peut entraîner une jointure très complexe pour être généré, que SQL Server ne peut pas traiter efficacement. Ou, si vous avez besoin accéder aux propriétés de navigation d’une entité uniquement pour un sous-ensemble d’un jeu d’entités que vous traitez, le chargement différé peuvent être plus performantes, car le chargement hâtif permet de récupérer plus de données que vous avez besoin. Si les performances sont essentielles, il est préférable de tester les performances des deux façons afin d’effectuer le meilleur choix.

Le chargement différé peut masquer le code qui provoque des problèmes de performances. Par exemple, le code qui ne spécifie pas le chargement hâtif ou explicit, mais traite un volume élevé d’entités et utilise plusieurs propriétés de navigation dans chaque itération peut être très inefficace (en raison des nombreuses boucles vers la base de données). Une application fonctionnant correctement dans le développement à l’aide d’un serveur SQL local peut avoir des problèmes de performances lors du déplacement vers la base de données SQL Azure en raison de la latence accrue et le chargement différé. Les requêtes de base de données avec une charge de test réaliste de profilage vous aidera à déterminer si le chargement différé est approprié. Pour plus d’informations, consultez [Démystification des stratégies Entity Framework : chargement des données connexes](https://msdn.microsoft.com/magazine/hh205756.aspx) et [à l’aide d’Entity Framework pour réduire la latence du réseau pour SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Désactiver le chargement différé avant la sérialisation

Si vous laissez le chargement différé est activée pendant la sérialisation, vous pouvez vous retrouver interrogation considérablement plus de données que vous aviez l’intention. En règle générale, la sérialisation fonctionne en accédant à chaque propriété sur une instance d’un type. Accès à la propriété déclenche le chargement différé, et ces entités chargées différées sont sérialisées. Le processus de sérialisation accède ensuite à chaque propriété des entités chargées en différé, ce qui peut entraîner davantage le chargement différé et la sérialisation. Pour empêcher cette réaction en chaîne de pertes de contrôle, activez le chargement désactivé avant de vous sérialisez une entité différé.

Sérialisation peut également être entravée par les classes de proxy par Entity Framework, comme expliqué dans la [didacticiel des scénarios avancés](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Pour éviter les problèmes de sérialisation consiste à sérialiser des objets de transfert de données (DTO) au lieu d’objets d’entité, comme indiqué dans le [à l’aide des API Web avec Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) didacticiel.

Si vous n’utilisez pas DTO, vous pouvez désactiver le chargement différé et éviter les problèmes de proxy par [la désactivation de la création de proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Voici un autre [façons de désactiver le chargement différé](https://msdn.microsoft.com/data/jj574232):

- Pour les propriétés de navigation spécifiques, omettez le `virtual` mot clé lorsque vous déclarez la propriété.
- Pour toutes les propriétés de navigation, définissez `LazyLoadingEnabled` à `false`, placez le code suivant dans le constructeur de votre classe de contexte : 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Créer une Page Courses ce nom affiche département

Le `Course` entité inclut une propriété de navigation qui contient le `Department` entité du service auquel le cours est affecté. Pour afficher le nom du département affecté dans une liste de cours, vous devez obtenir le `Name` propriété à partir de la `Department` entité qui se trouve dans le `Course.Department` propriété de navigation.

Créer un contrôleur appelé `CourseController` (pas CoursesController) pour le `Course` type d’entité, en utilisant les options de mêmes pour le **contrôleur MVC 5 avec vues, utilisant Entity Framework** Générateur de modèles automatique qui vous avez fait précédemment pour le `Student` contrôleur, comme indiqué dans l’illustration suivante :

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ouvrez *Controllers\CourseController.cs* et examinez le `Index` méthode :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La génération de modèles automatique a spécifié un chargement hâtif pour la propriété de navigation `Department` à l’aide de la méthode `Include`.

Ouvrez *Views\Course\Index.cshtml* et remplacez le code du modèle par le code suivant. Les modifications apparaissent en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Vous avez apporté les modifications suivantes au code généré automatiquement :

- Modifié le titre de **Index** à **cours**.
- Ajout d’une colonne **Number** qui affiche la valeur de la propriété `CourseID`. Par défaut, les clés primaires ne sont pas générées automatiquement, car il est normalement qu’ils n’ont aucune signification pour les utilisateurs finaux. Toutefois, dans le cas présent, la clé primaire est significative et vous voulez l’afficher.
- Déplacé le **département** colonne vers la droite et modifié son en-tête. Le Générateur de modèles automatique a correctement choisi afficher le `Name` propriété à partir de la `Department` entité, mais ici, dans la page de cours l’en-tête de colonne doit être **département** plutôt que **nom**.

Notez que pour la colonne Department, le code structuré affiche le `Name` propriété de la `Department` entité qui est chargée dans le `Department` propriété de navigation :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Exécution de la page (sélectionnez le **cours** onglet sur la page d’accueil de Contoso University) pour afficher la liste avec les noms de service.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Créer une Page Instructors qui affiche les cours et les inscriptions

Dans cette section, vous allez créer un contrôleur et afficher pour le `Instructor` entité afin d’afficher la page Instructors :

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Cette page lit et affiche les données associées comme suit :

- La liste des formateurs affiche les données associées à partir de la `OfficeAssignment` entité. Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`. Vous allez utiliser un chargement hâtif pour la `OfficeAssignment` entités. Comme expliqué précédemment, le chargement hâtif est généralement plus efficace lorsque vous avez besoin des données associées pour toutes les lignes extraites de la table primaire. Dans ce cas, vous souhaitez afficher les affectations de bureaux pour tous les formateurs affichés.
- Lorsque l’utilisateur sélectionne un formateur, lié `Course` les entités sont affichées. Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`. Vous allez utiliser un chargement hâtif pour la `Course` leur sont associées et les entités `Department` entités. Dans ce cas, le chargement différé peut être plus efficace, car vous avez besoin de cours uniquement pour le formateur sélectionné. Toutefois, cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent elles-mêmes dans des propriétés de navigation.
- Lorsque l’utilisateur sélectionne un cours, les données associées de la `Enrollments` jeu d’entités s’affiche. Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. Vous ajouterez le chargement explicite pour `Enrollment` leur sont associées et les entités `Student` entités. (Chargement explicite n’est pas nécessaire, car le chargement différé est activé, mais cet exemple montre comment effectuer le chargement explicite.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Créer un modèle de vue pour la vue d’Index de l’instructeur

La page Instructors affiche trois tables différentes. Par conséquent, vous allez créer un modèle de vue qui comprend trois propriétés, chacune contenant les données d’une des tables.

Dans le *ViewModels* dossier, créez *InstructorIndexData.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Créer le contrôleur de formateurs et des vues

Créer un `InstructorController` (pas InstructorsController) contrôleur avec actions de lecture/écriture EF comme indiqué dans l’illustration suivante :

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ouvrez *Controllers\InstructorController.cs* et ajoutez un `using` instruction pour la `ViewModels` espace de noms :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Le code généré automatiquement dans le `Index` méthode spécifie un chargement hâtif uniquement pour le `OfficeAssignment` propriété de navigation :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Remplacez le `Index` méthode avec le code suivant à la charge supplémentaire des données liées et placez-le dans le modèle de vue :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La méthode accepte des données de route facultatives (`id`) et un paramètre de chaîne de requête (`courseID`) qui fournissent les valeurs de l’ID du formateur sélectionné et du cours sélectionné et passe toutes les données nécessaires à la vue. Ces paramètres sont fournis par les liens hypertexte **Select** dans la page.

Le code commence par créer une instance du modèle de vue et la placer dans la liste des formateurs. Le code spécifie un chargement hâtif pour la `Instructor.OfficeAssignment` et `Instructor.Courses` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

La seconde `Include` méthode charge cours et pour chaque cours est chargé, il effectue un chargement hâtif la `Course.Department` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Comme mentionné précédemment, le chargement hâtif n’est pas obligatoire, mais vise à améliorer les performances. Étant donné que la vue nécessite toujours le `OfficeAssignment` entité, il est plus efficace de l’extraire dans la même requête. `Course` les entités sont requises quand un formateur est sélectionné dans la page web, par conséquent, le chargement hâtif est mieux que le chargement différé uniquement si la page s’affiche plus souvent avec un cours sélectionné que sans.

Si un ID de formateur a été sélectionné, le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle de vue. Le modèle de vue `Courses` propriété est alors chargée avec le `Course` entités à partir de ce formateur `Courses` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Le `Where` méthode retourne une collection, mais dans ce cas les critères transmis à cette méthode entraînent une seule `Instructor` entité renvoyée. Le `Single` méthode convertit la collection en un seul `Instructor` entité, qui vous permet d’accéder à cette entité `Courses` propriété.

Vous utilisez le [unique](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) méthode sur une collection lorsque vous savez que la collection possède un seul élément. Le `Single` méthode lève une exception si la collection transmise est vide ou s’il existe plusieurs éléments. Une alternative consiste [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), qui retourne une valeur par défaut (`null` dans ce cas) si la collection est vide. Toutefois, dans ce cas qui entraînerait encore une exception (en tentant de trouver un `Courses` propriété sur un `null` référence), et le message d’exception indique moins clairement la cause du problème. Lorsque vous appelez le `Single` (méthode), vous pouvez également transmettre le `Where` condition au lieu d’appeler le `Where` méthode séparément :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

À la place de :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Ensuite, si un cours a été sélectionné, le cours sélectionné est récupéré à partir de la liste des cours dans le modèle de vue. Du puis le modèle de vue `Enrollments` propriété est chargée avec le `Enrollment` entités à partir de ce cours `Enrollments` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modifier la vue d’Index de l’instructeur

Dans *Views\Instructor\Index.cshtml*, remplacez le code du modèle par le code suivant. Les modifications apparaissent en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Vous avez apporté les modifications suivantes au code existant :

- Vous avez changé la classe de modèle en `InstructorIndexData`.
- Vous avez changé le titre de la page en remplaçant **Index** par **Instructors**.
- Ajouter un **Office** colonne affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas null. (Il s’agit d’une relation un-à-zéro-ou-un, il peut ne pas être un connexes `OfficeAssignment` entity.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Ajouté un code qui ajoute dynamiquement `class="success"` à la `tr` élément du formateur sélectionné. Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide un [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) classe. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Ajouté une nouvelle `ActionLink` étiqueté **sélectionnez** immédiatement avant les autres liens dans chaque ligne, ce qui entraîne l’ID du formateur sélectionné à envoyer à la `Index` (méthode).

Exécutez l’application et sélectionnez le **formateurs** onglet. La page affiche le `Location` propriété de connexes `OfficeAssignment` entités et une table vide de cellule quand il est non lié `OfficeAssignment` entité.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Dans le *Views\Instructor\Index.cshtml* fichier, après la fermeture `table` élément (à la fin du fichier), ajoutez le code suivant. Ce code affiche la liste des cours associés à un formateur quand un formateur est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Ce code lit la propriété `Courses` du modèle de vue pour afficher la liste des cours. Il fournit également un `Select` lien hypertexte qui envoie l’ID du cours sélectionné à la `Index` méthode d’action.

Exécutez la page et sélectionnez un formateur. Vous voyez à présent une grille qui affiche les cours affectés au formateur sélectionné et, pour chaque cours, vous voyez le nom du département affecté.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Après le bloc de code que vous venez d’ajouter, ajoutez le code suivant. Ceci affiche la liste des étudiants qui sont inscrits à un cours quand ce cours est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ce code lit la `Enrollments` propriété du modèle de vue pour afficher une liste d’étudiants inscrits dans le cours.

Exécutez la page et sélectionnez un formateur. Ensuite, sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Ajout de chargement explicite

Ouvrez *InstructorController.cs* et observez comment la `Index` méthode obtient la liste des inscriptions pour un cours sélectionné :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Lorsque vous avez extrait la liste des formateurs, que vous avez spécifié un chargement hâtif pour la `Courses` propriété de navigation et pour le `Department` propriété de chaque cours. Vous placez le `Courses` collection dans le modèle de vue, et maintenant vous accédez à la `Enrollments` propriété de navigation d’une entité de la collection. Étant donné que vous n’avez pas spécifié un chargement hâtif pour la `Course.Enrollments` propriété de navigation, les données à partir de cette propriété s’affiche dans la page à la suite de chargement différé.

Si vous avez désactivé le chargement différé sans modifier le code de quelque autre manière, le `Enrollments` propriété aura la valeur null, quel que soit les inscriptions combien le cours avait réellement. Dans ce cas, pour charger le `Enrollments` propriété, vous devez spécifier un chargement hâtif ou le chargement explicite. Vous avez déjà vu comment effectuer un chargement hâtif. Pour voir un exemple de chargement explicite, remplacez le `Index` méthode avec le code suivant, qui charge explicitement le `Enrollments` propriété. Le code modifié sont mises en surbrillance.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Après avoir obtenu le texte sélectionné `Course` entité, le nouveau code charge explicitement de ce cours `Enrollments` propriété de navigation :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Ensuite, il charge explicitement chaque `Enrollment` relative à entity `Student` entité :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Notez que vous utilisez le `Collection` méthode pour charger une propriété de collection, mais pour une propriété contenant une seule entité, vous utilisez le `Reference` (méthode).

Exécuter maintenant de la page d’Index des formateurs et vous ne verrez aucune différence dans ce qui est affiché dans la page, même si vous avez modifié la façon dont les données sont récupérées.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant utilisé les trois façons (différés hâtif et explicites) pour charger des données connexes dans les propriétés de navigation. Dans le didacticiel suivant, vous apprendrez à mettre à jour les données associées.

Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Suivant](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
