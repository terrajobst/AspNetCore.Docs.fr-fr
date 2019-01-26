---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutoriel : Ajouter le tri, filtrage et pagination avec Entity Framework dans une application ASP.NET MVC | Microsoft Docs'
author: tdykstra
description: Dans ce didacticiel, vous ajoutez tri, filtrage et la fonctionnalité de pagination à la **étudiants** page d’Index. Vous créez également une page de regroupement simple.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: b7b5d3d3931f752f2effc044ca8cc52eab22da0a
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889858"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Tutoriel : Ajouter le tri, filtrage et pagination avec Entity Framework dans une application ASP.NET MVC

Dans le [didacticiel précédent](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), vous avez implémenté un ensemble de pages web pour les opérations CRUD de base pour `Student` entités. Dans ce didacticiel, vous ajoutez tri, filtrage et la fonctionnalité de pagination à la **étudiants** page d’Index. Vous créez également une page de regroupement simple.

L’illustration suivante montre à quoi ressemblera la page une lorsque vous avez terminé. Les en-têtes des colonnes sont des liens sur lesquels l’utilisateur peut cliquer pour trier selon les colonnes. Cliquer de façon répétée sur un en-tête de colonne permet de changer l’ordre de tri (croissant ou décroissant).

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des liens de tri de colonne
> * Ajouter une zone de recherche
> * Ajouter la pagination
> * Créer une page à propos

## <a name="prerequisites"></a>Prérequis

