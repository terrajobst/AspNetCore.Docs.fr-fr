---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Bien démarrer avec Entity Framework 4.0 Database First et 4 d’ASP.NET Web Forms - partie 3 | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide d’Entity Framework. L’exemple d’application est en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 51964f86e66e21d63105f1043641c0096d1d41b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391145"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Bien démarrer avec Entity Framework 4.0 Database First et 4 les Web Forms ASP.NET - partie 3
====================
par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application web Contoso University montre comment créer des applications Web Forms ASP.NET à l’aide de l’Entity Framework 4.0 et Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Tri, regroupement et filtrage des données

Dans le didacticiel précédent, vous avez utilisé le `EntityDataSource` contrôle pour afficher et modifier des données. Dans ce didacticiel, vous allez filtrer, classer et regrouper des données. Quand vous procédez ainsi en définissant des propriétés de la `EntityDataSource` contrôle, la syntaxe est différente des autres contrôles de source de données. Comme vous le verrez, cependant, vous pouvez utiliser la `QueryExtender` contrôle afin de réduire ces différences.

Vous allez modifier le *Students.aspx* page pour filtrer pour les étudiants, triez par nom et la recherche de nom. Vous devez également modifier le *Courses.aspx* page pour afficher les cours pour le département sélectionné et la recherche pour les cours par nom. Enfin, vous allez ajouter les statistiques des étudiants pour le *About.aspx* page.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>À l’aide de la EntityDataSource « Where » propriété pour filtrer les données

Ouvrez le *Students.aspx* page que vous avez créé dans le didacticiel précédent. Selon la configuration actuelle, le `GridView` contrôle dans la page affiche tous les noms à partir de la `People` jeu d’entités. Toutefois, vous souhaitez afficher uniquement les étudiants, que vous pouvez trouver en sélectionnant `Person` entités ayant des dates d’inscription non null.

Basculez vers **conception** afficher, puis sélectionnez le `EntityDataSource` contrôle. Dans le **propriétés** fenêtre, définissez la `Where` propriété `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La syntaxe à utiliser dans le `Where` propriété de la `EntityDataSource` contrôle est Entity SQL. Entity SQL est similaire à Transact-SQL, mais il est personnalisé pour une utilisation avec les entités plutôt que des objets de base de données. Dans l’expression `it.EnrollmentDate is not null`, le mot `it` représente une référence à l’entité retournée par la requête. Par conséquent, `it.EnrollmentDate` fait référence à la `EnrollmentDate` propriété de la `Person` entité qui le `EntityDataSource` contrôler retourne.

Exécutez la page. La liste des étudiants contient désormais uniquement les étudiants. (Aucune ligne affiché où il n’existe aucune date d’inscription.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>À l’aide de la propriété « OrderBy » EntityDataSource aux données de commande

Vous souhaitez également cette liste pour être dans l’ordre de nom lorsqu’il est tout d’abord affiché. Avec le *Students.aspx* page toujours ouverte dans **conception** vue et avec le `EntityDataSource` contrôle toujours sélectionné, dans le **propriétés** fenêtre, définissez la  **OrderBy** propriété `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Exécutez la page. La liste des étudiants est désormais dans l’ordre par nom de famille.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>À l’aide d’un paramètre de contrôle pour définir la propriété « Where »

Comme avec d’autres contrôles de source de données, vous pouvez passer des valeurs de paramètre à la `Where` propriété. Sur le *Courses.aspx* page que vous avez créé dans la partie 2 du didacticiel, vous pouvez utiliser cette méthode pour afficher les cours qui sont associés au service un utilisateur sélectionne dans la liste déroulante.

Ouvrez *Courses.aspx* et basculez vers **conception** vue. Ajoutez une deuxième `EntityDataSource` le contrôle à la page et nommez-le `CoursesEntityDataSource`. Se connecter à la `SchoolEntities` de modèle, puis sélectionnez `Courses` en tant que le **EntitySetName** valeur.

