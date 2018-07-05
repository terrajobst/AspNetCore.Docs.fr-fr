---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async et les procédures stockées avec Entity Framework dans une Application ASP.NET MVC | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c2deb53856d79a52415db48e04ea96111833b02
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381927"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async et les procédures stockées avec Entity Framework dans une Application ASP.NET MVC
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans les didacticiels précédents, vous avez appris comment lire et mettre à jour des données à l’aide du modèle de programmation synchrone. Dans ce didacticiel, vous allez apprendre à implémenter le modèle de programmation asynchrone. Code asynchrone peut vous aider à une application plus performantes, car il fait un meilleur usage des ressources du serveur.

Dans ce didacticiel, vous verrez également comment utiliser des procédures stockées pour insert, update et les opérations de suppression sur une entité.

Enfin, vous devez redéployer l’application vers Azure, ainsi que toutes les modifications de base de données que vous avez implémentées depuis la première fois que vous avez déployé.

Les illustrations suivantes montrent quelques-unes des pages que vous allez utiliser.

![Page départements](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Créer le service](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Pourquoi s’embêter avec le code asynchrone

Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés. Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés. Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent. Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes. Par conséquent, le code asynchrone permet à des ressources serveur à utiliser plus efficacement, et le serveur est activé pour traiter plus de trafic sans retard.

Dans les versions antérieures de .NET, écrire et tester le code asynchrone a été complexe, sujettes aux erreurs et difficile à déboguer. Dans .NET 4.5, écriture, test et débogage du code asynchrone sont tellement plus facile que vous devez généralement écrire du code asynchrone, sauf si vous avez une raison de ne pas le faire. Le code asynchrone introduit néanmoins une petite quantité de surcharge, mais pour les situations de faible trafic la baisse de performances est négligeable, tout en cas de trafic élevé, l’amélioration potentielle des performances est importante.

Pour plus d’informations sur la programmation asynchrone, consultez [prise en charge d’utiliser .NET 4.5 asynchrone pour éviter de bloquer les appels](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Créer le contrôleur de service

Créer un contrôleur de département Sélectionnez de la même façon que vous l’avez fait les contrôleurs antérieures, mais cette fois le **utiliser async contrôleur** actions case à cocher.

![Structure de contrôleur de service](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Les points importants suivants montrent ce qui a été ajouté au code synchrone pour le `Index` méthode pour la rendre asynchrone :

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Quatre modifications ont été appliquées pour activer la requête de base de données Entity Framework exécuter de façon asynchrone :

- La méthode est marquée avec le `async` mot clé, qui indique au compilateur de générer des rappels pour les parties du corps de méthode et pour créer automatiquement le `Task<ActionResult>` objet qui est retourné.
- Le type de retour a été remplacé par `ActionResult` à `Task<ActionResult>`. Le `Task<T>` type représente le travail en cours avec un résultat de type `T`.
- Le `await` mot clé a été appliqué à l’appel de service web. Lorsque le compilateur voit ce mot clé, il divise les coulisses la méthode en deux parties. La première partie se termine par l’opération qui est lancée de façon asynchrone. La deuxième partie est placée dans une méthode de rappel est appelée lorsque l’opération terminée.
- La version asynchrone de la `ToList` méthode d’extension a été appelée.

Pourquoi est la `departments.ToList` instruction a été modifiée, mais pas la `departments = db.Departments` instruction ? La raison est que seules les instructions qui provoquent des requêtes ou des commandes à envoyer à la base de données sont exécutées de façon asynchrone. Le `departments = db.Departments` instruction définit une requête, mais la requête n’est pas exécutée avant la `ToList` méthode est appelée. Par conséquent, seuls les `ToList` méthode est exécutée de façon asynchrone.

Dans le `Details` (méthode) et le `HttpGet` `Edit` et `Delete` méthodes, le `Find` méthode est celle qui provoque une requête à envoyer à la base de données, c’est la méthode qui est exécutée de façon asynchrone :

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

Dans le `Create`, `HttpPost Edit`, et `DeleteConfirmed` méthodes, il est le `SaveChanges` appel de méthode qui entraîne une commande à exécuter, pas les instructions telles que `db.Departments.Add(department)` qui seulement provoquer des entités en mémoire à modifier.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Ouvrez *Views\Department\Index.cshtml*et remplacez le code du modèle par le code suivant :

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Ce code modifie le titre de l’Index aux départements, déplace le nom de l’administrateur vers la droite et fournit le nom complet de l’administrateur.

Dans la création, suppression, détails et modifier des vues, changez la légende pour le `InstructorID` champ « Administrateur » de la même façon que vous avez modifié le champ de nom de service pour « Department » dans les vues des cours.

Dans le créer et modifier les vues utilisent le code suivant :

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Dans les vues Delete et Details, utilisez le code suivant :

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Exécutez l’application, puis cliquez sur le **départements** onglet.

![Page départements](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tout fonctionne comme dans les autres contrôleurs, mais dans ce contrôleur de toutes les requêtes SQL s’exécutent en mode asynchrone.

Voici quelques éléments à connaître lorsque vous utilisez la programmation asynchrone avec Entity Framework :

- Le code asynchrone n’est pas thread-safe. En d’autres termes, en d’autres termes, n’essayez pas d’effectuer plusieurs opérations en parallèle en utilisant la même instance de contexte.
- Si vous souhaitez tirer profit des meilleures performances du code asynchrone, assurez-vous que tous les packages de bibliothèque que vous utilisez (par exemple pour changer de page) utilisent également du code asynchrone s’ils appellent des méthodes Entity Framework qui provoquent l’envoi des requêtes à la base de données.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Utiliser des procédures stockées pour l’insertion, la mise à jour et suppression

Certains développeurs et les administrateurs préfèrent utiliser des procédures stockées pour l’accès de base de données. Dans les versions précédentes d’Entity Framework vous pouvez récupérer des données à l’aide d’une procédure stockée par [l’exécution d’une requête SQL brute](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), mais vous ne pouvez pas demander à EF d’utiliser des procédures stockées pour les opérations de mise à jour. Dans EF 6, il est facile à configurer Code First pour utiliser des procédures stockées.

1. Dans *DAL\SchoolContext.cs*, ajoutez le code en surbrillance à la `OnModelCreating` (méthode).

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Ce code indique à Entity Framework aux procédures d’utilisation stockée pour insérer, mettre à jour et supprimer des opérations sur le `Department` entité.
2. Dans la Console de gestion de Package, entrez la commande suivante :

    `add-migration DepartmentSP`

    Ouvrez *Migrations\&lt ; horodatage&gt;\_DepartmentSP.cs* pour visualiser le code dans le `Up` méthode créé par Insert, Update et Delete de procédures stockées :

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Dans la Console de gestion de Package, entrez la commande suivante :

     `update-database`
4. Exécutez l’application en mode débogage, cliquez sur le **départements** onglet, puis cliquez sur **créer un nouveau**.
5. Entrer des données pour un nouveau service, puis cliquez sur **créer**.

     ![Créer le service](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. Dans Visual Studio, consultez les journaux dans le **sortie** fenêtre pour voir qu’une procédure stockée a été utilisée pour insérer la nouvelle ligne de service.

     ![Département Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code crée d’abord les noms de procédures stockée de par défaut. Si vous utilisez une base de données existante, vous devrez peut-être personnaliser les noms des procédures stockées afin d’utiliser des procédures stockées prédéfinies dans la base de données. Pour savoir comment procéder, consultez [Entity Framework Code première Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).

Si vous souhaitez personnaliser ce qui généré do de procédures stockées, vous pouvez modifier le code généré automatiquement pour les migrations `Up` méthode qui crée la procédure stockée. De cette façon vos modifications sont répercutées lorsque que la migration est exécutée et est appliquée à votre base de données de production lors de la migration s’exécute automatiquement en production après le déploiement.

Si vous souhaitez modifier une procédure stockée existante qui a été créée dans une migration précédente, vous pouvez utiliser la commande Add-Migration pour générer une migration vide et ensuite manuellement écrire du code qui appelle le [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (méthode) .

## <a name="deploy-to-azure"></a>Déployer sur Azure

Cette section vous oblige à terminées facultatif **déploiement de l’application vers Azure** section dans le [Migrations et déploiement](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel de cette série. Si vous aviez des erreurs de migrations que vous avez résolu en supprimant la base de données dans votre projet local, ignorez cette section.

1. Dans Visual Studio, cliquez sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier** dans le menu contextuel.
2. Cliquez sur **Publier**.

    Visual Studio déploie l’application dans Azure, et l’application s’ouvre dans votre navigateur par défaut, s’exécutant dans Azure.
3. Tester l’application afin de vérifier qu’il fonctionne.

    La première fois que vous exécutez une page qui accède à la base de données, Entity Framework s’exécute toutes les migrations `Up` méthodes requis pour mettre à jour avec le modèle de données actuel de la base de données. Vous pouvez désormais utiliser toutes les pages web que vous avez ajouté depuis la dernière fois que vous avez déployé, y compris les pages de service que vous avez ajouté dans ce didacticiel.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel vous avez vu comment améliorer l’efficacité du serveur en écrivant du code qui s’exécute de façon asynchrone et comment utiliser des procédures stockées pour insérer, mettre à jour et les opérations de suppression. Dans le didacticiel suivant, vous verrez comment éviter la perte de données lorsque plusieurs utilisateurs essaient de modifier le même enregistrement en même temps.

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
