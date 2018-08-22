---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implémentation du référentiel et une unité de modèles de travail dans une Application ASP.NET MVC (partie 9 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 20f82744582faddaffdc5b4785bf208ecf0955a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835082"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implémentation du référentiel et une unité de modèles de travail dans une Application ASP.NET MVC (partie 9 sur 10)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels à partir du début ou [télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et commencez ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [télécharger le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire votre problème. Vous trouverez généralement la solution au problème en comparant votre code pour le code complet. Pour certaines erreurs courantes et comment les résoudre, consultez [erreurs et des solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Dans le didacticiel précédent, vous avez utilisé l’héritage pour réduire le code redondant dans le `Student` et `Instructor` classes d’entité. Dans ce didacticiel, vous verrez des façons d’utiliser le référentiel et une unité de travail des modèles pour les opérations CRUD. Comme dans le didacticiel précédent, dans celle-ci, vous allez modifier la façon dont votre code fonctionne avec des pages vous déjà créée plutôt que de créer de nouvelles pages.

## <a name="the-repository-and-unit-of-work-patterns"></a>Le référentiel et une unité de travail modèles

Le référentiel et unité de travail modèles sont destinés à créer une couche d’abstraction entre la couche d’accès aux données et la couche de logique métier d’une application. L’implémentation de ces modèles peut favoriser l’isolation de votre application face à des modifications dans le magasin de données et peut faciliter le test unitaire automatisé ou le développement piloté par les tests (TDD).

Dans ce didacticiel, vous allez implémenter une classe de référentiel pour chaque type d’entité. Pour le `Student` type d’entité vous allez créer une interface de référentiel et une classe de dépôt. Lorsque vous instanciez le référentiel dans votre contrôleur, vous allez utiliser l’interface afin que le contrôleur accepte une référence à n’importe quel objet qui implémente l’interface de référentiel. Lorsque le contrôleur s’exécute sous un serveur web, il reçoit un référentiel qui fonctionne avec Entity Framework. Lorsque le contrôleur s’exécute sous une classe de test unitaire, il reçoit un référentiel qui fonctionne avec les données stockées d’une manière qui vous pouvez de manipuler facilement pour le test, telle qu’une collection en mémoire.

Plus loin dans ce didacticiel, vous allez utiliser plusieurs référentiels et une classe d’unité de travail pour le `Course` et `Department` types d’entité dans le `Course` contrôleur. La classe d’unité de travail coordonne le travail de plusieurs référentiels en créant une classe de contexte de base de données unique partagée par tous les. Si vous souhaitez être en mesure d’effectuer des tests d’unités automatisés, vous créez et utilisez des interfaces pour ces classes de la même façon que vous l’avez fait pour le `Student` référentiel. Toutefois, pour simplifier le didacticiel, vous allez créer et utiliser ces classes sans interfaces.

L’illustration suivante montre une façon de conceptualiser les relations entre le contrôleur et les classes de contexte par rapport au ne pas à l’aide du référentiel ou modèle unité de travail du tout.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Vous ne créez des tests unitaires dans cette série de didacticiels. Pour une introduction au développement TDD avec une application MVC qui utilise le modèle de référentiel, consultez [procédure pas à pas : utilisation de TDD avec ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Pour plus d’informations sur le modèle de référentiel, consultez les ressources suivantes :

- [Le modèle dépôt](https://msdn.microsoft.com/library/ff649690.aspx) sur MSDN.
- [À l’aide de modèles de référentiel et unité de travail avec Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) sur le blog de l’équipe Entity Framework.
- [Agile Entity Framework 4 référentiel](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) série de billets sur le blog de Julie.
- [Création du compte à une Application de HTML5/jQuery coup de œil](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) sur le blog de Wahlin.

> [!NOTE]
> Il existe de nombreuses façons d’implémenter le référentiel et une unité de travail des modèles. Vous pouvez utiliser les classes du référentiel avec ou sans une classe d’unité de travail. Vous pouvez implémenter un référentiel unique pour tous les types d’entités, ou pour chaque type. Si vous implémentez un pour chaque type, vous pouvez utiliser des classes séparées, une classe de base générique et les classes dérivées, ou une classe de base abstraite et classes dérivées. Vous pouvez inclure la logique métier dans votre référentiel ou le limiter à la logique d’accès aux données. Vous pouvez également créer une couche d’abstraction dans votre classe de contexte de base de données à l’aide de [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfaces il au lieu de [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) types pour vos jeux d’entités. L’approche à l’implémentation d’une couche d’abstraction dans ce didacticiel est une option à prendre en compte, pas une recommandation pour tous les scénarios et environnements.


## <a name="creating-the-student-repository-class"></a>Création de la classe de référentiel étudiant

Dans le *DAL* dossier, créez un fichier de classe nommé *IStudentRepository.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ce code déclare un ensemble spécifique de méthodes CRUD, y compris les deux méthodes de lecture, qui retourne tous les `Student` entités et celui qui recherche un seul `Student` entité par ID.

Dans le *DAL* dossier, créez un fichier de classe nommé *StudentRepository.cs* fichier. Remplacez le code existant par le code suivant, qui implémente le `IStudentRepository` interface :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Le contexte de base de données est défini dans une variable de classe, et le constructeur attend l’objet à passer une instance du contexte de l’appelant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Vous pourriez instancier un nouveau contexte dans le référentiel, puis si vous avez utilisé plusieurs référentiels dans un seul contrôleur, chacun finiriez avec un contexte distinct. Plus tard, vous utiliserez plusieurs référentiels dans le `Course` contrôleur et vous verrez comment une classe d’unité de travail peut garantir que tous les dépôts utilisent le même contexte.

Implémente le référentiel [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) et supprime le contexte de base de données comme vous l’avez vu précédemment dans le contrôleur, et ses méthodes CRUD effectuer des appels vers le contexte de base de données dans la même façon que vous avez vu précédemment.

## <a name="change-the-student-controller-to-use-the-repository"></a>Modifier le contrôleur de l’étudiant pour utiliser le référentiel

Dans *StudentController.cs*, remplacez le code actuellement dans la classe par le code suivant. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Le contrôleur déclare maintenant une variable de classe pour un objet qui implémente le `IStudentRepository` interface au lieu de la classe de contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Le constructeur (sans paramètre) par défaut crée une nouvelle instance de contexte, et un constructeur facultatif permet à l’appelant de passer une instance de contexte.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Si vous utilisiez *l’injection de dépendances*, ou l’injection de dépendances, vous n’avez pas besoin du constructeur par défaut, car le logiciel de l’injection de dépendances garantit que l’objet de référentiel correcte serait toujours être fournie.)

Dans les méthodes CRUD, le référentiel est maintenant appelé à la place du contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Et le `Dispose` méthode supprime maintenant le référentiel au lieu du contexte :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Exécutez le site et cliquez sur le **étudiants** onglet.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La page recherche et fonctionne comme elle le faisait avant que vous avez modifié le code pour utiliser le référentiel, et les autres pages de l’étudiant fonctionnent également de la même. Toutefois, il existe une différence importante près de la façon la `Index` fait de la méthode du contrôleur de filtrage et d’organisation. La version d’origine de cette méthode contenait le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

La mise à jour `Index` méthode contient le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Seul le code en surbrillance a changé.

Dans la version d’origine du code, `students` est typé comme un `IQueryable` objet. La requête n’est envoyée à la base de données jusqu'à ce qu’il est converti en une collection à l’aide d’une méthode comme `ToList`, qui ne se produit pas jusqu'à ce que la vue Index accède au modèle d’étudiant. Le `Where` méthode dans le code d’origine ci-dessus devient un `WHERE` clause dans la requête SQL qui est envoyée à la base de données. À son tour, cela signifie que seules les entités sélectionnées sont retournées par la base de données. Toutefois, suite à la modification `context.Students` à `studentRepository.GetStudents()`, le `students` variable après cette instruction est un `IEnumerable` collection incluant tous les étudiants dans la base de données. Le résultat final de l’application de la `Where` méthode est identique, mais maintenant le travail est effectué dans la mémoire sur le serveur web et non par la base de données. Pour les requêtes qui renvoient de grands volumes de données, cela peut être inefficace.

> [!TIP]
> 
> **Visual Studio IQueryable. IEnumerable**
> 
> Une fois que vous implémentez le référentiel comme indiqué ici, même si vous entrez quelque chose dans le **recherche** boîte la requête envoyée à SQL Server retourne toutes les lignes d’étudiant, car il n’inclut pas vos critères de recherche :
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Cette requête retourne toutes les données de l’étudiant, car le référentiel exécuté la requête sans connaître les critères de recherche. Le processus de tri, l’application des critères de recherche et en sélectionnant un sous-ensemble des données pour la pagination (affichant uniquement 3 lignes dans ce cas) est effectuée dans la mémoire ultérieurement, lorsque le `ToPagedList` méthode est appelée sur le `IEnumerable` collection.
> 
> Dans la version précédente du code (avant que vous avez implémenté le référentiel), la requête n’est pas envoyée à la base de données jusqu'à une fois que vous appliquez les critères de recherche, lorsque `ToPagedList` est appelée sur le `IQueryable` objet.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Lorsque ToPagedList est appelée sur un `IQueryable` de l’objet, la requête envoyée à SQL Server spécifie la chaîne de recherche, ainsi que les lignes qui répondent aux critères de recherche sont retournés, et aucun filtrage ne doit être effectuée en mémoire.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Ce didacticiel explique comment examiner les requêtes envoyées à SQL Server.)


La section suivante montre comment implémenter des méthodes de référentiel qui vous permettent de spécifier que ce travail doit être effectué par la base de données.

Vous venez de créer une couche d’abstraction entre le contrôleur et le contexte de base de données Entity Framework. Si vous souhaitez effectuer des tests avec cette application d’unités automatisés, vous pouvez créer une classe de dépôt autre dans un projet de test unitaire qui implémente `IStudentRepository` *.* Au lieu d’appeler le contexte pour lire et écrire des données, cette classe de référentiel fictive peut manipuler des collections en mémoire afin de tester les fonctions du contrôleur.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implémenter un dépôt générique et une classe d’unité de travail

Création d’une classe de référentiel pour chaque type d’entité peut entraîner un grand nombre de code redondant, et cela peut entraîner des mises à jour partielles. Par exemple, supposons que vous devez mettre à jour les deux différents types d’entité dans le cadre de la même transaction. Si chacun utilise une instance de contexte de base de données distincte, un peut réussir et l’autre peut échouer. Permet de réduire le code redondant consiste à utiliser un dépôt générique et pour vous assurer que tous les dépôts utilisent le même contexte de base de données (et donc coordonnent les mises à jour) consiste à utiliser une classe d’unité de travail.

Dans cette section du didacticiel, vous allez créer un `GenericRepository` classe et un `UnitOfWork` classe et les utiliser dans le `Course` contrôleur pour accéder à la fois au `Department` et le `Course` jeux d’entités. Comme expliqué précédemment, pour simplifier cette partie du didacticiel, vous n’êtes pas Création d’interfaces de ces classes. Mais si vous souhaitez les utiliser pour faciliter le TDD, vous serez généralement les implémenter avec les interfaces de la même façon que vous l’avez fait la `Student` référentiel.

### <a name="create-a-generic-repository"></a>Créer un référentiel générique

Dans le *DAL* dossier, créez *GenericRepository.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Variables de classe sont déclarées pour le contexte de base de données et le jeu d’entités qui le référentiel est instancié pour :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Le constructeur accepte une instance de contexte de base de données et initialise la variable de jeu d’entités :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Le `Get` méthode utilise des expressions lambda pour permettre au code appelant de spécifier une condition de filtre et une colonne pour trier les résultats par, et un paramètre de chaîne permet à l’appelant de fournir une liste délimitée par des virgules des propriétés de navigation pour le chargement hâtif :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Le code `Expression<Func<TEntity, bool>> filter` signifie que l’appelant fournit une expression lambda selon le `TEntity` type et cette expression retourne une valeur booléenne. Par exemple, si le référentiel est instancié pour la `Student` type d’entité, le code dans la méthode appelante peut spécifier `student => student.LastName == "Smith` &quot; pour le `filter` paramètre.

Le code `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` signifie également que l’appelant fournit une expression lambda. Mais dans ce cas, l’entrée de l’expression est un `IQueryable` de l’objet pour le `TEntity` type. L’expression renvoie une version triée de qui `IQueryable` objet. Par exemple, si le référentiel est instancié pour la `Student` type d’entité, le code dans la méthode appelante peut spécifier `q => q.OrderBy(s => s.LastName)` pour le `orderBy` paramètre.

Le code dans le `Get` méthode crée un `IQueryable` de l’objet, puis applique l’expression de filtre le cas échéant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Il applique ensuite les expressions de chargement hâtif après l’analyse de la liste délimitée par des virgules :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Enfin, elle s’applique le `orderBy` expression s’il en existe un et retourne les résultats ; sinon, elle retourne les résultats à partir de la requête non classée :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Lorsque vous appelez le `Get` (méthode), vous pouvez effectuer le filtrage et le tri sur le `IEnumerable` collection retournée par la méthode au lieu de fournir des paramètres pour ces fonctions. Mais le tri et filtrage du travail puis reviennent en mémoire sur le serveur web. À l’aide de ces paramètres, vous assurer que le travail est effectué par la base de données plutôt que le serveur web. Une alternative consiste à créer des classes dérivées pour les types d’entité spécifique et ajouter spécialisé `Get` méthodes, telles que `GetStudentsInNameOrder` ou `GetStudentsByName`. Toutefois, dans une application complexe, cela peut entraîner un grand nombre de ces classes dérivées et les méthodes spécialisées, qui peut être plus de travail pour mettre à jour.

Le code dans le `GetByID`, `Insert`, et `Update` méthodes est similaire à ce que vous avez vu dans le référentiel non générique. (Vous n’êtes pas en fournissant un paramètre de chargement hâtif dans le `GetByID` signature, car vous ne pouvez pas effectuer un chargement hâtif avec la `Find` méthode.)

Deux surcharges sont fournies pour le `Delete` méthode :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Une des ces permet de vous passez simplement l’ID de l’entité à supprimer, et une prend une instance d’entité. Comme vous l’avez vu dans la [gère l’accès concurrentiel](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) (didacticiel), pour vous gestion concurrentielle nécessiter un `Delete` méthode qui prend une instance d’entité qui contient la valeur d’origine d’une propriété de suivi.

Ce dépôt générique gère les exigences types CRUD. Lorsqu’un type d’entité particulier a des exigences particulières, telles que le tri, ou de filtrage plus complexes, vous pouvez créer une classe dérivée qui propose des méthodes supplémentaires pour ce type.

## <a name="creating-the-unit-of-work-class"></a>Création de la classe d’unité de travail

La classe d’unité de travail a une fonction : pour vous assurer que lorsque vous utilisez plusieurs référentiels, ils partagent un contexte de base de données unique. Ainsi, quand une unité de travail est terminée vous pouvez appeler la `SaveChanges` méthode sur cette instance du contexte et s’assurer que toutes les associées modifications vont être coordonnées. Les besoins de la classe qu’un `Save` (méthode) et une propriété pour chaque référentiel. Chaque propriété de référentiel retourne une instance de référentiel qui a été instanciée à l’aide de la même instance de contexte de base de données que les autres instances de référentiel.

Dans le *DAL* dossier, créez un fichier de classe nommé *UnitOfWork.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Le code crée des variables de classe pour le contexte de base de données et chaque référentiel. Pour le `context` variable, un nouveau contexte est instancié :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Chaque propriété de référentiel vérifie si le référentiel existe déjà. Si ce n’est pas le cas, il instancie le référentiel, en passant l’instance de contexte. Par conséquent, tous les dépôts partagent la même instance de contexte.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Le `Save` les appels de méthode `SaveChanges` sur le contexte de base de données.

Comme n’importe quelle classe qui instancie un contexte de base de données dans une variable de classe, le `UnitOfWork` la classe implémente `IDisposable` et supprime le contexte.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Modification du contrôleur de cours pour utiliser les référentiels et les classe UnitOfWork

Remplacez le code que vous avez actuellement dans *CourseController.cs* avec le code suivant :

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Ce code ajoute une variable de classe pour la `UnitOfWork` classe. (Si vous utilisiez interfaces ici, vous n’aurait pas initialiser la variable ici ; au lieu de cela, vous devez implémenter un modèle des deux constructeurs tout comme vous l’avez fait le `Student` référentiel.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Dans le reste de la classe, toutes les références dans le contexte de base de données sont remplacées par des références à du référentiel approprié, à l’aide de `UnitOfWork` propriétés pour accéder au référentiel. Le `Dispose` méthode supprime le `UnitOfWork` instance.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Exécutez le site et cliquez sur le **cours** onglet.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La page recherche et fonctionne comme elle le faisait avant vos modifications et les autres pages des cours fonctionnent également de la même.

## <a name="summary"></a>Récapitulatif

Vous avez implémenté le référentiel et l’unité de travail des modèles. Vous avez utilisé des expressions lambda comme paramètres de méthode dans le référentiel générique. Pour plus d’informations sur l’utilisation de ces expressions avec un `IQueryable` d’objets, consultez [IQueryable(T) Interface (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) dans MSDN Library. Dans le prochain didacticiel, vous allez apprendre à gérer certains scénarios avancés.

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
