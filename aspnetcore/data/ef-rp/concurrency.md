---
title: Pages Razor avec EF Core dans ASP.NET Core - Accès concurrentiel - 8 sur 8
author: rick-anderson
description: Ce didacticiel montre comment gérer les conflits quand plusieurs utilisateurs mettent à jour la même entité en même temps.
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2018
uid: data/ef-rp/concurrency
ms.openlocfilehash: a6c264e460855c9f1d6f5a363eb7ee2cf69619ee
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346292"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="bc89f-103">Pages Razor avec EF Core dans ASP.NET Core - Accès concurrentiel - 8 sur 8</span><span class="sxs-lookup"><span data-stu-id="bc89f-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="bc89f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra) et [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="bc89f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="bc89f-105">Ce didacticiel montre comment gérer les conflits quand plusieurs utilisateurs mettent à jour une entité en même temps.</span><span class="sxs-lookup"><span data-stu-id="bc89f-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="bc89f-106">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, [téléchargez ou affichez l’application terminée](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="bc89f-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="bc89f-107">[Télécharger les instructions](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="bc89f-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="bc89f-108">Conflits d’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="bc89f-108">Concurrency conflicts</span></span>

<span data-ttu-id="bc89f-109">Un conflit d’accès concurrentiel se produit quand :</span><span class="sxs-lookup"><span data-stu-id="bc89f-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="bc89f-110">Un utilisateur accède à la page de modification d’une entité.</span><span class="sxs-lookup"><span data-stu-id="bc89f-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="bc89f-111">Un autre utilisateur met à jour la même entité avant que la modification du premier utilisateur soit écrite dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="bc89f-112">Si la détection d’accès concurrentiel n’est pas activée, quand des mises à jour simultanées se produisent :</span><span class="sxs-lookup"><span data-stu-id="bc89f-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="bc89f-113">La dernière mise à jour est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="bc89f-113">The last update wins.</span></span> <span data-ttu-id="bc89f-114">Autrement dit, les dernières valeurs mises à jour sont enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="bc89f-115">La première des mises à jour en cours est perdue.</span><span class="sxs-lookup"><span data-stu-id="bc89f-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="bc89f-116">Accès concurrentiel optimiste</span><span class="sxs-lookup"><span data-stu-id="bc89f-116">Optimistic concurrency</span></span>

<span data-ttu-id="bc89f-117">L’accès concurrentiel optimiste autorise la survenance des conflits d’accès concurrentiel, et réagit correctement quand ils surviennent.</span><span class="sxs-lookup"><span data-stu-id="bc89f-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="bc89f-118">Par exemple, Jane consulte la page de modification de département et change le montant de « Budget » pour le département « English » en le faisant passer de 350 000,00 $ à 0,00 $.</span><span class="sxs-lookup"><span data-stu-id="bc89f-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Modification de la valeur de budget sur 0](concurrency/_static/change-budget.png)

<span data-ttu-id="bc89f-120">Avant que Jane clique sur **Save**, John consulte la même page et change le champ Start Date de 01/09/2007 en 01/09/2013.</span><span class="sxs-lookup"><span data-stu-id="bc89f-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Modification de la date de début sur 2013](concurrency/_static/change-date.png)

<span data-ttu-id="bc89f-122">Jane clique la première sur **Save** et voit sa modification quand le navigateur revient à la page Index.</span><span class="sxs-lookup"><span data-stu-id="bc89f-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Le budget est passé à zéro](concurrency/_static/budget-zero.png)

