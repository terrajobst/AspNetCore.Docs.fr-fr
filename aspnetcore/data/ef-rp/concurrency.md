---
title: "Pages Razor avec EF Core - Accès concurrentiel - 8 sur 8"
author: rick-anderson
description: "Ce didacticiel montre comment gérer les conflits quand plusieurs utilisateurs mettent à jour la même entité en même temps."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2018
---
fr-fr/

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Gestion des conflits d’accès concurrentiel - Core EF avec Pages Razor (8 sur 8)

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) et [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Ce didacticiel montre comment gérer les conflits quand plusieurs utilisateurs mettent à jour une entité en même temps. Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez l’[application terminée pour cette phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Conflits d’accès concurrentiel

Un conflit d’accès concurrentiel se produit quand :

* Un utilisateur accède à la page de modification d’une entité.
* Un autre utilisateur met à jour la même entité avant que la modification du premier utilisateur soit écrite dans la base de données.

Si la détection d’accès concurrentiel n’est pas activée, quand des mises à jour simultanées se produisent :

* La dernière mise à jour est prioritaire. Autrement dit, les dernières valeurs mises à jour sont enregistrées dans la base de données.
* La première des mises à jour en cours est perdue.

### <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste

L’accès concurrentiel optimiste autorise la survenance des conflits d’accès concurrentiel, et réagit correctement quand ils surviennent. Par exemple, Jane consulte la page de modification de département et change le montant de « Budget » pour le département « English » en le faisant passer de 350 000,00 $ à 0,00 $.

![Modification de la valeur de budget sur 0](concurrency/_static/change-budget.png)

Avant que Jane clique sur **Save**, John consulte la même page et change le champ Start Date de 01/09/2007 en 01/09/2013.

![Modification de la date de début sur 2013](concurrency/_static/change-date.png)

Jane clique la première sur **Save** et voit sa modification quand le navigateur revient à la page Index.

![Le budget est passé à zéro](concurrency/_static/budget-zero.png)

John clique sur **Save** dans une page Edit qui affiche toujours un budget de 350 000,00 $. Ce qui se passe ensuite est déterminé par la façon dont vous gérez les conflits d’accès concurrentiel.

L’accès concurrentiel optimiste comprend les options suivantes :

* Vous pouvez effectuer le suivi des propriétés modifiées par un utilisateur et mettre à jour seulement les colonnes correspondantes dans la base de données.

 Dans le scénario, aucune donnée ne serait perdue. Des propriétés différentes ont été mises à jour par les deux utilisateurs. La prochaine fois que quelqu’un consultera le département « English », il verra à la fois les modifications de Jane et de John. Cette méthode de mise à jour peut réduire le nombre de conflits susceptibles d’entraîner une perte de données. Cette approche : * ne peut pas éviter une perte de données si des modifications concurrentes sont apportées à la même propriété.
        * N’est généralement pas pratique dans une application web. Elle nécessite la tenue à jour d’un état significatif afin d’effectuer le suivi de toutes les valeurs récupérées et des nouvelles valeurs. La maintenance de grandes quantités d’état peut affecter les performances de l’application.
        * Peut augmenter la complexité de l’application par rapport à la détection de l’accès concurrentiel sur une entité.

* Vous pouvez laisser les modifications de John remplacer celles de Jane.

 La prochaine fois que quelqu’un consultera le département « English », il verra la date 01/09/2013 et la valeur 350 000,00 $ récupérée. Cette approche est un scénario *Priorité au client* ou *Priorité au dernier*. (Toutes les valeurs du client sont prioritaires par rapport au contenu du magasin de données.) Si vous n’écrivez pas de code pour la gestion de l’accès concurrentiel, la Priorité au client est appliquée automatiquement.

* Vous pouvez empêcher les modifications de John d’être mises à jour dans la base de données. En règle générale, l’application : * Afficherait un message d’erreur.
        * Afficherait l’état actuel de l’état.
        * Autoriserait l’utilisateur à réappliquer les modifications.

 Il s’agit alors d’un scénario *Priorité au magasin*. (Les valeurs du magasin de données sont prioritaires par rapport à celles soumises par le client.) Nous allons implémenter le scénario Priorité au magasin dans ce didacticiel. Cette méthode garantit qu’aucune modification n’est remplacée sans qu’un utilisateur soit averti.

## <a name="handling-concurrency"></a>Gestion de l’accès concurrentiel 

Quand une propriété est configurée en tant que [jeton d’accès concurrentiel](https://docs.microsoft.com/ef/core/modeling/concurrency) :

* EF Core vérifie que cette propriété n’a pas été modifiée après avoir été récupérée. La vérification a lieu quand [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) ou [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) est appelée.
* Si la propriété a été modifiée après avoir été récupérée, une [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) est levée. 

Le modèle de données et de la base de données doivent être configurés pour prendre en charge la levée de `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Détection des conflits d’accès concurrentiel sur une propriété

Les conflits d’accès concurrentiel peuvent être détectés au niveau de la propriété avec l’attribut [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0). L’attribut peut être appliqué à plusieurs propriétés sur le modèle. Pour plus d’informations, consultez [Data Annotations-ConcurrencyCheck (Annotations de données-ConcurrencyCheck)](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).

Nous n’utilisons pas l’attribut `[ConcurrencyCheck]` dans ce didacticiel.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Détection des conflits d’accès concurrentiel sur une ligne

Pour détecter les conflits d’accès concurrentiel, une colonne de suivi [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) est ajoutée au modèle.  `rowversion` :

* est propre à SQL Server. D’autres bases de données peuvent ne pas fournir une fonctionnalité similaire.
* Sert à déterminer qu’une entité n’a pas été modifiée depuis qu’elle a été récupérée à partir de la base de données. 

La base de données génère un numéro `rowversion` séquentiel qui est incrémenté chaque fois que la ligne est mise à jour. Dans une commande `Update` ou `Delete`, la clause `Where` comprend la valeur récupérée de `rowversion`. Si la ligne mise à jour a changé :

 * `rowversion` ne correspond pas à la valeur récupérée.
 * Les commandes `Update` ou `Delete` ne trouvent pas de ligne, car la clause `Where` comprend la valeur `rowversion` récupérée.
 * Une `DbUpdateConcurrencyException` est levée.

Dans EF Core, quand aucune ligne n’a été mise à jour par une commande `Update` ou `Delete`, une exception d’accès concurrentiel est levée.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Ajouter une propriété de suivi à l’entité Department

Dans *Models/Department.cs*, ajoutez une propriété de suivi nommée RowVersion :

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

L’attribut [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) spécifie que cette colonne est incluse dans la clause `Where` des commandes `Update` et `Delete`. L’attribut se nomme `Timestamp`, car les versions précédentes de SQL Server utilisaient un type de données SQL `timestamp` avant son remplacement par le type SQL `rowversion`.

L’API Fluent peut également spécifier la propriété de suivi :

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

Le code suivant montre une partie du T-SQL généré par EF Core quand le nom du département est mis à jour :

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Le code en surbrillance ci-dessus montre la clause `WHERE` contenant `RowVersion`. Si la `RowVersion` de la base de données n’est pas égale au paramètre `RowVersion` (`@p2`), aucune ligne n’est mise à jour.

Le code en surbrillance suivant montre le T-SQL qui vérifie qu’une seule ligne a été mise à jour :

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) retourne le nombre de lignes affectées par la dernière instruction. Si aucune ligne n’est mise à jour, EF Core lève une `DbUpdateConcurrencyException`.

Vous pouvez voir le T-SQL généré par EF Core dans la fenêtre Sortie de Visual Studio.

### <a name="update-the-db"></a>Mettre à jour la base de données

L’ajout de la propriété `RowVersion` change le modèle de base de données, ce qui nécessite une migration.

Générez le projet. Entrez ce qui suit dans une fenêtre de commande :

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Les commandes précédentes :

* Ajoutent le fichier de migration *Migrations/{horodatage}_RowVersion.cs*.
* Mettent à jour le fichier *Migrations/SchoolContextModelSnapshot.cs*. La mise à jour ajoute le code en surbrillance suivant à la méthode `BuildModel` :

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Exécutent des migrations pour mettre à jour la base de données.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Générer automatiquement le modèle Departments

* Quittez Visual Studio.
* Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).
* Exécutez la commande suivante :

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

La commande précédente génère automatiquement le modèle `Department`. Ouvrez le projet dans Visual Studio.

Générez le projet. La build génère des erreurs telles que celle-ci :

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Remplacez globalement `_context.Department` par `_context.Departments` (autrement dit, ajoutez un « s » à `Department`). 7 occurrences sont trouvées et mises à jour.

### <a name="update-the-departments-index-page"></a>Mettre à jour la page d’index des départements

Le moteur de génération de modèles automatique créé une colonne `RowVersion` pour la page Index, mais ce champ ne doit pas être affiché. Dans ce didacticiel, le dernier octet de `RowVersion` est affiché afin d’aider à mieux comprendre l’accès concurrentiel. Il n’est pas garanti que le dernier octet soit unique. Une application réelle n’afficherait pas `RowVersion` ou le dernier octet de `RowVersion`.

Mettez à jour la page Index :

* Remplacez Index par Departments.
* Remplacez le balisage contenant `RowVersion` par le dernier octet de `RowVersion`.
* Remplacez FirstMidName par FullName.

Le balisage suivant montre la page mise à jour :

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Mettre à jour le modèle de page de modification

Mettez à jour *pages\departments\edit.cshtml.cs* avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Pour détecter un problème d’accès concurrentiel, [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) est mise à jour avec la valeur `rowVersion` de l’entité récupérée. EF Core génère une commande SQL UPDATE avec une clause WHERE contenant la valeur `RowVersion` d’origine. Si aucune ligne n’est affectée par la commande UPDATE (aucune ligne ne contient la valeur `RowVersion` d’origine), une exception `DbUpdateConcurrencyException` est levée.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

Dans le code précédent, `Department.RowVersion` est la valeur quand l’entité a été récupérée. `OriginalValue` est la valeur présente dans la base de données quand `FirstOrDefaultAsync` a été appelée dans cette méthode.

Le code suivant obtient les valeurs du client (celles envoyées à cette méthode) et les valeurs de la base de données :

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

Le code suivant ajoute un message d’erreur personnalisé pour chaque colonne dont les valeurs dans la base de données sont différentes de celles envoyées à `OnPostAsync` :

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

Le code en surbrillance suivant affecte à `RowVersion` la nouvelle valeur récupérée à partir de la base de données. La prochaine fois que l’utilisateur cliquera sur **Save**, seules les erreurs d’accès concurrentiel qui se sont produites depuis le dernier affichage de la page Edit seront interceptées.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

L’instruction `ModelState.Remove` est nécessaire car `ModelState` contient l’ancienne valeur `RowVersion`. Dans la page Razor, la valeur `ModelState` d’un champ est prioritaire par rapport aux valeurs de propriétés du modèle quand les deux sont présentes.

## <a name="update-the-edit-page"></a>Mettre à jour la page Edit

Mettez à jour *Pages/Departments/Edit.cshtml* avec le balisage suivant :

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

Le balisage précédent :

* Met à jour la directive `page` en remplaçant `@page` par `@page "{id:int}"`.
* Ajoute une version de ligne masquée. `RowVersion` doit être ajouté afin que la publication lie la valeur.
* Affiche le dernier octet de `RowVersion` à des fins de débogage.
* Remplace `ViewData` par le `InstructorNameSL` fortement typé.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Tester les conflits d’accès concurrentiel avec la page Edit

Ouvrez deux instances de navigateur de la page Edit sur le département English :

* Exécutez l’application et sélectionnez Departments.
* Cliquez avec le bouton droit sur le lien hypertexte **Edit** correspondant au département English, puis sélectionnez **Open in new tab**.
* Sous le premier onglet, cliquez sur le lien hypertexte **Edit** correspondant au département English.

Les deux onglets de navigateur affichent les mêmes informations.

Changez le nom sous le premier onglet de navigateur, puis cliquez sur **Save**.

![Page 1 de modification de département après changement](concurrency/_static/edit-after-change-1.png)

Le navigateur affiche la page Index avec la valeur modifiée et un indicateur rowVersion mis à jour. Notez l’indicateur rowVersion mis à jour ; il est affiché sur la deuxième publication (postback) sous l’autre onglet.

Changez un champ différent sous le deuxième onglet du navigateur.

![Page 2 de modification de département après changement](concurrency/_static/edit-after-change-2.png)

Cliquez sur **Save**. Des messages d’erreur s’affichent pour tous les champs qui ne correspondent pas aux valeurs de la base de données :

![Message d’erreur de page de modification de département](concurrency/_static/edit-error.png)

Cette fenêtre de navigateur n’avait pas l’intention de changer le champ Name. Copiez et collez la valeur actuelle (Languages) dans le champ Name. Appuyez sur Tab. La validation côté client supprime le message d’erreur.

![Message d’erreur de page de modification de département](concurrency/_static/cv.png)

Cliquez à nouveau sur **Save**. La valeur que vous avez entrée sous le deuxième onglet du navigateur est enregistrée. Les valeurs enregistrées sont visibles dans la page Index.

## <a name="update-the-delete-page"></a>Mettre à jour la page Delete

Mettez à jour le modèle de page de suppression avec le code suivant :

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

La page Delete détecte les conflits d’accès concurrentiel quand l’entité a changé après avoir été récupérée. `Department.RowVersion` est la version de ligne quand l’entité a été récupérée. Quand EF Core crée la commande SQL DELETE, il inclut une clause WHERE avec `RowVersion`. Si après l’exécution de la commande SQL DELETE aucune ligne n’est affectée :

* La valeur de `RowVersion` dans la commande SQL DELETE ne correspond pas à `RowVersion` dans la base de données.
* Une exception DbUpdateConcurrencyException est levée.
* `OnGetAsync` est appelée avec `concurrencyError`.

### <a name="update-the-delete-page"></a>Mettre à jour la page Delete

Mettez à jour *Pages/Departments/Delete.cshtml* avec le code suivant :

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


Le balisage précédent apporte les modifications suivantes :

* Il met à jour la directive `page` en remplaçant `@page` par `@page "{id:int}"`.
* Il ajoute un message d’erreur.
* Il remplace FirstMidName par FullName dans le champ **Administrator**.
* Il change `RowVersion` pour afficher le dernier octet.
* Ajoute une version de ligne masquée. `RowVersion` doit être ajouté afin que la publication lie la valeur.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Tester les conflits d’accès concurrentiel avec la page Delete

Créez un département test.

Ouvrez deux instances de navigateur de la page Delete sur le département test :

* Exécutez l’application et sélectionnez Departments.
* Cliquez avec le bouton droit sur le lien hypertexte **Delete** correspondant au département test, puis sélectionnez **Open in new tab**.
* Cliquez sur le lien hypertexte **Edit** correspondant au département test.

Les deux onglets de navigateur affichent les mêmes informations.

Changez le budget sous le premier onglet de navigateur, puis cliquez sur **Save**.

Le navigateur affiche la page Index avec la valeur modifiée et un indicateur rowVersion mis à jour. Notez l’indicateur rowVersion mis à jour ; il est affiché sur la deuxième publication (postback) sous l’autre onglet.

Supprimez le département test du deuxième onglet. Une erreur d’accès concurrentiel s’affiche, avec les valeurs présentes actuellement dans la base de données. Un clic sur **Delete** supprime l’entité, sauf si `RowVersion` a été mis à jour.

Pour découvrir comment hériter d’un modèle de données, consultez [Héritage](xref:data/ef-mvc/inheritance).

### <a name="additional-resources"></a>Ressources supplémentaires

* [Concurrency Tokens in EF Core (Jetons d’accès concurrentiel dans EF Core)](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [Handling concurrency in EF Core (Gestion de l’accès concurrentiel dans EF Core)](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Précédent](xref:data/ef-rp/update-related-data)
