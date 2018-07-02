---
title: ASP.NET Core MVC avec EF Core - Héritage - 9 sur 10
author: rick-anderson
description: Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données en utilisant Entity Framework Core dans une application ASP.NET Core.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a71954297f44f936893a7f1e9d3b0685f81378b9
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092995"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a><span data-ttu-id="2c528-103">ASP.NET Core MVC avec EF Core - Héritage - 9 sur 10</span><span class="sxs-lookup"><span data-stu-id="2c528-103">ASP.NET Core MVC with EF Core - Inheritance - 9 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2c528-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2c528-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2c528-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2c528-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="2c528-106">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="2c528-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="2c528-107">Dans le didacticiel précédent, vous avez géré les exceptions d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="2c528-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="2c528-108">Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="2c528-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="2c528-109">En programmation orientée objet, vous pouvez utiliser l’héritage pour faciliter la réutilisation du code.</span><span class="sxs-lookup"><span data-stu-id="2c528-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="2c528-110">Dans ce didacticiel, vous allez modifier les classes `Instructor` et `Student` afin qu’elles dérivent d’une classe de base `Person` qui contient des propriétés telles que `LastName`, communes aux formateurs et aux étudiants.</span><span class="sxs-lookup"><span data-stu-id="2c528-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="2c528-111">Vous n’ajouterez ni ne modifierez aucune page web, mais vous modifierez une partie du code et ces modifications seront automatiquement répercutées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2c528-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="2c528-112">Options pour mapper l’héritage aux tables de base de données</span><span class="sxs-lookup"><span data-stu-id="2c528-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="2c528-113">Les classes `Instructor` et `Student` du modèle de données School ont plusieurs propriétés identiques :</span><span class="sxs-lookup"><span data-stu-id="2c528-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Classes Student et Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="2c528-115">Supposons que vous souhaitez éliminer le code redondant pour les propriétés partagées par les entités `Instructor` et `Student`.</span><span class="sxs-lookup"><span data-stu-id="2c528-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="2c528-116">Ou vous souhaitez écrire un service capable de mettre en forme les noms sans se soucier du fait que le nom provienne d’un formateur ou d’un étudiant.</span><span class="sxs-lookup"><span data-stu-id="2c528-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="2c528-117">Vous pouvez créer une classe de base `Person` qui contient uniquement les propriétés partagées, puis paramétrer les classes `Instructor` et `Student` pour qu’elles héritent de cette classe de base, comme indiqué dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="2c528-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Classes Student et Instructor dérivant de la classe Person](inheritance/_static/inheritance.png)

<span data-ttu-id="2c528-119">Il existe plusieurs façons de représenter cette structure d’héritage dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2c528-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="2c528-120">Vous pouvez avoir une table de personnes qui inclut des informations sur les étudiants et les formateurs dans une table unique.</span><span class="sxs-lookup"><span data-stu-id="2c528-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="2c528-121">Certaines des colonnes pourraient s’appliquer uniquement aux formateurs (HireDate), certaines uniquement aux étudiants (EnrollmentDate) et certaines aux deux (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="2c528-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="2c528-122">En règle générale, vous pouvez avoir une colonne de discriminateur pour indiquer le type que chaque ligne représente.</span><span class="sxs-lookup"><span data-stu-id="2c528-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="2c528-123">Par exemple, la colonne de discriminateur peut avoir « Instructor » pour les formateurs et « Student » pour les étudiants.</span><span class="sxs-lookup"><span data-stu-id="2c528-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Exemple TPH (table par hiérarchie)](inheritance/_static/tph.png)

<span data-ttu-id="2c528-125">Ce modèle de génération d’une structure d’héritage d’entité à partir d’une table de base de données unique porte le nom d’héritage TPH (table par hiérarchie).</span><span class="sxs-lookup"><span data-stu-id="2c528-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="2c528-126">Une alternative consiste à faire en sorte que la base de données ressemble plus à la structure d’héritage.</span><span class="sxs-lookup"><span data-stu-id="2c528-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="2c528-127">Par exemple, vous pouvez avoir uniquement les champs de nom dans la table Person, et des tables Instructor et Student distinctes avec les champs de date.</span><span class="sxs-lookup"><span data-stu-id="2c528-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Héritage TPT (table par type)](inheritance/_static/tpt.png)