<span data-ttu-id="bc89f-124">John clique sur **Save** dans une page Edit qui affiche toujours un budget de 350 000,00 $.</span><span class="sxs-lookup"><span data-stu-id="bc89f-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="bc89f-125">Ce qui se passe ensuite est déterminé par la façon dont vous gérez les conflits d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="bc89f-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="bc89f-126">L’accès concurrentiel optimiste comprend les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc89f-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="bc89f-127">Vous pouvez effectuer le suivi des propriétés modifiées par un utilisateur et mettre à jour seulement les colonnes correspondantes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="bc89f-128">Dans le scénario, aucune donnée ne serait perdue.</span><span class="sxs-lookup"><span data-stu-id="bc89f-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="bc89f-129">Des propriétés différentes ont été mises à jour par les deux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="bc89f-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="bc89f-130">La prochaine fois que quelqu’un consultera le département « English », il verra à la fois les modifications de Jane et de John.</span><span class="sxs-lookup"><span data-stu-id="bc89f-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="bc89f-131">Cette méthode de mise à jour peut réduire le nombre de conflits susceptibles d’entraîner une perte de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="bc89f-132">Cette approche a les caractéristiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc89f-132">This approach:</span></span>
 
  * <span data-ttu-id="bc89f-133">Elle ne peut pas éviter une perte de données si des modifications concurrentes sont apportées à la même propriété.</span><span class="sxs-lookup"><span data-stu-id="bc89f-133">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="bc89f-134">Elle n’est généralement pas pratique dans une application web.</span><span class="sxs-lookup"><span data-stu-id="bc89f-134">Is generally not practical in a web app.</span></span> <span data-ttu-id="bc89f-135">Elle nécessite la tenue à jour d’un état significatif afin d’effectuer le suivi de toutes les valeurs récupérées et des nouvelles valeurs.</span><span class="sxs-lookup"><span data-stu-id="bc89f-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="bc89f-136">La maintenance de grandes quantités d’état peut affecter les performances de l’application.</span><span class="sxs-lookup"><span data-stu-id="bc89f-136">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="bc89f-137">Elle peut augmenter la complexité de l’application par rapport à la détection de l’accès concurrentiel sur une entité.</span><span class="sxs-lookup"><span data-stu-id="bc89f-137">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="bc89f-138">Vous pouvez laisser les modifications de John remplacer les modifications de Jane.</span><span class="sxs-lookup"><span data-stu-id="bc89f-138">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="bc89f-139">La prochaine fois que quelqu’un consultera le département « English », il verra la date 01/09/2013 et la valeur 350 000,00 $ récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-139">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="bc89f-140">Cette approche est un scénario *Priorité au client* ou *Priorité au dernier*.</span><span class="sxs-lookup"><span data-stu-id="bc89f-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="bc89f-141">(Toutes les valeurs du client sont prioritaires par rapport au contenu du magasin de données.) Si vous n’écrivez pas de code pour la gestion de l’accès concurrentiel, la Priorité au client est appliquée automatiquement.</span><span class="sxs-lookup"><span data-stu-id="bc89f-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="bc89f-142">Vous pouvez empêcher les modifications de John d’être mises à jour dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="bc89f-143">En règle générale, l’application :</span><span class="sxs-lookup"><span data-stu-id="bc89f-143">Typically, the app would:</span></span>

  * <span data-ttu-id="bc89f-144">affiche un message d’erreur ;</span><span class="sxs-lookup"><span data-stu-id="bc89f-144">Display an error message.</span></span>
  * <span data-ttu-id="bc89f-145">indique l’état actuel des données ;</span><span class="sxs-lookup"><span data-stu-id="bc89f-145">Show the current state of the data.</span></span>
  * <span data-ttu-id="bc89f-146">autorise l’utilisateur à réappliquer les modifications.</span><span class="sxs-lookup"><span data-stu-id="bc89f-146">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="bc89f-147">Il s’agit alors d’un scénario *Priorité au magasin*.</span><span class="sxs-lookup"><span data-stu-id="bc89f-147">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="bc89f-148">(Les valeurs du magasin de données sont prioritaires par rapport à celles soumises par le client.) Nous allons implémenter le scénario Priorité au magasin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="bc89f-148">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="bc89f-149">Cette méthode garantit qu’aucune modification n’est remplacée sans qu’un utilisateur soit averti.</span><span class="sxs-lookup"><span data-stu-id="bc89f-149">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="bc89f-150">Gestion de l’accès concurrentiel</span><span class="sxs-lookup"><span data-stu-id="bc89f-150">Handling concurrency</span></span> 

