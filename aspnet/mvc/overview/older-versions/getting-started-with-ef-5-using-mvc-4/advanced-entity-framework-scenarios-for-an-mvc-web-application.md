---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Advanced scénarios Entity Framework pour une Application Web MVC (10 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5fa6ff439790f70cd60d0985a791df26d69c7107
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827443"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scénarios Entity Framework avancés pour une Application Web MVC (10 sur 10)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels à partir du début ou [télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et commencez ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [télécharger le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire votre problème. Vous trouverez généralement la solution au problème en comparant votre code pour le code complet. Pour certaines erreurs courantes et comment les résoudre, consultez [erreurs et des solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Dans le didacticiel précédent vous implémenté le référentiel et une unité de travail des modèles. Ce didacticiel couvre les rubriques suivantes :

- Exécution de requêtes SQL brutes.
- Effectuer un pas de suivi des requêtes.
- Examen des requêtes envoyées à la base de données.
- Utilisation des classes de proxy.
- La désactivation de la détection automatique des modifications.
- Désactivation de la validation lors de l’enregistrement de modifications.
- [Erreurs et des solutions de contournement](#errors)

Pour la plupart de ces rubriques, vous allez travailler avec des pages que vous avez déjà créé. Pour utiliser des requêtes SQL brutes pour effectuer des mises à jour en bloc, vous allez créer une nouvelle page qui met à jour le nombre de crédits de tous les cours dans la base de données :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Et pour utiliser une requête de suivi de non, vous allez ajouter nouvelle logique de validation à la page Department Edit :

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Requêtes SQL brutes performantes

L’API Entity Framework Code First inclut des méthodes qui vous permettent de transmettre des commandes SQL directement à la base de données. Les options suivantes sont disponibles :

- Utilisez la méthode `DbSet.SqlQuery` pour les requêtes qui renvoient des types d’entités. Les objets retournés doivent être du type attendu par le `DbSet` objet et elles sont suivies automatiquement par le contexte de base de données, sauf si vous désactivez le suivi. (Consultez la section suivante le `AsNoTracking` méthode.)
- Utilisez le `Database.SqlQuery` méthode pour les requêtes qui retournent des types qui ne sont pas des entités. Les données renvoyées ne font pas l’objet d’un suivi par le contexte de base de données, même si vous utilisez cette méthode pour récupérer des types d’entités.
- Utilisez le [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) pour les commandes de requête non.

L’un des avantages d’utiliser Entity Framework est que cela évite de lier votre code trop étroitement à une méthode particulière de stockage des données. Il le fait en générant des requêtes et des commandes SQL pour vous, ce qui vous évite d’avoir à les écrire vous-même. Mais il existe des scénarios exceptionnels lorsque vous avez besoin exécuter des requêtes SQL spécifiques que vous avez créé manuellement, et ces méthodes permettent de vous permettant de gérer ces exceptions.

Comme c’est toujours le cas lorsque vous exécutez des commandes SQL dans une application web, vous devez prendre des précautions pour protéger votre site contre des attaques par injection de code SQL. Une manière de procéder consiste à utiliser des requêtes paramétrables pour vous assurer que les chaînes soumises par une page web ne peuvent pas être interprétées comme des commandes SQL. Dans ce didacticiel, vous utiliserez des requêtes paramétrables lors de l’intégration de l’entrée utilisateur dans une requête.

### <a name="calling-a-query-that-returns-entities"></a>Appel d’une requête qui retourne des entités

Supposons que vous vouliez la `GenericRepository` classe pour fournir plus de filtrage et de tri flexibilité sans nécessiter que vous créez une classe dérivée avec des méthodes supplémentaires. Pour cela, qui serait d’ajouter une méthode qui accepte une requête SQL. Vous pouvez ensuite spécifier n’importe quel type de filtrage ou de tri vous voulez dans le contrôleur, tel qu’un `Where` clause qui dépend d’un joint ou une sous-requête. Dans cette section, vous verrez comment implémenter une telle méthode.

Créer le `GetWithRawSql` méthode en ajoutant le code suivant à *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

Dans *CourseController.cs*, appelez la nouvelle méthode à partir de la `Details` méthode, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Dans ce cas vous auriez pu utiliser le `GetByID` (méthode), mais vous êtes à l’aide de la `GetWithRawSql` méthode pour vérifier que le `GetWithRawSQL` fonctionne de la méthode.

Exécutez la page de détails pour vérifier que la requête select works (sélectionnez le **cours** onglet, puis **détails** pour un cours).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Appel d’une requête qui retourne les autres Types d’objets

Précédemment, vous avez créé une grille de statistiques des étudiants pour la page About, qui montrait le nombre d’étudiants pour chaque date d’inscription. Le code qui s’en charge dans *HomeController.cs* utilise LINQ :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Supposons que vous voulez écrire le code qui récupère ces données directement dans SQL plutôt que de l’utilisation de LINQ. Pour faire que vous avez besoin exécuter une requête qui renvoie autre chose que les objets d’entité, ce qui signifie que vous devez utiliser le `Database.SqlQuery` (méthode).

Dans *HomeController.cs*, remplacez l’instruction LINQ dans le `About` méthode avec le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Exécutez la page About. Elle affiche les mêmes données qu’auparavant.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Appel d’une requête de mise à jour

Supposons que les administrateurs de Contoso University veulent être en mesure d’effectuer des modifications en bloc dans la base de données, par exemple en modifiant le nombre de crédits pour chaque cours. Si l’université a un grand nombre de cours, il serait inefficace de les récupérer tous sous forme d’entités et de les modifier individuellement. Dans cette section, vous allez implémenter une page web qui permet à l’utilisateur spécifier un taux de modification du nombre de crédits pour tous les cours, et vous effectuerez la modification en exécutant une instance SQL `UPDATE` instruction. La page web ressemblera à l’illustration suivante :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Dans le didacticiel précédent, vous avez utilisé le dépôt générique pour lire et mettre à jour `Course` entités dans le `Course` contrôleur. Pour cette opération de mise à jour en bloc, vous devez créer une nouvelle méthode de référentiel n’est pas dans le référentiel générique. Pour ce faire, vous allez créer un dédié `CourseRepository` classe qui dérive de la `GenericRepository` classe.

Dans le *DAL* dossier, créez *CourseRepository.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

Dans *UnitOfWork.cs*, modifiez le `Course` type de référentiel à partir de `GenericRepository<Course>` à `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

Dans *CourseContoller.cs*, ajoutez un `UpdateCourseCredits` méthode :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Cette méthode servira pour les deux `HttpGet` et `HttpPost`. Lorsque le `HttpGet` `UpdateCourseCredits` exécutions de méthode, le `multiplier` variable sera null et la vue affiche une zone de texte vide et un bouton d’envoi, comme illustré dans l’illustration précédente.

Lorsque le **mise à jour** bouton et le `HttpPost` exécutions de méthode, `multiplier` aura la valeur entrée dans la zone de texte. Le code appelle ensuite le référentiel `UpdateCourseCredits` (méthode), qui retourne le nombre de lignes d’affectées, et cette valeur est stockée dans le `ViewBag` objet. Lorsque la vue reçoit le nombre de lignes affectées dans le `ViewBag` de l’objet, il affiche ce nombre au lieu de la zone de texte et bouton, envoyer, comme indiqué dans l’illustration suivante :

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Créer une vue dans le *Views\Course* dossier pour la page de crédits de cours de mise à jour :

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

Dans *Views\Course\UpdateCourseCredits.cshtml*, remplacez le code du modèle par le code suivant :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Exécutez la méthode `UpdateCourseCredits` en sélectionnant l’onglet **Courses**, puis en ajoutant « /UpdateCourseCredits » à la fin de l’URL dans la barre d’adresse du navigateur (par exemple : `http://localhost:50205/Course/UpdateCourseCredits`). Entrez un nombre dans la zone de texte :

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Cliquez sur **Mettre à jour**. Vous voyez le nombre de lignes affectées :

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Cliquez sur **Revenir à la liste** pour afficher la liste des cours avec le nombre révisé de crédits.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Pour plus d’informations sur les requêtes SQL brutes, consultez [requêtes SQL brutes](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) sur le blog de l’équipe Entity Framework.

## <a name="no-tracking-queries"></a>sans suivi

Quand un contexte de base de données récupère les lignes de base de données et crée des objets d’entité qui les représentent, par défaut il effectue le suivi de si les entités en mémoire sont synchronisées avec les nouveautés de la base de données. Les données en mémoire agissent comme un cache et sont utilisées quand vous mettez à jour une entité. Cette mise en cache est souvent inutile dans une application web, car les instances de contexte ont généralement une durée de vie courte (une instance est créée puis supprimée pour chaque requête) et le contexte qui lit une entité est généralement supprimé avant que cette entité soit réutilisée.

Vous pouvez spécifier si le contexte effectue le suivi des objets d’entité pour une requête à l’aide de la `AsNoTracking` (méthode). Voici des scénarios classiques où vous voulez procéder ainsi :

- La requête récupère un gros volume de données que la désactivation du suivi peuvent améliorer sensiblement les performances.
- Vous souhaitez attacher une entité afin de mettre à jour, mais que vous avez récupérée précédemment la même entité à des fins différentes. Comme l’entité est déjà suivie par le contexte de base de données, vous ne pouvez pas attacher l’entité que vous voulez modifier. Un pour éviter ce problème consiste à utiliser le `AsNoTracking` option avec la requête précédente.

Dans cette section, vous allez implémenter une logique métier qui illustre le second de ces scénarios. Plus précisément, vous allez appliquer une règle d’entreprise qui indique qu’un formateur ne peut pas être l’administrateur de plusieurs départements.

Dans *DepartmentController.cs*, ajoutez une nouvelle méthode que vous pouvez appeler à partir de la `Edit` et `Create` méthodes pour vous assurer qu’aucun deux départements n’ont le même administrateur :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Ajouter du code dans le `try` de blocs à la `HttpPost` `Edit` méthode à appeler cette nouvelle méthode, s’il n’existe aucune erreur de validation. Le `try` bloc ressemble maintenant à l’exemple suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Exécutez la page Department Edit et essayez de modifier l’administrateur d’un service d’entreprise à un formateur qui est déjà l’administrateur d’un autre service. Vous obtenez le message d’erreur attendu :

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Exécutez maintenant à nouveau la page Department Edit et ce changement d’heure le **Budget** quantité. Lorsque vous cliquez sur **enregistrer**, vous voyez une page d’erreur :

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Le message d’erreur exception est «`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`» cela s’est produit en raison de la séquence d’événements suivante :

- Le `Edit` les appels de méthode le `ValidateOneAdministratorAssignmentPerInstructor` (méthode), qui Récupère tous les services qui ont Kim Abercrombie en tant que leur administrateur. Qui entraîne le département « English » à lire. Parce que c’est le service en cours de modification, aucune erreur n’est signalée. À la suite de cette opération de lecture, toutefois, l’entité de département « English » qui a été lu à partir de la base de données est maintenant suivie par le contexte de base de données.
- Le `Edit` méthode tente de définir la `Modified` indicateur sur l’anglais entité department créée par le binder de modèle MVC, mais cette tentative échoue, car le contexte suit déjà une entité pour le département « English ».

Une solution à ce problème consiste à conserver le contexte du suivi des entités en mémoire département récupérées par la requête de validation. Il n’existe aucun inconvénient à cela, étant donné que vous ne sont pas être mise à jour cette entité ou de les lire à nouveau d’une manière qui gagnerait à elle être mis en cache en mémoire.

Dans *DepartmentController.cs*, dans le `ValidateOneAdministratorAssignmentPerInstructor` (méthode), ne spécifiez aucun suivi, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Répétez votre tentative de modifier le **Budget** montant d’un service. Cette fois l’opération a réussi, et le site retourne normalement à la page d’Index des départements, affichage de la valeur de budget révisé.

## <a name="examining-queries-sent-to-the-database"></a>Examen des requêtes envoyées à la base de données

Il est parfois utile de pouvoir voir les requêtes SQL réelles qui sont envoyées à la base de données. Pour ce faire, vous pouvez examiner une variable de requête dans le débogueur ou appeler la requête `ToString` (méthode). Pour essayer, vous observez une requête simple et observez ce qui se passe à celui-ci dès que vous ajoutez des options telles chargement hâtif, filtrage et de tri.

Dans *contrôleurs/CourseController*, remplacez le `Index` méthode avec le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Maintenant définir un point d’arrêt dans *GenericRepository.cs* sur le `return query.ToList();` et `return orderBy(query).ToList();` instructions de la `Get` (méthode). Exécutez le projet en mode débogage et sélectionnez la page d’Index des cours. Lorsque le code atteint le point d’arrêt, examiner le `query` variable. Affiche la requête est envoyée à SQL Server. Il est une simple `Select` instruction :

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Requêtes peuvent être trop longs pour s’afficher dans les fenêtres de débogage dans Visual Studio. Pour afficher l’intégralité de la requête, vous pouvez copier la valeur de la variable et collez-le dans un éditeur de texte :

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Vous allez maintenant ajouter une liste déroulante à la page Index des cours afin que les utilisateurs peuvent filtrer pour un service particulier. Vous allez trier les cours par titre, et vous devez spécifier un chargement hâtif pour la `Department` propriété de navigation. Dans *CourseController.cs*, remplacez le `Index` méthode avec le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

La méthode reçoit la valeur sélectionnée de la liste déroulante dans le `SelectedDepartment` paramètre. Si rien n’est sélectionné, ce paramètre sera null.

Un `SelectList` collection contenant tous les services est passée à la vue pour obtenir la liste déroulante. Les paramètres passés à la `SelectList` constructeur spécifier le nom de champ de valeur, le nom de champ de texte et l’élément sélectionné.

Pour le `Get` méthode de la `Course` référentiel, le code spécifie une expression de filtre, un ordre de tri et un chargement hâtif pour la `Department` propriété de navigation. Retourne l’expression de filtre est toujours `true` si rien n’est sélectionné dans la liste déroulante (autrement dit, `SelectedDepartment` a la valeur null).

Dans *Views\Course\Index.cshtml*, immédiatement avant l’ouverture `table` , ajoutez le code suivant pour créer la liste déroulante et un bouton d’envoi :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Avec les points d’arrêt toujours définis le `GenericRepository` (classe), exécutez la page Index des cours. Suivez les instructions des tout d’abord deux fois que le code atteint un point d’arrêt, afin que la page s’affiche dans le navigateur. Sélectionnez un service dans la liste déroulante et cliquez sur **filtre**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Cette fois, le premier point d’arrêt sera pour la requête de services pour obtenir la liste déroulante. Ignorez qui et affichez la `query` variable la prochaine fois que le code atteint le point d’arrêt pour voir ce que le `Course` ressemble à présent de la requête. Vous verrez quelque chose comme ce qui suit :

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Vous pouvez voir que la requête est maintenant un `JOIN` requête charge `Department` données avec le `Course` données, et qu’elle inclut un `WHERE` clause.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Utilisation des Classes de Proxy

Quand Entity Framework crée des instances d’entité (par exemple, lorsque vous exécutez une requête), il crée souvent sous la forme d’instances d’un type dérivé généré dynamiquement qui agit comme un proxy pour l’entité. Ce proxy substitue certaines propriétés virtuelles de l’entité à insérer des raccordements pour effectuer des actions automatiquement lorsque la propriété est accessible. Par exemple, ce mécanisme est utilisé pour prendre en charge le chargement différé des relations.

La plupart du temps vous n’avez pas besoin de connaître cette utilisation de proxys, mais il existe des exceptions :

- Dans certains scénarios, vous souhaiterez empêcher la création d’instances de proxy d’Entity Framework. Par exemple, la sérialisation des instances de proxy non peut être plus efficace que la sérialisation des instances de proxy.
- Lorsque vous instanciez une classe d’entité à l’aide du `new` opérateur, vous n’obtenez pas une instance de proxy. Cela signifie que vous n’obtenez pas des fonctionnalités telles que le suivi des modifications automatique et le chargement différé. Il s’agit généralement OK ; Vous ne devez généralement le chargement différé, étant donné que vous créez une nouvelle entité qui n’est pas dans la base de données, et vous ne devez généralement pas le suivi des modifications si vous êtes marquant explicitement l’entité en tant que `Added`. Toutefois, si vous n’avez pas besoin d’un chargement différé et que vous avez besoin de suivi des modifications, vous pouvez créer des nouvelles instances d’entités avec les proxys à l’aide de la `Create` méthode de la `DbSet` classe.
- Vous souhaiterez peut-être obtenir un type d’entité réelle à partir d’un type de proxy. Vous pouvez utiliser la `GetObjectType` méthode de la `ObjectContext` classe pour obtenir le type d’entité réelle d’une instance de type de proxy.

Pour plus d’informations, consultez [utilisation de proxys](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) sur le blog de l’équipe Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>La désactivation de la détection automatique des modifications

Entity Framework détermine la manière dont une entité a changé (et par conséquent les mises à jour qui doivent être envoyées à la base de données) en comparant les valeurs en cours d’une entité avec les valeurs d’origine. Les valeurs d’origine sont stockées lorsque l’entité a été interrogée ou attachée. Certaines des méthodes qui provoquent la détection automatique des modifications sont les suivantes :

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si vous effectuez le suivi un grand nombre d’entités et que vous appelez une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives des performances en désactivant temporairement la détection de modification automatique à l’aide du [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) propriété. Pour plus d’informations, consultez [détecter automatiquement les modifications](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Désactivation de la Validation lors de l’enregistrement de modifications

Lorsque vous appelez le `SaveChanges` (méthode), par défaut, Entity Framework valide les données dans toutes les propriétés de toutes les entités modifiées avant la mise à jour de la base de données. Si vous avez mis à jour un grand nombre d’entités et vous avez déjà validé les données, ce travail n’est pas nécessaire et vous pouvez créer le processus d’enregistrement les modifications prennent moins de temps en désactivant temporairement la validation. Vous pouvez faire cela à l’aide du [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) propriété. Pour plus d’informations, consultez [Validation](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Récapitulatif

Cette étape termine cette série de didacticiels sur l’utilisation d’Entity Framework dans une application ASP.NET MVC. Vous trouverez des liens vers d’autres ressources Entity Framework dans le [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

Pour plus d’informations sur la façon de déployer votre application web une fois que vous l’avez créé, consultez [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx) dans MSDN Library.

Pour plus d’informations sur les autres rubriques relatives à MVC, telles que l’authentification et l’autorisation, consultez le [ressources recommandées de MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Remerciements

- Tom Dykstra a écrit la version d’origine de ce didacticiel et est rédactrice en programmation senior de l’équipe de contenu d’outils et de Microsoft Web Platform.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) co-écrit ce didacticiel et avez effectué la plupart du travail sa mise à jour d’EF 5 et MVC 4. Rick est rédactrice en programmation senior pour Microsoft Azure et de MVC.
- [Rowan Miller](http://www.romiller.com) et autres membres de l’équipe Entity Framework assisté avec les révisions de code et aidé au débogage de nombreux problèmes avec les migrations qui ont surgi pendant que nous étions en train de mettre à jour le didacticiel pour EF 5.

## <a name="vb"></a>VB

Lorsque le didacticiel a été généré au départ, nous avons fourni des versions de c# et VB de projet téléchargement terminé. Avec cette mise à jour, nous offrons un projet téléchargeable c# pour chaque chapitre faciliter la prise en main n’importe où dans la série, mais en raison de contraintes de temps et d’autres priorités que nous n’avez pas que pour VB. Si vous générez un projet Visual Basic à l’aide de ces didacticiels et que vous êtes prêt à partager avec d’autres personnes, faites-le nous savoir.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Erreurs et des solutions de contournement

### <a name="cannot-createshadow-copy"></a>Ne peut pas créer/shadow copy

Message d’erreur :

*Ne peut pas créer/shadow copy 'DotNetOpenAuth.OpenId' lorsque ce fichier existe déjà.*

Solution :

Attendez quelques secondes et actualisez la page.

### <a name="update-database-not-recognized"></a>Mise à jour la base de données n’est ne pas reconnu.

Message d’erreur :

*Le terme « Update-Database » n’est pas reconnu comme le nom de l’applet de commande, fonction, fichier de script ou programme exécutable. Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct et réessayez.* (À partir de la *`Update-Database`* dans PMC.)

Solution :

Quittez Visual Studio. Rouvrez le projet, puis réessayez.

### <a name="validation-failed"></a>Échoué de la validation

Message d’erreur :

*Échec de la validation pour une ou plusieurs entités. Consultez la propriété 'EntityValidationErrors' pour plus d’informations.* (À partir de la *`Update-Database`* dans PMC.)

Solution :

Une des causes de ce problème sont d’erreurs de validation lorsque la `Seed` exécutions de méthode. Consultez [amorçage et débogage Entity Framework (EF) des bases de données](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) pour obtenir des conseils sur le débogage de la `Seed` (méthode).

### <a name="http-50019-error"></a>HTTP 500.19 erreur

Message d’erreur :

*Erreur HTTP 500.19 - erreur interne du serveur la page demandée est inaccessible, car les données de configuration de la page ne sont pas valides.*

Solution :

Vous pouvez obtenir cette erreur consiste à partir de plusieurs copies de la solution, chacun d’eux à l’aide du même numéro de port. Vous pouvez généralement résoudre ce problème en quitter toutes les instances de Visual Studio, puis le redémarrage de votre travail sur le projet. Si cela ne fonctionne pas, essayez de modifier le numéro de port. Cliquez avec le bouton droit sur le fichier projet, puis sur Propriétés. Sélectionnez le **Web** onglet et modifiez le numéro de port dans le **Url du projet** zone de texte.

### <a name="error-locating-sql-server-instance"></a>Erreur lors de la localisation de l’instance SQL Server

Message d’erreur :

*Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server. Le serveur est introuvable ou n’est pas accessible. Vérifiez que le nom d’instance est correct et que SQL Server est configuré pour autoriser les connexions à distance. (fournisseur : Interfaces de réseau SQL, erreur : 26 - erreur de localisation Server/Instance spécifié)*

Solution :

Vérifiez la chaîne de connexion. Si vous avez supprimé manuellement la base de données, modifiez le nom de la base de données dans la chaîne de construction.

> [!div class="step-by-step"]
> [Précédent](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Suivant](building-the-ef5-mvc4-chapter-downloads.md)
