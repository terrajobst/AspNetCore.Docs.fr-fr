---
title: "Pages Razor avec EF Core - d’accès concurrentiel - 8 de 8"
author: rick-anderson
description: "Ce didacticiel montre comment gérer les conflits lorsque plusieurs utilisateurs mettre à jour la même entité en même temps."
keywords: "Concurrence d’accès ASP.NET Core, Entity Framework Core,"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 0c49376fd1b602fe03ef2a152d19b58513ae2710
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2017
---
<span data-ttu-id="cf940-104">en-us /</span><span class="sxs-lookup"><span data-stu-id="cf940-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="cf940-105">La gestion des conflits d’accès concurrentiel - Core EF avec les Pages Razor (8 sur 8)</span><span class="sxs-lookup"><span data-stu-id="cf940-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="cf940-106">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), et [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="cf940-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="cf940-107">Ce didacticiel montre comment gérer les conflits lorsque plusieurs utilisateurs mettre à jour une entité simultanément (simultanément).</span><span class="sxs-lookup"><span data-stu-id="cf940-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="cf940-108">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="cf940-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="cf940-109">Conflits d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="cf940-109">Concurrency conflicts</span></span>

<span data-ttu-id="cf940-110">Un conflit de concurrence se produit lorsque :</span><span class="sxs-lookup"><span data-stu-id="cf940-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="cf940-111">Un utilisateur accède à la page de modification pour une entité.</span><span class="sxs-lookup"><span data-stu-id="cf940-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="cf940-112">Un autre utilisateur met à jour la même entité avant la première modification utilisateur est écrit dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="cf940-113">Si la détection d’accès concurrentiel n’est pas activée, lorsque des mises à jour simultanées se produisent :</span><span class="sxs-lookup"><span data-stu-id="cf940-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="cf940-114">La dernière mise à jour est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="cf940-114">The last update wins.</span></span> <span data-ttu-id="cf940-115">Autrement dit, les dernières valeurs de mise à jour sont enregistrés dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="cf940-116">La première mises à jour en cours sont perdues.</span><span class="sxs-lookup"><span data-stu-id="cf940-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="cf940-117">Accès concurrentiel optimiste</span><span class="sxs-lookup"><span data-stu-id="cf940-117">Optimistic concurrency</span></span>

<span data-ttu-id="cf940-118">L’accès concurrentiel optimiste permet de conflits d’accès concurrentiel se produire et puis réagit en conséquence s’ils ne.</span><span class="sxs-lookup"><span data-stu-id="cf940-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="cf940-119">Par exemple, Jane visite de la page de modification de service et modifie l’allocation de réserve pour le service en anglais à partir de $350,000.00 à 0,00 $.</span><span class="sxs-lookup"><span data-stu-id="cf940-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![La modification de budget à 0](concurrency/_static/change-budget.png)

<span data-ttu-id="cf940-121">Avant de Jane clique sur **enregistrer**, John consulte la même page et modifie le champ Date de début à partir de 1/9/2007, à 1/9/2013.</span><span class="sxs-lookup"><span data-stu-id="cf940-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![La modification de la date de début pour 2013](concurrency/_static/change-date.png)

<span data-ttu-id="cf940-123">Jane clique sur **enregistrer** première et voit sa change lorsque le navigateur affiche la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="cf940-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Allocation de réserve modifiée à zéro](concurrency/_static/budget-zero.png)

<span data-ttu-id="cf940-125">John clique sur **enregistrer** sur une page d’édition qui affiche toujours un budget de $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="cf940-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="cf940-126">Que se passe-t-il ensuite est déterminé par la façon dont vous gérez les conflits d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="cf940-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="cf940-127">L’accès concurrentiel optimiste inclut les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="cf940-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="cf940-128">Vous pouvez effectuer le suivi des dont un utilisateur a modifié la propriété et mettre à jour uniquement les colonnes correspondantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="cf940-129">Dans le scénario, aucune donnée n’a été perdue.</span><span class="sxs-lookup"><span data-stu-id="cf940-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="cf940-130">Des propriétés différentes ont été mis à jour par les deux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="cf940-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="cf940-131">La prochaine fois qu’un utilisateur parcourt le service en anglais, il voit les modifications à la fois de John et Jane.</span><span class="sxs-lookup"><span data-stu-id="cf940-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="cf940-132">Cette méthode de mise à jour peut réduire le nombre de conflits qui peuvent entraîner une perte de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="cf940-133">Cette approche : * ne peut pas éviter une perte de données si des modifications concurrentes sont apportées à la même propriété.</span><span class="sxs-lookup"><span data-stu-id="cf940-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="cf940-134">* N’est généralement pas pratique dans une application web.</span><span class="sxs-lookup"><span data-stu-id="cf940-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="cf940-135">Elle nécessite la gestion de l’état significatif pour effectuer le suivi d’extraites de toutes les valeurs et les nouvelles valeurs.</span><span class="sxs-lookup"><span data-stu-id="cf940-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="cf940-136">Maintenance de grandes quantités d’état peut affecter les performances de l’application.</span><span class="sxs-lookup"><span data-stu-id="cf940-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="cf940-137">* Peut augmenter la complexité des applications par rapport à la détection de concurrence sur une entité.</span><span class="sxs-lookup"><span data-stu-id="cf940-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="cf940-138">Vous pouvez laisser les modifications de John écrase les modifications de Jeanne.</span><span class="sxs-lookup"><span data-stu-id="cf940-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="cf940-139">La prochaine fois que quelqu'un accède le service en anglais, il voit le 1/9/2013 et la valeur de $350,000.00 extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="cf940-140">Cette approche est appelée un *Client Wins* ou *dernier dans Wins* scénario.</span><span class="sxs-lookup"><span data-stu-id="cf940-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="cf940-141">(Toutes les valeurs à partir du client sont prioritaires sur les nouveautés dans le magasin de données). Si vous ne le faites pas de codage pour la gestion d’accès concurrentiel, priorité au Client se produit automatiquement.</span><span class="sxs-lookup"><span data-stu-id="cf940-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="cf940-142">Vous pouvez empêcher la modification de Jean à partir de la mise à jour dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="cf940-143">En règle générale, l’application serait : * affiche un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="cf940-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="cf940-144">* Afficher l’état actuel des données.</span><span class="sxs-lookup"><span data-stu-id="cf940-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="cf940-145">* Autoriser l’utilisateur à réappliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="cf940-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="cf940-146">Cela s’appelle un *magasin Wins* scénario.</span><span class="sxs-lookup"><span data-stu-id="cf940-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="cf940-147">(Les valeurs du magasin de données sont prioritaires sur les valeurs soumis par le client). Implémenter le scénario de magasin Wins dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="cf940-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="cf940-148">Cette méthode garantit qu’aucune modification n’est remplacées sans que l’utilisateur est averti.</span><span class="sxs-lookup"><span data-stu-id="cf940-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="cf940-149">Gestion d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="cf940-149">Handling concurrency</span></span> 

<span data-ttu-id="cf940-150">Quand une propriété est configurée comme un [jeton d’accès concurrentiel](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="cf940-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="cf940-151">EF Core vérifie que propriété n’a pas été modifiée après que qu’elle a été extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="cf940-152">La vérification se produit lorsque [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) ou [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) est appelée.</span><span class="sxs-lookup"><span data-stu-id="cf940-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="cf940-153">Si la propriété a été modifiée après qu’elle a été extraite, un [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) est levée.</span><span class="sxs-lookup"><span data-stu-id="cf940-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="cf940-154">Le modèle de données et de la base de données doit être configuré pour prendre en charge de lever `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="cf940-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="cf940-155">Détection des conflits d’accès concurrentiel sur une propriété</span><span class="sxs-lookup"><span data-stu-id="cf940-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="cf940-156">Conflits d’accès concurrentiel peuvent être détectées au niveau de la propriété avec le [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribut.</span><span class="sxs-lookup"><span data-stu-id="cf940-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="cf940-157">L’attribut peut être appliqué à plusieurs propriétés sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="cf940-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="cf940-158">Pour plus d’informations, consultez [Annotations de données-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="cf940-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="cf940-159">Le `[ConcurrencyCheck]` attribut n’est pas utilisé dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="cf940-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="cf940-160">Détection des conflits d’accès concurrentiel sur une ligne</span><span class="sxs-lookup"><span data-stu-id="cf940-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="cf940-161">Pour détecter les conflits d’accès concurrentiel, une [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) le suivi de colonne est ajouté au modèle.</span><span class="sxs-lookup"><span data-stu-id="cf940-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="cf940-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="cf940-162">`rowversion` :</span></span>

* <span data-ttu-id="cf940-163">SQL Server est spécifique.</span><span class="sxs-lookup"><span data-stu-id="cf940-163">Is SQL Server specific.</span></span> <span data-ttu-id="cf940-164">Autres bases de données peut ne pas fournissent une fonctionnalité similaire.</span><span class="sxs-lookup"><span data-stu-id="cf940-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="cf940-165">Permet de déterminer qu’une entité n’a pas été modifiée depuis qu’elle a été extraite à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="cf940-166">La base de données génère une liste séquentielle `rowversion` nombre qui est incrémentée chaque fois que la ligne est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="cf940-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="cf940-167">Dans un `Update` ou `Delete` commande, le `Where` clause inclut la valeur extraite de `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="cf940-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="cf940-168">Si la ligne mise à jour a changé :</span><span class="sxs-lookup"><span data-stu-id="cf940-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="cf940-169">`rowversion`ne correspond pas à la valeur extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="cf940-170">Le `Update` ou `Delete` commandes ne trouve pas une ligne, car le `Where` clause inclut l’extraction `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="cf940-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="cf940-171">A `DbUpdateConcurrencyException` est levée.</span><span class="sxs-lookup"><span data-stu-id="cf940-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="cf940-172">Dans EF Core, lorsque aucune ligne n’a été mis à jour par une `Update` ou `Delete` de commande, une exception d’accès concurrentiel est levée.</span><span class="sxs-lookup"><span data-stu-id="cf940-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="cf940-173">Ajouter une propriété de suivi à l’entité du service</span><span class="sxs-lookup"><span data-stu-id="cf940-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="cf940-174">Dans *Models/Department.cs*, ajouter une propriété de suivi nommée RowVersion :</span><span class="sxs-lookup"><span data-stu-id="cf940-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="cf940-175">Le [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribut spécifie que cette colonne est incluse dans le `Where` clause de `Update` et `Delete` commandes.</span><span class="sxs-lookup"><span data-stu-id="cf940-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="cf940-176">L’attribut est appelé `Timestamp` , car les versions précédentes de SQL Server utilisaient SQL `timestamp` type de données avant l’instruction SQL `rowversion` type remplacé.</span><span class="sxs-lookup"><span data-stu-id="cf940-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="cf940-177">L’API fluent peut également spécifier la propriété de suivi :</span><span class="sxs-lookup"><span data-stu-id="cf940-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="cf940-178">Le code suivant montre une partie de T-SQL générée par EF Core lorsque le nom du service est mis à jour :</span><span class="sxs-lookup"><span data-stu-id="cf940-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="cf940-179">L’exemple précédent en surbrillance montre le code le `WHERE` clause contenant `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="cf940-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="cf940-180">Si la base de données `RowVersion` n’est pas égal le `RowVersion` paramètre (`@p2`), aucune ligne n’est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="cf940-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="cf940-181">Le code en surbrillance suivant montre le code T-SQL qui vérifie qu’une seule ligne a été mis à jour :</span><span class="sxs-lookup"><span data-stu-id="cf940-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="cf940-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) retourne le nombre de lignes affectées par la dernière instruction.</span><span class="sxs-lookup"><span data-stu-id="cf940-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="cf940-183">Absence lignes sont mises à jour, EF Core lève une `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="cf940-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="cf940-184">Vous pouvez voir que le cœur de EF T-SQL génère dans la fenêtre Sortie de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cf940-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="cf940-185">Mise à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="cf940-185">Update the DB</span></span>

<span data-ttu-id="cf940-186">Ajout de la `RowVersion` propriété modifie le modèle de base de données, ce qui requiert une migration.</span><span class="sxs-lookup"><span data-stu-id="cf940-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="cf940-187">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="cf940-187">Build the project.</span></span> <span data-ttu-id="cf940-188">Dans une fenêtre de commande, entrez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="cf940-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="cf940-189">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="cf940-189">The preceding commands:</span></span>

* <span data-ttu-id="cf940-190">Ajoute le *Migrations / {temps stamp}_RowVersion.cs* fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="cf940-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="cf940-191">Les mises à jour le *Migrations/SchoolContextModelSnapshot.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="cf940-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="cf940-192">La mise à jour ajoute le code en surbrillance suivant à la `BuildModel` méthode :</span><span class="sxs-lookup"><span data-stu-id="cf940-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="cf940-193">Exécute des migrations pour mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="cf940-194">Structure du modèle de services</span><span class="sxs-lookup"><span data-stu-id="cf940-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="cf940-195">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cf940-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="cf940-196">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="cf940-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="cf940-197">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cf940-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="cf940-198">Les structures de commande précédent la `Department` modèle.</span><span class="sxs-lookup"><span data-stu-id="cf940-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="cf940-199">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cf940-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="cf940-200">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="cf940-200">Build the project.</span></span> <span data-ttu-id="cf940-201">La build génère des erreurs comme suit :</span><span class="sxs-lookup"><span data-stu-id="cf940-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="cf940-202">Modifier globalement `_context.Department` à `_context.Departments` (autrement dit, ajoutez un « s » pour `Department`).</span><span class="sxs-lookup"><span data-stu-id="cf940-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="cf940-203">7 occurrences sont trouvées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="cf940-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="cf940-204">Mise à jour de la page d’Index de services</span><span class="sxs-lookup"><span data-stu-id="cf940-204">Update the Departments Index page</span></span>

<span data-ttu-id="cf940-205">Le moteur de génération de modèles automatique créé un `RowVersion` colonne pour la page d’Index, mais ce champ ne doit pas être affichée.</span><span class="sxs-lookup"><span data-stu-id="cf940-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="cf940-206">Dans ce didacticiel, le dernier octet de la `RowVersion` s’affiche pour aider à comprendre la concurrence.</span><span class="sxs-lookup"><span data-stu-id="cf940-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="cf940-207">Le dernier octet n’est pas garanti pour être unique.</span><span class="sxs-lookup"><span data-stu-id="cf940-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="cf940-208">Dans une application réelle n’aurait aucun affichage `RowVersion` ou le dernier octet de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="cf940-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="cf940-209">Mise à jour de la page d’Index :</span><span class="sxs-lookup"><span data-stu-id="cf940-209">Update the Index page:</span></span>

* <span data-ttu-id="cf940-210">Avec les services de remplacer l’Index.</span><span class="sxs-lookup"><span data-stu-id="cf940-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="cf940-211">Remplacez le balisage contenant `RowVersion` avec le dernier octet de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="cf940-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="cf940-212">Remplacez FirstMidName par nom complet.</span><span class="sxs-lookup"><span data-stu-id="cf940-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="cf940-213">Le balisage suivant montre la page de mise à jour :</span><span class="sxs-lookup"><span data-stu-id="cf940-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="cf940-214">Mettre à jour le modèle de page de modification</span><span class="sxs-lookup"><span data-stu-id="cf940-214">Update the Edit page model</span></span>

<span data-ttu-id="cf940-215">Mise à jour *pages\departments\edit.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cf940-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="cf940-216">Pour détecter un problème d’accès concurrentiel, le [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) est mis à jour avec la `rowVersion` la valeur de l’entité a été qu’elle a été extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity was it was fetched.</span></span> <span data-ttu-id="cf940-217">EF Core génère une commande SQL UPDATE avec une clause WHERE contenant la version d’origine `RowVersion` valeur.</span><span class="sxs-lookup"><span data-stu-id="cf940-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="cf940-218">Si aucune ligne n’est affectées par la commande de mise à jour (aucune ligne n’ont d’origine `RowVersion` valeur), un `DbUpdateConcurrencyException` exception est levée.</span><span class="sxs-lookup"><span data-stu-id="cf940-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="cf940-219">Dans le code précédent, `Department.RowVersion` est la valeur lorsque l’entité a été extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="cf940-220">`OriginalValue`est la valeur de la base de données lorsque `FirstOrDefaultAsync` a été appelée dans cette méthode.</span><span class="sxs-lookup"><span data-stu-id="cf940-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="cf940-221">Le code suivant obtient les valeurs de client (les valeurs publiées dans cette méthode) et les valeurs de la base de données :</span><span class="sxs-lookup"><span data-stu-id="cf940-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="cf940-222">Le code suivant ajoute un message d’erreur personnalisé pour chaque colonne dont la base de données les valeurs différentes à partir de ce qui a été validé dans `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="cf940-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="cf940-223">Les éléments suivants mis en surbrillance le code définit la `RowVersion` valeur à la nouvelle valeur récupérée à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="cf940-224">La prochaine fois que l’utilisateur clique sur **enregistrer**, seules les erreurs d’accès concurrentiel qui se produisent dans la mesure où le dernier affichage de la page de modification est interceptée.</span><span class="sxs-lookup"><span data-stu-id="cf940-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="cf940-225">Le `ModelState.Remove` instruction est requise car `ModelState` a l’ancien `RowVersion` valeur.</span><span class="sxs-lookup"><span data-stu-id="cf940-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="cf940-226">Dans la Page Razor, le `ModelState` valeur pour un champ est prioritaire sur les valeurs de propriété de modèle lorsque les deux sont présents.</span><span class="sxs-lookup"><span data-stu-id="cf940-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="cf940-227">Mise à jour de la page de modification</span><span class="sxs-lookup"><span data-stu-id="cf940-227">Update the Edit page</span></span>

<span data-ttu-id="cf940-228">Mise à jour *Pages/Departments/Edit.cshtml* par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="cf940-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="cf940-229">Le balisage précédent :</span><span class="sxs-lookup"><span data-stu-id="cf940-229">The preceding markup:</span></span>

* <span data-ttu-id="cf940-230">Les mises à jour le `page` de `@page` à `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="cf940-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="cf940-231">Ajoute une version de ligne masquée.</span><span class="sxs-lookup"><span data-stu-id="cf940-231">Adds a hidden row version.</span></span> <span data-ttu-id="cf940-232">`RowVersion`doit être ajouté pour la publication lie la valeur.</span><span class="sxs-lookup"><span data-stu-id="cf940-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="cf940-233">Affiche le dernier octet de `RowVersion` à des fins de débogage.</span><span class="sxs-lookup"><span data-stu-id="cf940-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="cf940-234">Remplace `ViewData` avec le fortement typé `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="cf940-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="cf940-235">Conflits d’accès concurrentiel de test avec la page de modification</span><span class="sxs-lookup"><span data-stu-id="cf940-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="cf940-236">Ouvrez deux instances de navigateurs de modification sur le service en anglais :</span><span class="sxs-lookup"><span data-stu-id="cf940-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="cf940-237">Exécutez l’application et sélectionnez Services.</span><span class="sxs-lookup"><span data-stu-id="cf940-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="cf940-238">Avec le bouton droit le **modifier** lien hypertexte pour le service en anglais, puis sélectionnez **ouvrir dans un nouvel onglet**.</span><span class="sxs-lookup"><span data-stu-id="cf940-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="cf940-239">Dans le premier onglet, cliquez sur le **modifier** lien hypertexte pour le service en anglais.</span><span class="sxs-lookup"><span data-stu-id="cf940-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="cf940-240">Les onglets de deux navigateur affichent les mêmes informations.</span><span class="sxs-lookup"><span data-stu-id="cf940-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="cf940-241">Modifier le nom dans le premier onglet de navigateur, puis cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="cf940-241">Change the name in the first browser tab and click **Save**.</span></span>

![Édition de service page 1 après modification](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="cf940-243">Le navigateur affiche la page d’Index de la valeur modifiée et d’un indicateur de mise à jour rowVersion.</span><span class="sxs-lookup"><span data-stu-id="cf940-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="cf940-244">Notez l’indicateur rowVersion mis à jour, il est affiché sur la deuxième publication dans l’autre onglet.</span><span class="sxs-lookup"><span data-stu-id="cf940-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="cf940-245">Modifier un champ différent dans le deuxième onglet du navigateur.</span><span class="sxs-lookup"><span data-stu-id="cf940-245">Change a different field in the second browser tab.</span></span>

![Modification du service page 2 après la modification](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="cf940-247">Cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="cf940-247">Click **Save**.</span></span> <span data-ttu-id="cf940-248">Vous consultez les messages d’erreur pour tous les champs ne correspondent pas aux valeurs de la base de données :</span><span class="sxs-lookup"><span data-stu-id="cf940-248">You see error messages for all fields that don't match the DB values:</span></span>

![Message d’erreur service modifier page](concurrency/_static/edit-error.png)

<span data-ttu-id="cf940-250">Cette fenêtre de navigateur ne souhaitez pas modifier le champ nom.</span><span class="sxs-lookup"><span data-stu-id="cf940-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="cf940-251">Copiez et collez la valeur actuelle (langues) dans le champ nom.</span><span class="sxs-lookup"><span data-stu-id="cf940-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="cf940-252">Appuyez sur TAB. La validation côté client supprime le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="cf940-252">Tab out. Client-side validation removes the error message.</span></span>

![Message d’erreur service modifier page](concurrency/_static/cv.png)

<span data-ttu-id="cf940-254">Cliquez sur **enregistrer** à nouveau.</span><span class="sxs-lookup"><span data-stu-id="cf940-254">Click **Save** again.</span></span> <span data-ttu-id="cf940-255">La valeur que vous avez entré dans le deuxième onglet navigateur est enregistrée.</span><span class="sxs-lookup"><span data-stu-id="cf940-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="cf940-256">Vous voyez les valeurs enregistrées dans la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="cf940-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="cf940-257">Mise à jour de la page de suppression</span><span class="sxs-lookup"><span data-stu-id="cf940-257">Update the Delete page</span></span>

<span data-ttu-id="cf940-258">Mettre à jour le modèle de page de suppression par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cf940-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="cf940-259">La page de suppression détecte les conflits d’accès concurrentiel lorsque l’entité a été modifiée après que qu’elle a été extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="cf940-260">`Department.RowVersion`est la version de ligne lorsque l’entité a été extraite.</span><span class="sxs-lookup"><span data-stu-id="cf940-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="cf940-261">Lorsque EF Core crée la commande SQL DELETE, il inclut une clause WHERE avec `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="cf940-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="cf940-262">Si les résultats de la commande SQL DELETE dans des lignes nulles affectés :</span><span class="sxs-lookup"><span data-stu-id="cf940-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="cf940-263">Le `RowVersion` dans le SQL DELETE commande ne correspond pas à `RowVersion` dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="cf940-264">Une exception DbUpdateConcurrencyException est levée.</span><span class="sxs-lookup"><span data-stu-id="cf940-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="cf940-265">`OnGetAsync`est appelée avec le `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="cf940-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="cf940-266">Mise à jour de la page de suppression</span><span class="sxs-lookup"><span data-stu-id="cf940-266">Update the Delete page</span></span>

<span data-ttu-id="cf940-267">Mise à jour *Pages/Departments/Delete.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cf940-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="cf940-268">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="cf940-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="cf940-269">Les mises à jour le `page` de `@page` à `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="cf940-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="cf940-270">Ajoute un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="cf940-270">Adds an error message.</span></span>
* <span data-ttu-id="cf940-271">Remplace FirstMidName FullName dans le **administrateur** champ.</span><span class="sxs-lookup"><span data-stu-id="cf940-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="cf940-272">Modifications `RowVersion` pour afficher le dernier octet.</span><span class="sxs-lookup"><span data-stu-id="cf940-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="cf940-273">Ajoute une version de ligne masquée.</span><span class="sxs-lookup"><span data-stu-id="cf940-273">Adds a hidden row version.</span></span> <span data-ttu-id="cf940-274">`RowVersion`doit être ajouté pour la publication lie la valeur.</span><span class="sxs-lookup"><span data-stu-id="cf940-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="cf940-275">Conflits d’accès concurrentiel de test avec la page de suppression</span><span class="sxs-lookup"><span data-stu-id="cf940-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="cf940-276">Créer un service de test.</span><span class="sxs-lookup"><span data-stu-id="cf940-276">Create a test department.</span></span>

<span data-ttu-id="cf940-277">Ouvrez deux instances de navigateurs de suppression sur le service de test :</span><span class="sxs-lookup"><span data-stu-id="cf940-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="cf940-278">Exécutez l’application et sélectionnez Services.</span><span class="sxs-lookup"><span data-stu-id="cf940-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="cf940-279">Avec le bouton droit le **supprimer** lien hypertexte pour le service de test, puis sélectionnez **ouvrir dans un nouvel onglet**.</span><span class="sxs-lookup"><span data-stu-id="cf940-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="cf940-280">Cliquez sur le **modifier** lien hypertexte pour le service de test.</span><span class="sxs-lookup"><span data-stu-id="cf940-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="cf940-281">Les onglets de deux navigateur affichent les mêmes informations.</span><span class="sxs-lookup"><span data-stu-id="cf940-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="cf940-282">Modifier l’allocation de réserve dans le premier onglet de navigateur, cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="cf940-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="cf940-283">Le navigateur affiche la page d’Index de la valeur modifiée et d’un indicateur de mise à jour rowVersion.</span><span class="sxs-lookup"><span data-stu-id="cf940-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="cf940-284">Notez l’indicateur rowVersion mis à jour, il est affiché sur la deuxième publication dans l’autre onglet.</span><span class="sxs-lookup"><span data-stu-id="cf940-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="cf940-285">Supprimer le service de test à partir du deuxième onglet. Une erreur d’accès concurrentiel est s’affichent avec les valeurs actuelles à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="cf940-286">En cliquant sur **supprimer** supprime l’entité, sauf si `RowVersion` a été updated.department a été supprimé.</span><span class="sxs-lookup"><span data-stu-id="cf940-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="cf940-287">Consultez [héritage](xref:data/ef-mvc/inheritance) pour obtenir des instructions sur la façon d’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="cf940-287">See [Inheritance](xref:data/ef-mvc/inheritance) for instruction on how to inheritance in the data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="cf940-288">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cf940-288">Additional resources</span></span>

* [<span data-ttu-id="cf940-289">Jetons d’accès concurrentiel dans EF Core</span><span class="sxs-lookup"><span data-stu-id="cf940-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="cf940-290">Gestion d’accès concurrentiel dans EF Core</span><span class="sxs-lookup"><span data-stu-id="cf940-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="cf940-291">Précédent</span><span class="sxs-lookup"><span data-stu-id="cf940-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
