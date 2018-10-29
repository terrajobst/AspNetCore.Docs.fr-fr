---
title: Pages Razor avec EF Core dans ASP.NET Core - Modèle de données - 5 sur 8
author: rick-anderson
description: Dans ce tutoriel, vous ajoutez des entités et des relations, et vous personnalisez le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 9a0d5a8e722487ccf7e08aadb39f838a0963451d
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090965"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="b89ac-103">Pages Razor avec EF Core dans ASP.NET Core - Modèle de données - 5 sur 8</span><span class="sxs-lookup"><span data-stu-id="b89ac-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b89ac-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b89ac-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="b89ac-105">Dans les didacticiels précédents, nous avons travaillé avec un modèle de données de base composé de trois entités.</span><span class="sxs-lookup"><span data-stu-id="b89ac-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="b89ac-106">Dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="b89ac-106">In this tutorial:</span></span>

* <span data-ttu-id="b89ac-107">Nous allons ajouter d’autres entités et relations</span><span class="sxs-lookup"><span data-stu-id="b89ac-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="b89ac-108">Nous allons personnaliser le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage de base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="b89ac-109">Les classes d’entités pour le modèle de données sont illustrées ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b89ac-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagramme des entités](complex-data-model/_static/diagram.png)

<span data-ttu-id="b89ac-111">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="b89ac-111">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="b89ac-112">Personnaliser le modèle de données avec des attributs</span><span class="sxs-lookup"><span data-stu-id="b89ac-112">Customize the data model with attributes</span></span>

<span data-ttu-id="b89ac-113">Dans cette section, nous allons personnaliser le modèle de données à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="b89ac-114">Attribut DataType</span><span class="sxs-lookup"><span data-stu-id="b89ac-114">The DataType attribute</span></span>