<span data-ttu-id="bc89f-151">Quand une propriété est configurée en tant que [jeton d’accès concurrentiel](/ef/core/modeling/concurrency) :</span><span class="sxs-lookup"><span data-stu-id="bc89f-151">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="bc89f-152">EF Core vérifie que cette propriété n’a pas été modifiée après avoir été récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-152">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="bc89f-153">La vérification a lieu quand [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) ou [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) est appelée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-153">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="bc89f-154">Si la propriété a été modifiée après avoir été récupérée, une [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) est levée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-154">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="bc89f-155">Le modèle de données et de la base de données doivent être configurés pour prendre en charge la levée de `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-155">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="bc89f-156">Détection des conflits d’accès concurrentiel sur une propriété</span><span class="sxs-lookup"><span data-stu-id="bc89f-156">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="bc89f-157">Les conflits d’accès concurrentiel peuvent être détectés au niveau de la propriété avec l’attribut [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="bc89f-157">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="bc89f-158">L’attribut peut être appliqué à plusieurs propriétés sur le modèle.</span><span class="sxs-lookup"><span data-stu-id="bc89f-158">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="bc89f-159">Pour plus d’informations, consultez [Data Annotations-ConcurrencyCheck (Annotations de données-ConcurrencyCheck)](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="bc89f-159">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="bc89f-160">Nous n’utilisons pas l’attribut `[ConcurrencyCheck]` dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="bc89f-160">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="bc89f-161">Détection des conflits d’accès concurrentiel sur une ligne</span><span class="sxs-lookup"><span data-stu-id="bc89f-161">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="bc89f-162">Pour détecter les conflits d’accès concurrentiel, une colonne de suivi [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) est ajoutée au modèle.</span><span class="sxs-lookup"><span data-stu-id="bc89f-162">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="bc89f-163">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="bc89f-163">`rowversion` :</span></span>

* <span data-ttu-id="bc89f-164">est propre à SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bc89f-164">Is SQL Server specific.</span></span> <span data-ttu-id="bc89f-165">D’autres bases de données peuvent ne pas fournir une fonctionnalité similaire.</span><span class="sxs-lookup"><span data-stu-id="bc89f-165">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="bc89f-166">Sert à déterminer qu’une entité n’a pas été modifiée depuis qu’elle a été récupérée à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-166">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="bc89f-167">La base de données génère un numéro `rowversion` séquentiel qui est incrémenté chaque fois que la ligne est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="bc89f-167">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="bc89f-168">Dans une commande `Update` ou `Delete`, la clause `Where` comprend la valeur récupérée de `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-168">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="bc89f-169">Si la ligne mise à jour a changé :</span><span class="sxs-lookup"><span data-stu-id="bc89f-169">If the row being updated has changed:</span></span>

 * <span data-ttu-id="bc89f-170">`rowversion` ne correspond pas à la valeur récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-170">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="bc89f-171">Les commandes `Update` ou `Delete` ne trouvent pas de ligne, car la clause `Where` comprend la valeur `rowversion` récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-171">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="bc89f-172">Une `DbUpdateConcurrencyException` est levée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-172">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="bc89f-173">Dans EF Core, quand aucune ligne n’a été mise à jour par une commande `Update` ou `Delete`, une exception d’accès concurrentiel est levée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-173">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="bc89f-174">Ajouter une propriété de suivi à l’entité Department</span><span class="sxs-lookup"><span data-stu-id="bc89f-174">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="bc89f-175">Dans *Models/Department.cs*, ajoutez une propriété de suivi nommée RowVersion :</span><span class="sxs-lookup"><span data-stu-id="bc89f-175">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="bc89f-176">L’attribut [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) spécifie que cette colonne est incluse dans la clause `Where` des commandes `Update` et `Delete`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-176">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="bc89f-177">L’attribut se nomme `Timestamp`, car les versions précédentes de SQL Server utilisaient un type de données SQL `timestamp` avant son remplacement par le type SQL `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-177">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="bc89f-178">L’API Fluent peut également spécifier la propriété de suivi :</span><span class="sxs-lookup"><span data-stu-id="bc89f-178">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="bc89f-179">Le code suivant montre une partie du T-SQL généré par EF Core quand le nom du département est mis à jour :</span><span class="sxs-lookup"><span data-stu-id="bc89f-179">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="bc89f-180">Le code en surbrillance ci-dessus montre la clause `WHERE` contenant `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-180">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="bc89f-181">Si la `RowVersion` de la base de données n’est pas égale au paramètre `RowVersion` (`@p2`), aucune ligne n’est mise à jour.</span><span class="sxs-lookup"><span data-stu-id="bc89f-181">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="bc89f-182">Le code en surbrillance suivant montre le T-SQL qui vérifie qu’une seule ligne a été mise à jour :</span><span class="sxs-lookup"><span data-stu-id="bc89f-182">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="bc89f-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) retourne le nombre de lignes affectées par la dernière instruction.</span><span class="sxs-lookup"><span data-stu-id="bc89f-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="bc89f-184">Si aucune ligne n’est mise à jour, EF Core lève une `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-184">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="bc89f-185">Vous pouvez voir le T-SQL généré par EF Core dans la fenêtre Sortie de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc89f-185">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="bc89f-186">Mettre à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="bc89f-186">Update the DB</span></span>

<span data-ttu-id="bc89f-187">L’ajout de la propriété `RowVersion` change le modèle de base de données, ce qui nécessite une migration.</span><span class="sxs-lookup"><span data-stu-id="bc89f-187">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="bc89f-188">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="bc89f-188">Build the project.</span></span> <span data-ttu-id="bc89f-189">Entrez ce qui suit dans une fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="bc89f-189">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="bc89f-190">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="bc89f-190">The preceding commands:</span></span>

* <span data-ttu-id="bc89f-191">Ajoutent le fichier de migration *Migrations/{horodatage}_RowVersion.cs*.</span><span class="sxs-lookup"><span data-stu-id="bc89f-191">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="bc89f-192">Mettent à jour le fichier *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="bc89f-192">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="bc89f-193">La mise à jour ajoute le code en surbrillance suivant à la méthode `BuildModel` :</span><span class="sxs-lookup"><span data-stu-id="bc89f-193">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="bc89f-194">Exécutent des migrations pour mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-194">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="bc89f-195">Générer automatiquement le modèle Departments</span><span class="sxs-lookup"><span data-stu-id="bc89f-195">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc89f-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc89f-196">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bc89f-197">Suivez les instructions fournies dans [Générer automatiquement le modèle d’étudiant](xref:data/ef-rp/intro#scaffold-the-student-model) et utilisez `Department` pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="bc89f-197">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Department` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bc89f-198">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="bc89f-198">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="bc89f-199">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="bc89f-199">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

------

<span data-ttu-id="bc89f-200">La commande précédente génère automatiquement le modèle `Department`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-200">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="bc89f-201">Ouvrez le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc89f-201">Open the project in Visual Studio.</span></span>

<span data-ttu-id="bc89f-202">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="bc89f-202">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="bc89f-203">Mettre à jour la page d’index des départements</span><span class="sxs-lookup"><span data-stu-id="bc89f-203">Update the Departments Index page</span></span>

<span data-ttu-id="bc89f-204">Le moteur de génération de modèles automatique créé une colonne `RowVersion` pour la page Index, mais ce champ ne doit pas être affiché.</span><span class="sxs-lookup"><span data-stu-id="bc89f-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="bc89f-205">Dans ce didacticiel, le dernier octet de `RowVersion` est affiché afin d’aider à mieux comprendre l’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="bc89f-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="bc89f-206">Il n’est pas garanti que le dernier octet soit unique.</span><span class="sxs-lookup"><span data-stu-id="bc89f-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="bc89f-207">Une application réelle n’afficherait pas `RowVersion` ou le dernier octet de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="bc89f-208">Mettez à jour la page Index :</span><span class="sxs-lookup"><span data-stu-id="bc89f-208">Update the Index page:</span></span>

* <span data-ttu-id="bc89f-209">Remplacez Index par Departments.</span><span class="sxs-lookup"><span data-stu-id="bc89f-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="bc89f-210">Remplacez le balisage contenant `RowVersion` par le dernier octet de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="bc89f-211">Remplacez FirstMidName par FullName.</span><span class="sxs-lookup"><span data-stu-id="bc89f-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="bc89f-212">Le balisage suivant montre la page mise à jour :</span><span class="sxs-lookup"><span data-stu-id="bc89f-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="bc89f-213">Mettre à jour le modèle de page de modification</span><span class="sxs-lookup"><span data-stu-id="bc89f-213">Update the Edit page model</span></span>

<span data-ttu-id="bc89f-214">Mettez à jour *pages\departments\edit.cshtml.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc89f-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="bc89f-215">Pour détecter un problème d’accès concurrentiel, [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) est mise à jour avec la valeur `rowVersion` de l’entité récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="bc89f-216">EF Core génère une commande SQL UPDATE avec une clause WHERE contenant la valeur `RowVersion` d’origine.</span><span class="sxs-lookup"><span data-stu-id="bc89f-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="bc89f-217">Si aucune ligne n’est affectée par la commande UPDATE (aucune ligne ne contient la valeur `RowVersion` d’origine), une exception `DbUpdateConcurrencyException` est levée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="bc89f-218">Dans le code précédent, `Department.RowVersion` est la valeur quand l’entité a été récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="bc89f-219">`OriginalValue` est la valeur présente dans la base de données quand `FirstOrDefaultAsync` a été appelée dans cette méthode.</span><span class="sxs-lookup"><span data-stu-id="bc89f-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="bc89f-220">Le code suivant obtient les valeurs du client (celles envoyées à cette méthode) et les valeurs de la base de données :</span><span class="sxs-lookup"><span data-stu-id="bc89f-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="bc89f-221">Le code suivant ajoute un message d’erreur personnalisé pour chaque colonne dont les valeurs dans la base de données sont différentes de celles envoyées à `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="bc89f-221">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="bc89f-222">Le code en surbrillance suivant affecte à `RowVersion` la nouvelle valeur récupérée à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="bc89f-223">La prochaine fois que l’utilisateur cliquera sur **Save**, seules les erreurs d’accès concurrentiel qui se sont produites depuis le dernier affichage de la page Edit seront interceptées.</span><span class="sxs-lookup"><span data-stu-id="bc89f-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="bc89f-224">L’instruction `ModelState.Remove` est nécessaire car `ModelState` contient l’ancienne valeur `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="bc89f-225">Dans la page Razor, la valeur `ModelState` d’un champ est prioritaire par rapport aux valeurs de propriétés du modèle quand les deux sont présentes.</span><span class="sxs-lookup"><span data-stu-id="bc89f-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="bc89f-226">Mettre à jour la page Edit</span><span class="sxs-lookup"><span data-stu-id="bc89f-226">Update the Edit page</span></span>

<span data-ttu-id="bc89f-227">Mettez à jour *Pages/Departments/Edit.cshtml* avec le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="bc89f-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="bc89f-228">Le balisage précédent :</span><span class="sxs-lookup"><span data-stu-id="bc89f-228">The preceding markup:</span></span>

* <span data-ttu-id="bc89f-229">Met à jour la directive `page` en remplaçant `@page` par `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="bc89f-230">Ajoute une version de ligne masquée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-230">Adds a hidden row version.</span></span> <span data-ttu-id="bc89f-231">`RowVersion` doit être ajouté afin que la publication lie la valeur.</span><span class="sxs-lookup"><span data-stu-id="bc89f-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="bc89f-232">Affiche le dernier octet de `RowVersion` à des fins de débogage.</span><span class="sxs-lookup"><span data-stu-id="bc89f-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="bc89f-233">Remplace `ViewData` par le `InstructorNameSL` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="bc89f-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="bc89f-234">Tester les conflits d’accès concurrentiel avec la page Edit</span><span class="sxs-lookup"><span data-stu-id="bc89f-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="bc89f-235">Ouvrez deux instances de navigateur de la page Edit sur le département English :</span><span class="sxs-lookup"><span data-stu-id="bc89f-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="bc89f-236">Exécutez l’application et sélectionnez Departments.</span><span class="sxs-lookup"><span data-stu-id="bc89f-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="bc89f-237">Cliquez avec le bouton droit sur le lien hypertexte **Edit** correspondant au département English, puis sélectionnez **Open in new tab**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="bc89f-238">Sous le premier onglet, cliquez sur le lien hypertexte **Edit** correspondant au département English.</span><span class="sxs-lookup"><span data-stu-id="bc89f-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="bc89f-239">Les deux onglets de navigateur affichent les mêmes informations.</span><span class="sxs-lookup"><span data-stu-id="bc89f-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="bc89f-240">Changez le nom sous le premier onglet de navigateur, puis cliquez sur **Save**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-240">Change the name in the first browser tab and click **Save**.</span></span>

![Page 1 de modification de département après changement](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="bc89f-242">Le navigateur affiche la page Index avec la valeur modifiée et un indicateur rowVersion mis à jour.</span><span class="sxs-lookup"><span data-stu-id="bc89f-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="bc89f-243">Notez l’indicateur rowVersion mis à jour ; il est affiché sur la deuxième publication (postback) sous l’autre onglet.</span><span class="sxs-lookup"><span data-stu-id="bc89f-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="bc89f-244">Changez un champ différent sous le deuxième onglet du navigateur.</span><span class="sxs-lookup"><span data-stu-id="bc89f-244">Change a different field in the second browser tab.</span></span>

![Page Edit 2 du département après changement](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="bc89f-246">Cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-246">Click **Save**.</span></span> <span data-ttu-id="bc89f-247">Des messages d’erreur s’affichent pour tous les champs qui ne correspondent pas aux valeurs de la base de données :</span><span class="sxs-lookup"><span data-stu-id="bc89f-247">You see error messages for all fields that don't match the DB values:</span></span>

![Message d’erreur de page de modification de département](concurrency/_static/edit-error.png)

<span data-ttu-id="bc89f-249">Cette fenêtre de navigateur n’avait pas l’intention de changer le champ Name.</span><span class="sxs-lookup"><span data-stu-id="bc89f-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="bc89f-250">Copiez et collez la valeur actuelle (Languages) dans le champ Name.</span><span class="sxs-lookup"><span data-stu-id="bc89f-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="bc89f-251">Appuyez sur Tab. La validation côté client supprime le message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="bc89f-251">Tab out. Client-side validation removes the error message.</span></span>

![Message d’erreur de page de modification de département](concurrency/_static/cv.png)

<span data-ttu-id="bc89f-253">Cliquez à nouveau sur **Save**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-253">Click **Save** again.</span></span> <span data-ttu-id="bc89f-254">La valeur que vous avez entrée sous le deuxième onglet du navigateur est enregistrée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="bc89f-255">Les valeurs enregistrées sont visibles dans la page Index.</span><span class="sxs-lookup"><span data-stu-id="bc89f-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="bc89f-256">Mettre à jour la page Delete</span><span class="sxs-lookup"><span data-stu-id="bc89f-256">Update the Delete page</span></span>

<span data-ttu-id="bc89f-257">Mettez à jour le modèle de page de suppression avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc89f-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="bc89f-258">La page Delete détecte les conflits d’accès concurrentiel quand l’entité a changé après avoir été récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="bc89f-259">`Department.RowVersion` est la version de ligne quand l’entité a été récupérée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="bc89f-260">Quand EF Core crée la commande SQL DELETE, il inclut une clause WHERE avec `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="bc89f-261">Si après l’exécution de la commande SQL DELETE aucune ligne n’est affectée :</span><span class="sxs-lookup"><span data-stu-id="bc89f-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="bc89f-262">La valeur de `RowVersion` dans la commande SQL DELETE ne correspond pas à `RowVersion` dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="bc89f-263">Une exception DbUpdateConcurrencyException est levée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="bc89f-264">`OnGetAsync` est appelée avec `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="bc89f-265">Mettre à jour la page Delete</span><span class="sxs-lookup"><span data-stu-id="bc89f-265">Update the Delete page</span></span>

<span data-ttu-id="bc89f-266">Mettez à jour *Pages/Departments/Delete.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc89f-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]


<span data-ttu-id="bc89f-267">Le balisage précédent apporte les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc89f-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="bc89f-268">Il met à jour la directive `page` en remplaçant `@page` par `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="bc89f-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="bc89f-269">Il ajoute un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="bc89f-269">Adds an error message.</span></span>
* <span data-ttu-id="bc89f-270">Il remplace FirstMidName par FullName dans le champ **Administrator**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="bc89f-271">Il change `RowVersion` pour afficher le dernier octet.</span><span class="sxs-lookup"><span data-stu-id="bc89f-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="bc89f-272">Ajoute une version de ligne masquée.</span><span class="sxs-lookup"><span data-stu-id="bc89f-272">Adds a hidden row version.</span></span> <span data-ttu-id="bc89f-273">`RowVersion` doit être ajouté afin que la publication lie la valeur.</span><span class="sxs-lookup"><span data-stu-id="bc89f-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="bc89f-274">Tester les conflits d’accès concurrentiel avec la page Delete</span><span class="sxs-lookup"><span data-stu-id="bc89f-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="bc89f-275">Créez un département test.</span><span class="sxs-lookup"><span data-stu-id="bc89f-275">Create a test department.</span></span>

<span data-ttu-id="bc89f-276">Ouvrez deux instances de navigateur de la page Delete sur le département test :</span><span class="sxs-lookup"><span data-stu-id="bc89f-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="bc89f-277">Exécutez l’application et sélectionnez Departments.</span><span class="sxs-lookup"><span data-stu-id="bc89f-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="bc89f-278">Cliquez avec le bouton droit sur le lien hypertexte **Delete** correspondant au département test, puis sélectionnez **Open in new tab**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="bc89f-279">Cliquez sur le lien hypertexte **Edit** correspondant au département test.</span><span class="sxs-lookup"><span data-stu-id="bc89f-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="bc89f-280">Les deux onglets de navigateur affichent les mêmes informations.</span><span class="sxs-lookup"><span data-stu-id="bc89f-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="bc89f-281">Changez le budget sous le premier onglet de navigateur, puis cliquez sur **Save**.</span><span class="sxs-lookup"><span data-stu-id="bc89f-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="bc89f-282">Le navigateur affiche la page Index avec la valeur modifiée et un indicateur rowVersion mis à jour.</span><span class="sxs-lookup"><span data-stu-id="bc89f-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="bc89f-283">Notez l’indicateur rowVersion mis à jour ; il est affiché sur la deuxième publication (postback) sous l’autre onglet.</span><span class="sxs-lookup"><span data-stu-id="bc89f-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="bc89f-284">Supprimez le département test du deuxième onglet. Une erreur d’accès concurrentiel s’affiche, avec les valeurs présentes actuellement dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc89f-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="bc89f-285">Un clic sur **Delete** supprime l’entité, sauf si `RowVersion` a été mis à jour.</span><span class="sxs-lookup"><span data-stu-id="bc89f-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="bc89f-286">Pour découvrir comment hériter d’un modèle de données, consultez [Héritage](xref:data/ef-mvc/inheritance).</span><span class="sxs-lookup"><span data-stu-id="bc89f-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="bc89f-287">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bc89f-287">Additional resources</span></span>

* [<span data-ttu-id="bc89f-288">Concurrency Tokens in EF Core (Jetons d’accès concurrentiel dans EF Core)</span><span class="sxs-lookup"><span data-stu-id="bc89f-288">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="bc89f-289">Gestion de l’accès concurrentiel dans EF Core</span><span class="sxs-lookup"><span data-stu-id="bc89f-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="bc89f-290">Version YouTube de ce tutoriel(gestion des conflits d’accès concurrentiel)</span><span class="sxs-lookup"><span data-stu-id="bc89f-290">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="bc89f-291">Version YouTube de ce tutoriel(Partie 2)</span><span class="sxs-lookup"><span data-stu-id="bc89f-291">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="bc89f-292">Version YouTube de ce tutoriel(Partie 3)</span><span class="sxs-lookup"><span data-stu-id="bc89f-292">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [<span data-ttu-id="bc89f-293">Précédent</span><span class="sxs-lookup"><span data-stu-id="bc89f-293">Previous</span></span>](xref:data/ef-rp/update-related-data)
