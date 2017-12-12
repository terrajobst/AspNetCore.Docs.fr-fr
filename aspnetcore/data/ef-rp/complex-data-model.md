---
title: "Pages Razor avec EF Core - modèle de données - 5 de 8"
author: rick-anderson
description: "Dans ce didacticiel, vous ajoutez des entités et relations et personnalisez le modèle de données en spécifiant la mise en forme, la validation et les règles de mappage de base de données."
keywords: "Annotations de données ASP.NET Core, Entity Framework Core,"
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="77788-104">Création d’un modèle de données complexes - Core EF avec le didacticiel de Pages Razor (5 de 8)</span><span class="sxs-lookup"><span data-stu-id="77788-104">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="77788-105">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="77788-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="77788-106">Les didacticiels précédents travaillé avec un modèle de base de données qui a été composé de trois entités.</span><span class="sxs-lookup"><span data-stu-id="77788-106">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="77788-107">Dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="77788-107">In this tutorial:</span></span>

* <span data-ttu-id="77788-108">Plusieurs entités et les relations sont ajoutées.</span><span class="sxs-lookup"><span data-stu-id="77788-108">More entities and relationships are added.</span></span>
* <span data-ttu-id="77788-109">Le modèle de données est personnalisé en spécifiant la mise en forme, la validation et les règles de mappage de base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-109">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="77788-110">Les classes d’entité pour le modèle de données est indiqué dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="77788-110">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagramme d’entité](complex-data-model/_static/diagram.png)

<span data-ttu-id="77788-112">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="77788-112">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="77788-113">Personnaliser le modèle de données avec des attributs</span><span class="sxs-lookup"><span data-stu-id="77788-113">Customize the data model with attributes</span></span>

<span data-ttu-id="77788-114">Dans cette section, le modèle de données personnalisé à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="77788-114">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="77788-115">L’attribut de type de données</span><span class="sxs-lookup"><span data-stu-id="77788-115">The DataType attribute</span></span>