<span data-ttu-id="2c528-129">Ce modèle consistant à créer une table de base de données pour chaque classe d’entité est appelé héritage TPT (table par type).</span><span class="sxs-lookup"><span data-stu-id="2c528-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="2c528-130">Une autre option encore consiste à mapper tous les types non abstraits à des tables individuelles.</span><span class="sxs-lookup"><span data-stu-id="2c528-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="2c528-131">Toutes les propriétés d’une classe, y compris les propriétés héritées, sont mappées aux colonnes de la table correspondante.</span><span class="sxs-lookup"><span data-stu-id="2c528-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="2c528-132">Ce modèle porte le nom d’héritage TPC (table par classe concrète).</span><span class="sxs-lookup"><span data-stu-id="2c528-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="2c528-133">Si vous avez implémenté l’héritage TPC pour les classes Person, Student et Instructor comme indiqué précédemment, les tables Student et Instructor ne seraient pas différentes avant et après l’implémentation de l’héritage.</span><span class="sxs-lookup"><span data-stu-id="2c528-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="2c528-134">Les modèles d’héritage TPC et TPH fournissent généralement de meilleures performances que les modèles d’héritage TPT, car les modèles TPT peuvent entraîner des requêtes de jointure complexes.</span><span class="sxs-lookup"><span data-stu-id="2c528-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="2c528-135">Ce didacticiel montre comment implémenter l’héritage TPH.</span><span class="sxs-lookup"><span data-stu-id="2c528-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="2c528-136">TPH est le seul modèle d’héritage pris en charge par Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2c528-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="2c528-137">Vous allez créer une classe `Person`, modifier les classes `Instructor` et `Student` à dériver de `Person`, ajouter la nouvelle classe à `DbContext` et créer une migration.</span><span class="sxs-lookup"><span data-stu-id="2c528-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP]
> <span data-ttu-id="2c528-138">Pensez à enregistrer une copie du projet avant d’apporter les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="2c528-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="2c528-139">Ensuite, si vous rencontrez des problèmes et devez recommencer, il sera plus facile de démarrer à partir du projet enregistré que d’annuler les étapes effectuées pour ce didacticiel ou de retourner au début de la série entière.</span><span class="sxs-lookup"><span data-stu-id="2c528-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="2c528-140">Créer la classe Person</span><span class="sxs-lookup"><span data-stu-id="2c528-140">Create the Person class</span></span>

<span data-ttu-id="2c528-141">Dans le dossier Models, créez Person.cs et remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2c528-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="2c528-142">Paramétrer les classes Student et Instructor pour qu’elles héritent de Person</span><span class="sxs-lookup"><span data-stu-id="2c528-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="2c528-143">Dans *Instructor.cs*, dérivez la classe Instructor de la classe Person et supprimez les champs de clé et de nom.</span><span class="sxs-lookup"><span data-stu-id="2c528-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="2c528-144">Le code ressemblera à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="2c528-144">The code will look like the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="2c528-145">Apportez les mêmes modifications dans *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="2c528-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="2c528-146">Ajouter le type d’entité Person au modèle de données</span><span class="sxs-lookup"><span data-stu-id="2c528-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="2c528-147">Ajoutez le type d’entité Person à *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="2c528-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="2c528-148">Les nouvelles lignes apparaissent en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="2c528-148">The new lines are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="2c528-149">C’est là tout ce dont Entity Framework a besoin pour configurer l’héritage TPH (table par hiérarchie).</span><span class="sxs-lookup"><span data-stu-id="2c528-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="2c528-150">Comme vous le verrez, lorsque la base de données sera mise à jour, elle aura une table Person à la place des tables Student et Instructor.</span><span class="sxs-lookup"><span data-stu-id="2c528-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="2c528-151">Créer et personnaliser le code de migration</span><span class="sxs-lookup"><span data-stu-id="2c528-151">Create and customize migration code</span></span>

