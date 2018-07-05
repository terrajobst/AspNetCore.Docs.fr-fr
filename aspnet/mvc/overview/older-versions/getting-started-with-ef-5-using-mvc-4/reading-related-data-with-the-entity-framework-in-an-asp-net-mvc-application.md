---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lecture des données connexes avec Entity Framework dans une Application ASP.NET MVC (5 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 6fa85f561c85d1761ddcbc7167aa4b39a6c6eda3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374323"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lecture de données associées avec Entity Framework dans une Application ASP.NET MVC (5 sur 10)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels à partir du début ou [télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et commencez ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [télécharger le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire votre problème. Vous trouverez généralement la solution au problème en comparant votre code pour le code complet. Pour certaines erreurs courantes et comment les résoudre, consultez [erreurs et des solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Dans le didacticiel précédent vous terminé le modèle de données School. Dans ce didacticiel, vous allez lire et afficher les données associées, autrement dit, les données qu’Entity Framework charge dans les propriétés de navigation.

Les illustrations suivantes montrent les pages que vous allez utiliser.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Chargement différé et hâtif, Explicit des données associées

Il existe plusieurs façons que Entity Framework peut charger des données associées dans les propriétés de navigation d’une entité :

- *Chargement différé*. Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées. Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement. Il en résulte plusieurs requêtes sont envoyées à la base de données : une pour l’entité elle-même et chaque fois que les données pour l’entité associées doivent être récupérées. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Chargement hâtif*. Quand l’entité est lue, ses données associées sont également récupérées. Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires. Vous spécifiez un chargement hâtif à l’aide de la `Include` (méthode).

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Chargement explicite*. Cela revient à chargement différé, à ceci près que vous récupériez les données associées dans le code. Il ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Vous chargez manuellement les données associées en obtenant l’entrée de gestionnaire d’état objet pour une entité et en appelant le `Collection.Load` la méthode des collections ou les `Reference.Load` méthode pour les propriétés qui contiennent une seule entité. (Dans l’exemple suivant, si vous souhaitez charger la propriété de navigation administrateur, vous remplacez `Collection(x => x.Courses)` avec `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Car ils ne récupèrent pas immédiatement les valeurs de propriété, le chargement différé et le chargement explicite sont également tous deux appelés *le chargement différé*.

En règle générale, si vous savez que vous devez les données associées pour chaque entité récupéré, hâtif chargement offre les meilleures performances, car une requête unique envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée. Par exemple, dans les exemples ci-dessus, supposons que chaque département a dix cours associés. L’exemple de chargement hâtif entraînerait une requête unique (jointure) et un seul aller-retour à la base de données. Le chargement différé et des exemples de chargement explicite à la fois entraînerait onze requêtes et les onze allers-retours à la base de données. Les allers-retours supplémentaires à la base de données sont particulièrement nuisibles pour les performances lorsque la latence est élevée.

En revanche, dans certains scénarios de chargement différé est plus efficace. Le chargement hâtif peut entraîner une jointure très complexe pour être généré, que SQL Server ne peut pas traiter efficacement. Ou, si vous avez besoin accéder aux propriétés de navigation d’une entité uniquement pour un sous-ensemble d’un jeu d’entités que vous traitez, le chargement différé peuvent être plus performantes, car le chargement hâtif permet de récupérer plus de données que vous avez besoin. Si les performances sont essentielles, il est préférable de tester les performances des deux façons afin d’effectuer le meilleur choix.

En règle générale, vous utiliseriez le chargement explicite uniquement lorsque vous avez activé le chargement désactivé différé. Un scénario lorsque vous devez activer le chargement désactivé différé est pendant la sérialisation. Sérialisation et le chargement différé ne mélangez pas correctement, et si vous ne faites pas attention, que vous risquez d’interrogation considérablement plus de données que vous aviez l’intention quand il y a différé le chargement est activé. En règle générale, la sérialisation fonctionne en accédant à chaque propriété sur une instance d’un type. Accès à la propriété déclenche le chargement différé, et ces entités chargées différées sont sérialisées. Le processus de sérialisation accède ensuite à chaque propriété des entités chargées en différé, ce qui peut entraîner davantage le chargement différé et la sérialisation. Pour empêcher cette réaction en chaîne de pertes de contrôle, activez le chargement désactivé avant de vous sérialisez une entité différé.

La classe de contexte de base de données effectue le chargement différé par défaut. Il existe deux façons de désactiver le chargement différé :

- Pour les propriétés de navigation spécifiques, omettez le `virtual` mot clé lorsque vous déclarez la propriété.
- Pour toutes les propriétés de navigation, définissez `LazyLoadingEnabled` à `false`. Par exemple, vous pouvez placer le code suivant dans le constructeur de votre classe de contexte : 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Le chargement différé peut masquer le code qui provoque des problèmes de performances. Par exemple, le code qui ne spécifie pas le chargement hâtif ou explicit, mais traite un volume élevé d’entités et utilise plusieurs propriétés de navigation dans chaque itération peut être très inefficace (en raison des nombreuses boucles vers la base de données). Une application fonctionnant correctement dans le développement à l’aide d’un serveur SQL local peut avoir des problèmes de performances lors du déplacement vers la base de données SQL Azure en raison de la latence accrue et le chargement différé. Les requêtes de base de données avec une charge de test réaliste de profilage vous aidera à déterminer si le chargement différé est approprié. Pour plus d’informations, consultez [Démystification des stratégies Entity Framework : chargement des données connexes](https://msdn.microsoft.com/magazine/hh205756.aspx) et [à l’aide d’Entity Framework pour réduire la latence du réseau pour SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Créer une Page d’Index des cours de ce nom de service de l’affiche

Le `Course` entité inclut une propriété de navigation qui contient le `Department` entité du service auquel le cours est affecté. Pour afficher le nom du département affecté dans une liste de cours, vous devez obtenir le `Name` propriété à partir de la `Department` entité qui se trouve dans le `Course.Department` propriété de navigation.

Créer un contrôleur appelé `CourseController` pour le `Course` type d’entité, à l’aide des mêmes options que vous avez fait précédemment pour le `Student` contrôleur, comme indiqué dans l’illustration suivante (sauf à la différence de l’image, votre classe de contexte est dans l’espace de noms de la couche DAL pas le modèles espace de noms) :

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Ouvrez *Controllers\CourseController.cs* et examinez le `Index` méthode :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La génération de modèles automatique a spécifié un chargement hâtif pour la propriété de navigation `Department` à l’aide de la méthode `Include`.

Ouvrez *Views\Course\Index.cshtml* et remplacez le code existant par le code suivant. Les modifications apparaissent en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Vous avez apporté les modifications suivantes au code généré automatiquement :

- Modifié le titre de **Index** à **cours**.
- Déplacer les liens de la ligne vers la gauche.
- Ajouter une colonne sous le titre **nombre** qui affiche le `CourseID` valeur de propriété. (Par défaut, les clés primaires ne sont pas générées automatiquement, car ils sont normalement pas de sens pour les utilisateurs finaux. Toutefois, dans ce cas, la clé primaire est significative et vous voulez l’afficher.)
- Modifié le dernier en-tête de colonne à partir de **DepartmentID** (le nom de la clé étrangère à la `Department` entité) à **département**.

Notez que pour la dernière colonne, le code structuré affiche le `Name` propriété de la `Department` entité qui est chargée dans le `Department` propriété de navigation :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Exécution de la page (sélectionnez le **cours** onglet sur la page d’accueil de Contoso University) pour afficher la liste avec les noms de service.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Créer une Page d’Index Instructors qui affiche les cours et les inscriptions

Dans cette section, vous allez créer un contrôleur et afficher pour le `Instructor` entité afin d’afficher la page d’Index des formateurs :

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Cette page lit et affiche les données associées comme suit :

- La liste des formateurs affiche les données associées à partir de la `OfficeAssignment` entité. Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`. Vous allez utiliser un chargement hâtif pour la `OfficeAssignment` entités. Comme expliqué précédemment, le chargement hâtif est généralement plus efficace lorsque vous avez besoin des données associées pour toutes les lignes extraites de la table primaire. Dans ce cas, vous souhaitez afficher les affectations de bureaux pour tous les formateurs affichés.
- Lorsque l’utilisateur sélectionne un formateur, lié `Course` les entités sont affichées. Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`. Vous allez utiliser un chargement hâtif pour la `Course` leur sont associées et les entités `Department` entités. Dans ce cas, le chargement différé peut être plus efficace, car vous avez besoin de cours uniquement pour le formateur sélectionné. Toutefois, cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent elles-mêmes dans des propriétés de navigation.
- Lorsque l’utilisateur sélectionne un cours, les données associées de la `Enrollments` jeu d’entités s’affiche. Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. Vous ajouterez le chargement explicite pour `Enrollment` leur sont associées et les entités `Student` entités. (Chargement explicite n’est pas nécessaire, car le chargement différé est activé, mais cet exemple montre comment effectuer le chargement explicite.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Créer un modèle de vue pour la vue d’Index de l’instructeur

La page Index des formateurs affiche les trois tables différentes. Par conséquent, vous allez créer un modèle de vue qui comprend trois propriétés, chacune contenant les données d’une des tables.

Dans le *ViewModels* dossier, créez *InstructorIndexData.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Ajout d’un Style pour les lignes sélectionnées

Pour marquer les lignes sélectionnées, vous avez besoin d’une couleur d’arrière-plan différente. Pour fournir un style pour cette interface utilisateur, ajoutez le code en surbrillance suivant à la section `/* info and errors */` dans *Content\Site.css*, comme illustré ci-dessous :

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Création du contrôleur de formateurs et de vues

Créer un `InstructorController` contrôleur comme indiqué dans l’illustration suivante :

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Ouvrez *Controllers\InstructorController.cs* et ajoutez un `using` instruction pour la `ViewModels` espace de noms :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Le code généré automatiquement dans le `Index` méthode spécifie un chargement hâtif uniquement pour le `OfficeAssignment` propriété de navigation :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Remplacez le `Index` méthode avec le code suivant à la charge supplémentaire des données liées et placez-le dans le modèle de vue :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

La méthode accepte des données de route facultatives (`id`) et un paramètre de chaîne de requête (`courseID`) qui fournissent les valeurs de l’ID du formateur sélectionné et du cours sélectionné et passe toutes les données nécessaires à la vue. Ces paramètres sont fournis par les liens hypertexte **Select** dans la page.

> [!TIP]
> 
> **Données d’itinéraire**
> 
> Données d’itinéraire sont données qui le binder de modèle se trouvée dans un segment d’URL spécifié dans la table de routage. Par exemple, l’itinéraire par défaut spécifie `controller`, `action`, et `id` segments :
> 
> routes.MapRoute(  
>  nom : « Default »,  
>  URL : « {controller} / {action} / {id} »,  
>  valeurs par défaut : new {contrôleur = « Home », action = « Index », id = UrlParameter.Optional}  
> );
> 
> Dans l’URL suivante, mappe l’itinéraire par défaut `Instructor` en tant que le `controller`, `Index` en tant que le `action` et 1 comme le `id`; il s’agit des valeurs de données d’itinéraire.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> « ? courseID = 2021 » est une valeur de chaîne de requête. Le binder de modèle fonctionne également si vous passez le `id` en tant que valeur de chaîne de requête :
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Les URL sont créés par `ActionLink` instructions dans la vue Razor. Dans le code suivant, le `id` paramètre correspond à l’itinéraire par défaut, par conséquent, `id` est ajouté pour les données d’itinéraire.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Dans le code suivant, `courseID` ne correspond pas à un paramètre dans l’itinéraire par défaut, donc il est ajouté en tant qu’une chaîne de requête.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Le code commence par créer une instance du modèle de vue et la placer dans la liste des formateurs. Le code spécifie un chargement hâtif pour la `Instructor.OfficeAssignment` et `Instructor.Courses` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

La seconde `Include` méthode charge cours et pour chaque cours est chargé, il effectue un chargement hâtif la `Course.Department` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Comme mentionné précédemment, le chargement hâtif n’est pas obligatoire, mais vise à améliorer les performances. Étant donné que la vue nécessite toujours le `OfficeAssignment` entité, il est plus efficace de l’extraire dans la même requête. `Course` les entités sont requises quand un formateur est sélectionné dans la page web, par conséquent, le chargement hâtif est mieux que le chargement différé uniquement si la page s’affiche plus souvent avec un cours sélectionné que sans.

Si un ID de formateur a été sélectionné, le formateur sélectionné est récupéré à partir de la liste des formateurs dans le modèle de vue. Le modèle de vue `Courses` propriété est alors chargée avec le `Course` entités à partir de ce formateur `Courses` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Le `Where` méthode retourne une collection, mais dans ce cas les critères transmis à cette méthode entraînent une seule `Instructor` entité renvoyée. Le `Single` méthode convertit la collection en un seul `Instructor` entité, qui vous permet d’accéder à cette entité `Courses` propriété.

Vous utilisez le [unique](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) méthode sur une collection lorsque vous savez que la collection possède un seul élément. Le `Single` méthode lève une exception si la collection transmise est vide ou s’il existe plusieurs éléments. Une alternative consiste [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), qui retourne une valeur par défaut (`null` dans ce cas) si la collection est vide. Toutefois, dans ce cas qui entraînerait encore une exception (en tentant de trouver un `Courses` propriété sur un `null` référence), et le message d’exception indique moins clairement la cause du problème. Lorsque vous appelez le `Single` (méthode), vous pouvez également transmettre le `Where` condition au lieu d’appeler le `Where` méthode séparément :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

À la place de :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ensuite, si un cours a été sélectionné, le cours sélectionné est récupéré à partir de la liste des cours dans le modèle de vue. Du puis le modèle de vue `Enrollments` propriété est chargée avec le `Enrollment` entités à partir de ce cours `Enrollments` propriété de navigation.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modification de la vue d’Index de l’instructeur

Dans *Views\Instructor\Index.cshtml*, remplacez le code existant par le code suivant. Les modifications apparaissent en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Vous avez apporté les modifications suivantes au code existant :

- Vous avez changé la classe de modèle en `InstructorIndexData`.
- Vous avez changé le titre de la page en remplaçant **Index** par **Instructors**.
- Déplacer les colonnes de lien de ligne à gauche.
- Supprimé le **FullName** colonne.
- Ajouter un **Office** colonne affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’est pas null. (Il s’agit d’une relation un-à-zéro-ou-un, il peut ne pas être un connexes `OfficeAssignment` entity.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Ajouté un code qui ajoute dynamiquement `class="selectedrow"` à la `tr` élément du formateur sélectionné. Cela définit une couleur d’arrière-plan de la ligne sélectionnée à l’aide de la classe CSS que vous avez créé précédemment. (Le `valign` attribut sera utile dans le didacticiel suivant lorsque vous ajoutez une colonne à plusieurs ligne à la table.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Ajouté une nouvelle `ActionLink` étiqueté **sélectionnez** immédiatement avant les autres liens dans chaque ligne, ce qui entraîne l’ID du formateur sélectionné à envoyer à la `Index` (méthode).

Exécutez l’application et sélectionnez le **formateurs** onglet. La page affiche le `Location` propriété de connexes `OfficeAssignment` entités et une table vide de cellule quand il est non lié `OfficeAssignment` entité.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Dans le *Views\Instructor\Index.cshtml* fichier, après la fermeture `table` élément (à la fin du fichier), ajoutez le code en surbrillance suivant. Cela affiche une liste de cours associés à un formateur quand un formateur est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Ce code lit la propriété `Courses` du modèle de vue pour afficher la liste des cours. Il fournit également un `Select` lien hypertexte qui envoie l’ID du cours sélectionné à la `Index` méthode d’action.

> [!NOTE]
> Le *.css* fichier est mis en cache par les navigateurs. Si vous ne voyez pas les modifications lorsque vous exécutez l’application, effectuez une actualisation dure (maintenez la touche CTRL enfoncée tout en cliquant sur le **Actualiser** bouton, ou appuyez sur CTRL + F5).


Exécutez la page et sélectionnez un formateur. Vous voyez à présent une grille qui affiche les cours affectés au formateur sélectionné et, pour chaque cours, vous voyez le nom du département affecté.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Après le bloc de code que vous venez d’ajouter, ajoutez le code suivant. Ceci affiche la liste des étudiants qui sont inscrits à un cours quand ce cours est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Ce code lit la `Enrollments` propriété du modèle de vue pour afficher une liste d’étudiants inscrits dans le cours.

Exécutez la page et sélectionnez un formateur. Ensuite, sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Ajout de chargement explicite

Ouvrez *InstructorController.cs* et observez comment la `Index` méthode obtient la liste des inscriptions pour un cours sélectionné :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Lorsque vous avez extrait la liste des formateurs, que vous avez spécifié un chargement hâtif pour la `Courses` propriété de navigation et pour le `Department` propriété de chaque cours. Vous placez le `Courses` collection dans le modèle de vue, et maintenant vous accédez à la `Enrollments` propriété de navigation d’une entité de la collection. Étant donné que vous n’avez pas spécifié un chargement hâtif pour la `Course.Enrollments` propriété de navigation, les données à partir de cette propriété s’affiche dans la page à la suite de chargement différé.

Si vous avez désactivé le chargement différé sans modifier le code de quelque autre manière, le `Enrollments` propriété aura la valeur null, quel que soit les inscriptions combien le cours avait réellement. Dans ce cas, pour charger le `Enrollments` propriété, vous devez spécifier un chargement hâtif ou le chargement explicite. Vous avez déjà vu comment effectuer un chargement hâtif. Pour voir un exemple de chargement explicite, remplacez le `Index` méthode avec le code suivant, qui charge explicitement le `Enrollments` propriété. Le code modifié sont mises en surbrillance.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Après avoir obtenu le texte sélectionné `Course` entité, le nouveau code charge explicitement de ce cours `Enrollments` propriété de navigation :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ensuite, il charge explicitement chaque `Enrollment` relative à entity `Student` entité :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Notez que vous utilisez le `Collection` méthode pour charger une propriété de collection, mais pour une propriété contenant une seule entité, vous utilisez le `Reference` (méthode). Vous pouvez exécuter maintenant la page Index des formateurs et vous ne verrez aucune différence dans ce qui est affiché dans la page, même si vous avez modifié la façon dont les données sont récupérées.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant utilisé les trois façons (différés hâtif et explicites) pour charger des données connexes dans les propriétés de navigation. Dans le didacticiel suivant, vous apprendrez à mettre à jour les données associées.

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Suivant](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