<span data-ttu-id="77788-116">Actuellement, les pages de l’étudiant affiche l’heure de la date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="77788-116">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="77788-117">En règle générale, les champs de date ne montrent que la date et non à l’heure.</span><span class="sxs-lookup"><span data-stu-id="77788-117">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="77788-118">Mise à jour *Models/Student.cs* avec les éléments suivants en surbrillance le code :</span><span class="sxs-lookup"><span data-stu-id="77788-118">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="77788-119">Le [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribut spécifie un type de données qui est plus spécifique que le type intrinsèque de la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-119">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="77788-120">Dans ce cas que la date doit être affichée, pas la date et l’heure.</span><span class="sxs-lookup"><span data-stu-id="77788-120">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="77788-121">Le [énumération DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fournit de nombreux types de données, telles que Date, heure, numéro de téléphone, devise, EmailAddress, etc. Le `DataType` attribut peut également permettre à l’application fournir automatiquement les fonctionnalités spécifiques au type.</span><span class="sxs-lookup"><span data-stu-id="77788-121">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="77788-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="77788-122">For example:</span></span>

* <span data-ttu-id="77788-123">Le `mailto:` lien est automatiquement créé pour `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="77788-123">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="77788-124">Le sélecteur de date est fourni pour `DataType.Date` dans la plupart des navigateurs.</span><span class="sxs-lookup"><span data-stu-id="77788-124">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="77788-125">Le `DataType` émet un attribut HTML 5 `data-` attributs (données prononcé tiret) qui utilisent des navigateurs de HTML 5.</span><span class="sxs-lookup"><span data-stu-id="77788-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="77788-126">Le `DataType` attributs ne fournissent pas de validation.</span><span class="sxs-lookup"><span data-stu-id="77788-126">The `DataType` attributes do not provide validation.</span></span>

<span data-ttu-id="77788-127">`DataType.Date` ne spécifie pas le format de la date qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77788-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="77788-128">Par défaut, le champ de date est affiché selon les formats par défaut basés sur le serveur [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="77788-128">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="77788-129">L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :</span><span class="sxs-lookup"><span data-stu-id="77788-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="77788-130">Le `ApplyFormatInEditMode` paramètre spécifie que la mise en forme doit également être appliqué à l’interface utilisateur de modification.</span><span class="sxs-lookup"><span data-stu-id="77788-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="77788-131">Certains champs ne doivent pas utiliser `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="77788-131">Some fields should not use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="77788-132">Par exemple, le symbole de devise ne doit généralement pas être affiché dans une zone de texte d’édition.</span><span class="sxs-lookup"><span data-stu-id="77788-132">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="77788-133">Le `DisplayFormat` attribut peut être utilisé par lui-même.</span><span class="sxs-lookup"><span data-stu-id="77788-133">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="77788-134">Il est généralement judicieux d’utiliser le `DataType` d’attribut avec le `DisplayFormat` attribut.</span><span class="sxs-lookup"><span data-stu-id="77788-134">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="77788-135">Le `DataType` attribut transmet la sémantique des données par opposition à comment effectuer le rendu sur un écran.</span><span class="sxs-lookup"><span data-stu-id="77788-135">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="77788-136">Le `DataType` attribut offre les avantages suivants ne sont pas disponibles dans `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="77788-136">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="77788-137">Le navigateur peut activer des fonctionnalités HTML5.</span><span class="sxs-lookup"><span data-stu-id="77788-137">The browser can enable HTML5 features.</span></span> <span data-ttu-id="77788-138">Par exemple, afficher un contrôle calendrier, le symbole monétaire approprié de paramètres régionaux, des liens de messagerie, la validation d’entrée côté client, etc.</span><span class="sxs-lookup"><span data-stu-id="77788-138">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="77788-139">Par défaut, le navigateur restitue les données en utilisant le format approprié en fonction des paramètres régionaux.</span><span class="sxs-lookup"><span data-stu-id="77788-139">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="77788-140">Pour plus d’informations, consultez la [ \<d’entrée > documentation de l’application d’assistance de balise](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="77788-140">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="77788-141">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="77788-141">Run the app.</span></span> <span data-ttu-id="77788-142">Accédez à la page d’Index d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="77788-142">Navigate to the Students Index page.</span></span> <span data-ttu-id="77788-143">Heures ne sont plus affichés.</span><span class="sxs-lookup"><span data-stu-id="77788-143">Times are no longer displayed.</span></span> <span data-ttu-id="77788-144">Toutes les vues qui utilisent le `Student` modèle affiche la date sans heure.</span><span class="sxs-lookup"><span data-stu-id="77788-144">Every view that uses the `Student` model displays the date without time.</span></span>

![Affichage des dates sans heures dans la page d’index les étudiants](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="77788-146">L’attribut StringLength</span><span class="sxs-lookup"><span data-stu-id="77788-146">The StringLength attribute</span></span>

<span data-ttu-id="77788-147">Les règles de validation de données et messages d’erreur de validation peuvent être spécifiés avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="77788-147">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="77788-148">Le [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribut spécifie la longueur minimale et maximale de caractères autorisés dans un champ de données.</span><span class="sxs-lookup"><span data-stu-id="77788-148">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="77788-149">Le `StringLength` attribut fournit également la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="77788-149">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="77788-150">La valeur minimale n’a aucun impact sur le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-150">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="77788-151">Mise à jour le `Student` modèle avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-151">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="77788-152">Le code précédent limite les noms et pas plus de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="77788-152">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="77788-153">Le `StringLength` attribut n’empêche pas un utilisateur d’entrer un espace blanc pour un nom.</span><span class="sxs-lookup"><span data-stu-id="77788-153">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="77788-154">Le [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribut est utilisé pour appliquer des restrictions à l’entrée.</span><span class="sxs-lookup"><span data-stu-id="77788-154">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="77788-155">Par exemple, le code suivant requiert le premier caractère en majuscules et les autres caractères alphabétique :</span><span class="sxs-lookup"><span data-stu-id="77788-155">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="77788-156">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="77788-156">Run the app:</span></span>

* <span data-ttu-id="77788-157">Accédez à la page d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="77788-157">Navigate to the Students page.</span></span>
* <span data-ttu-id="77788-158">Sélectionnez **créer un nouveau**et entrez un nom de plus de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="77788-158">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="77788-159">Sélectionnez **créer**, la validation côté client affiche un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="77788-159">Select **Create**, client-side validation shows an error message.</span></span>

![Page affichant les erreurs de longueur de chaîne d’index les étudiants](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="77788-161">Dans **l’Explorateur d’objets SQL Server** (SSOX), ouvrez le Concepteur de tables Student en double-cliquant sur le **étudiant** table.</span><span class="sxs-lookup"><span data-stu-id="77788-161">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Table d’étudiants dans SSOX avant la migration](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="77788-163">L’illustration précédente montre le schéma pour le `Student` table.</span><span class="sxs-lookup"><span data-stu-id="77788-163">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="77788-164">Les champs nom ont type `nvarchar(MAX)` , car les migrations n’a pas été exécuté sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-164">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="77788-165">Lors de la migration est exécutées plus loin dans ce didacticiel, les champs de nom deviennent `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="77788-165">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="77788-166">L’attribut de colonne</span><span class="sxs-lookup"><span data-stu-id="77788-166">The Column attribute</span></span>

<span data-ttu-id="77788-167">Les attributs peuvent contrôler comment les classes et les propriétés sont mappées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="77788-168">Dans cette section, le `Column` attribut est utilisé pour mapper le nom de la `FirstMidName` propriété « FirstName » dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-168">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="77788-169">Lors de la création de la base de données, les noms de propriété sur le modèle sont utilisés pour les noms de colonne (sauf lorsque le `Column` attribut est utilisé).</span><span class="sxs-lookup"><span data-stu-id="77788-169">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="77788-170">Le `Student` modèle utilise `FirstMidName` pour le prénom, car le champ peut également contenir un deuxième prénom.</span><span class="sxs-lookup"><span data-stu-id="77788-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="77788-171">Mise à jour la *Student.cs* fichier avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-171">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="77788-172">Avec la modification précédente, `Student.FirstMidName` dans l’application est mappée à la `FirstName` colonne de la `Student` table.</span><span class="sxs-lookup"><span data-stu-id="77788-172">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="77788-173">L’ajout de la `Column` attribut modifie la sauvegarde du modèle le `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="77788-173">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="77788-174">La sauvegarde du modèle le `SchoolContext` ne correspond plus à la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-174">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="77788-175">Si l’application est exécutée avant d’appliquer des migrations, l’exception suivante est générée :</span><span class="sxs-lookup"><span data-stu-id="77788-175">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="77788-176">Pour mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="77788-176">To update the DB:</span></span>

* <span data-ttu-id="77788-177">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="77788-177">Build the project.</span></span>
* <span data-ttu-id="77788-178">Ouvrez une fenêtre de commande dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="77788-178">Open a command window in the project folder.</span></span> <span data-ttu-id="77788-179">Entrez les commandes suivantes pour créer une nouvelle migration et mise à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="77788-179">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="77788-180">Le `dotnet ef migrations add ColumnFirstName` commande génère le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-180">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="77788-181">L’avertissement est généré, car les champs de nom sont désormais limité à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="77788-181">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="77788-182">Si un nom dans la base de données avait plus de 50 caractères, le 51 jusqu’au dernier caractère a été perdue.</span><span class="sxs-lookup"><span data-stu-id="77788-182">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="77788-183">Tester l’application.</span><span class="sxs-lookup"><span data-stu-id="77788-183">Test the app.</span></span>

<span data-ttu-id="77788-184">Ouvrez la table de l’étudiant dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="77788-184">Open the Student table in SSOX:</span></span>

![Table d’étudiants dans SSOX après la migration](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="77788-186">Avant de la migration a été appliquée, les colonnes Nom étaient de type [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="77788-186">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="77788-187">Les colonnes de nom sont maintenant `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="77788-187">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="77788-188">Le nom de la colonne a changé de `FirstMidName` à `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="77788-188">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="77788-189">Dans la section suivante, la génération de l’application à certaines étapes génère les erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="77788-189">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="77788-190">Les instructions de spécifient le moment générer l’application.</span><span class="sxs-lookup"><span data-stu-id="77788-190">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="77788-191">Mise à jour des entités étudiant</span><span class="sxs-lookup"><span data-stu-id="77788-191">Student entity update</span></span>

![Entité Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="77788-193">Mise à jour *Models/Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-193">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="77788-194">L’attribut requis</span><span class="sxs-lookup"><span data-stu-id="77788-194">The Required attribute</span></span>

<span data-ttu-id="77788-195">Le `Required` attribut rend les champs obligatoires de propriétés de nom.</span><span class="sxs-lookup"><span data-stu-id="77788-195">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="77788-196">Le `Required` attribut n’est pas nécessaire pour les types tels que des types valeur non nullable (`DateTime`, `int`, `double`, etc.).</span><span class="sxs-lookup"><span data-stu-id="77788-196">The `Required` attribute is not needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="77788-197">Les types qui ne peut pas être null sont traitées automatiquement en tant que champs requis.</span><span class="sxs-lookup"><span data-stu-id="77788-197">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="77788-198">Le `Required` attribut peut être remplacé par un paramètre de longueur minimale dans le `StringLength` attribut :</span><span class="sxs-lookup"><span data-stu-id="77788-198">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="77788-199">L’attribut d’affichage</span><span class="sxs-lookup"><span data-stu-id="77788-199">The Display attribute</span></span>

<span data-ttu-id="77788-200">Le `Display` attribut spécifie que la légende pour les zones de texte doit être « Prénom », « Last Name », « Nom complet » et « Date d’inscription. »</span><span class="sxs-lookup"><span data-stu-id="77788-200">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="77788-201">Légendes par défaut ne dispose pas d’espace divisant les mots, par exemple « nom ».</span><span class="sxs-lookup"><span data-stu-id="77788-201">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="77788-202">La propriété FullName calculée</span><span class="sxs-lookup"><span data-stu-id="77788-202">The FullName calculated property</span></span>

<span data-ttu-id="77788-203">`FullName`est une propriété calculée qui retourne une valeur qui est obtenue par concaténation de deux autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="77788-203">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="77788-204">`FullName`ne peut pas être défini, qu'il a uniquement un accesseur get.</span><span class="sxs-lookup"><span data-stu-id="77788-204">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="77788-205">Ne `FullName` colonne est créée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-205">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="77788-206">Créer l’entité Instructor</span><span class="sxs-lookup"><span data-stu-id="77788-206">Create the Instructor Entity</span></span>

![Entité Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="77788-208">Créer *Models/Instructor.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-208">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="77788-209">Notez que plusieurs propriétés sont identiques dans les `Student` et `Instructor` entités.</span><span class="sxs-lookup"><span data-stu-id="77788-209">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="77788-210">Dans le didacticiel de mise en œuvre de l’héritage, plus loin dans cette série, ce code est refactorisé pour éliminer la redondance.</span><span class="sxs-lookup"><span data-stu-id="77788-210">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="77788-211">Plusieurs attributs peuvent être sur une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="77788-211">Multiple attributes can be on one line.</span></span> <span data-ttu-id="77788-212">Le `HireDate` attributs peut être écrite comme suit :</span><span class="sxs-lookup"><span data-stu-id="77788-212">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="77788-213">Les propriétés de navigation CourseAssignments et OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="77788-213">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="77788-214">Le `CourseAssignments` et `OfficeAssignment` propriétés sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="77788-214">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="77788-215">Formateur pouvez animer n’importe quel nombre de cours, de sorte que `CourseAssignments` est défini comme une collection.</span><span class="sxs-lookup"><span data-stu-id="77788-215">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="77788-216">Si une propriété de navigation conserve plusieurs entités :</span><span class="sxs-lookup"><span data-stu-id="77788-216">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="77788-217">Il doit être un type de liste dans laquelle les entrées peuvent être ajoutées, supprimées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="77788-217">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="77788-218">Types de propriétés de navigation sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="77788-218">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="77788-219">Si `ICollection<T>` est spécifié, EF Core crée un `HashSet<T>` collection par défaut.</span><span class="sxs-lookup"><span data-stu-id="77788-219">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="77788-220">Le `CourseAssignment` entité est expliquée dans la section sur les relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="77788-220">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="77788-221">État qu’un formateur peut avoir au plus un bureau des règles d’entreprise de l’Université de Contoso.</span><span class="sxs-lookup"><span data-stu-id="77788-221">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="77788-222">Le `OfficeAssignment` propriété contient un seul `OfficeAssignment` entité.</span><span class="sxs-lookup"><span data-stu-id="77788-222">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="77788-223">`OfficeAssignment`a la valeur null si aucune office n’est affecté.</span><span class="sxs-lookup"><span data-stu-id="77788-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="77788-224">Créer l’entité OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="77788-224">Create the OfficeAssignment entity</span></span>

![Entité OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="77788-226">Créer *Models/OfficeAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="77788-227">L’attribut de clé</span><span class="sxs-lookup"><span data-stu-id="77788-227">The Key attribute</span></span>

<span data-ttu-id="77788-228">Le `[Key]` attribut est utilisé pour identifier une propriété comme clé primaire (PK) lorsque le nom de propriété est un élément autre que classnameID ou de code.</span><span class="sxs-lookup"><span data-stu-id="77788-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="77788-229">Il existe une relation un-à-zéro-ou-un entre le `Instructor` et `OfficeAssignment` entités.</span><span class="sxs-lookup"><span data-stu-id="77788-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="77788-230">Une attribution existe uniquement en relation avec le formateur qu'auquel elle est assignée.</span><span class="sxs-lookup"><span data-stu-id="77788-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="77788-231">Le `OfficeAssignment` PK est également sa clé étrangère (FK) pour le `Instructor` entité.</span><span class="sxs-lookup"><span data-stu-id="77788-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="77788-232">EF principal ne peut pas reconnaître automatiquement `InstructorID` en tant que la clé publique de `OfficeAssignment` , car :</span><span class="sxs-lookup"><span data-stu-id="77788-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="77788-233">`InstructorID`ne suit pas la convention d’affectation de noms ID ou classnameID.</span><span class="sxs-lookup"><span data-stu-id="77788-233">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="77788-234">Par conséquent, le `Key` attribut est utilisé pour identifier les `InstructorID` en tant que la clé publique :</span><span class="sxs-lookup"><span data-stu-id="77788-234">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="77788-235">Par défaut, EF Core traite la touche non généré à la base de données, car la colonne est pour une relation d’identification.</span><span class="sxs-lookup"><span data-stu-id="77788-235">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="77788-236">La propriété de navigation de formateur</span><span class="sxs-lookup"><span data-stu-id="77788-236">The Instructor navigation property</span></span>

<span data-ttu-id="77788-237">Le `OfficeAssignment` propriété de navigation pour la `Instructor` entité est nullable, car :</span><span class="sxs-lookup"><span data-stu-id="77788-237">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="77788-238">Les types référence (tels que les classes sont nullables).</span><span class="sxs-lookup"><span data-stu-id="77788-238">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="77788-239">Formateur n’est peut-être pas une attribution.</span><span class="sxs-lookup"><span data-stu-id="77788-239">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="77788-240">Le `OfficeAssignment` entité a non-nullable `Instructor` propriété de navigation, car :</span><span class="sxs-lookup"><span data-stu-id="77788-240">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="77788-241">`InstructorID`est non nullable.</span><span class="sxs-lookup"><span data-stu-id="77788-241">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="77788-242">Une attribution ne peut pas exister sans un formateur.</span><span class="sxs-lookup"><span data-stu-id="77788-242">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="77788-243">Lorsqu’un `Instructor` entité associée à une `OfficeAssignment` entité, chaque entité a une référence à l’autre dans sa propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="77788-243">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="77788-244">Le `[Required]` attribut peut être appliqué à la `Instructor` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="77788-244">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="77788-245">Le code précédent Spécifie qu’il doit y avoir un formateur connexe.</span><span class="sxs-lookup"><span data-stu-id="77788-245">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="77788-246">Le code précédent n’est pas nécessaire, car le `InstructorID` clé étrangère (qui est également la clé publique) est non nullable.</span><span class="sxs-lookup"><span data-stu-id="77788-246">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="77788-247">Modifier l’entité de cours</span><span class="sxs-lookup"><span data-stu-id="77788-247">Modify the Course Entity</span></span>

![Entité de cours](complex-data-model/_static/course-entity.png)

<span data-ttu-id="77788-249">Mise à jour *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-249">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="77788-250">Le `Course` entité a une propriété de clé étrangère (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="77788-250">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="77788-251">`DepartmentID`pointe vers le `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="77788-251">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="77788-252">Le `Course` entité a un `Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="77788-252">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="77788-253">EF Core ne nécessite pas une propriété FK pour un modèle de données lorsque le modèle a une propriété de navigation pour une entité associée.</span><span class="sxs-lookup"><span data-stu-id="77788-253">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="77788-254">EF Core crée automatiquement les clés étrangères dans la base de données là où elles sont requises.</span><span class="sxs-lookup"><span data-stu-id="77788-254">EF Core automatically creates FKs in the database wherever they are needed.</span></span> <span data-ttu-id="77788-255">EF Core crée [occulter les propriétés](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pour les clés étrangères du créés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="77788-255">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="77788-256">La présence de la clé étrangère dans le modèle de données peut simplifier les mises à jour et plus efficace.</span><span class="sxs-lookup"><span data-stu-id="77788-256">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="77788-257">Par exemple, considérez un modèle où la propriété FK `DepartmentID` est *pas* inclus.</span><span class="sxs-lookup"><span data-stu-id="77788-257">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="77788-258">Lorsqu’une entité de cours est extraite à modifier :</span><span class="sxs-lookup"><span data-stu-id="77788-258">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="77788-259">Le `Department` entité a la valeur null s’il n’est pas explicitement chargé.</span><span class="sxs-lookup"><span data-stu-id="77788-259">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="77788-260">Pour mettre à jour de l’entité de cours, le `Department` entité doit tout d’abord être extraites.</span><span class="sxs-lookup"><span data-stu-id="77788-260">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="77788-261">Lorsque la propriété FK `DepartmentID` est inclus dans le modèle de données, il n’est pas nécessaire d’extraire le `Department` entité avant une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="77788-261">When the FK property `DepartmentID` is included in the data model, there is no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="77788-262">L’attribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="77788-262">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="77788-263">Le `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribut spécifie que la clé est fournie par l’application au lieu de générés par la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-263">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="77788-264">Par défaut, EF Core suppose que les valeurs de clé primaire sont générés par la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-264">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="77788-265">Base de données généré PK valeurs est généralement la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="77788-265">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="77788-266">Pour `Course` entités, l’utilisateur spécifie le PK.</span><span class="sxs-lookup"><span data-stu-id="77788-266">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="77788-267">Par exemple, un nombre de cours comme une série de 1000 pour le service de mathématiques, une série de 2000 pour le service en anglais.</span><span class="sxs-lookup"><span data-stu-id="77788-267">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="77788-268">Le `DatabaseGenerated` attribut peut également être utilisé pour générer des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="77788-268">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="77788-269">Par exemple, la base de données peut générer automatiquement un champ de date pour enregistrer la date d’une ligne a été créée ou mis à jour.</span><span class="sxs-lookup"><span data-stu-id="77788-269">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="77788-270">Pour plus d’informations, consultez [généré des propriétés](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="77788-270">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="77788-271">Propriétés de navigation et de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="77788-271">Foreign key and navigation properties</span></span>

<span data-ttu-id="77788-272">Les propriétés de clé étrangère (FK) et les propriétés de navigation dans les `Course` entité reflète les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="77788-272">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="77788-273">Un cours est affecté à un service, donc il est un `DepartmentID` FK et un `Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="77788-273">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="77788-274">Un cours peut avoir n’importe quel nombre d’élèves inscrits, par conséquent, le `Enrollments` propriété de navigation est une collection :</span><span class="sxs-lookup"><span data-stu-id="77788-274">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="77788-275">Un cours peut être dirigée par des instructeurs plusieurs, donc la `CourseAssignments` propriété de navigation est une collection :</span><span class="sxs-lookup"><span data-stu-id="77788-275">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="77788-276">`CourseAssignment`est expliquée [ultérieurement](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="77788-276">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="77788-277">Créer l’entité du service</span><span class="sxs-lookup"><span data-stu-id="77788-277">Create the Department entity</span></span>

![Entité de service](complex-data-model/_static/department-entity.png)

<span data-ttu-id="77788-279">Créer *Models/Department.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-279">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="77788-280">L’attribut de colonne</span><span class="sxs-lookup"><span data-stu-id="77788-280">The Column attribute</span></span>

<span data-ttu-id="77788-281">Précédemment le `Column` attribut a été utilisé pour modifier le mappage de nom de colonne.</span><span class="sxs-lookup"><span data-stu-id="77788-281">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="77788-282">Dans le code de la `Department` entité, le `Column` attribut est utilisé pour modifier le mappage de type de données SQL.</span><span class="sxs-lookup"><span data-stu-id="77788-282">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="77788-283">Le `Budget` colonne est définie à l’aide du type de money SQL Server dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="77788-283">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="77788-284">Mappage de colonnes n’est généralement pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="77788-284">Column mapping is generally not required.</span></span> <span data-ttu-id="77788-285">EF Core choisit généralement le type de données SQL Server approprié en fonction du type CLR pour la propriété.</span><span class="sxs-lookup"><span data-stu-id="77788-285">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="77788-286">Le CLR `decimal` type correspond à un serveur SQL Server `decimal` type.</span><span class="sxs-lookup"><span data-stu-id="77788-286">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="77788-287">`Budget`est de devise, et le type de données money est plus approprié pour la devise.</span><span class="sxs-lookup"><span data-stu-id="77788-287">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="77788-288">Propriétés de navigation et de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="77788-288">Foreign key and navigation properties</span></span>

<span data-ttu-id="77788-289">Les propriétés de navigation et FK reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="77788-289">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="77788-290">Un service peut ou ne peut pas avoir un administrateur.</span><span class="sxs-lookup"><span data-stu-id="77788-290">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="77788-291">Un administrateur est toujours un formateur.</span><span class="sxs-lookup"><span data-stu-id="77788-291">An administrator is always an instructor.</span></span> <span data-ttu-id="77788-292">Par conséquent la `InstructorID` propriété n’est incluse en tant que la clé étrangère pour la `Instructor` entité.</span><span class="sxs-lookup"><span data-stu-id="77788-292">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="77788-293">La propriété de navigation est nommée `Administrator` mais contient un `Instructor` entité :</span><span class="sxs-lookup"><span data-stu-id="77788-293">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="77788-294">Le point d’interrogation ( ?) dans le code précédent Spécifie que la propriété est nullable.</span><span class="sxs-lookup"><span data-stu-id="77788-294">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="77788-295">Un service peut avoir de nombreux cours, est une propriété de navigation du cours :</span><span class="sxs-lookup"><span data-stu-id="77788-295">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="77788-296">Remarque : Par convention, EF Core permet de suppression en cascade pour les clés étrangères du non nullable et pour les relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="77788-296">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="77788-297">Les règles de suppression en cascade circulaire peut entraîner une suppression en cascade.</span><span class="sxs-lookup"><span data-stu-id="77788-297">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="77788-298">Circulaire suppression en cascade entraîne des règles d’exception lors de l’ajout d’une migration.</span><span class="sxs-lookup"><span data-stu-id="77788-298">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="77788-299">Par exemple, si le `Department.InstructorID` propriété n’a pas a été définie comme nullable :</span><span class="sxs-lookup"><span data-stu-id="77788-299">For example, if the `Department.InstructorID` property was not defined as nullable:</span></span>

* <span data-ttu-id="77788-300">EF Core configure une règle de suppression en cascade pour supprimer le formateur lorsque le service est supprimé.</span><span class="sxs-lookup"><span data-stu-id="77788-300">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="77788-301">La suppression du formateur lorsque le service est supprimé n’est pas du comportement souhaité.</span><span class="sxs-lookup"><span data-stu-id="77788-301">Deleting the instructor when the department is deleted is not the intended behavior.</span></span>

<span data-ttu-id="77788-302">Si les règles d’entreprise nécessaire le `InstructorID` propriété être non nullable, utilisez l’instruction d’API fluent suivante :</span><span class="sxs-lookup"><span data-stu-id="77788-302">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="77788-303">Le code précédent désactive la suppression en cascade sur la relation de formateur de service.</span><span class="sxs-lookup"><span data-stu-id="77788-303">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="77788-304">Mise à jour de l’entité de l’inscription</span><span class="sxs-lookup"><span data-stu-id="77788-304">Update the Enrollment entity</span></span>

<span data-ttu-id="77788-305">Un enregistrement d’inscription concerne un un cours effectuée par un étudiant.</span><span class="sxs-lookup"><span data-stu-id="77788-305">An enrollment record is for a one course taken by one student.</span></span>

![Entité de l’inscription](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="77788-307">Mise à jour *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-307">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="77788-308">Propriétés de navigation et de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="77788-308">Foreign key and navigation properties</span></span>

<span data-ttu-id="77788-309">Les propriétés de navigation et les propriétés FK reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="77788-309">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="77788-310">Un enregistrement d’inscription est pour un cours, donc il est un `CourseID` les propriété FK et un `Course` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="77788-310">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="77788-311">Un enregistrement d’inscription est pour un étudiant, donc il est un `StudentID` les propriété FK et un `Student` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="77788-311">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="77788-312">Relations plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="77788-312">Many-to-Many Relationships</span></span>

<span data-ttu-id="77788-313">Il existe une relation plusieurs-à-plusieurs entre la `Student` et `Course` entités.</span><span class="sxs-lookup"><span data-stu-id="77788-313">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="77788-314">Le `Enrollment` entité fonctionne comme une table de jointure plusieurs-à-plusieurs *avec une charge utile* dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-314">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="77788-315">« Avec une charge utile » signifie que la `Enrollment` table contient des données supplémentaires en plus des clés étrangères pour les tables jointes (dans ce cas, les clés publiques et `Grade`).</span><span class="sxs-lookup"><span data-stu-id="77788-315">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="77788-316">L’illustration suivante montre l’aspect de ces relations dans un diagramme d’entité.</span><span class="sxs-lookup"><span data-stu-id="77788-316">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="77788-317">(Ce diagramme a été généré à l’aide de EF Power Tools pour EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="77788-317">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="77788-318">Créer le diagramme ne fait pas partie de ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="77788-318">Creating the diagram isn't part of the tutorial.)</span></span>

![Cours de l’étudiant plusieurs à plusieurs relations](complex-data-model/_static/student-course.png)

<span data-ttu-id="77788-320">Chaque ligne de relation comporte un 1 à une extrémité et un astérisque (*) à l’autre, qui indique une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="77788-320">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="77788-321">Si le `Enrollment` table n’a pas inclure les informations de catégorie, il doit uniquement contenir le clés étrangères deux du (`CourseID` et `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="77788-321">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="77788-322">Une table de jointure plusieurs-à-plusieurs sans la charge utile est parfois appelée une table de jonction pure (PJT).</span><span class="sxs-lookup"><span data-stu-id="77788-322">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="77788-323">Le `Instructor` et `Course` les entités ont une relation plusieurs-à-plusieurs à l’aide d’une table de jonction pure.</span><span class="sxs-lookup"><span data-stu-id="77788-323">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="77788-324">Remarque : EF 6.x prend en charge n’est pas le cas de tables de jointure implicite pour les relations plusieurs-à-plusieurs, mais EF Core.</span><span class="sxs-lookup"><span data-stu-id="77788-324">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="77788-325">Pour plus d’informations, consultez [plusieurs-à-plusieurs relations dans EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="77788-325">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="77788-326">L’entité CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="77788-326">The CourseAssignment entity</span></span>

![Entité de CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="77788-328">Créer *Models/CourseAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-328">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="77788-329">Au cours de formateur</span><span class="sxs-lookup"><span data-stu-id="77788-329">Instructor-to-Courses</span></span>

![Formateur à cours m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="77788-331">La relation plusieurs-à-plusieurs de formateur à cours :</span><span class="sxs-lookup"><span data-stu-id="77788-331">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="77788-332">Requiert une table de jointure qui doit être représentée par un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="77788-332">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="77788-333">Est une table de jonction pure (table sans la charge utile).</span><span class="sxs-lookup"><span data-stu-id="77788-333">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="77788-334">Il est courant de nommer une entité de jointure `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="77788-334">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="77788-335">Par exemple, la table de jointure de formateur à cours à l’aide de ce modèle est `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="77788-335">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="77788-336">Toutefois, nous recommandons à l’aide d’un nom qui décrit la relation.</span><span class="sxs-lookup"><span data-stu-id="77788-336">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="77788-337">Modèles de données démarrent simple et croître.</span><span class="sxs-lookup"><span data-stu-id="77788-337">Data models start out simple and grow.</span></span> <span data-ttu-id="77788-338">Les jointures non-charge utile (PJTs) évoluent fréquemment pour inclure la charge utile.</span><span class="sxs-lookup"><span data-stu-id="77788-338">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="77788-339">En commençant par un nom descriptif d’entité, le nom n’a pas besoin modifier lorsque la table de jointure change.</span><span class="sxs-lookup"><span data-stu-id="77788-339">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="77788-340">Dans l’idéal, l’entité de jointure aurait son propre nom (word éventuellement unique) naturelle dans le domaine d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="77788-340">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="77788-341">Par exemple, la documentation et les clients pu être liés à une entité de jointure appelée contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="77788-341">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="77788-342">Pour la relation plusieurs-à-plusieurs de formateur à cours, `CourseAssignment` vaut mieux utiliser `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="77788-342">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="77788-343">Clé composite</span><span class="sxs-lookup"><span data-stu-id="77788-343">Composite key</span></span>

<span data-ttu-id="77788-344">Clés étrangères du n’est pas nullable.</span><span class="sxs-lookup"><span data-stu-id="77788-344">FKs are not nullable.</span></span> <span data-ttu-id="77788-345">Le deux clés étrangères dans `CourseAssignment` (`InstructorID` et `CourseID`) ensemble identifier de façon unique chaque ligne de la `CourseAssignment` table.</span><span class="sxs-lookup"><span data-stu-id="77788-345">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="77788-346">`CourseAssignment`ne nécessite pas un PK de dédié.</span><span class="sxs-lookup"><span data-stu-id="77788-346">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="77788-347">Le `InstructorID` et `CourseID` propriétés fonctionnent comme un composite PK.</span><span class="sxs-lookup"><span data-stu-id="77788-347">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="77788-348">La seule façon de spécifier primaires composites EF cœur est avec le *API fluent*.</span><span class="sxs-lookup"><span data-stu-id="77788-348">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="77788-349">La section suivante montre comment configurer le PK de composite.</span><span class="sxs-lookup"><span data-stu-id="77788-349">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="77788-350">La clé composite garantit :</span><span class="sxs-lookup"><span data-stu-id="77788-350">The composite key ensures:</span></span>

* <span data-ttu-id="77788-351">Plusieurs lignes sont autorisées pour un cours.</span><span class="sxs-lookup"><span data-stu-id="77788-351">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="77788-352">Plusieurs lignes sont autorisées pour un formateur.</span><span class="sxs-lookup"><span data-stu-id="77788-352">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="77788-353">Plusieurs lignes pour la même formateur et cours n’est pas autorisée.</span><span class="sxs-lookup"><span data-stu-id="77788-353">Multiple rows for the same instructor and course is not allowed.</span></span>

<span data-ttu-id="77788-354">Le `Enrollment` jointure entité définit sa propre clé de performance, les doublons de ce type sont possibles.</span><span class="sxs-lookup"><span data-stu-id="77788-354">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="77788-355">Pour éviter ces doublons :</span><span class="sxs-lookup"><span data-stu-id="77788-355">To prevent such duplicates:</span></span>

* <span data-ttu-id="77788-356">Ajouter un index unique sur les champs FK, ou</span><span class="sxs-lookup"><span data-stu-id="77788-356">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="77788-357">Configurer `Enrollment` avec une clé primaire composite similaire à `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="77788-357">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="77788-358">Pour plus d’informations, consultez [index](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="77788-358">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="77788-359">Contexte de la mise à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="77788-359">Update the DB context</span></span>

<span data-ttu-id="77788-360">Ajoutez le code en surbrillance suivant à *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="77788-360">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="77788-361">Le code précédent ajoute de nouvelles entités et configure le `CourseAssignment` PK. composite de l’entité</span><span class="sxs-lookup"><span data-stu-id="77788-361">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="77788-362">Alternative d’API Fluent aux attributs</span><span class="sxs-lookup"><span data-stu-id="77788-362">Fluent API alternative to attributes</span></span>

<span data-ttu-id="77788-363">Le `OnModelCreating` méthode dans l’exemple précédent de code utilise le *API fluent* pour configurer le comportement EF principal.</span><span class="sxs-lookup"><span data-stu-id="77788-363">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="77788-364">L’API est appelée « fluent », car il est souvent utilisée par tirage une série d’appels de méthode dans une instruction unique.</span><span class="sxs-lookup"><span data-stu-id="77788-364">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="77788-365">Le [suivant code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) est un exemple de l’API fluent :</span><span class="sxs-lookup"><span data-stu-id="77788-365">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="77788-366">Dans ce didacticiel, l’API fluent est utilisée uniquement pour le mappage de base de données qui ne peut pas être effectuée avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="77788-366">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="77788-367">Toutefois, l’API fluent peut spécifier la majeure partie de la mise en forme, les règles de validation et mappage qui peuvent être effectuées avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="77788-367">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="77788-368">Certains attributs, tels que `MinimumLength` ne peut pas être appliqué avec l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="77788-368">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="77788-369">`MinimumLength`ne modifiez pas le schéma, elle s’applique uniquement une règle de validation de longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="77788-369">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="77788-370">Certains développeurs préfèrent utiliser l’API fluent exclusivement afin qu’ils peuvent conserver leurs classes d’entité « propre ».</span><span class="sxs-lookup"><span data-stu-id="77788-370">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="77788-371">Attributs et l’API fluent peuvent être mélangés.</span><span class="sxs-lookup"><span data-stu-id="77788-371">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="77788-372">Il existe des configurations qui peuvent uniquement être effectuées avec l’API fluent (spécification d’une clé primaire composite).</span><span class="sxs-lookup"><span data-stu-id="77788-372">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="77788-373">Certaines configurations qui peut uniquement être effectuée avec des attributs (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="77788-373">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="77788-374">La méthode recommandée pour l’utilisation des API fluent ou attributs :</span><span class="sxs-lookup"><span data-stu-id="77788-374">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="77788-375">Choisissez une de ces deux approches.</span><span class="sxs-lookup"><span data-stu-id="77788-375">Choose one of these two approaches.</span></span>
* <span data-ttu-id="77788-376">Utiliser l’approche choisie constamment autant que possible.</span><span class="sxs-lookup"><span data-stu-id="77788-376">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="77788-377">Certains des attributs utilisés dans ce didacticiel sont utilisées pour :</span><span class="sxs-lookup"><span data-stu-id="77788-377">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="77788-378">Validation uniquement (par exemple, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="77788-378">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="77788-379">Configuration de base EF uniquement (par exemple, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="77788-379">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="77788-380">Configuration de la validation et EF Core (par exemple, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="77788-380">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="77788-381">Pour plus d’informations sur les attributs et l’API fluent, consultez [méthodes de configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="77788-381">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="77788-382">Entité diagramme montrant les relations</span><span class="sxs-lookup"><span data-stu-id="77788-382">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="77788-383">L’illustration suivante montre le diagramme EF Power Tools créer pour le modèle School terminé.</span><span class="sxs-lookup"><span data-stu-id="77788-383">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramme d’entité](complex-data-model/_static/diagram.png)

<span data-ttu-id="77788-385">Le diagramme précédent montre :</span><span class="sxs-lookup"><span data-stu-id="77788-385">The preceding diagram shows:</span></span>

* <span data-ttu-id="77788-386">Plusieurs lignes de relation un-à-plusieurs (1 à \*).</span><span class="sxs-lookup"><span data-stu-id="77788-386">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="77788-387">La ligne de relation d’un-à-zéro-ou-un (1 à 0. 1) entre la `Instructor` et `OfficeAssignment` entités.</span><span class="sxs-lookup"><span data-stu-id="77788-387">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="77788-388">La ligne zéro-ou-relation un-à-plusieurs (0. 1 à *) entre la `Instructor` et `Department` entités.</span><span class="sxs-lookup"><span data-stu-id="77788-388">The zero-or-one-to-many relationship line (0..1 to *) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="77788-389">Valeur initiale de la base de données avec des données de Test</span><span class="sxs-lookup"><span data-stu-id="77788-389">Seed the DB with Test Data</span></span>

<span data-ttu-id="77788-390">Mettre à jour le code dans *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="77788-390">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="77788-391">Le code précédent fournit des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="77788-391">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="77788-392">La majeure partie de ce code crée des objets d’entité et charge les exemples de données.</span><span class="sxs-lookup"><span data-stu-id="77788-392">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="77788-393">Les exemples de données sont utilisés pour le test.</span><span class="sxs-lookup"><span data-stu-id="77788-393">The sample data is used for testing.</span></span> <span data-ttu-id="77788-394">Le code précédent crée les relations plusieurs-à-plusieurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="77788-394">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="77788-395">Remarque : [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) prend en charge [amorçage de données](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="77788-395">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="77788-396">Ajouter une migration</span><span class="sxs-lookup"><span data-stu-id="77788-396">Add a migration</span></span>

<span data-ttu-id="77788-397">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="77788-397">Build the project.</span></span> <span data-ttu-id="77788-398">Ouvrez une fenêtre de commande dans le dossier du projet, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="77788-398">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="77788-399">La commande précédente affiche un avertissement concernant les pertes de données possibles.</span><span class="sxs-lookup"><span data-stu-id="77788-399">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="77788-400">Si la `database update` commande est exécutée, l’erreur suivante se produit :</span><span class="sxs-lookup"><span data-stu-id="77788-400">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="77788-401">Lorsque les migrations sont exécutées avec des données existantes, il peut y avoir des contraintes FK qui ne sont pas remplies avec les données de sortie.</span><span class="sxs-lookup"><span data-stu-id="77788-401">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="77788-402">Pour ce didacticiel, une nouvelle base de données est créé, il n’y a aucune violation de contrainte FK.</span><span class="sxs-lookup"><span data-stu-id="77788-402">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="77788-403">Consultez [fixation des contraintes de clé étrangère avec des données héritées](#fk) pour obtenir des instructions sur la façon de corriger les violations FK sur la base de données actuelle.</span><span class="sxs-lookup"><span data-stu-id="77788-403">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="77788-404">Modifier la chaîne de connexion et de mettre à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="77788-404">Change the connection string and update the DB</span></span>

<span data-ttu-id="77788-405">Le code de mise à jour `DbInitializer` ajoute des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="77788-405">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="77788-406">Pour forcer Core EF pour créer une nouvelle base de données vide :</span><span class="sxs-lookup"><span data-stu-id="77788-406">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="77788-407">Modifier le nom de chaîne de connexion de base de données dans *appsettings.json* à ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="77788-407">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="77788-408">Le nouveau nom doit être un nom qui n’a pas été utilisé sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="77788-408">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="77788-409">Vous pouvez également supprimer la base de données à l’aide de :</span><span class="sxs-lookup"><span data-stu-id="77788-409">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="77788-410">**L’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="77788-410">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="77788-411">Le `database drop` commande CLI :</span><span class="sxs-lookup"><span data-stu-id="77788-411">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="77788-412">Exécutez `database update` dans la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="77788-412">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="77788-413">La commande précédente s’exécute toutes les migrations.</span><span class="sxs-lookup"><span data-stu-id="77788-413">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="77788-414">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="77788-414">Run the app.</span></span> <span data-ttu-id="77788-415">L’exécution de l’application en cours d’exécution le `DbInitializer.Initialize` (méthode).</span><span class="sxs-lookup"><span data-stu-id="77788-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="77788-416">Le `DbInitializer.Initialize` remplit la nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-416">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="77788-417">Ouvrez la base de données dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="77788-417">Open the DB in SSOX:</span></span>

* <span data-ttu-id="77788-418">Développez le **Tables** nœud.</span><span class="sxs-lookup"><span data-stu-id="77788-418">Expand the **Tables** node.</span></span> <span data-ttu-id="77788-419">Les tables sont affichés.</span><span class="sxs-lookup"><span data-stu-id="77788-419">The created tables are displayed.</span></span>
* <span data-ttu-id="77788-420">Si SSOX a été ouverte, cliquez sur le **Actualiser** bouton.</span><span class="sxs-lookup"><span data-stu-id="77788-420">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tables de SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="77788-422">Examinez le **CourseAssignment** table :</span><span class="sxs-lookup"><span data-stu-id="77788-422">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="77788-423">Avec le bouton droit le **CourseAssignment** de table et sélectionnez **des données d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="77788-423">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="77788-424">Vérifiez le **CourseAssignment** table contient des données.</span><span class="sxs-lookup"><span data-stu-id="77788-424">Verify the **CourseAssignment** table contains data.</span></span>

![Données CourseAssignment dans SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="77788-426">Résolution des contraintes de clé étrangère avec des données héritées</span><span class="sxs-lookup"><span data-stu-id="77788-426">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="77788-427">Cette section est facultative.</span><span class="sxs-lookup"><span data-stu-id="77788-427">This section is optional.</span></span>

<span data-ttu-id="77788-428">Lorsque les migrations sont exécutées avec des données existantes, il peut y avoir des contraintes FK qui ne sont pas remplies avec les données de sortie.</span><span class="sxs-lookup"><span data-stu-id="77788-428">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="77788-429">Données de production, les étapes nécessaires pour migrer les données existantes.</span><span class="sxs-lookup"><span data-stu-id="77788-429">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="77788-430">Cette section fournit un exemple de résolution des violations de contrainte FK.</span><span class="sxs-lookup"><span data-stu-id="77788-430">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="77788-431">Ne pas rendre ces modifications de code sans une sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="77788-431">Don't make these code changes without a backup.</span></span> <span data-ttu-id="77788-432">Ne modifiez ces code si la fin de la section précédente et la mise à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="77788-432">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="77788-433">Le *{timestamp}_ComplexDataModel.cs* fichier contient le code suivant :</span><span class="sxs-lookup"><span data-stu-id="77788-433">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="77788-434">Le code précédent ajoute un non-nullable `DepartmentID` FK à la `Course` table.</span><span class="sxs-lookup"><span data-stu-id="77788-434">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="77788-435">Contient des lignes dans la base de données à partir du didacticiel précédent `Course`, de sorte que cette table ne peut pas être mis à jour par des migrations.</span><span class="sxs-lookup"><span data-stu-id="77788-435">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="77788-436">Pour rendre la `ComplexDataModel` travail de migration avec des données existantes :</span><span class="sxs-lookup"><span data-stu-id="77788-436">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="77788-437">Modifier le code pour permettre à la nouvelle colonne (`DepartmentID`) comme valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="77788-437">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="77788-438">Créer un service factice nommé « Temp » en tant que le service par défaut.</span><span class="sxs-lookup"><span data-stu-id="77788-438">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="77788-439">Résoudre les contraintes de clé étrangères</span><span class="sxs-lookup"><span data-stu-id="77788-439">Fix the foreign key constraints</span></span>

<span data-ttu-id="77788-440">Mise à jour la `ComplexDataModel` classes `Up` méthode :</span><span class="sxs-lookup"><span data-stu-id="77788-440">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="77788-441">Ouvrez le *{timestamp}_ComplexDataModel.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="77788-441">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="77788-442">Commentez la ligne de code qui ajoute le `DepartmentID` colonne à la `Course` table.</span><span class="sxs-lookup"><span data-stu-id="77788-442">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="77788-443">Ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="77788-443">Add the following highlighted code.</span></span> <span data-ttu-id="77788-444">Le nouveau code va après le `.CreateTable( name: "Department"` bloc :[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="77788-444">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="77788-445">Avec les modifications précédentes, existant `Course` lignes sont liées au service « Temp » après la `ComplexDataModel` `Up` s’exécute la méthode.</span><span class="sxs-lookup"><span data-stu-id="77788-445">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="77788-446">Une application de production serait :</span><span class="sxs-lookup"><span data-stu-id="77788-446">A production app would:</span></span>

* <span data-ttu-id="77788-447">Inclure le code ou des scripts pour ajouter `Department` lignes et liées `Course` lignes à la nouvelle `Department` lignes.</span><span class="sxs-lookup"><span data-stu-id="77788-447">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="77788-448">N’utilisez pas le service « Temp » ou la valeur par défaut `Course.DepartmentID `.</span><span class="sxs-lookup"><span data-stu-id="77788-448">Would not use the "Temp" department or the default value for `Course.DepartmentID `.</span></span>

<span data-ttu-id="77788-449">Le didacticiel suivant traite les données associées.</span><span class="sxs-lookup"><span data-stu-id="77788-449">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="77788-450">[Précédent](xref:data/ef-rp/migrations)
[Suivant](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="77788-450">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>