---
title: Pages Razor avec EF Core dans ASP.NET Core - Tri, filtre, pagination - 3 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez ajouter des fonctionnalités de tri, de filtrage et de changement de page à une page à l’aide d’ASP.NET Core et d’Entity Framework Core.
ms.author: riande
ms.date: 10/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 26f516716864bdce81cf3acdacb0f9d2f98407b7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - Tri, filtre, pagination - 3 sur 8

Par [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Dans ce didacticiel, nous allons ajouter des fonctionnalités de tri, de filtrage, de regroupement et de pagination.

L’illustration suivante présente une page complète. Les en-têtes de colonne sont des liens hypertexte permettant de trier la colonne. Un clic sur un en-tête de colonne permet de changer l’ordre de tri (croissant ou décroissant).

![Page d’index des étudiants](sort-filter-page/_static/paging.png)

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

## <a name="add-sorting-to-the-index-page"></a>Ajouter le tri à la page Index

Ajouter des chaînes au `PageModel` *Students/Index.cshtml.cs* pour contenir les paramètres de tri :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


Mettez à jour le `OnGetAsync` *Students/Index.cshtml.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Le code précédent reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL. L’URL (y compris la chaîne de requête) est générée par le [Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
).

Le paramètre `sortOrder` est « Name » ou « Date ». Le paramètre `sortOrder` peut être suivi de « _desc » pour spécifier l’ordre décroissant. L'ordre de tri par défaut est le tri croissant.

Quand la page Index est demandée à partir du lien **Students**, il n’existe aucune chaîne de requête. Les étudiants sont affichés par nom de famille dans l’ordre croissant. Le tri croissant par nom de famille est la valeur par défaut dans l’instruction `switch`. Quand l’utilisateur clique sur un lien d’en-tête de colonne, la valeur `sortOrder` appropriée est fournie dans la valeur de chaîne de requête.

`NameSort` et `DateSort` sont utilisés par la page Razor pour configurer les liens hypertexte d’en-tête de colonne avec les valeurs de chaîne de requête appropriées :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Le code suivant contient [l’opérateur ?:](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator) C#.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

La première ligne indique que quand `sortOrder` est null ou vide, `NameSort` prend la valeur « name_desc ». Si `sortOrder` n’est **pas** null ou vide, `NameSort` prend pour valeur une chaîne vide.

`?: operator` est également appelé opérateur ternaire.

Ces deux instructions permettent à la page de définir les liens hypertexte d’en-tête de colonne comme suit :

| Ordre de tri actuel | Lien hypertexte Nom de famille | Lien hypertexte Date |
|:--------------------:|:-------------------:|:--------------:|
| Nom de famille croissant | descending        | ascending      |
| Nom de famille décroissant | ascending           | ascending      |
| Date croissante       | ascending           | descending     |
| Date décroissante      | ascending           | ascending      |

La méthode utilise LINQ to Entities pour spécifier la colonne d’après laquelle effectuer le tri. Le code initialise un `IQueryable<Student> ` avant l’instruction switch, et le modifie dans l’instruction switch :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Quand un `IQueryable` est créé ou modifié, aucune requête n’est envoyée à la base de données. La requête n’est pas exécutée tant que l’objet `IQueryable` n’a pas été converti en collection. Les `IQueryable` sont convertis en collection en appelant une méthode telle que `ToListAsync`. Ainsi, le code `IQueryable` génère une requête unique qui n’est pas exécutée avant l’instruction suivante :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` peut contenir un grand nombre de colonnes.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Ajouter des liens hypertexte d’en-tête de colonne à la page d’index des étudiants

Remplacez le code dans *Students/Index.cshtml* par le code en surbrillance suivant :

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Le code précédent :

* Ajoute des liens hypertexte aux en-têtes de colonne `LastName` et `EnrollmentDate`.
* Utilise les informations contenues dans `NameSort` et `DateSort` pour définir des liens hypertexte avec les valeurs d’ordre de tri actuelles.

Pour vérifier que le tri fonctionne

* Exécutez l’application et sélectionnez l’onglet **Students**.
* Cliquez sur **Last Name**.
* Cliquez sur **Enrollment Date**.

Pour mieux comprendre le fonctionnement du code

* Dans *Student/Index.cshtml.cs*, définissez un point d’arrêt sur `switch (sortOrder)`.
* Ajoutez un espion pour `NameSort` et `DateSort`.
* Dans *Student/Index.cshtml.cshtml*, définissez un point d’arrêt sur `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Effectuez un pas à pas détaillé dans le débogueur.

## <a name="add-a-search-box-to-the-students-index-page"></a>Ajouter une zone de recherche à la page d’index des étudiants

Pour ajouter le filtrage à la page d’index des étudiants :

* Une zone de texte et un bouton d’envoi sont ajoutés à la page Razor. La zone de texte fournit une chaîne de recherche sur le prénom ou le nom de famille.
* Le modèle de page est mis à jour pour utiliser la valeur de zone de texte.

### <a name="add-filtering-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de filtrage à la méthode Index

Mettez à jour le `OnGetAsync` *Students/Index.cshtml.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Le code précédent :

* Ajoute le paramètre `searchString` à la méthode `OnGetAsync`. La valeur de chaîne de recherche est reçue à partir d’une zone de texte qui est ajoutée dans la section suivante.
* A ajouté une clause `Where` à l’instruction LINQ. La clause `Where` sélectionne uniquement les étudiants dont le prénom ou le nom de famille contient la chaîne de recherche. L’instruction LINQ est exécutée uniquement s’il y a une valeur à rechercher.

Remarque : Le code précédent appelle la méthode `Where` sur un objet `IQueryable`, et le filtre est traité sur le serveur. Dans certains scénarios, l’application peut appeler la méthode `Where` en tant que méthode d’extension sur une collection en mémoire. Par exemple, supposez que `_context.Students` passe de `DbSet` EF Core à une méthode de référentiel qui retourne une collection `IEnumerable`. Le résultat serait normalement le même, mais dans certains cas il peut être différent.

Par exemple, l’implémentation .NET Framework de `Contains` effectue par défaut une comparaison respectant la casse. Dans SQL Server, le respect de la casse de `Contains` est déterminé par le paramètre de classement de l’instance de SQL Server. Par défaut, SQL Server ne respecte pas la casse. `ToUpper` peut être appelée pour que le test ne respecte pas la casse de manière explicite :

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Le code précédent garantit que les résultats ne respectent pas la casse si le code change et utilise `IEnumerable`. Quand `Contains` est appelée sur une collection `IEnumerable`, l’implémentation .NET Core est utilisée. Quand `Contains` est appelée sur un objet `IQueryable`, l’implémentation de base de données est utilisée. Retourner un `IEnumerable` à partir d’un référentiel peut entraîner une dégradation significative des performances :

1. Toutes les lignes sont retournées à partir du serveur de base de données.
1. Le filtre est appliqué à toutes les lignes retournées dans l’application.

Il existe un coût en matière de performances en cas d’appel à `ToUpper`. Le code `ToUpper` ajoute une fonction dans la clause WHERE de l’instruction TSQL SELECT. La fonction ajoutée empêche l’optimiseur d’utiliser un index. Étant donné que SQL est installé sans respect de la casse, il est préférable d’éviter l’appel à `ToUpper` quand il n’est pas nécessaire.

### <a name="add-a-search-box-to-the-student-index-page"></a>Ajouter une zone de recherche à la page d’index des étudiants

Dans *Pages/Students/Index.cshtml*, ajoutez le code en surbrillance suivant pour créer un bouton **Search** et le chrome assorti.

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Le code précédent utilise le [Tag Helper](xref:mvc/views/tag-helpers/intro) `<form>` pour ajouter le bouton et la zone de texte de recherche. Par défaut, le Tag Helper `<form>` envoie les données de formulaire avec un POST. Avec POST, les paramètres sont passés dans le corps du message HTTP et non dans l’URL. Quand HTTP GET est utilisé, les données du formulaire sont transmises dans l’URL sous forme de chaînes de requête. La transmission des données avec des chaînes de requête permet aux utilisateurs d’ajouter l’URL aux favoris. Les [recommandations du W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) stipulent que GET doit être utilisé quand l’action ne produit pas de mise à jour.

Testez l’application :

* Sélectionnez l’onglet **Students** et entrez une chaîne de recherche.
* Sélectionnez **Search**.

Notez que l’URL contient la chaîne de recherche.

```html
http://localhost:5000/Students?SearchString=an
```

Si la page est dans les favoris, le favori contient l’URL de la page et la chaîne de requête `SearchString`. `method="get"` dans la balise `form` est ce qui a provoqué la génération de la chaîne de requête.

Actuellement, quand un lien de tri d’en-tête de colonne est sélectionné, la valeur du filtre de la zone **Search** est perdue. La valeur de filtre perdue est corrigée dans la section suivante.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Ajouter la fonctionnalité de pagination à la page d’index des étudiants

Dans cette section, nous allons créer une classe `PaginatedList` pour prendre en charge la pagination. La classe `PaginatedList` utilise des instructions `Skip` et `Take` pour filtrer les données sur le serveur au lieu de récupérer toutes les lignes de la table. L’illustration suivante montre les boutons de pagination.

![Page d’index des étudiants avec liens de pagination](sort-filter-page/_static/paging.png)

Dans le dossier du projet, créez `PaginatedList.cs` avec le code suivant :

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

La méthode `CreateAsync` dans le code précédent prend la taille de page et le numéro de page, et applique les instructions `Skip` et `Take` appropriées au `IQueryable`. Quand `ToListAsync` est appelée sur le `IQueryable`, elle retourne une liste contenant uniquement la page demandée. Les propriétés `HasPreviousPage` et `HasNextPage` sont utilisées pour activer ou désactiver les boutons de pagination **Previous** et **Next**.

La méthode `CreateAsync` est utilisée pour créer le `PaginatedList<T>`. Un constructeur ne peut pas créer l’objet `PaginatedList<T>` ; les constructeurs ne peuvent pas exécuter du code asynchrone.

## <a name="add-paging-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de pagination à la méthode Index

Dans *Students/Index.cshtml.cs*, mettez à jour le type de `Student` en remplaçant `IList<Student>` par `PaginatedList<Student>` :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Mettez à jour le `OnGetAsync` *Students/Index.cshtml.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Le code précédent ajoute l’index de page, le `sortOrder` actuel et le `currentFilter` à la signature de méthode.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Tous les paramètres sont null quand :

* La page est appelée à partir du lien **Students**.
* L’utilisateur n’a pas cliqué sur un lien de pagination ou de tri.

Quand l’utilisateur clique sur un lien de pagination, la variable d’index de page contient le numéro de page à afficher.

`CurrentSort` fournit l’ordre de tri actuel à la page Razor. L’ordre de tri actuel doit être inclus dans les liens de pagination afin de conserver l’ordre de tri lors de la pagination.

`CurrentFilter` fournit la chaîne de filtre actuelle à la page Razor. La valeur `CurrentFilter` :

* Doit être incluse dans les liens de pagination afin de conserver les paramètres de filtre lors de la pagination.
* Doit être restaurée à la zone de texte quand la page est réaffichée.

Si la chaîne de recherche est modifiée pendant la pagination, la page est réinitialisée à 1. La page doit être réinitialisée à 1, car le nouveau filtre peut entraîner l’affichage de données différentes. Quand une valeur de recherche est entrée et que le bouton **Submit** est sélectionné :

* La chaîne de recherche est changée.
* Le paramètre `searchString` n’est pas null.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

La méthode `PaginatedList.CreateAsync` convertit la requête d’étudiant en une seule page d’étudiants dans un type de collection qui prend en charge la pagination. Cette page unique d’étudiants est passée à la page Razor.

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Les deux points d’interrogation dans `PaginatedList.CreateAsync` représentent [l’opérateur de fusion de Null](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). L’opérateur de fusion de Null définit une valeur par défaut pour un type nullable. L’expression `(pageIndex ?? 1)` signifie qu’il faut retourner la valeur de `pageIndex` s’il a une valeur. Si `pageIndex` n’a pas de valeur, il faut retourner 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Ajouter des liens de pagination à la page Razor d’étudiant

Mettez à jour le balisage dans *Students/Index.cshtml*. Les modifications sont mises en surbrillance :

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Les liens d’en-tête de colonne utilisent la chaîne de requête pour passer la chaîne de recherche actuelle à la méthode `OnGetAsync` afin que l’utilisateur puisse trier dans les résultats du filtrage :

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

Les boutons de pagination sont affichés par des Tag Helpers :

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

Exécutez l’application et accédez à la page des étudiants.

* Pour vérifier que la pagination fonctionne, cliquez sur les liens de pagination dans différents ordres de tri.
* Pour vérifier que la pagination fonctionne correctement avec le tri et le filtrage, entrez une chaîne de recherche et essayez de changer de page.

![Page d’index des étudiants avec liens de pagination](sort-filter-page/_static/paging.png)

Pour mieux comprendre le fonctionnement du code

* Dans *Student/Index.cshtml.cs*, définissez un point d’arrêt sur `switch (sortOrder)`.
* Ajoutez un espion pour `NameSort`, `DateSort`, `CurrentSort` et `Model.Student.PageIndex`.
* Dans *Student/Index.cshtml.cshtml*, définissez un point d’arrêt sur `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Effectuez un pas à pas détaillé dans le débogueur.

## <a name="update-the-about-page-to-show-student-statistics"></a>Mettez à jour la page About pour afficher les statistiques sur les étudiants

Lors de cette étape, nous allons mettre à jour *Pages/About.cshtml* afin d’afficher le nombre d’étudiants qui se sont inscrits pour chaque date d’inscription. La mise à jour utilise le regroupement et comprend les étapes suivantes :

* Créer une classe de modèle d’affichage pour les données utilisées par la page **About**.
* Modifier la page Razor About et le modèle de page.

### <a name="create-the-view-model"></a>Créer le modèle d’affichage

Créez un dossier *SchoolViewModels* dans le dossier *Models*.

Dans le dossier *SchoolViewModels*, ajoutez un *EnrollmentDateGroup.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Mettre à jour le modèle de page About

Mettez à jour le fichier *Pages/About.cshtml.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/About.cshtml.cs)]

L’instruction LINQ regroupe les entités student par date d’inscription, calcule le nombre d’entités dans chaque groupe, et stocke les résultats dans une collection d’objets de modèle d’affichage `EnrollmentDateGroup`.

Remarque : La commande LINQ `group` n’est pas prise en charge actuellement par EF Core. Dans le code précédent, tous les enregistrements d’étudiants sont retournés à partir de SQL Server. La commande `group` est appliquée dans l’application Pages Razor, et non sur le serveur SQL Server. EF Core 2.1 prendra en charge cet opérateur LINQ `group`, et le regroupement se produira sur le serveur SQL Server. Consultez [Relational: Support translating GroupBy() to SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341) (Relationnel : Prendre en charge la traduction de GroupBy() vers SQL). [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) sera publié avec .NET Core 2.1. Pour plus d’informations, consultez la [Feuille de route .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Modifier la page Razor About

Remplacez le code du fichier *Pages/About.cshtml* par le code suivant :

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

Exécutez l’application et accédez à la page About. Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Page About](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Débogage d’une source ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)

Dans le didacticiel suivant, l’application utilise des migrations pour mettre à jour le modèle de données.

> [!div class="step-by-step"]
> [Précédent](xref:data/ef-rp/crud)
> [Suivant](xref:data/ef-rp/migrations)