* [Implémentation de la fonctionnalité CRUD de base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Ajouter des liens de tri de colonne

Pour ajouter le tri à la page d’Index des étudiants, vous allez modifier le `Index` méthode de la `Student` contrôleur et ajouter du code pour le `Student` vue d’Index.

### <a name="add-sorting-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de tri à la méthode Index

- Dans *Controllers\StudentController.cs*, remplacez le `Index` méthode avec le code suivant :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ce code reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL. La valeur de chaîne de requête est fournie par ASP.NET MVC en tant que paramètre à la méthode d’action. Le paramètre est une chaîne qui est « Name » ou « Date », éventuellement suivie d’un trait de soulignement et de la chaîne « desc » pour spécifier l’ordre décroissant. L'ordre de tri par défaut est le tri croissant.

La première fois que la page d’index est demandée, il n’y a pas de chaîne de requête. Les étudiants sont affichés par ordre croissant selon `LastName`, qui est la valeur par défaut établie par le cas de passage dans la `switch` instruction. Quand l’utilisateur clique sur un lien hypertexte d’en-tête de colonne, la valeur `sortOrder` appropriée est fournie dans la chaîne de requête.

Les deux `ViewBag` variables sont utilisées afin que la vue peut configurer les liens hypertexte d’en-tête de colonne avec les valeurs de chaîne de requête appropriée :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il s’agit d’instructions ternaires. La première Spécifie que si le `sortOrder` paramètre est null ou vide, `ViewBag.NameSortParm` doit être défini sur « nom\_desc » ; sinon, elle doit être définie sur une chaîne vide. Ces deux instructions permettent à la vue de définir les liens hypertexte d’en-tête de colonne comme suit :

| Ordre de tri actuel | Lien hypertexte Nom de famille | Lien hypertexte Date |
| --- | --- | --- |
| Nom de famille croissant | descending | ascending |
| Nom de famille décroissant | croissant | croissant |
| Date par ordre croissant | croissant | décroissant |
| Date par ordre décroissant | croissant | ascending |

Utilise la méthode [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) pour spécifier la colonne à trier. Le code crée un <xref:System.Linq.IQueryable%601> variable avant le `switch` instruction, modifie dans le `switch` instruction et appelle le `ToList` méthode après le `switch` instruction. Lorsque vous créez et modifiez des variables `IQueryable`, aucune requête n’est envoyée à la base de données. La requête n’est pas exécutée jusqu'à ce que vous convertissez le `IQueryable` objet dans une collection en appelant une méthode telle que `ToList`. Par conséquent, ce code génère une requête unique qui n’est pas exécutée avant la `return View` instruction.

Comme alternative à l’écriture des instructions LINQ différents pour chaque ordre de tri, vous pouvez créer dynamiquement une instruction LINQ. Pour plus d’informations sur LINQ dynamique, consultez [LINQ dynamiques](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Ajouter des liens hypertexte du titre de colonne à la vue index étudiant

1. Dans *Views\Student\Index.cshtml*, remplacez le `<tr>` et `<th>` éléments pour la ligne d’en-tête avec le code en surbrillance :

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Ce code utilise les informations contenues dans le `ViewBag` les valeurs de chaîne de propriétés à définir des liens hypertexte avec la requête appropriée.

2. Exécutez la page et cliquez sur le **nom** et **Date d’inscription** des en-têtes de colonne pour vérifier que le tri fonctionne.

   Après avoir cliqué sur le **nom** titre, les étudiants sont affichés dans le dernier nom ordre décroissant.

## <a name="add-a-search-box"></a>Ajouter une zone de recherche

Pour ajouter le filtrage à la page index des étudiants, vous allez ajouter une zone de texte et un bouton Envoyer à la vue et apporter les modifications correspondantes dans le `Index` (méthode). La zone de texte vous permet d’entrer une chaîne à rechercher dans le prénom et le champ.

### <a name="add-filtering-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de filtrage à la méthode Index

- Dans *Controllers\StudentController.cs*, remplacez le `Index` méthode avec le code suivant (les modifications sont mises en surbrillance) :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Le code ajoute un `searchString` paramètre à la `Index` (méthode). La valeur de chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue Index. Il ajoute également un `where` clause à l’instruction LINQ qui sélectionne uniquement les étudiants dont le prénom ou le nom contient la chaîne de recherche. L’instruction qui ajoute le <xref:System.Linq.Queryable.Where%2A> clause s’exécute uniquement s’il existe une valeur à rechercher.

> [!NOTE]
> Dans de nombreux cas, vous pouvez appeler la même méthode sur un jeu d’entités Entity Framework ou comme méthode d’extension sur une collection en mémoire. Les résultats sont normalement les mêmes, mais dans certains cas, peuvent être différents.
>
> Par exemple, l’implémentation de .NET Framework de la `Contains` méthode retourne toutes les lignes lorsque vous lui passez une chaîne vide, mais le fournisseur Entity Framework pour SQL Server Compact 4.0 retourne zéro ligne pour les chaînes vides. Par conséquent, le code dans l’exemple (placer les `Where` instruction à l’intérieur d’un `if` instruction) permet de s’assurer que vous obtenez les mêmes résultats pour toutes les versions de SQL Server. En outre, l’implémentation de .NET Framework de la `Contains` méthode effectue une comparaison respectant la casse par défaut, mais les fournisseurs Entity Framework SQL Server effectuent des comparaisons sans respecter la casse par défaut. Par conséquent, l’appel la `ToUpper` méthode le test doit être explicitement non-respect de la casse garantit que les résultats ne changent pas lorsque vous modifiez le code ultérieurement d’utiliser un référentiel, ce qui retournera un `IEnumerable` collection au lieu d’un `IQueryable` objet. (Lorsque vous appelez la méthode `Contains` sur une collection `IEnumerable`, vous obtenez l’implémentation du .NET Framework ; lorsque vous l’appelez sur un objet `IQueryable`, vous obtenez l’implémentation du fournisseur de base de données.)
>
> Gestion des valeurs null peuvent également être différentes pour des fournisseurs de base de données différente ou lorsque vous utilisez un `IQueryable` objet par rapport à quand vous utilisez un `IEnumerable` collection. Par exemple, dans certains scénarios un `Where` condition comme `table.Column != 0` peuvent ne pas retourner les colonnes qui ont `null` comme valeur. Pour plus d’informations, consultez [une gestion incorrecte des variables null dans la clause « where »](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).

### <a name="add-a-search-box-to-the-student-index-view"></a>Ajouter une zone de recherche à la vue index étudiant

1. Dans *Views\Student\Index.cshtml*, ajoutez le code en surbrillance immédiatement avant l’ouverture `table` balise afin de créer une légende, une zone de texte et un **recherche** bouton.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Exécutez la page, entrez une chaîne de recherche, puis cliquez sur **recherche** pour vérifier que le filtrage fonctionne.

   Notez que l’URL ne contient pas la « une » chaîne de recherche, ce qui signifie que si vous ajoutez cette page aux Favoris, vous n’obtiendrez la liste filtrée lorsque vous utilisez le signet. Cela s’applique également aux liens de tri de colonne, comme ils pour trier la liste entière. Vous allez modifier le **recherche** bouton à utiliser des chaînes de requête pour les critères de filtre plus loin dans le didacticiel.

## <a name="add-paging"></a>Ajouter la pagination

Pour ajouter la pagination à la page index des étudiants, nous allons commencer par installer le **PagedList.Mvc** package NuGet. Puis vous apporterez des modifications supplémentaires dans le `Index` (méthode) et ajouter des liens de pagination à la `Index` vue. **PagedList.Mvc** est un des nombreux pagination bon et le tri des packages pour ASP.NET MVC et son utilisation ici n’est destinée uniquement, par exemple, pas comme une recommandation pour elle sur les autres options.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installez le package NuGet de PagedList.MVC

Le package NuGet **PagedList.Mvc** package installe automatiquement le **PagedList** package en tant que dépendance. Le **PagedList** package installe un `PagedList` les méthodes de type et extension de collecte pour `IQueryable` et `IEnumerable` collections. Les méthodes d’extension créent une seule page de données dans un `PagedList` collection hors de votre `IQueryable` ou `IEnumerable`et le `PagedList` collection fournit plusieurs propriétés et méthodes qui facilitent la pagination. Le **PagedList.Mvc** package installe une application d’assistance de pagination qui affiche les boutons de pagination.

1. À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet** , puis **Console du Gestionnaire de Package**.

2. Dans le **Console du Gestionnaire de Package** fenêtre, assurez-vous que le **source du Package** est **nuget.org** et **projet par défaut** est **ContosoUniversity**, puis entrez la commande suivante :

   ```text
   Install-Package PagedList.Mvc
   ```

3. Générez le projet.

### <a name="add-paging-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de changement de page à la méthode Index

1. Dans *Controllers\StudentController.cs*, ajoutez un `using` instruction pour la `PagedList` espace de noms :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Remplacez la méthode `Index` par le code suivant :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Ce code ajoute un `page` paramètre, un paramètre d’ordre de tri actuel et un paramètre de filtre actuel à la signature de méthode :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   La première fois que la page s’affiche, ou si l’utilisateur n’a pas cliqué sur une pagination ou lien de tri, tous les paramètres sont null. Si l’utilisateur clique sur un lien de pagination, le `page` variable contient le numéro de page à afficher.

   Un `ViewBag` propriété fournit une vue avec l’ordre de tri actuel, car il doit être inclus dans les liens de pagination afin de conserver l’ordre de tri lors de la pagination :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Une autre propriété, `ViewBag.CurrentFilter`, fournit une vue avec la chaîne de filtre actuelle. Cette valeur doit être incluse dans les liens de changement de page pour que les paramètres de filtre soient conservés lors du changement de page, et elle doit être restaurée dans la zone de texte lorsque la page est réaffichée. Si la chaîne de recherche est modifiée au cours du changement de page, la page doit être réinitialisée à 1, car le nouveau filtre peut entraîner l’affichage de données différentes. La chaîne de recherche est modifiée quand une valeur est entrée dans la zone de texte et le bouton d’envoi est enfoncé. Dans ce cas, le `searchString` paramètre n’est pas null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   À la fin de la méthode, le `ToPagedList` méthode d’extension sur les étudiants `IQueryable` objet convertit la requête d’étudiant en une seule page d’étudiants dans un type de collection qui prend en charge la pagination. Cette page individuelle d’étudiants est ensuite passée à la vue :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   La méthode `ToPagedList` accepte un numéro de page. Les deux points d’interrogation représentent le [opérateur de fusion null](/dotnet/csharp/language-reference/operators/null-coalescing-operator). L’opérateur de fusion Null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` indique de renvoyer la valeur de `page` si elle a une valeur, ou de renvoyer 1 si `page` a la valeur Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Ajouter des liens de pagination à la vue index étudiant

1. Dans *Views\Student\Index.cshtml*, remplacez le code existant par le code suivant. Les modifications sont mises en surbrillance.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   L’instruction `@model` en haut de la page spécifie que la vue obtient désormais un objet `PagedList` à la place d’un objet `List`.

   Le `using` instruction pour `PagedList.Mvc` donne accès à l’application d’assistance MVC de boutons de pagination.

   Le code utilise une surcharge de [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) qui lui permet de spécifier [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   La valeur par défaut [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) envoie les données de formulaire avec un POST, ce qui signifie que les paramètres sont passés dans le corps du message HTTP et non dans l’URL sous forme de chaînes de requête. Lorsque vous spécifiez HTTP GET, les données de formulaire sont transmises dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs d’ajouter l’URL aux favoris. Le [recommandations du W3C pour l’utilisation de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) est recommandé que vous devez utiliser GET quand l’action n’entraîne pas une mise à jour.

   La zone de texte est initialisée avec la chaîne de recherche actuelle lorsque vous cliquez sur une nouvelle page, vous voyez la chaîne de recherche actuel.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Les liens d’en-tête de colonne utilisent la chaîne de requête pour transmettre la chaîne de recherche actuelle au contrôleur afin que l’utilisateur puisse trier les résultats de filtrage :

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Le nombre actuel de page et le nombre total de pages est affiché.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   S’il n’y a aucune page à afficher, « Page 0 0 » s’affiche. (Dans ce cas, le numéro de page est supérieur au nombre de page car `Model.PageNumber` est 1, et `Model.PageCount` est 0.)

   Les boutons de pagination sont affichés par le `PagedListPager` helper :

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   Le `PagedListPager` helper fournit un nombre d’options que vous pouvez personnaliser, y compris les URL et styles. Pour plus d’informations, consultez [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) sur le site GitHub.

2. Exécutez la page.

   Cliquez sur les liens de changement de page dans différents ordres de tri pour vérifier que le changement de page fonctionne. Ensuite, entrez une chaîne de recherche et essayez de changer de page à nouveau pour vérifier que le changement de page fonctionne correctement avec le tri et le filtrage.

## <a name="create-an-about-page"></a>Créer une page à propos

Pour Contoso University du site Web sur la page, vous afficherez le nombre d’étudiants inscrits pour chaque date d’inscription. Cela nécessite un regroupement et des calculs simples sur les groupes. Pour ce faire, vous devez effectuer les opérations suivantes :

- Créez une classe de modèle de vue pour les données que vous devez transmettre à la vue.
- Modifier le `About` méthode dans la `Home` contrôleur.
- Modifier le `About` vue.

### <a name="create-the-view-model"></a>Créer le modèle de vue

Créer un *ViewModels* dossier dans le dossier du projet. Dans ce dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modifier le contrôleur Home

1. Dans *HomeController.cs*, ajoutez le code suivant `using` instructions en haut du fichier :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Ajoutez une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Remplacez la méthode `About` par le code suivant :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   L’instruction LINQ regroupe les entités Student par date d’inscription, calcule le nombre d’entités dans chaque groupe et stocke les résultats dans une collection d’objets de modèle de vue `EnrollmentDateGroup`.

4. Ajouter un `Dispose` méthode :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modifier la vue About

1. Remplacez le code dans le *Views\Home\About.cshtml* fichier par le code suivant :

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Exécutez l’application et cliquez sur le **sur** lien.

   Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources Entity Framework dans [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des liens de tri de colonne
> * Ajouter une zone de recherche
> * Ajouter la pagination
> * Créer une page à propos

Passez à l’article suivant pour apprendre à utiliser l’interception de la résilience et la commande de connexion.
> [!div class="nextstepaction"]
> [Interception de la résilience et la commande de connexion](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)