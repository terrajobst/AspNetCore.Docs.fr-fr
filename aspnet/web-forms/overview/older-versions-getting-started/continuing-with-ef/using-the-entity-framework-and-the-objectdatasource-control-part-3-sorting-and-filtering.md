---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'À l’aide d’Entity Framework 4.0 et le contrôle ObjectDataSource, partie 3 : tri et filtrage | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application web Contoso University créé par la mise en route avec la série de didacticiels Entity Framework 4.0. J’AI...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 15d9a7ecd3119af02576cbcf728828353748a513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806996"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>À l’aide d’Entity Framework 4.0 et le contrôle ObjectDataSource, partie 3 : tri et filtrage
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application web Contoso University créé par le [mise en route avec Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels. Si vous n’avez pas effectué les didacticiels précédents, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous l’auriez créée. Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée. Si vous avez des questions sur les didacticiels, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Dans le didacticiel précédent, vous avez implémenté le modèle de référentiel dans une application web des applications multicouches qui utilise Entity Framework et le `ObjectDataSource` contrôle. Ce didacticiel montre comment effectuer le tri et de filtrage et de gérer les scénarios maître / détail. Vous allez ajouter les améliorations suivantes à la *Departments.aspx* page :

- Une zone de texte pour permettre aux utilisateurs de sélectionner les services par nom.
- Une liste de cours pour chaque service qui est indiqué dans la grille.
- La possibilité de trier en cliquant sur les en-têtes de colonne.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Ajout de la capacité à trier les colonnes GridView

Ouvrez le *Departments.aspx* page et ajoutez un `SortParameterName="sortExpression"` attribut le `ObjectDataSource` contrôle nommé `DepartmentsObjectDataSource`. (Vous allez créer ultérieurement un `GetDepartments` méthode qui accepte un paramètre nommé `sortExpression`.) Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Ajouter le `AllowSorting="true"` d’attribut à la balise d’ouverture de la `GridView` contrôle. Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

Dans *Departments.aspx.cs*, définir l’ordre de tri par défaut en appelant le `GridView` du contrôle `Sort` méthode à partir de la `Page_Load` méthode :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Vous pouvez ajouter du code qui trie ou de filtre dans la classe de logique métier ou de la classe de dépôt. Si vous le faites dans la classe de logique métier, le tri ou le filtrage de travail n’est effectué une fois que les données sont récupérées à partir de la base de données, car l’utilisation de la classe de logique métier avec un `IEnumerable` objet retourné par le référentiel. Si vous ajoutez le tri et filtrage de code dans la classe de dépôt et faites-le avant une expression LINQ ou requête d’objet a été convertie en un `IEnumerable` de l’objet, vos commandes sont transmises à la base de données pour le traitement, qui est généralement plus efficace. Dans ce didacticiel vous allez implémenter le tri et filtrage d’une manière qui provoque le traitement à effectuer par la base de données, autrement dit, dans le référentiel.

Pour ajouter la fonctionnalité de tri, vous devez ajouter une nouvelle méthode à l’interface de référentiel et les classes du référentiel ainsi que pour la classe de logique métier. Dans le *ISchoolRepository.cs* , ajoutez un nouveau `GetDepartments` méthode qui prend un `sortExpression` paramètre qui sera utilisé pour trier la liste des services qui est retournée :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Le `sortExpression` paramètre spécifie la colonne à trier et le sens du tri.

Ajoutez du code pour la nouvelle méthode à la *SchoolRepository.cs* fichier :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Modifier existants sans paramètre `GetDepartments` méthode à appeler la nouvelle méthode :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Dans le projet de test, ajoutez la nouvelle méthode suivante à *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Si vous souhaitez créer des tests unitaires qui dépendaient de cette méthode qui retourne une liste triée, vous devez trier la liste avant de le renvoyer. Vous ne sont pas créer de tests comme celle-ci dans ce didacticiel, donc la méthode peut retourner simplement la liste non triée de services.

Dans le *SchoolBL.cs* , ajoutez la nouvelle méthode suivante à la classe de logique métier :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Ce code passe le paramètre de tri à la méthode de référentiel.

Exécutez le *Departments.aspx* page.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Vous pouvez maintenant cliquer sur n’importe quel en-tête de colonne pour trier par cette colonne. Si la colonne est déjà triée, en cliquant sur l’en-tête inverse le sens du tri.

## <a name="adding-a-search-box"></a>Ajout d’une zone de recherche

Dans cette section, vous allez ajouter une zone de texte de recherche, liez-le à la `ObjectDataSource` contrôler à l’aide d’un paramètre de contrôle et ajoutez une méthode à la classe de logique métier pour prendre en charge le filtrage.

Ouvrez le *Departments.aspx* page et ajoutez le balisage suivant entre le titre et le premier `ObjectDataSource` contrôle :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

Dans le `ObjectDataSource` contrôle nommé `DepartmentsObjectDataSource`, procédez comme suit :

- Ajouter un `SelectParameters` élément pour un paramètre nommé `nameSearchString` qui obtient la valeur entrée dans le `SearchTextBox` contrôle.
- Modifier le `SelectMethod` attribut valeur à `GetDepartmentsByName`. (Vous allez créer cette méthode ultérieurement.)

Le balisage pour le `ObjectDataSource` contrôle ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

Dans *ISchoolRepository.cs*, ajoutez un `GetDepartmentsByName` méthode qui accepte les deux `sortExpression` et `nameSearchString` paramètres :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

Dans *SchoolRepository.cs*, ajoutez la nouvelle méthode suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Ce code utilise un `Where` méthode afin de sélectionner les éléments qui contiennent la chaîne de recherche. Si la chaîne de recherche est vide, tous les enregistrements seront sélectionnés. Notez que lorsque vous spécifiez la méthode appelle ensemble dans une instruction comme celle-ci (`Include`, puis `OrderBy`, puis `Where`), la `Where` méthode doit toujours être le dernier.

Modifier existants `GetDepartments` méthode qui prend un `sortExpression` paramètre pour appeler la nouvelle méthode :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

Dans *MockSchoolRepository.cs* dans le projet de test, ajoutez la nouvelle méthode suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

Dans *SchoolBL.cs*, ajoutez la nouvelle méthode suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Exécutez le *Departments.aspx* page et entrez une chaîne de recherche pour vous assurer que la logique de sélection fonctionne. Laissez la zone de texte vide et essayez une recherche pour vous assurer que tous les enregistrements sont renvoyés.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Ajout d’une colonne de détails pour chaque ligne de grille

Ensuite, vous souhaitez afficher tous les cours pour chaque département affiché dans la cellule de droite de la grille. Pour ce faire, vous allez utiliser imbriquée `GridView` contrôle et lier les données à des données à partir de la `Courses` propriété de navigation de la `Department` entité.

Open *Departments.aspx* et dans le balisage pour le `GridView` contrôler, spécifiez un gestionnaire pour le `RowDataBound` événement. Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Ajouter un nouveau `TemplateField` élément après le `Administrator` champ du modèle :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Ce balisage crée imbriquée `GridView` contrôle qui affiche le numéro de cours et le titre d’une liste de cours. Elle ne spécifie pas une source de données, car vous allez databind dans un code dans le `RowDataBound` gestionnaire.

Ouvrez *Departments.aspx.cs* et ajoutez le gestionnaire suivant pour le `RowDataBound` événement :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Ce code obtient le `Department` convertit de l’entité à partir des arguments d’événement, le `Courses` propriété de navigation vers un `List` collection et établit l’imbriquée `GridView` à la collection.

Ouvrir le *SchoolRepository.cs* fichier et spécifiez un chargement hâtif pour la `Courses` propriété de navigation en appelant le `Include` méthode dans la requête d’objet que vous créez dans le `GetDepartmentsByName` (méthode). Le `return` instruction dans le `GetDepartmentsByName` méthode ressemble maintenant à l’exemple suivant.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Exécutez la page. Outre le tri et filtrage de fonctionnalité que vous avez ajouté précédemment, le contrôle GridView affiche maintenant les détails du cours imbriquée pour chaque service.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Cette étape termine l’introduction aux scénarios de tri, filtrage et maître-détail. Dans le didacticiel suivant, vous allez apprendre à gérer l’accès concurrentiel.

> [!div class="step-by-step"]
> [Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