Dans le **propriétés** fenêtre, cliquez sur le bouton de sélection dans le **où** zone de propriété. (Assurez-vous que le `CoursesEntityDataSource` contrôle est toujours sélectionné avant d’utiliser le **propriétés** fenêtre.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Le **Éditeur d’Expression** boîte de dialogue s’affiche. Dans cette boîte de dialogue, sélectionnez **générer automatiquement Where expression basée sur les paramètres fournis**, puis cliquez sur **ajouter un paramètre**. Nommez le paramètre `DepartmentID`, sélectionnez **contrôle** en tant que le **source du paramètre** valeur, puis sélectionnez **DepartmentsDropDownList** comme le **ControlID**  valeur.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Cliquez sur **afficher les propriétés avancées**, puis, dans le **propriétés** fenêtre de la **Éditeur d’Expression** boîte de dialogue, choisissez le `Type` propriété `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Lorsque vous avez terminé, cliquez sur **OK**.

Sous la liste déroulante, ajoutez un `GridView` le contrôle à la page et nommez-le `CoursesGridView`. Se connecter à la `CoursesEntityDataSource` contrôle de source de données cliquez sur **actualiser le schéma**, cliquez sur **modifier les colonnes**et supprimer la `DepartmentID` colonne. Le `GridView` balisage du contrôle ressemble à l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Lorsque l’utilisateur modifie le département sélectionné dans la liste déroulante, vous souhaitez la liste de cours associés à modifier automatiquement. Pour permettre cela se produit, sélectionnez la liste déroulante, puis, dans le **propriétés** fenêtre, définissez la `AutoPostBack` propriété `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Maintenant que vous avez terminé de l’aide du concepteur, basculez vers **Source** Affichez et remplacez le `ConnectionString` et `DefaultContainer` nomment les propriétés de la `CoursesEntityDataSource` contrôler avec le `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attribut. Lorsque vous avez terminé, le balisage pour le contrôle se présente comme l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Exécutez la page et utilisez la liste déroulante pour sélectionner les différents services. Seuls les cours proposés par le département sélectionné sont affichent dans le `GridView` contrôle.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>À l’aide de la propriété « GroupBy » EntityDataSource pour regrouper les données

Supposons que Contoso University souhaite placer certaines statistiques de corps de l’étudiant sur sa page About. Plus précisément, elle souhaite afficher une répartition du nombre d’étudiants par la date de que leur inscription.

Ouvrez *About.aspx*et dans **Source** afficher, remplacez le contenu existant de la `BodyContent` contrôler avec « Statistiques de corps étudiant » entre `h2` balises :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Après l’en-tête, ajoutez un `EntityDataSource` contrôler et nommez-le `StudentStatisticsEntityDataSource`. Connectez-le à `SchoolEntities`, sélectionnez le `People` entité définie et laissez le **sélectionnez** zone dans l’Assistant inchangé. Définissez les propriétés suivantes le **propriétés** fenêtre :

- Pour filtrer uniquement pour les étudiants, définissez le `Where` propriété `it.EnrollmentDate is not null`.
- Pour regrouper les résultats par la date d’inscription, définissez le `GroupBy` propriété `it.EnrollmentDate`.
- Pour sélectionner la date d’inscription et le nombre d’élèves, définissez le `Select` propriété `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Pour trier les résultats par date d’inscription, définissez le `OrderBy` propriété `it.EnrollmentDate`.

Dans **Source** afficher, remplacez le `ConnectionString` et `DefaultContainer` nomment les propriétés avec un `ContextTypeName` propriété. Le `EntityDataSource` balisage du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La syntaxe de la `Select`, `GroupBy`, et `Where` propriétés ressemble à Transact-SQL à l’exception de la `it` mot clé qui spécifie l’entité actuelle.

Ajoutez le balisage suivant pour créer un `GridView` contrôle pour afficher les données.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Exécutez la page pour afficher une liste indiquant le nombre d’élèves par date d’inscription.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>À l’aide du contrôle QueryExtender pour le filtrage et tri

Le `QueryExtender` contrôle fournit un moyen de spécifier le filtrage et le tri dans le balisage. La syntaxe est indépendante du système de gestion de base de données (SGBD) que vous utilisez. Il est également généralement indépendante de l’Entity Framework, avec l’exception que vous utilisez pour les propriétés de navigation de syntaxe est unique à Entity Framework.

Dans cette partie du didacticiel, vous allez utiliser un `QueryExtender` contrôle aux données de filtre et ordre et un seul champ order by sera une propriété de navigation.

(Si vous préférez utiliser le code au lieu de balisage pour étendre les requêtes qui sont générés automatiquement par le `EntityDataSource` contrôle, vous pouvez le faire gestion de la `QueryCreated` événement. Voici comment la `QueryExtender` contrôle s’étend `EntityDataSource` contrôle également les requêtes.)

Ouvrez le *Courses.aspx* page et sous le balisage que vous avez ajoutée précédemment, insérez le balisage suivant pour créer un en-tête, une zone de texte permettant d’entrer des chaînes de recherche, un bouton de recherche et un `EntityDataSource` contrôle qui est lié à la `Courses` jeu d’entités.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Notez que le `EntityDataSource` du contrôle `Include` propriété est définie sur `Department`. Dans la base de données, le `Course` table ne contient pas le nom du service ; il contient un `DepartmentID` colonne clé étrangère. Si vous ont été interrogeant directement, la base de données pour obtenir le nom du service, ainsi que les données de cours, vous seriez obligé de joindre le `Course` et `Department` tables. En définissant le `Include` propriété `Department`, vous spécifiez que Entity Framework doit effectuer le travail de mise en relation `Department` entité lorsqu’il atteint un `Course` entité. Le `Department` entité est ensuite stockée dans le `Department` propriété de navigation de la `Course` entité. (Par défaut, le `SchoolEntities` classe qui a été généré par le Concepteur de modèle de données récupère les données associées lorsque cela est nécessaire, et vous avez lié le contrôle de source de données à cette classe, affectant ainsi la `Include` propriété n’est pas nécessaire. Toutefois, configurez-la améliore les performances de la page, car sinon Entity Framework rendrait des appels séparés à la base de données pour récupérer des données pour le `Course` entités et pour connexe `Department` entités.)

Après le `EntityDataSource` contrôle que vous avez créé, insérez le balisage suivant pour créer un `QueryExtender` contrôle qui est lié à celui `EntityDataSource` contrôle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Le `SearchExpression` élément spécifie que vous souhaitez sélectionner des cours dont le titre correspond à la valeur entrée dans la zone de texte. Uniquement comme le nombre de caractères sont entrés dans la zone de texte est comparée, car le `SearchType` propriété spécifie `StartsWith`.

Le `OrderByExpression` élément spécifie que le jeu de résultats sera trié par titre du cours dans le nom du service. Notez la façon dont le nom du service est spécifiée : `Department.Name`. Étant donné que l’association entre le `Course` entité et la `Department` entité est-à-un, le `Department` contient de la propriété de navigation un `Department` entité. (S’il s’agissait d’une relation un-à-plusieurs, la propriété contient une collection.) Pour obtenir le nom du service, vous devez spécifier le `Name` propriété de la `Department` entité.

Enfin, ajoutez un `GridView` contrôle pour afficher la liste des cours :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La première colonne est un champ de modèle qui affiche le nom du département. Spécifie l’expression de liaison de données `Department.Name`, tout comme vous l’avez vu dans la `QueryExtender` contrôle.

Exécutez la page. L’affichage initial affiche une liste de tous les cours dans l’ordre par département, puis par titre du cours.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Entrez un « m » et cliquez sur **recherche** pour voir tous les cours dont le titre commence par « m » (la recherche n'est pas la casse).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>À l’aide de l’opérateur « Like » pour filtrer les données

Vous pouvez obtenir un effet similaire à la `QueryExtender` du contrôle `StartsWith`, `Contains`, et `EndsWith` rechercher des types à l’aide un `Like` opérateur dans le `EntityDataSource` du contrôle `Where` propriété. Dans cette partie du didacticiel, vous allez apprendre à utiliser le `Like` opérateur à rechercher pour un étudiant par nom.

Ouvrez *Students.aspx* dans **Source** vue. Après le `GridView` de contrôle, ajoutez le balisage suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Ce balisage est similaire à ce que vous avez vu précédemment à l’exception de la `Where` valeur de propriété. La deuxième partie de la `Where` expression définit une recherche de sous-chaîne (`LIKE %FirstMidName% or LIKE %LastName%`) qui recherche les noms premier et derniers pour tout ce qui est entré dans la zone de texte.

Exécutez la page. Initialement vous voir tous les étudiants, car la valeur par défaut pour le `StudentName` paramètre est « % ».

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Entrez la lettre « g » dans la zone de texte et cliquez sur **recherche**. Vous voyez une liste d’étudiants qui ont un « g » dans soit le premier ou le nom.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Vous avez maintenant affichés, mis à jour, filtrés, classés et groupé des données à partir des tables individuelles. Dans le didacticiel suivant, vous commencerez à travailler avec les données associées (scénarios maître / détail).

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-4.md)