<span data-ttu-id="b89ac-115">Actuellement, les pages sur les étudiants affichent l’heure et la date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="b89ac-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="b89ac-116">En règle générale, les champs de date ne montrent que la date, et non l’heure.</span><span class="sxs-lookup"><span data-stu-id="b89ac-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="b89ac-117">Mettez à jour *Models/Student.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="b89ac-118">L’attribut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) spécifie un type de données qui est plus spécifique que le type intrinsèque de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-118">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="b89ac-119">Ici, seule la date doit être affichée (pas la date et l’heure).</span><span class="sxs-lookup"><span data-stu-id="b89ac-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="b89ac-120">L’énumération [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fournit de nombreux types de données, tels que Date, Time, PhoneNumber, Currency, EmailAddress, et ainsi de suite. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type.</span><span class="sxs-lookup"><span data-stu-id="b89ac-120">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="b89ac-121">Exemple :</span><span class="sxs-lookup"><span data-stu-id="b89ac-121">For example:</span></span>

* <span data-ttu-id="b89ac-122">Le lien `mailto:` est créé automatiquement pour `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="b89ac-123">Le sélecteur de date est fourni pour `DataType.Date` dans la plupart des navigateurs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="b89ac-124">L’attribut `DataType` émet des attributs HTML 5 `data-` utilisés par les navigateurs HTML 5.</span><span class="sxs-lookup"><span data-stu-id="b89ac-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="b89ac-125">Les attributs `DataType` ne fournissent aucune validation.</span><span class="sxs-lookup"><span data-stu-id="b89ac-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="b89ac-126">`DataType.Date` ne spécifie pas le format de la date qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b89ac-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="b89ac-127">Par défaut, le champ de date est affiché conformément aux formats par défaut basés sur l’objet [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) du serveur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="b89ac-128">L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :</span><span class="sxs-lookup"><span data-stu-id="b89ac-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="b89ac-129">Le paramètre `ApplyFormatInEditMode` spécifie que la mise en forme doit également être appliquée à l’interface utilisateur de modification.</span><span class="sxs-lookup"><span data-stu-id="b89ac-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="b89ac-130">Certains champs ne doivent pas utiliser `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="b89ac-131">Par exemple, le symbole monétaire ne doit généralement pas être affiché dans une zone de texte d’édition.</span><span class="sxs-lookup"><span data-stu-id="b89ac-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="b89ac-132">L’attribut `DisplayFormat` peut être utilisé seul.</span><span class="sxs-lookup"><span data-stu-id="b89ac-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="b89ac-133">Il est généralement préférable d’utiliser l’attribut `DataType` avec l’attribut `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="b89ac-134">L’attribut `DataType` transmet la sémantique des données, plutôt que la manière de l’afficher à l’écran.</span><span class="sxs-lookup"><span data-stu-id="b89ac-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="b89ac-135">L’attribut `DataType` offre les avantages suivants qui ne sont pas disponibles dans `DisplayFormat` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="b89ac-136">Le navigateur peut activer des fonctionnalités HTML5</span><span class="sxs-lookup"><span data-stu-id="b89ac-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="b89ac-137">(par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, une validation des entrées côté client, et ainsi de suite).</span><span class="sxs-lookup"><span data-stu-id="b89ac-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="b89ac-138">Par défaut, le navigateur affiche les données à l’aide du format correspondant aux paramètres régionaux.</span><span class="sxs-lookup"><span data-stu-id="b89ac-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="b89ac-139">Pour plus d’informations, consultez la [documentation relative au Tag Helper \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="b89ac-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="b89ac-140">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b89ac-140">Run the app.</span></span> <span data-ttu-id="b89ac-141">Accédez à la page d’index des étudiants.</span><span class="sxs-lookup"><span data-stu-id="b89ac-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="b89ac-142">Les heures ne sont plus affichées.</span><span class="sxs-lookup"><span data-stu-id="b89ac-142">Times are no longer displayed.</span></span> <span data-ttu-id="b89ac-143">Tous les affichages qui utilisent le modèle `Student` affichent la date sans heure.</span><span class="sxs-lookup"><span data-stu-id="b89ac-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Page d’index des étudiants affichant les dates sans les heures](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="b89ac-145">Attribut StringLength</span><span class="sxs-lookup"><span data-stu-id="b89ac-145">The StringLength attribute</span></span>

<span data-ttu-id="b89ac-146">Vous pouvez également spécifier des règles de validation de données et des messages d’erreur de validation à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="b89ac-147">L’attribut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) spécifie les longueurs minimale et maximale de caractères autorisées dans un champ de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="b89ac-148">L’attribut `StringLength` fournit également la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="b89ac-149">La valeur minimale n’a aucun impact sur le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="b89ac-150">Mettez à jour le modèle `Student` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="b89ac-151">Le code précédent limite la longueur des noms à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="b89ac-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="b89ac-152">L’attribut `StringLength` n’empêche pas un utilisateur d’entrer un espace blanc comme nom.</span><span class="sxs-lookup"><span data-stu-id="b89ac-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="b89ac-153">L’attribut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) est utilisé pour appliquer des restrictions à l’entrée.</span><span class="sxs-lookup"><span data-stu-id="b89ac-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="b89ac-154">Par exemple, le code suivant exige que le premier caractère soit une majuscule et que les autres caractères soient alphabétiques :</span><span class="sxs-lookup"><span data-stu-id="b89ac-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="b89ac-155">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="b89ac-155">Run the app:</span></span>

* <span data-ttu-id="b89ac-156">Accédez à la page Students.</span><span class="sxs-lookup"><span data-stu-id="b89ac-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="b89ac-157">Sélectionnez **Create New** et entrez un nom de plus de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="b89ac-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="b89ac-158">Sélectionnez **Create**. La validation côté client affiche un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-158">Select **Create**, client-side validation shows an error message.</span></span>

![Page d’index des étudiants affichant des erreurs de longueur de chaîne](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="b89ac-160">Dans **l’Explorateur d’objets SQL Server** (SSOX), ouvrez le concepteur de tables Student en double-cliquant sur la table **Student**.</span><span class="sxs-lookup"><span data-stu-id="b89ac-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Table Students dans SSOX avant les migrations](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="b89ac-162">L’image précédente montre le schéma pour la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="b89ac-163">Les champs de nom sont de type `nvarchar(MAX)`, car les migrations n’ont pas été exécutées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="b89ac-164">Quand nous exécuterons les migrations plus loin dans ce didacticiel, les champs de nom deviendront `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="b89ac-165">Attribut Column</span><span class="sxs-lookup"><span data-stu-id="b89ac-165">The Column attribute</span></span>

<span data-ttu-id="b89ac-166">Les attributs peuvent contrôler comment les classes et les propriétés sont mappées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="b89ac-167">Dans cette section, nous utilisons l’attribut `Column` pour mapper le nom de la propriété `FirstMidName` « FirstName » dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="b89ac-168">Lors de la création de la base de données, les noms des propriétés sur le modèle sont utilisés comme noms de colonne (sauf quand l’attribut `Column` est utilisé).</span><span class="sxs-lookup"><span data-stu-id="b89ac-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="b89ac-169">Le modèle `Student` utilise `FirstMidName` pour le champ de prénom, car le champ peut également contenir un deuxième prénom.</span><span class="sxs-lookup"><span data-stu-id="b89ac-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="b89ac-170">Mettez à jour *Student.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="b89ac-171">Avec la modification précédente, `Student.FirstMidName` dans l’application est mappé à la colonne `FirstName` de la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="b89ac-172">L’ajout de l’attribut `Column` change le modèle sur lequel repose `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="b89ac-173">Le modèle sur lequel repose le `SchoolContext` ne correspond plus à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="b89ac-174">Si vous exécutez l’application avant d’appliquer les migrations, l’exception suivante est générée :</span><span class="sxs-lookup"><span data-stu-id="b89ac-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="b89ac-175">Pour mettre à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="b89ac-175">To update the DB:</span></span>

* <span data-ttu-id="b89ac-176">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b89ac-176">Build the project.</span></span>
* <span data-ttu-id="b89ac-177">Ouvrez une fenêtre de commande dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="b89ac-177">Open a command window in the project folder.</span></span> <span data-ttu-id="b89ac-178">Entrez les commandes suivantes pour créer une migration et mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="b89ac-178">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b89ac-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b89ac-179">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b89ac-180">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="b89ac-180">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

<span data-ttu-id="b89ac-181">La commande `migrations add ColumnFirstName` génère le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-181">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="b89ac-182">Cet avertissement est généré, car les champs de nom sont désormais limités à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="b89ac-182">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="b89ac-183">Si un nom dans la base de données a plus de 50 caractères, tous les caractères au-delà du cinquantième sont perdus.</span><span class="sxs-lookup"><span data-stu-id="b89ac-183">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="b89ac-184">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="b89ac-184">Test the app.</span></span>

<span data-ttu-id="b89ac-185">Ouvrez la table Student dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="b89ac-185">Open the Student table in SSOX:</span></span>

![Table Students dans SSOX après les migrations](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="b89ac-187">Avant l’application de la migration, les colonnes de nom étaient de type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="b89ac-187">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="b89ac-188">Les colonnes de nom sont maintenant `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-188">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="b89ac-189">Le nom de la colonne est passé de `FirstMidName` à `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-189">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="b89ac-190">Dans la section suivante, la génération de l’application à certaines étapes génère des erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-190">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="b89ac-191">Les instructions indiquent quand générer l’application.</span><span class="sxs-lookup"><span data-stu-id="b89ac-191">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="b89ac-192">Mise à jour de l’entité Student</span><span class="sxs-lookup"><span data-stu-id="b89ac-192">Student entity update</span></span>

![Entité Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="b89ac-194">Mettez à jour *Models/Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-194">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="b89ac-195">Attribut Required</span><span class="sxs-lookup"><span data-stu-id="b89ac-195">The Required attribute</span></span>

<span data-ttu-id="b89ac-196">L’attribut `Required` fait des propriétés de nom des champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="b89ac-196">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="b89ac-197">L’attribut `Required` n’est pas nécessaire pour les types non nullables tels que les types valeur (`DateTime`, `int`, `double` et ainsi de suite).</span><span class="sxs-lookup"><span data-stu-id="b89ac-197">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="b89ac-198">Les types qui n’acceptent pas les valeurs Null sont traités automatiquement comme des champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="b89ac-198">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="b89ac-199">L’attribut `Required` peut être remplacé par un paramètre de longueur minimale dans l’attribut `StringLength` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-199">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="b89ac-200">Attribut Display</span><span class="sxs-lookup"><span data-stu-id="b89ac-200">The Display attribute</span></span>

<span data-ttu-id="b89ac-201">L’attribut `Display` indique que la légende des zones de texte doit être « First Name », « Last Name », « Full Name » et « Enrollment Date ».</span><span class="sxs-lookup"><span data-stu-id="b89ac-201">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="b89ac-202">Les légendes par défaut ne comportaient pas d’espace entre les mots, par exemple « Lastname ».</span><span class="sxs-lookup"><span data-stu-id="b89ac-202">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="b89ac-203">Propriété calculée FullName</span><span class="sxs-lookup"><span data-stu-id="b89ac-203">The FullName calculated property</span></span>

<span data-ttu-id="b89ac-204">`FullName` est une propriété calculée qui retourne une valeur créée par concaténation de deux autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="b89ac-204">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="b89ac-205">`FullName` ne peut pas être définie. Elle a uniquement un accesseur get.</span><span class="sxs-lookup"><span data-stu-id="b89ac-205">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="b89ac-206">Aucune colonne `FullName` n’est créée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-206">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="b89ac-207">Créer l’entité Instructor</span><span class="sxs-lookup"><span data-stu-id="b89ac-207">Create the Instructor Entity</span></span>

![Entité Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="b89ac-209">Créez *Models/Instructor.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-209">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="b89ac-210">Plusieurs attributs peuvent être sur une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="b89ac-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="b89ac-211">Les attributs `HireDate` peuvent être écrits comme suit :</span><span class="sxs-lookup"><span data-stu-id="b89ac-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="b89ac-212">Propriétés de navigation CourseAssignments et OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="b89ac-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="b89ac-213">Les propriétés `CourseAssignments` et `OfficeAssignment` sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b89ac-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="b89ac-214">Un formateur pouvant animer un nombre quelconque de cours, `CourseAssignments` est défini comme une collection.</span><span class="sxs-lookup"><span data-stu-id="b89ac-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="b89ac-215">Si une propriété de navigation contient plusieurs entités :</span><span class="sxs-lookup"><span data-stu-id="b89ac-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="b89ac-216">Elle doit être un type de liste où les entrées peuvent être ajoutées, supprimées et mises à jour.</span><span class="sxs-lookup"><span data-stu-id="b89ac-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="b89ac-217">Les types de propriétés de navigation sont :</span><span class="sxs-lookup"><span data-stu-id="b89ac-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="b89ac-218">Si `ICollection<T>` est spécifié, EF Core crée une collection `HashSet<T>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="b89ac-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="b89ac-219">L’entité `CourseAssignment` est expliquée dans la section sur les relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="b89ac-220">Les règles professionnelles de Contoso University stipulent qu’un formateur peut avoir au plus un bureau.</span><span class="sxs-lookup"><span data-stu-id="b89ac-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="b89ac-221">La propriété `OfficeAssignment` contient une seule entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="b89ac-222">`OfficeAssignment` a la valeur Null si aucun bureau n’est affecté.</span><span class="sxs-lookup"><span data-stu-id="b89ac-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="b89ac-223">Créer l’entité OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="b89ac-223">Create the OfficeAssignment entity</span></span>

![Entité OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="b89ac-225">Créez *Models/OfficeAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="b89ac-226">Attribut Key</span><span class="sxs-lookup"><span data-stu-id="b89ac-226">The Key attribute</span></span>

<span data-ttu-id="b89ac-227">L’attribut `[Key]` est utilisé pour identifier une propriété comme clé primaire quand le nom de la propriété est autre que classnameID ou ID.</span><span class="sxs-lookup"><span data-stu-id="b89ac-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="b89ac-228">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="b89ac-229">Une affectation de bureau existe uniquement relativement au formateur auquel elle est assignée.</span><span class="sxs-lookup"><span data-stu-id="b89ac-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="b89ac-230">La clé primaire `OfficeAssignment` est également sa clé étrangère pour l’entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="b89ac-231">EF Core ne peut pas reconnaître automatiquement `InstructorID` comme clé primaire de `OfficeAssignment`, car :</span><span class="sxs-lookup"><span data-stu-id="b89ac-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="b89ac-232">`InstructorID` ne suit pas la convention de nommage ID ou classnameID.</span><span class="sxs-lookup"><span data-stu-id="b89ac-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="b89ac-233">Ainsi, l’attribut `Key` est utilisé pour identifier `InstructorID` comme clé primaire :</span><span class="sxs-lookup"><span data-stu-id="b89ac-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="b89ac-234">Par défaut, EF Core traite la clé comme n’étant pas générée par la base de données, car la colonne est utilisée pour une relation d’identification.</span><span class="sxs-lookup"><span data-stu-id="b89ac-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="b89ac-235">Propriété de navigation Instructor</span><span class="sxs-lookup"><span data-stu-id="b89ac-235">The Instructor navigation property</span></span>

<span data-ttu-id="b89ac-236">La propriété de navigation `OfficeAssignment` pour l’entité `Instructor` est nullable car :</span><span class="sxs-lookup"><span data-stu-id="b89ac-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="b89ac-237">Les types référence (tels que les classes) sont nullables.</span><span class="sxs-lookup"><span data-stu-id="b89ac-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="b89ac-238">Un formateur peut ne pas avoir d’affectation de bureau.</span><span class="sxs-lookup"><span data-stu-id="b89ac-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="b89ac-239">L’entité `OfficeAssignment` a une propriété de navigation `Instructor` non-nullable car :</span><span class="sxs-lookup"><span data-stu-id="b89ac-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="b89ac-240">`InstructorID` n'autorise pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="b89ac-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="b89ac-241">Une affectation de bureau ne peut pas exister sans un formateur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="b89ac-242">Quand une entité `Instructor` a une entité `OfficeAssignment` associée, chaque entité a une référence à l’autre dans sa propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="b89ac-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="b89ac-243">L’attribut `[Required]` peut être appliqué à la propriété de navigation `Instructor` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="b89ac-244">Le code précédent spécifie qu’il doit y avoir un formateur associé.</span><span class="sxs-lookup"><span data-stu-id="b89ac-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="b89ac-245">Le code précédent n’est pas nécessaire, car la clé étrangère `InstructorID` (qui est également la clé primaire) n'autorise pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="b89ac-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="b89ac-246">Modifier l’entité Course</span><span class="sxs-lookup"><span data-stu-id="b89ac-246">Modify the Course Entity</span></span>

![Entité Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="b89ac-248">Mettez à jour *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="b89ac-249">L’entité `Course` a une propriété de clé étrangère `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="b89ac-250">`DepartmentID` pointe vers l’entité `Department` associée.</span><span class="sxs-lookup"><span data-stu-id="b89ac-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="b89ac-251">L’entité `Course` a une propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="b89ac-252">EF Core n’exige pas de propriété de clé étrangère pour un modèle de données quand le modèle a une propriété de navigation pour une entité associée.</span><span class="sxs-lookup"><span data-stu-id="b89ac-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="b89ac-253">EF Core crée automatiquement des clés étrangères dans la base de données partout où elles sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="b89ac-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="b89ac-254">EF Core crée des [propriétés cachées](/ef/core/modeling/shadow-properties) pour les clés étrangères créées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b89ac-254">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="b89ac-255">Le fait d’avoir la clé étrangère dans le modèle de données peut rendre les mises à jour plus simples et plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="b89ac-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="b89ac-256">Par exemple, considérez un modèle où la propriété de clé étrangère `DepartmentID` n’est *pas* incluse.</span><span class="sxs-lookup"><span data-stu-id="b89ac-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="b89ac-257">Quand une entité Course est récupérée en vue d’une modification :</span><span class="sxs-lookup"><span data-stu-id="b89ac-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="b89ac-258">L’entité `Department` a la valeur Null si elle n’est pas chargée explicitement.</span><span class="sxs-lookup"><span data-stu-id="b89ac-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="b89ac-259">Pour mettre à jour l’entité Course, vous devez d’abord récupérer l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="b89ac-260">Quand la propriété de clé étrangère `DepartmentID` est incluse dans le modèle de données, il n’est pas nécessaire de récupérer l’entité `Department` avant une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="b89ac-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="b89ac-261">Attribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="b89ac-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="b89ac-262">L’attribut `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indique que la clé primaire est fournie par l’application plutôt que générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="b89ac-263">Par défaut, EF Core part du principe que les valeurs de clé primaire sont générées par la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="b89ac-264">Les valeurs de clé primaire générées par la base de données constituent en général la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="b89ac-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="b89ac-265">Pour les entités `Course`, l’utilisateur spécifie la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="b89ac-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="b89ac-266">Par exemple, un numéro de cours comme une série 1000 pour le département Mathématiques et une série 2000 pour le département Anglais.</span><span class="sxs-lookup"><span data-stu-id="b89ac-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="b89ac-267">Vous pouvez aussi utiliser l’attribut `DatabaseGenerated` pour générer des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="b89ac-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="b89ac-268">Par exemple, la base de données peut générer automatiquement un champ de date pour enregistrer la date de création ou de mise à jour d’une ligne.</span><span class="sxs-lookup"><span data-stu-id="b89ac-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="b89ac-269">Pour plus d’informations, consultez [Propriétés générées](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="b89ac-269">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b89ac-270">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="b89ac-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="b89ac-271">Les propriétés de clé étrangère et les propriétés de navigation dans l’entité `Course` reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b89ac-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="b89ac-272">Un cours étant affecté à un seul département, il y a une clé étrangère `DepartmentID` et une propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="b89ac-273">Un cours pouvant avoir un nombre quelconque d’étudiants inscrits, la propriété de navigation `Enrollments` est une collection :</span><span class="sxs-lookup"><span data-stu-id="b89ac-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="b89ac-274">Un cours pouvant être animé par plusieurs formateurs, la propriété de navigation `CourseAssignments` est une collection :</span><span class="sxs-lookup"><span data-stu-id="b89ac-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="b89ac-275">`CourseAssignment` est expliquée [plus loin](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="b89ac-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="b89ac-276">Créer l’entité Department</span><span class="sxs-lookup"><span data-stu-id="b89ac-276">Create the Department entity</span></span>

![Entité Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="b89ac-278">Créez *Models/Department.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="b89ac-279">Attribut Column</span><span class="sxs-lookup"><span data-stu-id="b89ac-279">The Column attribute</span></span>

<span data-ttu-id="b89ac-280">Précédemment, vous avez utilisé l’attribut `Column` pour changer le mappage des noms de colonne.</span><span class="sxs-lookup"><span data-stu-id="b89ac-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="b89ac-281">Dans le code de l’entité `Department`, l’attribut `Column` est utilisé pour changer le mappage des types de données SQL.</span><span class="sxs-lookup"><span data-stu-id="b89ac-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="b89ac-282">La colonne `Budget` est définie à l’aide du type monétaire de SQL Server dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="b89ac-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="b89ac-283">Le mappage de colonnes n’est généralement pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="b89ac-283">Column mapping is generally not required.</span></span> <span data-ttu-id="b89ac-284">EF Core choisit en général le type de données SQL Server approprié en fonction du type CLR de la propriété.</span><span class="sxs-lookup"><span data-stu-id="b89ac-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="b89ac-285">Le type CLR `decimal` est mappé à un type SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="b89ac-286">`Budget` étant une valeur monétaire, le type de données money est plus approprié.</span><span class="sxs-lookup"><span data-stu-id="b89ac-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b89ac-287">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="b89ac-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="b89ac-288">Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b89ac-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="b89ac-289">Un département peut avoir ou ne pas avoir un administrateur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="b89ac-290">Un administrateur est toujours un formateur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-290">An administrator is always an instructor.</span></span> <span data-ttu-id="b89ac-291">Ainsi, la propriété `InstructorID` est incluse en tant que clé étrangère de l’entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="b89ac-292">La propriété de navigation se nomme `Administrator`, mais elle contient une entité `Instructor` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="b89ac-293">Le point d’interrogation (?) dans le code précédent indique que la propriété est nullable.</span><span class="sxs-lookup"><span data-stu-id="b89ac-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="b89ac-294">Un département pouvant avoir de nombreux cours, il existe une propriété de navigation Courses :</span><span class="sxs-lookup"><span data-stu-id="b89ac-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="b89ac-295">Remarque : Par convention, EF Core autorise la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs à plusieurs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="b89ac-296">Les suppressions en cascade peuvent engendrer des règles de suppression en cascade circulaires.</span><span class="sxs-lookup"><span data-stu-id="b89ac-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="b89ac-297">Les règles de suppression en cascade circulaires provoquent une exception quand une migration est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="b89ac-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="b89ac-298">Par exemple, si la propriété `Department.InstructorID` n’a pas été définie comme nullable :</span><span class="sxs-lookup"><span data-stu-id="b89ac-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="b89ac-299">EF Core configure une règle de suppression en cascade pour supprimer le formateur quand le département est supprimé.</span><span class="sxs-lookup"><span data-stu-id="b89ac-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="b89ac-300">Supprimer le formateur quand le département est supprimé n’est pas le comportement souhaité.</span><span class="sxs-lookup"><span data-stu-id="b89ac-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="b89ac-301">Si les règles d’entreprise exigent que la propriété `InstructorID` soit non nullable, utilisez l’instruction d’API Fluent suivante :</span><span class="sxs-lookup"><span data-stu-id="b89ac-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="b89ac-302">Le code précédent désactive la suppression en cascade sur la relation formateur-département.</span><span class="sxs-lookup"><span data-stu-id="b89ac-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="b89ac-303">Mettre à jour l’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="b89ac-303">Update the Enrollment entity</span></span>

<span data-ttu-id="b89ac-304">Un enregistrement d’inscription correspond à un cours suivi par un étudiant.</span><span class="sxs-lookup"><span data-stu-id="b89ac-304">An enrollment record is for one course taken by one student.</span></span>

![Entité Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="b89ac-306">Mettez à jour *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b89ac-307">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="b89ac-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="b89ac-308">Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b89ac-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="b89ac-309">Un enregistrement d’inscription ne concernant qu’un seul cours, il y a une propriété de clé étrangère `CourseID` et une propriété de navigation `Course` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="b89ac-310">Un enregistrement d’inscription ne concernant qu’un seul étudiant, il y a une propriété de clé étrangère `StudentID` et une propriété de navigation `Student` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="b89ac-311">Relations plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="b89ac-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="b89ac-312">Il existe une relation plusieurs-à-plusieurs entre les entités `Student` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="b89ac-313">L’entité `Enrollment` joue le rôle de table de jointure plusieurs-à-plusieurs *avec charge utile* dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="b89ac-314">« Avec charge utile » signifie que la table `Enrollment` contient des données supplémentaires en plus des clés étrangères pour les tables jointes (dans le cas présent, la clé primaire et `Grade`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="b89ac-315">L’illustration suivante montre à quoi ressemblent ces relations dans un diagramme d’entité.</span><span class="sxs-lookup"><span data-stu-id="b89ac-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="b89ac-316">(Ce diagramme a été généré à l’aide de [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) pour EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="b89ac-316">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="b89ac-317">Sa création ne fait pas partie de ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="b89ac-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Relation plusieurs à plusieurs Student-Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="b89ac-319">Chaque ligne de relation comporte un 1 à une extrémité et un astérisque (\*) à l’autre, ce qui indique une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="b89ac-320">Si la table `Enrollment` n’incluait pas d’informations de notes, elle aurait uniquement besoin de contenir les deux clés étrangères (`CourseID` et `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="b89ac-321">Une table de jointure plusieurs-à-plusieurs sans charge utile est parfois appelée « table de jointure pure ».</span><span class="sxs-lookup"><span data-stu-id="b89ac-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="b89ac-322">Les entités `Instructor` et `Course` ont une relation plusieurs-à-plusieurs à l’aide d’une table de jointure pure.</span><span class="sxs-lookup"><span data-stu-id="b89ac-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="b89ac-323">Remarque : EF 6.x prend en charge les tables de jointure implicites pour les relations plusieurs-à-plusieurs, mais pas EF Core.</span><span class="sxs-lookup"><span data-stu-id="b89ac-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="b89ac-324">Pour plus d’informations, consultez [Many-to-many relationships in EF Core 2.0 (Relations plusieurs-à-plusieurs dans EF Core 2.0)](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="b89ac-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="b89ac-325">Entité CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="b89ac-325">The CourseAssignment entity</span></span>

![Entité CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="b89ac-327">Créez *Models/CourseAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="b89ac-328">Instructor-Courses</span><span class="sxs-lookup"><span data-stu-id="b89ac-328">Instructor-to-Courses</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="b89ac-330">La relation plusieurs-à-plusieurs Instructor-Courses :</span><span class="sxs-lookup"><span data-stu-id="b89ac-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="b89ac-331">Nécessite une table de jointure qui doit être représentée par un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="b89ac-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="b89ac-332">Est une table de jointure pure (table sans charge utile).</span><span class="sxs-lookup"><span data-stu-id="b89ac-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="b89ac-333">Il est courant de nommer une entité de jointure `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="b89ac-334">Par exemple, la table de jointure Instructor-to-Courses utilisant ce modèle est `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="b89ac-335">Toutefois, nous vous recommandons d’utiliser un nom qui décrit la relation.</span><span class="sxs-lookup"><span data-stu-id="b89ac-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="b89ac-336">Les modèles de données sont simples au début, puis ils augmentent en complexité.</span><span class="sxs-lookup"><span data-stu-id="b89ac-336">Data models start out simple and grow.</span></span> <span data-ttu-id="b89ac-337">Les jointures sans charge utile (tables de jointure pures) évoluent fréquemment pour inclure une charge utile.</span><span class="sxs-lookup"><span data-stu-id="b89ac-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="b89ac-338">En commençant par un nom d’entité descriptif, vous n’aurez pas besoin de modifier le nom quand la table de jointure changera.</span><span class="sxs-lookup"><span data-stu-id="b89ac-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="b89ac-339">Dans l’idéal, l’entité de jointure aura son propre nom (éventuellement un mot unique) naturel dans le domaine d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="b89ac-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="b89ac-340">Par exemple, les livres et les clients pourraient être liés avec une entité de jointure nommée Évaluations.</span><span class="sxs-lookup"><span data-stu-id="b89ac-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="b89ac-341">Pour la relation plusieurs-à-plusieurs Instructor-Courses, il vaut mieux utiliser `CourseAssignment` que `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="b89ac-342">Clé composite</span><span class="sxs-lookup"><span data-stu-id="b89ac-342">Composite key</span></span>

<span data-ttu-id="b89ac-343">Les clés étrangères ne sont pas nullables.</span><span class="sxs-lookup"><span data-stu-id="b89ac-343">FKs are not nullable.</span></span> <span data-ttu-id="b89ac-344">Ensemble, le deux clés étrangères dans `CourseAssignment` (`InstructorID` et `CourseID`) identifient de façon unique chaque ligne de la table `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="b89ac-345">`CourseAssignment` ne nécessite pas de clé primaire dédiée.</span><span class="sxs-lookup"><span data-stu-id="b89ac-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="b89ac-346">Les propriétés `InstructorID` et `CourseID` fonctionnent comme une clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="b89ac-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="b89ac-347">Le seul moyen de spécifier des clés primaires composites dans EF Core consiste à faire appel à l’*API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="b89ac-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="b89ac-348">La section suivante montre comment configurer la clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="b89ac-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="b89ac-349">La clé composite garantit que :</span><span class="sxs-lookup"><span data-stu-id="b89ac-349">The composite key ensures:</span></span>

* <span data-ttu-id="b89ac-350">Plusieurs lignes sont autorisées pour un cours.</span><span class="sxs-lookup"><span data-stu-id="b89ac-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="b89ac-351">Plusieurs lignes sont autorisées pour un formateur.</span><span class="sxs-lookup"><span data-stu-id="b89ac-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="b89ac-352">Plusieurs lignes pour la même paire formateur-cours ne sont pas autorisées.</span><span class="sxs-lookup"><span data-stu-id="b89ac-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="b89ac-353">Comme l’entité de jointure `Enrollment` définit sa propre clé primaire, des doublons de ce type sont possibles.</span><span class="sxs-lookup"><span data-stu-id="b89ac-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="b89ac-354">Pour éviter ces doublons :</span><span class="sxs-lookup"><span data-stu-id="b89ac-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="b89ac-355">Ajoutez un index unique sur les champs de clé primaire, ou</span><span class="sxs-lookup"><span data-stu-id="b89ac-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="b89ac-356">Configurez `Enrollment` avec une clé primaire composite similaire à `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="b89ac-357">Pour plus d'informations, consultez [Index](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="b89ac-357">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="b89ac-358">Mettre à jour le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="b89ac-358">Update the DB context</span></span>

<span data-ttu-id="b89ac-359">Ajoutez le code en surbrillance suivant à *Data/SchoolContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="b89ac-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="b89ac-360">Le code précédent ajoute les nouvelles entités et configure la clé primaire composite de l’entité `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="b89ac-361">Alternative d’API Fluent aux attributs</span><span class="sxs-lookup"><span data-stu-id="b89ac-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="b89ac-362">La méthode `OnModelCreating` du code précédent utilise l’*API Fluent* pour configurer le comportement d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="b89ac-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="b89ac-363">L’API est appelée « Fluent », car elle est souvent utilisée en enchaînant une série d’appels de méthode en une seule instruction.</span><span class="sxs-lookup"><span data-stu-id="b89ac-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="b89ac-364">Le [code suivant](/ef/core/modeling/#methods-of-configuration) est un exemple de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="b89ac-364">The [following code](/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="b89ac-365">Dans ce didacticiel, l’API Fluent est utilisée uniquement pour le mappage de base de données qui ne peut pas être effectué avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="b89ac-366">Toutefois, l’API Fluent peut spécifier la plupart des règles de mise en forme, de validation et de mappage pouvant être spécifiées à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="b89ac-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="b89ac-367">Certains attributs, tels que `MinimumLength`, ne peuvent pas être appliqués avec l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b89ac-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="b89ac-368">`MinimumLength` ne change pas le schéma. Il applique uniquement une règle de validation de longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="b89ac-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="b89ac-369">Certains développeurs préfèrent utiliser exclusivement l’API Fluent afin de conserver des classes d’entité « propres ».</span><span class="sxs-lookup"><span data-stu-id="b89ac-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="b89ac-370">Vous pouvez combiner des attributs et l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b89ac-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="b89ac-371">Certaines configurations peuvent être effectuées uniquement avec l’API Fluent (spécification d’une clé primaire composite).</span><span class="sxs-lookup"><span data-stu-id="b89ac-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="b89ac-372">Certaines autres peuvent être effectuées uniquement avec des attributs (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="b89ac-373">Voici ce que nous recommandons pour l’utilisation des API Fluent ou des attributs :</span><span class="sxs-lookup"><span data-stu-id="b89ac-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="b89ac-374">Choisissez l’une de ces deux approches.</span><span class="sxs-lookup"><span data-stu-id="b89ac-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="b89ac-375">Dans la mesure du possible, utilisez l’approche choisie de manière cohérente.</span><span class="sxs-lookup"><span data-stu-id="b89ac-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="b89ac-376">Certains des attributs utilisés dans ce didacticiel sont utilisés pour :</span><span class="sxs-lookup"><span data-stu-id="b89ac-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="b89ac-377">La validation uniquement (par exemple, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="b89ac-378">La configuration d’EF Core uniquement (par exemple, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="b89ac-379">La validation et la configuration d’EF Core (par exemple, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="b89ac-380">Pour plus d’informations sur les attributs et l’API Fluent, consultez [Méthodes de configuration](/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="b89ac-380">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="b89ac-381">Diagramme des entités montrant les relations</span><span class="sxs-lookup"><span data-stu-id="b89ac-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="b89ac-382">L’illustration suivante montre le diagramme que les outils EF Power créent pour le modèle School complet.</span><span class="sxs-lookup"><span data-stu-id="b89ac-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramme des entités](complex-data-model/_static/diagram.png)

<span data-ttu-id="b89ac-384">Le diagramme précédent montre :</span><span class="sxs-lookup"><span data-stu-id="b89ac-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="b89ac-385">Plusieurs lignes de relations un-à-plusieurs (1 à \*).</span><span class="sxs-lookup"><span data-stu-id="b89ac-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="b89ac-386">La ligne de relation un-à-zéro-ou-un (1 à 0..1) entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="b89ac-387">La ligne de relation zéro-ou-un-à-plusieurs (0..1 à \*) entre les entités `Instructor` et `Department`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="b89ac-388">Amorcer la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="b89ac-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="b89ac-389">Mettez à jour le code dans *Data/DbInitializer.cs* :</span><span class="sxs-lookup"><span data-stu-id="b89ac-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="b89ac-390">Le code précédent fournit des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="b89ac-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="b89ac-391">La majeure partie de ce code crée des objets d’entités et charge des exemples de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="b89ac-392">Les exemples de données sont utilisés à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="b89ac-392">The sample data is used for testing.</span></span> <span data-ttu-id="b89ac-393">Le code précédent crée les relations plusieurs-à-plusieurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="b89ac-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

## <a name="add-a-migration"></a><span data-ttu-id="b89ac-394">Ajouter une migration</span><span class="sxs-lookup"><span data-stu-id="b89ac-394">Add a migration</span></span>

<span data-ttu-id="b89ac-395">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b89ac-395">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b89ac-396">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b89ac-396">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b89ac-397">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="b89ac-397">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

<span data-ttu-id="b89ac-398">La commande précédente affiche un avertissement concernant les pertes de données possibles.</span><span class="sxs-lookup"><span data-stu-id="b89ac-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="b89ac-399">Si la commande `database update` est exécutée, l’erreur suivante se produit :</span><span class="sxs-lookup"><span data-stu-id="b89ac-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="b89ac-400">Appliquer la migration</span><span class="sxs-lookup"><span data-stu-id="b89ac-400">Apply the migration</span></span>

<span data-ttu-id="b89ac-401">Disposant à présent d’une base de données, vous devez réfléchir à la façon dont vous y apporterez des modifications.</span><span class="sxs-lookup"><span data-stu-id="b89ac-401">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="b89ac-402">Ce tutoriel montre deux approches :</span><span class="sxs-lookup"><span data-stu-id="b89ac-402">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="b89ac-403">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="b89ac-403">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="b89ac-404">[Appliquer la migration à la base de données](#applyexisting)</span><span class="sxs-lookup"><span data-stu-id="b89ac-404">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="b89ac-405">Bien que cette méthode soit plus longue et complexe, elle constitue l’approche privilégiée pour les environnements de production réels.</span><span class="sxs-lookup"><span data-stu-id="b89ac-405">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="b89ac-406">**Remarque** : Cette section du tutoriel est facultative.</span><span class="sxs-lookup"><span data-stu-id="b89ac-406">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="b89ac-407">Vous pouvez effectuer les étapes de suppression et de recréation et ignorer cette section.</span><span class="sxs-lookup"><span data-stu-id="b89ac-407">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="b89ac-408">Si vous souhaitez suivre les étapes décrites dans cette section, n’effectuez pas les étapes de suppression et de recréation.</span><span class="sxs-lookup"><span data-stu-id="b89ac-408">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="b89ac-409">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="b89ac-409">Drop and re-create the database</span></span>

<span data-ttu-id="b89ac-410">Le code dans le `DbInitializer` mis à jour ajoute des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="b89ac-410">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="b89ac-411">Pour forcer EF Core à créer une autre base de données, supprimez et mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="b89ac-411">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b89ac-412">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b89ac-412">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b89ac-413">Dans la **console du Gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b89ac-413">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="b89ac-414">Exécutez `Get-Help about_EntityFrameworkCore` à partir de la console du Gestionnaire de package pour obtenir des informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="b89ac-414">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b89ac-415">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="b89ac-415">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b89ac-416">Ouvrez une fenêtre de commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="b89ac-416">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="b89ac-417">Le dossier du projet contient le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b89ac-417">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="b89ac-418">Entrez ce qui suit dans la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="b89ac-418">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

<span data-ttu-id="b89ac-419">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b89ac-419">Run the app.</span></span> <span data-ttu-id="b89ac-420">L’exécution de l’application entraîne l’exécution de la méthode `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-420">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="b89ac-421">La méthode `DbInitializer.Initialize` remplit la nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-421">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="b89ac-422">Ouvrez la base de données dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="b89ac-422">Open the DB in SSOX:</span></span>

* <span data-ttu-id="b89ac-423">Si SSOX était déjà ouvert, cliquez sur le bouton **Actualiser**.</span><span class="sxs-lookup"><span data-stu-id="b89ac-423">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="b89ac-424">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="b89ac-424">Expand the **Tables** node.</span></span> <span data-ttu-id="b89ac-425">Les tables créées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="b89ac-425">The created tables are displayed.</span></span>

![Tables dans SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="b89ac-427">Examinez la table **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="b89ac-427">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="b89ac-428">Cliquez avec le bouton droit sur la table **CourseAssignment** et sélectionnez **Afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="b89ac-428">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="b89ac-429">Vérifiez que la table **CourseAssignment** contient des données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-429">Verify the **CourseAssignment** table contains data.</span></span>

![Données CourseAssignment dans SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="b89ac-431">Appliquer la migration à la base de données</span><span class="sxs-lookup"><span data-stu-id="b89ac-431">Apply the migration to the existing database</span></span>

<span data-ttu-id="b89ac-432">Cette section est facultative.</span><span class="sxs-lookup"><span data-stu-id="b89ac-432">This section is optional.</span></span> <span data-ttu-id="b89ac-433">Ces étapes fonctionnent seulement si vous avez ignoré la section [Supprimer et recréer la base de données](#drop) précédente.</span><span class="sxs-lookup"><span data-stu-id="b89ac-433">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="b89ac-434">Quand des migrations sont exécutées avec des données existantes, il peut y avoir des contraintes de clé étrangère qui ne sont pas satisfaites avec les données existantes.</span><span class="sxs-lookup"><span data-stu-id="b89ac-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="b89ac-435">Avec des données de production, vous devez effectuer certaines actions pour migrer les données existantes.</span><span class="sxs-lookup"><span data-stu-id="b89ac-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="b89ac-436">Cette section fournit un exemple de correction des violations de contraintes de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="b89ac-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="b89ac-437">N’effectuez pas ces modifications de code sans une sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="b89ac-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="b89ac-438">N’effectuez pas ces modifications de code si vous avez terminé la section précédente et mis à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="b89ac-438">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="b89ac-439">Le fichier *{horodatage}_ComplexDataModel.cs* contient le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b89ac-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="b89ac-440">Le code précédent ajoute une clé étrangère `DepartmentID` non-nullable à la table `Course`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="b89ac-441">La base de données du didacticiel précédent contient des lignes dans `Course` ; cette table ne peut donc pas être mise à jour par des migrations.</span><span class="sxs-lookup"><span data-stu-id="b89ac-441">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="b89ac-442">Pour faire en sorte que la migration `ComplexDataModel` fonctionne avec des données existantes</span><span class="sxs-lookup"><span data-stu-id="b89ac-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="b89ac-443">Modifiez le code pour affecter une valeur par défaut à la nouvelle colonne (`DepartmentID`).</span><span class="sxs-lookup"><span data-stu-id="b89ac-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="b89ac-444">Créez un département fictif nommé « Temp » assumant la fonction de département par défaut.</span><span class="sxs-lookup"><span data-stu-id="b89ac-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="b89ac-445">Corriger les contraintes de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="b89ac-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="b89ac-446">Mettez à jour la méthode `Up` de la classe `ComplexDataModel` :</span><span class="sxs-lookup"><span data-stu-id="b89ac-446">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="b89ac-447">Ouvrez le fichier *{horodatage}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="b89ac-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="b89ac-448">Commentez la ligne de code qui ajoute la colonne `DepartmentID` à la table `Course`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="b89ac-449">Ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="b89ac-449">Add the following highlighted code.</span></span> <span data-ttu-id="b89ac-450">Le nouveau code va après le bloc `.CreateTable( name: "Department"` : [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="b89ac-450">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="b89ac-451">Avec les modifications précédentes, les lignes `Course` existantes seront toutes associées au département « Temp » après l’exécution de la méthode `Up` de `ComplexDataModel`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-451">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="b89ac-452">Une application de production :</span><span class="sxs-lookup"><span data-stu-id="b89ac-452">A production app would:</span></span>

* <span data-ttu-id="b89ac-453">Comprendrait du code ou des scripts pour ajouter des lignes `Department` et des lignes `Course` associées aux nouvelles lignes `Department`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-453">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="b89ac-454">N’utiliserait pas le département « Temp » ou la valeur par défaut pour `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="b89ac-454">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="b89ac-455">Le didacticiel suivant traite des données associées.</span><span class="sxs-lookup"><span data-stu-id="b89ac-455">The next tutorial covers related data.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="b89ac-456">[Précédent](xref:data/ef-rp/migrations)
> [Suivant](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="b89ac-456">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