<span data-ttu-id="2c528-152">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="2c528-152">Save your changes and build the project.</span></span> <span data-ttu-id="2c528-153">Ensuite, ouvrez la fenêtre de commande dans le dossier du projet et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2c528-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="2c528-154">N’exécutez pas encore la commande `database update`.</span><span class="sxs-lookup"><span data-stu-id="2c528-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="2c528-155">Cette commande entraîne une perte de données, car elle supprime la table Instructor et renomme la table Student en Person.</span><span class="sxs-lookup"><span data-stu-id="2c528-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="2c528-156">Vous devez fournir un code personnalisé pour préserver les données existantes.</span><span class="sxs-lookup"><span data-stu-id="2c528-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="2c528-157">Ouvrez *Migrations/\<horodatage>_Inheritance.cs* et remplacez la méthode `Up` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2c528-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="2c528-158">Ce code prend en charge les tâches de mise à jour de base de données suivantes :</span><span class="sxs-lookup"><span data-stu-id="2c528-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="2c528-159">Supprime les contraintes de clé étrangère et les index qui pointent vers la table Student.</span><span class="sxs-lookup"><span data-stu-id="2c528-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="2c528-160">Renomme la table Instructor en Person et apporte les modifications nécessaires pour qu’elle stocke les données des étudiants :</span><span class="sxs-lookup"><span data-stu-id="2c528-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="2c528-161">Ajoute une EnrollmentDate nullable pour les étudiants.</span><span class="sxs-lookup"><span data-stu-id="2c528-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="2c528-162">Ajoute la colonne Discriminator pour indiquer si une ligne est pour un étudiant ou un formateur.</span><span class="sxs-lookup"><span data-stu-id="2c528-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="2c528-163">Rend HireDate nullable étant donné que les lignes d’étudiant n’ont pas de dates d’embauche.</span><span class="sxs-lookup"><span data-stu-id="2c528-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="2c528-164">Ajoute un champ temporaire qui sera utilisé pour mettre à jour les clés étrangères qui pointent vers les étudiants.</span><span class="sxs-lookup"><span data-stu-id="2c528-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="2c528-165">Lorsque vous copiez des étudiants dans la table Person, ils obtiennent de nouvelles valeurs de clés primaires.</span><span class="sxs-lookup"><span data-stu-id="2c528-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="2c528-166">Copie des données à partir de la table Student dans la table Person.</span><span class="sxs-lookup"><span data-stu-id="2c528-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="2c528-167">Cela entraîne l’affectation de nouvelles valeurs de clés primaires aux étudiants.</span><span class="sxs-lookup"><span data-stu-id="2c528-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="2c528-168">Corrige les valeurs de clés étrangères qui pointent vers les étudiants.</span><span class="sxs-lookup"><span data-stu-id="2c528-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="2c528-169">Crée de nouveau les index et les contraintes de clé étrangère, désormais pointées vers la table Person.</span><span class="sxs-lookup"><span data-stu-id="2c528-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="2c528-170">(Si vous aviez utilisé un GUID à la place d’un entier comme type de clé primaire, les valeurs des clés primaires des étudiants n’auraient pas changé, et plusieurs de ces étapes auraient pu être omises.)</span><span class="sxs-lookup"><span data-stu-id="2c528-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="2c528-171">Exécutez la commande `database update` :</span><span class="sxs-lookup"><span data-stu-id="2c528-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="2c528-172">(Dans un système de production, vous apporteriez les modifications correspondantes à la méthode `Down` au cas où vous auriez à l’utiliser pour revenir à la version précédente de la base de données.</span><span class="sxs-lookup"><span data-stu-id="2c528-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="2c528-173">Pour ce didacticiel, vous n’utiliserez pas la méthode `Down`.)</span><span class="sxs-lookup"><span data-stu-id="2c528-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE]
> <span data-ttu-id="2c528-174">Vous pouvez obtenir d’autres erreurs en apportant des modifications au schéma dans une base de données qui comporte déjà des données.</span><span class="sxs-lookup"><span data-stu-id="2c528-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="2c528-175">Si vous obtenez des erreurs de migration que vous ne pouvez pas résoudre, vous pouvez changer le nom de la base de données dans la chaîne de connexion ou supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="2c528-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="2c528-176">Avec une nouvelle base de données, il n’y a pas de données à migrer et la commande de mise à jour de base de données a plus de chances de s’exécuter sans erreur.</span><span class="sxs-lookup"><span data-stu-id="2c528-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="2c528-177">Pour supprimer la base de données, utilisez SSOX ou exécutez la commande CLI `database drop`.</span><span class="sxs-lookup"><span data-stu-id="2c528-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="2c528-178">Tester avec l’héritage implémenté</span><span class="sxs-lookup"><span data-stu-id="2c528-178">Test with inheritance implemented</span></span>

<span data-ttu-id="2c528-179">Exécutez l’application et essayez différentes pages.</span><span class="sxs-lookup"><span data-stu-id="2c528-179">Run the app and try various pages.</span></span> <span data-ttu-id="2c528-180">Tout fonctionne comme avant.</span><span class="sxs-lookup"><span data-stu-id="2c528-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="2c528-181">Dans l’**Explorateur d’objets SQL Server**, développez **Data Connections/SchoolContext** puis **Tables**, et vous constatez que les tables Student et Instructor ont été remplacées par une table Person.</span><span class="sxs-lookup"><span data-stu-id="2c528-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="2c528-182">Ouvrez le concepteur de la table Person et vous constatez qu’elle possède toutes les colonnes qui existaient dans les tables Student et Instructor.</span><span class="sxs-lookup"><span data-stu-id="2c528-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Table Person dans SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="2c528-184">Cliquez avec le bouton droit sur la table Person, puis cliquez sur **Afficher les données de la table** pour voir la colonne de discriminateur.</span><span class="sxs-lookup"><span data-stu-id="2c528-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Table Person dans SSOX - données de la table](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="2c528-186">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="2c528-186">Summary</span></span>

<span data-ttu-id="2c528-187">Vous avez implémenté l’héritage TPH (table par hiérarchie) pour les classes `Person`, `Student` et `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="2c528-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="2c528-188">Pour plus d’informations sur l’héritage dans Entity Framework Core, consultez [Héritage](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="2c528-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="2c528-189">Dans le prochain didacticiel, vous allez apprendre à gérer divers scénarios Entity Framework relativement avancés.</span><span class="sxs-lookup"><span data-stu-id="2c528-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="2c528-190">[Précédent](concurrency.md)
> [Suivant](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="2c528-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>
