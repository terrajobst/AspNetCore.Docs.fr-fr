---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Advanced scénarios Entity Framework 6 pour une Application Web 5 de MVC (partie 12 sur 12) | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio...
ms.author: aspnetcontent
ms.date: 12/08/2014
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 82fe650712d39558668ab02b7cc410ab0540d00f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836692"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Scénarios Entity Framework avancés 6 pour une Application Web 5 de MVC (partie 12 sur 12)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans le didacticiel précédent, vous avez implémenté l’héritage table par hiérarchie. Ce didacticiel inclut présente plusieurs rubriques qui sont utiles à connaître lorsque vous allez au-delà les principes fondamentaux du développement d’applications web ASP.NET qui utilisent Entity Framework Code First. Instructions pas à pas vous guident tout le code et à l’aide de Visual Studio pour les rubriques suivantes :

- [Exécution de requêtes SQL brutes](#rawsql)
- [Pas de suivi des requêtes](#notracking)
- [Examen SQL envoyées à la base de données](#sql)

Le didacticiel présente plusieurs rubriques avec de brèves présentations suivies des liens vers des ressources pour plus d’informations :

- [Unité de modèles de travail et de référentiel](#repo)
- [Classes de proxy](#proxies)
- [Détection automatique des modifications](#changedetection)
- [Validation automatique](#validation)
- [Outils Entity Framework pour Visual Studio](#tools)
- [Code source d’Entity Framework](#source)

Le didacticiel comprend également les sections suivantes :

- [Résumé](#summary)
- [Accusés de réception](#acknowledgments)
- [Remarque à propos des VB](#vb)
- [Les erreurs courantes et les solutions ou les solutions de contournement pour les](#errors)

Pour la plupart de ces rubriques, vous allez travailler avec des pages que vous avez déjà créé. Pour utiliser des requêtes SQL brutes pour effectuer des mises à jour en bloc, vous allez créer une nouvelle page qui met à jour le nombre de crédits de tous les cours dans la base de données :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Requêtes SQL brutes performantes

L’API Entity Framework Code First inclut des méthodes qui vous permettent de transmettre des commandes SQL directement à la base de données. Les options suivantes sont disponibles :

- Utilisez le [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) méthode pour les requêtes qui retournent des types d’entité. Les objets retournés doivent être du type attendu par le `DbSet` objet et elles sont suivies automatiquement par le contexte de base de données, sauf si vous désactivez le suivi. (Consultez la section suivante le [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) méthode.)
- Utilisez le [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) méthode pour les requêtes qui retournent des types qui ne sont pas des entités. Les données renvoyées ne font pas l’objet d’un suivi par le contexte de base de données, même si vous utilisez cette méthode pour récupérer des types d’entités.
- Utilisez le [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) pour les commandes de requête non.

L’un des avantages d’utiliser Entity Framework est que cela évite de lier votre code trop étroitement à une méthode particulière de stockage des données. Il le fait en générant des requêtes et des commandes SQL pour vous, ce qui vous évite d’avoir à les écrire vous-même. Mais il existe des scénarios exceptionnels lorsque vous avez besoin exécuter des requêtes SQL spécifiques que vous avez créé manuellement, et ces méthodes permettent de vous permettant de gérer ces exceptions.

Comme c’est toujours le cas lorsque vous exécutez des commandes SQL dans une application web, vous devez prendre des précautions pour protéger votre site contre des attaques par injection de code SQL. Une manière de procéder consiste à utiliser des requêtes paramétrables pour vous assurer que les chaînes soumises par une page web ne peuvent pas être interprétées comme des commandes SQL. Dans ce didacticiel, vous utiliserez des requêtes paramétrables lors de l’intégration de l’entrée utilisateur dans une requête.

### <a name="calling-a-query-that-returns-entities"></a>Appel d’une requête qui retourne des entités

Le [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) classe fournit une méthode que vous pouvez utiliser pour exécuter une requête qui retourne une entité de type `TEntity`. Pour voir comment cela fonctionne vous allez modifier le code dans le `Details` méthode de la `Department` contrôleur.

Dans *DepartmentController.cs*, dans le `Details` (méthode), remplacez le `db.Departments.FindAsync` appel de méthode avec un `db.Departments.SqlQuery` appel de méthode, comme indiqué dans le code en surbrillance suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Pour vérifier que le nouveau code fonctionne correctement, sélectionnez l’onglet **Departments**, puis **Details** pour l’un des services.

![Détails du service](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Appel d’une requête qui retourne les autres Types d’objets

Précédemment, vous avez créé une grille de statistiques des étudiants pour la page About, qui montrait le nombre d’étudiants pour chaque date d’inscription. Le code qui s’en charge dans *HomeController.cs* utilise LINQ :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Supposons que vous voulez écrire le code qui récupère ces données directement dans SQL plutôt que de l’utilisation de LINQ. Pour faire que vous avez besoin exécuter une requête qui renvoie autre chose que les objets d’entité, ce qui signifie que vous devez utiliser le [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) (méthode).

Dans *HomeController.cs*, remplacez l’instruction LINQ dans le `About` méthode avec une instruction SQL, comme indiqué dans le code en surbrillance suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Exécutez la page About. Elle affiche les mêmes données qu’auparavant.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Appel d’une requête de mise à jour

Supposons que les administrateurs de Contoso University veulent être en mesure d’effectuer des modifications en bloc dans la base de données, par exemple en modifiant le nombre de crédits pour chaque cours. Si l’université a un grand nombre de cours, il serait inefficace de les récupérer tous sous forme d’entités et de les modifier individuellement. Dans cette section, vous allez implémenter une page web qui permet à l’utilisateur spécifier un taux de modification du nombre de crédits pour tous les cours, et vous effectuerez la modification en exécutant une instance SQL `UPDATE` instruction. La page web ressemblera à l’illustration suivante :

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

Dans *CourseContoller.cs*, ajouter `UpdateCourseCredits` méthodes pour `HttpGet` et `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Lorsque le contrôleur traite une `HttpGet` demande, rien n’est retourné dans le `ViewBag.RowsAffected` variable et la vue affiche une zone de texte vide et un bouton d’envoi, comme indiqué dans l’illustration précédente.

Lorsque le **mise à jour** bouton est activé, le `HttpPost` méthode est appelée, et `multiplier` a la valeur entrée dans la zone de texte. Le code exécute ensuite l’instruction SQL qui met à jour des cours et retourne le nombre de lignes affectées à la vue dans le `ViewBag.RowsAffected` variable. Lorsque la vue Obtient une valeur dans cette variable, il affiche le nombre de lignes mises à jour au lieu de la zone de texte et bouton, envoyer, comme indiqué dans l’illustration suivante :

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Dans *CourseController.cs*, avec le bouton droit de la `UpdateCourseCredits` méthodes, puis cliquez sur **ajouter une vue**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Dans *Views\Course\UpdateCourseCredits.cshtml*, remplacez le code du modèle par le code suivant :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Exécutez la méthode `UpdateCourseCredits` en sélectionnant l’onglet **Courses**, puis en ajoutant « /UpdateCourseCredits » à la fin de l’URL dans la barre d’adresse du navigateur (par exemple : `http://localhost:50205/Course/UpdateCourseCredits`). Entrez un nombre dans la zone de texte :

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Cliquez sur **Mettre à jour**. Vous voyez le nombre de lignes affectées :

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Cliquez sur **Revenir à la liste** pour afficher la liste des cours avec le nombre révisé de crédits.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Pour plus d’informations sur les requêtes SQL brutes, consultez [requêtes SQL brutes](https://msdn.microsoft.com/data/jj592907) sur MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>sans suivi

Quand un contexte de base de données récupère des lignes de table et crée des objets entité qui les représentent, par défaut, il effectue le suivi du fait que les entités en mémoire sont ou non synchronisées avec ce qui se trouve dans la base de données. Les données en mémoire agissent comme un cache et sont utilisées quand vous mettez à jour une entité. Cette mise en cache est souvent inutile dans une application web, car les instances de contexte ont généralement une durée de vie courte (une instance est créée puis supprimée pour chaque requête) et le contexte qui lit une entité est généralement supprimé avant que cette entité soit réutilisée.

Vous pouvez désactiver le suivi d’objets d’entité dans la mémoire à l’aide de la [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) (méthode). Voici des scénarios classiques où vous voulez procéder ainsi :

- Une requête récupère un gros volume de données que la désactivation du suivi peuvent améliorer sensiblement les performances.
- Vous souhaitez attacher une entité afin de mettre à jour, mais que vous avez récupérée précédemment la même entité à des fins différentes. Comme l’entité est déjà suivie par le contexte de base de données, vous ne pouvez pas attacher l’entité que vous voulez modifier. Pour gérer cette situation consiste à utiliser le `AsNoTracking` option avec la requête précédente.

Pour obtenir un exemple qui montre comment utiliser le [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) (méthode), consultez [la version antérieure de ce didacticiel](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Cette version du didacticiel ne définit pas l’indicateur modifié sur une entité de binder de modèle créé dans la méthode Edit, donc il n’a pas besoin `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Examen SQL envoyées à la base de données

Il est parfois utile de pouvoir voir les requêtes SQL réelles qui sont envoyées à la base de données. Dans un tutoriel précédent, vous avez vu comment procéder dans le code de l’intercepteur. vous voyez maintenant quelques méthodes permettant de le faire sans écrire de code de l’intercepteur. Pour essayer, vous observez une requête simple et observez ce qui se passe à celui-ci dès que vous ajoutez des options telles chargement hâtif, filtrage et de tri.

Dans *contrôleurs/CourseController*, remplacez le `Index` méthode avec le code suivant, pour arrêter temporairement le chargement hâtif :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Maintenant définir un point d’arrêt sur la `return` instruction (F9 avec le curseur sur cette ligne). Appuyez sur F5 pour exécuter le projet en mode débogage, puis sélectionnez la page d’Index des cours. Lorsque le code atteint le point d’arrêt, examiner le `sql` variable. Affiche la requête est envoyée à SQL Server. Il est une simple `Select` instruction.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Cliquez sur la classe loupe pour afficher la requête dans le **visualiseur de texte**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Vous allez maintenant ajouter une liste déroulante à la page d’Index des cours afin que les utilisateurs peuvent filtrer pour un service particulier. Vous allez trier les cours par titre, et vous devez spécifier un chargement hâtif pour la `Department` propriété de navigation.

Dans *CourseController.cs*, remplacez le `Index` méthode avec le code suivant :

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaurer le point d’arrêt sur la `return` instruction.

La méthode reçoit la valeur sélectionnée de la liste déroulante dans le `SelectedDepartment` paramètre. Si rien n’est sélectionné, ce paramètre sera null.

Un `SelectList` collection contenant tous les services est passée à la vue pour obtenir la liste déroulante. Les paramètres passés à la `SelectList` constructeur spécifier le nom de champ de valeur, le nom de champ de texte et l’élément sélectionné.

Pour le `Get` méthode de la `Course` référentiel, le code spécifie une expression de filtre, un ordre de tri et un chargement hâtif pour la `Department` propriété de navigation. Retourne l’expression de filtre est toujours `true` si rien n’est sélectionné dans la liste déroulante (autrement dit, `SelectedDepartment` a la valeur null).

Dans *Views\Course\Index.cshtml*, immédiatement avant l’ouverture `table` , ajoutez le code suivant pour créer la liste déroulante et un bouton d’envoi :

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Avec le point d’arrêt toujours de définir, d’exécuter la page d’Index des cours. Suivez les instructions des première fois que le code atteint un point d’arrêt, afin que la page s’affiche dans le navigateur. Sélectionnez un service dans la liste déroulante et cliquez sur **filtre**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Cette fois, le premier point d’arrêt sera pour la requête de services pour obtenir la liste déroulante. Ignorez qui et affichez la `query` variable la prochaine fois que le code atteint le point d’arrêt pour voir ce que le `Course` ressemble à présent de la requête. Vous verrez quelque chose comme ce qui suit :

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Vous pouvez voir que la requête est maintenant un `JOIN` requête charge `Department` données avec le `Course` données, et qu’elle inclut un `WHERE` clause.

Supprimer le `var sql = courses.ToString()` ligne.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Modèles d’unité de travail et de référentiel

De nombreux développeurs écrivent du code pour implémenter les modèles d’unité de travail et de référentiel comme un wrapper autour du code qui fonctionne avec Entity Framework. Ces modèles sont destinés à créer une couche d’abstraction entre la couche d’accès aux données et la couche de logique métier d’une application. L’implémentation de ces modèles peut favoriser l’isolation de votre application face à des modifications dans le magasin de données et peut faciliter le test unitaire automatisé ou le développement piloté par les tests (TDD). Toutefois, écriture de code supplémentaire pour implémenter ces modèles n’est pas toujours le meilleur choix pour les applications qui utilisent EF, et pour plusieurs raisons :

- La classe de contexte EF elle-même isole votre code face au code spécifique de magasin de données.
- La classe de contexte EF peut agir comme une classe d’unité de travail pour les mises à jour de base de données que vous effectuez à l’aide d’EF.
- Fonctionnalités introduites dans Entity Framework 6 rendent plus faciles à implémenter TDD sans écrire de code du référentiel.

Pour plus d’informations sur la façon d’implémenter le référentiel et une unité de travail des modèles, consultez [la version d’Entity Framework 5 de cette série de didacticiels](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Pour plus d’informations sur les façons d’implémenter TDD dans Entity Framework 6, consultez les ressources suivantes :

- [Comment EF6 permet une simulation DbSets plus facilement](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Test avec une infrastructure de simulation](https://msdn.microsoft.com/data/dn314429)
- [Test avec votre propre doubles de test](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Classes de proxy

Quand Entity Framework crée des instances d’entité (par exemple, lorsque vous exécutez une requête), il crée souvent sous la forme d’instances d’un type dérivé généré dynamiquement qui agit comme un proxy pour l’entité. Par exemple, consultez les deux images suivantes de débogueur. Dans la première image, vous constatez que le `student` variable est attendu `Student` tapez immédiatement une fois que vous instanciez l’entité. Dans la deuxième image, une fois que EF a été utilisé pour lire une entité student à partir de la base de données, vous voyez la classe proxy.

![Avant de classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Après la classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Cette classe de proxy substitue certaines propriétés virtuelles de l’entité à insérer des raccordements pour effectuer des actions automatiquement lorsque la propriété est accessible. Une fonction pour que ce mécanisme est utilisé est le chargement différé.

La plupart du temps vous n’avez pas besoin de connaître cette utilisation de proxys, mais il existe des exceptions :

- Dans certains scénarios, vous souhaiterez empêcher la création d’instances de proxy d’Entity Framework. Par exemple, lors de la sérialisation d’entités vous souhaitez généralement les classes POCO, pas les classes de proxy. Pour éviter les problèmes de sérialisation consiste à sérialiser des objets de transfert de données (DTO) au lieu d’objets d’entité, comme indiqué dans le [à l’aide des API Web avec Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) didacticiel. Une autre méthode consiste à [désactiver la création de proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Lorsque vous instanciez une classe d’entité à l’aide du `new` opérateur, vous n’obtenez pas une instance de proxy. Cela signifie que vous n’obtenez pas des fonctionnalités telles que le suivi des modifications automatique et le chargement différé. Il s’agit généralement OK ; Vous ne devez généralement le chargement différé, étant donné que vous créez une nouvelle entité qui n’est pas dans la base de données, et vous ne devez généralement pas le suivi des modifications si vous êtes marquant explicitement l’entité en tant que `Added`. Toutefois, si vous n’avez pas besoin d’un chargement différé et que vous avez besoin de suivi des modifications, vous pouvez créer des nouvelles instances d’entités avec les proxys à l’aide de la [créer](https://msdn.microsoft.com/library/gg679504.aspx) méthode de la `DbSet` classe.
- Vous souhaiterez peut-être obtenir un type d’entité réelle à partir d’un type de proxy. Vous pouvez utiliser la [GetObjectType a](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) méthode de la `ObjectContext` classe pour obtenir le type d’entité réelle d’une instance de type de proxy.

Pour plus d’informations, consultez [utilisation de proxys](https://msdn.microsoft.com/data/JJ592886.aspx) sur MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Détection automatique des modifications

Entity Framework détermine la manière dont une entité a changé (et par conséquent les mises à jour qui doivent être envoyées à la base de données) en comparant les valeurs en cours d’une entité avec les valeurs d’origine. Les valeurs d’origine sont stockées lorsque l’entité fait l’objet d’une requête ou d’une jointure. Certaines des méthodes qui provoquent la détection automatique des modifications sont les suivantes :

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si vous effectuez le suivi un grand nombre d’entités et que vous appelez une de ces méthodes plusieurs fois dans une boucle, vous pouvez obtenir des améliorations significatives des performances en désactivant temporairement la détection de modification automatique à l’aide du [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) propriété. Pour plus d’informations, consultez [détecter automatiquement les modifications](https://msdn.microsoft.com/data/jj556205) sur MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Validation automatique

Lorsque vous appelez le `SaveChanges` (méthode), par défaut, Entity Framework valide les données dans toutes les propriétés de toutes les entités modifiées avant la mise à jour de la base de données. Si vous avez mis à jour un grand nombre d’entités et vous avez déjà validé les données, ce travail n’est pas nécessaire et vous pouvez créer le processus d’enregistrement les modifications prennent moins de temps en désactivant temporairement la validation. Vous pouvez faire cela à l’aide du [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) propriété. Pour plus d’informations, consultez [Validation](https://msdn.microsoft.com/data/gg193959) sur MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) est un complément Visual Studio qui a été utilisé pour créer les diagrammes de modèle de données indiqué dans ces didacticiels. Les outils peuvent également effectuer autre fonction, telles que générer des classes d’entité basée sur les tables dans une base de données existant afin que vous pouvez utiliser la base de données avec Code First. Après avoir installé les outils, des options supplémentaires apparaissent dans les menus contextuels. Par exemple, lorsque vous cliquez sur votre classe de contexte dans **l’Explorateur de solutions**, vous obtenez une option pour générer un diagramme. Lorsque vous utilisez Code First, vous ne pouvez pas modifier le modèle de données dans le diagramme, mais vous pouvez déplacer des éléments pour le rendre plus facile à comprendre.

![EF dans le menu contextuel](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![Diagramme d’EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Code source d’Entity Framework

Le code source pour Entity Framework 6 est disponible à l’adresse [GitHub](https://github.com/aspnet/EntityFramework6). Vous pouvez signaler des bogues, et vous pouvez contribuer à vos propres améliorations au code source EF.

Bien que le code source est ouvert, Entity Framework est entièrement pris en charge comme un produit Microsoft. L’équipe Microsoft Entity Framework garde le contrôle sur le choix des contributions qui sont acceptées et teste toutes les modifications du code pour garantir la qualité de chaque version.

<a id="summary"></a>
## <a name="summary"></a>Récapitulatif

Cette étape termine cette série de didacticiels sur l’utilisation d’Entity Framework dans une application ASP.NET MVC. Pour plus d’informations sur l’utilisation des données à l’aide d’Entity Framework, consultez le [page de documentation Entity Framework sur MSDN](https://msdn.microsoft.com/data/ee712907) et [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

Pour plus d’informations sur la façon de déployer votre application web une fois que vous l’avez créé, consultez [le déploiement Web ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-web-deployment-content-map.md) dans MSDN Library.

Pour plus d’informations sur les autres rubriques relatives à MVC, telles que l’authentification et l’autorisation, consultez le [MVC ASP.NET - ressources recommandées](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Remerciements

- Tom Dykstra a écrit la version d’origine de ce didacticiel, co-écrit sur la mise à jour d’EF 5 et écrit la mise à jour d’EF 6. Tom est rédactrice en programmation senior de l’équipe de contenu d’outils et de Microsoft Web Platform.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) n’a l’essentiel du travail de mise à jour de ce didacticiel pour EF 5 et MVC 4 et co-écrit sur la mise à jour d’EF 6. Rick est rédactrice en programmation senior pour Microsoft Azure et de MVC.
- [Rowan Miller](http://www.romiller.com) et autres membres de l’équipe Entity Framework assisté avec les révisions de code et aidé au débogage de nombreux problèmes avec les migrations qui ont surgi pendant que nous étions en train de mise à jour le didacticiel pour EF 5 et 6 d’Entity Framework.

<a id="vb"></a>
## <a name="vb"></a>VB

Lorsque le didacticiel a été développé pour EF 4.1, nous avons fourni des versions de c# et VB de projet téléchargement terminé. En raison de contraintes de temps et d’autres priorités nous le n'avons pas fait que pour cette version. Si vous générez un projet Visual Basic à l’aide de ces didacticiels et que vous êtes prêt à partager avec d’autres personnes, faites-le nous savoir.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Les erreurs courantes et les solutions ou les solutions de contournement pour les

### <a name="cannot-createshadow-copy"></a>Ne peut pas créer/shadow copy

Message d’erreur :

> Ne peut pas créer/shadow copy '&lt;filename&gt;» lorsque ce fichier existe déjà.


Solution

Attendez quelques secondes et actualisez la page.

### <a name="update-database-not-recognized"></a>Mise à jour la base de données n’est ne pas reconnu.

Message d’erreur (à partir de la `Update-Database` dans PMC) :

> Le terme « Update-Database » n’est pas reconnu comme le nom de l’applet de commande, fonction, fichier de script ou programme exécutable. Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct et réessayez.


Solution

Quittez Visual Studio. Rouvrez le projet, puis réessayez.

### <a name="validation-failed"></a>Échoué de la validation

Message d’erreur (à partir de la `Update-Database` dans PMC) :

> Échec de la validation pour une ou plusieurs entités. Consultez la propriété 'EntityValidationErrors' pour plus d’informations.


Solution

Une des causes de ce problème sont d’erreurs de validation lorsque la `Seed` exécutions de méthode. Consultez [amorçage et débogage Entity Framework (EF) des bases de données](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) pour obtenir des conseils sur le débogage de la `Seed` (méthode).

### <a name="http-50019-error"></a>HTTP 500.19 erreur

Message d’erreur :

> Erreur HTTP 500.19 - erreur interne du serveur  
> La page demandée est inaccessible, car les données de configuration de la page ne sont pas valides.


Solution

Vous pouvez obtenir cette erreur consiste à partir de plusieurs copies de la solution, chacun d’eux à l’aide du même numéro de port. Vous pouvez généralement résoudre ce problème en quitter toutes les instances de Visual Studio, puis redémarrer le projet sur lequel vous travaillez. Si cela ne fonctionne pas, essayez de modifier le numéro de port. Cliquez avec le bouton droit sur le fichier projet, puis sur Propriétés. Sélectionnez le **Web** onglet et modifiez le numéro de port dans le **Url du projet** zone de texte.

### <a name="error-locating-sql-server-instance"></a>Erreur lors de la localisation de l’instance SQL Server

Message d’erreur :

> Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server. Le serveur est introuvable ou n’est pas accessible. Vérifiez que le nom de l’instance est correct et que SQL Server est configuré pour autoriser les connexions distantes. (fournisseur : interfaces réseau SQL, erreur : 26 - Erreur lors de la localisation du serveur/de l’instance spécifiés)


Solution

Vérifiez la chaîne de connexion. Si vous avez supprimé manuellement la base de données, modifiez le nom de la base de données dans la chaîne de construction.

> [!div class="step-by-step"]
> [Précédent](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
