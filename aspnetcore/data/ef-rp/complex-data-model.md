---
title: Pages Razor avec EF Core dans ASP.NET Core - Modèle de données - 5 sur 8
author: tdykstra
description: Dans ce tutoriel, vous ajoutez des entités et des relations, et vous personnalisez le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 8a1c0759453b02f4ce1c45471a8f93da626f8261
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583287"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="a2fab-103">Pages Razor avec EF Core dans ASP.NET Core - Modèle de données - 5 sur 8</span><span class="sxs-lookup"><span data-stu-id="a2fab-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="a2fab-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2fab-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2fab-105">Dans les didacticiels précédents, nous avons travaillé avec un modèle de données de base composé de trois entités.</span><span class="sxs-lookup"><span data-stu-id="a2fab-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="a2fab-106">Dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="a2fab-106">In this tutorial:</span></span>

* <span data-ttu-id="a2fab-107">Nous allons ajouter d’autres entités et relations</span><span class="sxs-lookup"><span data-stu-id="a2fab-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="a2fab-108">Nous allons personnaliser le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage de base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="a2fab-109">Le modèle de données final est présenté dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-109">The completed data model is shown in the following illustration:</span></span>

![Diagramme des entités](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="a2fab-111">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="a2fab-111">The Student entity</span></span>

![Entité Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="a2fab-113">Remplacez le code de *MainWindow.xaml.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="a2fab-114">Le code précédent ajoute une propriété `FullName` et les attributs suivants aux propriétés existantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="a2fab-115">Propriété calculée FullName</span><span class="sxs-lookup"><span data-stu-id="a2fab-115">The FullName calculated property</span></span>

<span data-ttu-id="a2fab-116">`FullName` est une propriété calculée qui retourne une valeur créée par concaténation de deux autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="a2fab-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="a2fab-117">`FullName` ne pouvant pas être définie, elle n’a qu’un seul accesseur get.</span><span class="sxs-lookup"><span data-stu-id="a2fab-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="a2fab-118">Aucune colonne `FullName` n’est créée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="a2fab-119">Attribut DataType</span><span class="sxs-lookup"><span data-stu-id="a2fab-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="a2fab-120">Pour les dates d’inscription des étudiants, toutes les pages affichent actuellement l’heure du jour avec la date, alors que seule la date présente un intérêt.</span><span class="sxs-lookup"><span data-stu-id="a2fab-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="a2fab-121">Vous pouvez avoir recours aux attributs d’annotation de données pour apporter une modification au code, permettant de corriger le format d’affichage dans chaque page qui affiche ces données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="a2fab-122">L’attribut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) spécifie un type de données qui est plus spécifique que le type intrinsèque de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="a2fab-123">Ici, seule la date doit être affichée (pas la date et l’heure).</span><span class="sxs-lookup"><span data-stu-id="a2fab-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="a2fab-124">L’énumération [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fournit de nombreux types de données, tels que Date, Time, PhoneNumber, Currency, EmailAddress, et ainsi de suite. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type.</span><span class="sxs-lookup"><span data-stu-id="a2fab-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="a2fab-125">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a2fab-125">For example:</span></span>

* <span data-ttu-id="a2fab-126">Le lien `mailto:` est créé automatiquement pour `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="a2fab-127">Le sélecteur de date est fourni pour `DataType.Date` dans la plupart des navigateurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="a2fab-128">L’attribut `DataType` émet des attributs HTML 5 `data-` (prononcé data dash en anglais).</span><span class="sxs-lookup"><span data-stu-id="a2fab-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="a2fab-129">Les attributs `DataType` ne fournissent aucune validation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="a2fab-130">Attribut DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="a2fab-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="a2fab-131">`DataType.Date` ne spécifie pas le format de la date qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a2fab-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="a2fab-132">Par défaut, le champ de date est affiché conformément aux formats par défaut basés sur l’objet [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) du serveur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="a2fab-133">L’attribut `DisplayFormat` sert à spécifier explicitement le format de date.</span><span class="sxs-lookup"><span data-stu-id="a2fab-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="a2fab-134">Le paramètre `ApplyFormatInEditMode` spécifie que la mise en forme doit également être appliquée à l’interface utilisateur de modification.</span><span class="sxs-lookup"><span data-stu-id="a2fab-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="a2fab-135">Certains champs ne doivent pas utiliser `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="a2fab-136">Par exemple, le symbole monétaire ne doit généralement pas être affiché dans une zone de texte d’édition.</span><span class="sxs-lookup"><span data-stu-id="a2fab-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="a2fab-137">L’attribut `DisplayFormat` peut être utilisé seul.</span><span class="sxs-lookup"><span data-stu-id="a2fab-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="a2fab-138">Il est généralement préférable d’utiliser l’attribut `DataType` avec l’attribut `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="a2fab-139">L’attribut `DataType` transmet la sémantique des données, plutôt que la manière de l’afficher à l’écran.</span><span class="sxs-lookup"><span data-stu-id="a2fab-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="a2fab-140">L’attribut `DataType` offre les avantages suivants qui ne sont pas disponibles dans `DisplayFormat` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="a2fab-141">Le navigateur peut activer des fonctionnalités HTML5</span><span class="sxs-lookup"><span data-stu-id="a2fab-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="a2fab-142">(par exemple, pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, une validation d’entrées côté client).</span><span class="sxs-lookup"><span data-stu-id="a2fab-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="a2fab-143">Par défaut, le navigateur affiche les données à l’aide du format correspondant aux paramètres régionaux.</span><span class="sxs-lookup"><span data-stu-id="a2fab-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="a2fab-144">Pour plus d’informations, consultez la [documentation relative au Tag Helper \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a2fab-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="a2fab-145">Attribut StringLength</span><span class="sxs-lookup"><span data-stu-id="a2fab-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="a2fab-146">Vous pouvez également spécifier des règles de validation de données et des messages d’erreur de validation à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="a2fab-147">L’attribut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) spécifie les longueurs minimale et maximale de caractères autorisées dans un champ de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="a2fab-148">Le code présenté limite la longueur des noms à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="a2fab-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="a2fab-149">Un exemple qui définit la longueur de chaîne minimale est présenté [plus loin](#the-required-attribute).</span><span class="sxs-lookup"><span data-stu-id="a2fab-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="a2fab-150">L’attribut `StringLength` fournit également la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="a2fab-151">La valeur minimale n’a aucun impact sur le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="a2fab-152">L’attribut `StringLength` n’empêche pas un utilisateur d’entrer un espace blanc comme nom.</span><span class="sxs-lookup"><span data-stu-id="a2fab-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="a2fab-153">L’attribut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) peut être utilisé pour appliquer des restrictions à l’entrée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="a2fab-154">Par exemple, le code suivant exige que le premier caractère soit en majuscule et que les autres caractères soient alphabétiques :</span><span class="sxs-lookup"><span data-stu-id="a2fab-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2fab-156">Dans **l’Explorateur d’objets SQL Server** (SSOX), ouvrez le concepteur de tables Student en double-cliquant sur la table **Student**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Table Students dans SSOX avant les migrations](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="a2fab-158">L’image précédente montre le schéma pour la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="a2fab-159">Les champs de nom sont de type `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="a2fab-160">Quand une migration est créée et appliquée plus loin dans ce tutoriel, les champs de nom deviennent `nvarchar(50)` en raison des attributs de longueur de chaîne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a2fab-162">Dans votre outil SQLite, examinez les définitions de colonne pour la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="a2fab-163">Les champs de nom sont de type `Text`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-163">The name fields have type `Text`.</span></span> <span data-ttu-id="a2fab-164">Notez que le champ de prénom (« first name ») est appelé `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="a2fab-165">Dans la section suivante, vous allez remplacer le nom de cette colonne par `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="a2fab-166">Attribut Column</span><span class="sxs-lookup"><span data-stu-id="a2fab-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="a2fab-167">Les attributs peuvent contrôler comment les classes et les propriétés sont mappées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="a2fab-168">Dans le modèle `Student`, l’attribut `Column` sert à mapper le nom de la propriété `FirstMidName` à « FirstName » dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="a2fab-169">Pendant la création de la base de données, les noms de propriétés du modèle sont utilisés comme noms de colonnes (sauf quand l’attribut `Column` est utilisé).</span><span class="sxs-lookup"><span data-stu-id="a2fab-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="a2fab-170">Le modèle `Student` utilise `FirstMidName` pour le champ de prénom, car le champ peut également contenir un deuxième prénom.</span><span class="sxs-lookup"><span data-stu-id="a2fab-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="a2fab-171">Avec l’attribut `[Column]`, dans le modèle de données, `Student.FirstMidName` est mappé à la colonne `FirstName` de la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="a2fab-172">L’ajout de l’attribut `Column` change le modèle sur lequel repose `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="a2fab-173">Le modèle sur lequel repose le `SchoolContext` ne correspond plus à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="a2fab-174">Cette différence sera résolue en ajoutant une migration plus loin dans ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a2fab-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="a2fab-175">Attribut Required</span><span class="sxs-lookup"><span data-stu-id="a2fab-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="a2fab-176">L’attribut `Required` fait des propriétés de nom des champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="a2fab-177">L’attribut `Required` n’est pas nécessaire pour les types qui n’autorisent pas les valeurs Null comme les types valeur (par exemple, `DateTime`, `int` et `double`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="a2fab-178">Les types qui n’acceptent pas les valeurs Null sont traités automatiquement comme des champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="a2fab-179">L’attribut `Required` peut être remplacé par un paramètre de longueur minimale dans l’attribut `StringLength` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-179">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="a2fab-180">Attribut Display</span><span class="sxs-lookup"><span data-stu-id="a2fab-180">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="a2fab-181">L’attribut `Display` indique que la légende des zones de texte doit être « First Name », « Last Name », « Full Name » et « Enrollment Date ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-181">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="a2fab-182">Les légendes par défaut ne comportaient pas d’espace entre les mots, par exemple « Lastname ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-182">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="a2fab-183">Créer une migration</span><span class="sxs-lookup"><span data-stu-id="a2fab-183">Create a migration</span></span>

<span data-ttu-id="a2fab-184">Exécutez l’application et accédez à la page des étudiants.</span><span class="sxs-lookup"><span data-stu-id="a2fab-184">Run the app and go to the Students page.</span></span> <span data-ttu-id="a2fab-185">Une exception est levée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-185">An exception is thrown.</span></span> <span data-ttu-id="a2fab-186">En raison de l’attribut `[Column]`, EF s’attend à trouver une colonne nommée `FirstName`, mais le nom de la colonne dans la base de données est toujours `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-186">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-187">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2fab-188">Le message d’erreur est semblable à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-188">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="a2fab-189">Dans PMC, entrez les commandes suivantes pour créer une migration et mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-189">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="a2fab-190">La première de ces commandes génère le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-190">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="a2fab-191">Cet avertissement est généré, car les champs de nom sont désormais limités à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="a2fab-191">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="a2fab-192">Si la base de données comporte un nom de plus 50 caractères, tous les caractères au-delà du cinquantième sont perdus.</span><span class="sxs-lookup"><span data-stu-id="a2fab-192">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="a2fab-193">Ouvrez la table Student dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="a2fab-193">Open the Student table in SSOX:</span></span>

  ![Table Students dans SSOX après les migrations](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="a2fab-195">Avant l’application de la migration, les colonnes de noms étaient de type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="a2fab-195">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="a2fab-196">Les colonnes de nom sont maintenant `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-196">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="a2fab-197">Le nom de la colonne est passé de `FirstMidName` à `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-197">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a2fab-199">Le message d’erreur est semblable à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-199">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="a2fab-200">Ouvrez une fenêtre de commande dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-200">Open a command window in the project folder.</span></span> <span data-ttu-id="a2fab-201">Entrez les commandes suivantes pour créer une migration et mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-201">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="a2fab-202">La commande de mise à jour de base de données affiche une erreur semblable à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-202">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="a2fab-203">Pour ce tutoriel, la façon de passer cette erreur consiste à supprimer et à recréer la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="a2fab-203">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="a2fab-204">Pour plus d’informations, consultez la note d’avertissement SQLite au début du [tutoriel sur les migrations](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="a2fab-204">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="a2fab-205">Supprimez le dossier *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-205">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="a2fab-206">Exécutez les commandes suivantes pour supprimer la base de données, créer une migration initiale et appliquer la migration :</span><span class="sxs-lookup"><span data-stu-id="a2fab-206">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="a2fab-207">Examinez la table Student dans votre outil SQLite.</span><span class="sxs-lookup"><span data-stu-id="a2fab-207">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="a2fab-208">La colonne FirstMidName est désormais FirstName.</span><span class="sxs-lookup"><span data-stu-id="a2fab-208">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="a2fab-209">Exécutez l’application et accédez à la page des étudiants.</span><span class="sxs-lookup"><span data-stu-id="a2fab-209">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="a2fab-210">Notez que les heures ne sont pas entrées ou affichées avec les dates.</span><span class="sxs-lookup"><span data-stu-id="a2fab-210">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="a2fab-211">Sélectionnez **Create New** et essayez d’entrer un nom de plus de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="a2fab-211">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="a2fab-212">Dans les sections suivantes, la génération de l’application à certaines étapes génère des erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-212">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="a2fab-213">Les instructions indiquent quand générer l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-213">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="a2fab-214">Entité Instructor</span><span class="sxs-lookup"><span data-stu-id="a2fab-214">The Instructor Entity</span></span>

![Entité Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="a2fab-216">Créez *Models/Instructor.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-216">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="a2fab-217">Plusieurs attributs peuvent être sur une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-217">Multiple attributes can be on one line.</span></span> <span data-ttu-id="a2fab-218">Les attributs `HireDate` peuvent être écrits comme suit :</span><span class="sxs-lookup"><span data-stu-id="a2fab-218">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="a2fab-219">Propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-219">Navigation properties</span></span>

<span data-ttu-id="a2fab-220">Les propriétés `CourseAssignments` et `OfficeAssignment` sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-220">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="a2fab-221">Un formateur pouvant animer un nombre quelconque de cours, `CourseAssignments` est défini comme une collection.</span><span class="sxs-lookup"><span data-stu-id="a2fab-221">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="a2fab-222">Un formateur ne pouvant avoir au plus un bureau, la propriété `OfficeAssignment` contient une seule entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-222">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="a2fab-223">`OfficeAssignment` a la valeur Null si aucun bureau n’est affecté.</span><span class="sxs-lookup"><span data-stu-id="a2fab-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="a2fab-224">Entité OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="a2fab-224">The OfficeAssignment entity</span></span>

![Entité OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="a2fab-226">Créez *Models/OfficeAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="a2fab-227">Attribut Key</span><span class="sxs-lookup"><span data-stu-id="a2fab-227">The Key attribute</span></span>

<span data-ttu-id="a2fab-228">L’attribut `[Key]` est utilisé pour identifier une propriété comme clé primaire quand le nom de la propriété est autre que classnameID ou ID.</span><span class="sxs-lookup"><span data-stu-id="a2fab-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="a2fab-229">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="a2fab-230">Une affectation de bureau existe uniquement relativement au formateur auquel elle est assignée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="a2fab-231">La clé primaire `OfficeAssignment` est également sa clé étrangère pour l’entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="a2fab-232">EF Core ne peut pas reconnaître automatiquement `InstructorID` comme clé primaire de `OfficeAssignment`, car `InstructorID` ne respecte pas la convention d’affectation de noms d’ID ou de classnameID.</span><span class="sxs-lookup"><span data-stu-id="a2fab-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="a2fab-233">Ainsi, l’attribut `Key` est utilisé pour identifier `InstructorID` comme clé primaire :</span><span class="sxs-lookup"><span data-stu-id="a2fab-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="a2fab-234">Par défaut, EF Core traite la clé comme n’étant pas générée par la base de données, car la colonne est utilisée pour une relation d’identification.</span><span class="sxs-lookup"><span data-stu-id="a2fab-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="a2fab-235">Propriété de navigation Instructor</span><span class="sxs-lookup"><span data-stu-id="a2fab-235">The Instructor navigation property</span></span>

<span data-ttu-id="a2fab-236">La propriété de navigation `Instructor.OfficeAssignment` peut avoir la valeur null, car il n’est pas certain qu’il existe une ligne `OfficeAssignment` pour un formateur donné.</span><span class="sxs-lookup"><span data-stu-id="a2fab-236">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="a2fab-237">Un formateur peut ne pas avoir d’affectation de bureau.</span><span class="sxs-lookup"><span data-stu-id="a2fab-237">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="a2fab-238">La propriété de navigation `OfficeAssignment.Instructor` aura toujours une entité Instructor, car le type `InstructorID` de clé étrangère est `int`, type valeur qui n’autorise pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="a2fab-238">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="a2fab-239">Une affectation de bureau ne peut pas exister sans un formateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-239">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="a2fab-240">Quand une entité `Instructor` a une entité `OfficeAssignment` associée, chaque entité a une référence à l’autre dans sa propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-240">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="a2fab-241">Entité Course</span><span class="sxs-lookup"><span data-stu-id="a2fab-241">The Course Entity</span></span>

![Entité Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="a2fab-243">Mettez à jour *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-243">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="a2fab-244">L’entité `Course` a une propriété de clé étrangère `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-244">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="a2fab-245">`DepartmentID` pointe vers l’entité `Department` associée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-245">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="a2fab-246">L’entité `Course` a une propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-246">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="a2fab-247">EF Core n’exige pas de propriété de clé étrangère pour un modèle de données quand le modèle a une propriété de navigation pour une entité associée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-247">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="a2fab-248">EF Core crée automatiquement des clés étrangères dans la base de données partout où elles sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-248">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="a2fab-249">EF Core crée des [propriétés cachées](/ef/core/modeling/shadow-properties) pour les clés étrangères créées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a2fab-249">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="a2fab-250">Cependant, le fait d’inclure la clé étrangère dans le modèle de données peut rendre les mises à jour plus simples et plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="a2fab-250">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="a2fab-251">Par exemple, considérez un modèle où la propriété de clé étrangère `DepartmentID` n’est *pas* incluse.</span><span class="sxs-lookup"><span data-stu-id="a2fab-251">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="a2fab-252">Quand une entité Course est récupérée en vue d’une modification :</span><span class="sxs-lookup"><span data-stu-id="a2fab-252">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="a2fab-253">La propriété `Department` a la valeur Null si elle n’est pas chargée explicitement.</span><span class="sxs-lookup"><span data-stu-id="a2fab-253">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="a2fab-254">Pour mettre à jour l’entité Course, vous devez d’abord récupérer l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-254">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="a2fab-255">Quand la propriété de clé étrangère `DepartmentID` est incluse dans le modèle de données, il n’est pas nécessaire de récupérer l’entité `Department` avant une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="a2fab-255">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="a2fab-256">Attribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="a2fab-256">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="a2fab-257">L’attribut `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indique que la clé primaire est fournie par l’application plutôt que générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-257">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="a2fab-258">Par défaut, EF Core part du principe que les valeurs de clé primaire sont générées par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-258">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="a2fab-259">La génération par la base de données est généralement la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="a2fab-259">Database-generated is generally the best approach.</span></span> <span data-ttu-id="a2fab-260">Pour les entités `Course`, l’utilisateur spécifie la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a2fab-260">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="a2fab-261">Par exemple, un numéro de cours comme une série 1000 pour le département Mathématiques et une série 2000 pour le département Anglais.</span><span class="sxs-lookup"><span data-stu-id="a2fab-261">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="a2fab-262">Vous pouvez aussi utiliser l’attribut `DatabaseGenerated` pour générer des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="a2fab-262">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="a2fab-263">Par exemple, la base de données peut générer automatiquement un champ de date pour enregistrer la date de création ou de mise à jour d’une ligne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-263">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="a2fab-264">Pour plus d’informations, consultez [Propriétés générées](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="a2fab-264">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a2fab-265">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="a2fab-266">Les propriétés de clé étrangère et les propriétés de navigation dans l’entité `Course` reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-266">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="a2fab-267">Un cours étant affecté à un seul département, il y a une clé étrangère `DepartmentID` et une propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-267">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="a2fab-268">Un cours pouvant avoir un nombre quelconque d’étudiants inscrits, la propriété de navigation `Enrollments` est une collection :</span><span class="sxs-lookup"><span data-stu-id="a2fab-268">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="a2fab-269">Un cours pouvant être animé par plusieurs formateurs, la propriété de navigation `CourseAssignments` est une collection :</span><span class="sxs-lookup"><span data-stu-id="a2fab-269">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="a2fab-270">`CourseAssignment` est expliquée [plus loin](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="a2fab-270">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="a2fab-271">Entité Department</span><span class="sxs-lookup"><span data-stu-id="a2fab-271">The Department entity</span></span>

![Entité Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="a2fab-273">Créez *Models/Department.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-273">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="a2fab-274">Attribut Column</span><span class="sxs-lookup"><span data-stu-id="a2fab-274">The Column attribute</span></span>

<span data-ttu-id="a2fab-275">Précédemment, vous avez utilisé l’attribut `Column` pour changer le mappage des noms de colonne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-275">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="a2fab-276">Dans le code de l’entité `Department`, l’attribut `Column` est utilisé pour changer le mappage des types de données SQL.</span><span class="sxs-lookup"><span data-stu-id="a2fab-276">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="a2fab-277">La colonne `Budget` est définie à l’aide du type monétaire SQL Server dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-277">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="a2fab-278">Le mappage de colonnes n’est généralement pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a2fab-278">Column mapping is generally not required.</span></span> <span data-ttu-id="a2fab-279">EF Core choisit le type de données SQL Server approprié en fonction du type CLR de la propriété.</span><span class="sxs-lookup"><span data-stu-id="a2fab-279">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="a2fab-280">Le type CLR `decimal` est mappé à un type SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-280">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="a2fab-281">`Budget` étant une valeur monétaire, le type de données money est plus approprié.</span><span class="sxs-lookup"><span data-stu-id="a2fab-281">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a2fab-282">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-282">Foreign key and navigation properties</span></span>

<span data-ttu-id="a2fab-283">Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-283">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="a2fab-284">Un département peut avoir ou ne pas avoir un administrateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-284">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="a2fab-285">Un administrateur est toujours un formateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-285">An administrator is always an instructor.</span></span> <span data-ttu-id="a2fab-286">Ainsi, la propriété `InstructorID` est incluse en tant que clé étrangère de l’entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-286">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="a2fab-287">La propriété de navigation se nomme `Administrator`, mais elle contient une entité `Instructor` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-287">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="a2fab-288">Le point d’interrogation (?) dans le code précédent indique que la propriété est nullable.</span><span class="sxs-lookup"><span data-stu-id="a2fab-288">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="a2fab-289">Un département peut avoir de nombreux cours, si bien qu’il existe une propriété de navigation Courses :</span><span class="sxs-lookup"><span data-stu-id="a2fab-289">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="a2fab-290">Par convention, EF Core autorise la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs à plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-290">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="a2fab-291">Ce comportement par défaut peut engendrer des règles de suppression en cascade circulaires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-291">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="a2fab-292">Les règles de suppression en cascade circulaires provoquent une exception quand une migration est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-292">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="a2fab-293">Par exemple, si la propriété `Department.InstructorID` a été définie comme n’acceptant pas les valeurs Null, EF Core configure une règle de suppression en cascade.</span><span class="sxs-lookup"><span data-stu-id="a2fab-293">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="a2fab-294">Dans ce cas, le service est supprimé quand le formateur désigné comme étant son administrateur est supprimé.</span><span class="sxs-lookup"><span data-stu-id="a2fab-294">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="a2fab-295">Dans ce scénario, une règle de restriction est plus logique.</span><span class="sxs-lookup"><span data-stu-id="a2fab-295">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="a2fab-296">L’API Fluent suivante définit une règle de restriction et désactive la suppression en cascade.</span><span class="sxs-lookup"><span data-stu-id="a2fab-296">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="a2fab-297">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="a2fab-297">The Enrollment entity</span></span>

<span data-ttu-id="a2fab-298">Un enregistrement d’inscription correspond à un cours suivi par un étudiant.</span><span class="sxs-lookup"><span data-stu-id="a2fab-298">An enrollment record is for one course taken by one student.</span></span>

![Entité Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="a2fab-300">Mettez à jour *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-300">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a2fab-301">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-301">Foreign key and navigation properties</span></span>

<span data-ttu-id="a2fab-302">Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-302">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="a2fab-303">Un enregistrement d’inscription ne concernant qu’un seul cours, il y a une propriété de clé étrangère `CourseID` et une propriété de navigation `Course` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-303">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="a2fab-304">Un enregistrement d’inscription ne concernant qu’un seul étudiant, il y a une propriété de clé étrangère `StudentID` et une propriété de navigation `Student` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-304">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="a2fab-305">Relations plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="a2fab-305">Many-to-Many Relationships</span></span>

<span data-ttu-id="a2fab-306">Il existe une relation plusieurs-à-plusieurs entre les entités `Student` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-306">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="a2fab-307">L’entité `Enrollment` joue le rôle de table de jointure plusieurs-à-plusieurs *avec charge utile* dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-307">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="a2fab-308">« Avec charge utile » signifie que la table `Enrollment` contient des données supplémentaires en plus des clés étrangères pour les tables jointes (dans le cas présent, la clé primaire et `Grade`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-308">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="a2fab-309">L’illustration suivante montre à quoi ressemblent ces relations dans un diagramme d’entité.</span><span class="sxs-lookup"><span data-stu-id="a2fab-309">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="a2fab-310">(Ce diagramme a été généré à l’aide de [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) pour EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="a2fab-310">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="a2fab-311">Sa création ne fait pas partie de ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="a2fab-311">Creating the diagram isn't part of the tutorial.)</span></span>

![Relation plusieurs à plusieurs Student-Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="a2fab-313">Chaque ligne de relation comporte un 1 à une extrémité et un astérisque (\*) à l’autre, ce qui indique une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-313">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="a2fab-314">Si la table `Enrollment` n’incluait pas d’informations de notes, elle aurait uniquement besoin de contenir les deux clés étrangères (`CourseID` et `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-314">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="a2fab-315">Une table de jointure plusieurs-à-plusieurs sans charge utile est parfois appelée « table de jointure pure ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-315">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="a2fab-316">Les entités `Instructor` et `Course` ont une relation plusieurs-à-plusieurs à l’aide d’une table de jointure pure.</span><span class="sxs-lookup"><span data-stu-id="a2fab-316">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="a2fab-317">Remarque : EF 6.x prend en charge les tables de jointure implicites pour les relations plusieurs-à-plusieurs, mais EF Core ne le fait pas.</span><span class="sxs-lookup"><span data-stu-id="a2fab-317">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="a2fab-318">Pour plus d’informations, consultez [Many-to-many relationships in EF Core 2.0 (Relations plusieurs-à-plusieurs dans EF Core 2.0)](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="a2fab-318">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="a2fab-319">Entité CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="a2fab-319">The CourseAssignment entity</span></span>

![Entité CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="a2fab-321">Créez *Models/CourseAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-321">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="a2fab-322">La relation « plusieurs-à-plusieurs » entre le formateur et les cours nécessite une table de jointure, et l’entité pour cette table de jointure est CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="a2fab-322">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="a2fab-324">Il est courant de nommer une entité de jointure `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-324">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="a2fab-325">Par exemple, la table de jointure Instructor-to-Courses utilisant ce modèle est `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-325">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="a2fab-326">Toutefois, nous vous recommandons d’utiliser un nom qui décrit la relation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-326">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="a2fab-327">Les modèles de données sont simples au début, puis ils augmentent en complexité.</span><span class="sxs-lookup"><span data-stu-id="a2fab-327">Data models start out simple and grow.</span></span> <span data-ttu-id="a2fab-328">Les tables de jointure sans charge utile évoluent fréquemment pour inclure une charge utile.</span><span class="sxs-lookup"><span data-stu-id="a2fab-328">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="a2fab-329">En commençant par un nom d’entité descriptif, vous n’aurez pas besoin de modifier le nom quand la table de jointure changera.</span><span class="sxs-lookup"><span data-stu-id="a2fab-329">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="a2fab-330">Dans l’idéal, l’entité de jointure aura son propre nom (éventuellement un mot unique) naturel dans le domaine d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="a2fab-330">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="a2fab-331">Par exemple, les livres et les clients pourraient être liés avec une entité de jointure nommée Évaluations.</span><span class="sxs-lookup"><span data-stu-id="a2fab-331">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="a2fab-332">Pour la relation plusieurs-à-plusieurs Instructor-Courses, il vaut mieux utiliser `CourseAssignment` que `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-332">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="a2fab-333">Clé composite</span><span class="sxs-lookup"><span data-stu-id="a2fab-333">Composite key</span></span>

<span data-ttu-id="a2fab-334">Ensemble, le deux clés étrangères dans `CourseAssignment` (`InstructorID` et `CourseID`) identifient de façon unique chaque ligne de la table `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-334">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="a2fab-335">`CourseAssignment` ne nécessite pas de clé primaire dédiée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-335">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="a2fab-336">Les propriétés `InstructorID` et `CourseID` fonctionnent comme une clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="a2fab-336">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="a2fab-337">Le seul moyen de spécifier des clés primaires composites dans EF Core consiste à faire appel à l’*API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-337">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="a2fab-338">La section suivante montre comment configurer la clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="a2fab-338">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="a2fab-339">La clé composite garantit que :</span><span class="sxs-lookup"><span data-stu-id="a2fab-339">The composite key ensures that:</span></span>

* <span data-ttu-id="a2fab-340">Plusieurs lignes sont autorisées pour un cours.</span><span class="sxs-lookup"><span data-stu-id="a2fab-340">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="a2fab-341">Plusieurs lignes sont autorisées pour un formateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-341">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="a2fab-342">Plusieurs lignes ne sont pas autorisées pour la même paire formateur-cours.</span><span class="sxs-lookup"><span data-stu-id="a2fab-342">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="a2fab-343">Comme l’entité de jointure `Enrollment` définit sa propre clé primaire, des doublons de ce type sont possibles.</span><span class="sxs-lookup"><span data-stu-id="a2fab-343">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="a2fab-344">Pour éviter ces doublons :</span><span class="sxs-lookup"><span data-stu-id="a2fab-344">To prevent such duplicates:</span></span>

* <span data-ttu-id="a2fab-345">Ajoutez un index unique sur les champs de clé primaire, ou</span><span class="sxs-lookup"><span data-stu-id="a2fab-345">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="a2fab-346">Configurez `Enrollment` avec une clé primaire composite similaire à `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-346">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="a2fab-347">Pour plus d’informations, consultez [Index](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="a2fab-347">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="a2fab-348">Mettre à jour le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-348">Update the database context</span></span>

<span data-ttu-id="a2fab-349">Mettez à jour *Data/SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-349">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="a2fab-350">Le code précédent ajoute les nouvelles entités et configure la clé primaire composite de l’entité `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-350">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="a2fab-351">Alternative d’API Fluent aux attributs</span><span class="sxs-lookup"><span data-stu-id="a2fab-351">Fluent API alternative to attributes</span></span>

<span data-ttu-id="a2fab-352">La méthode `OnModelCreating` du code précédent utilise l’*API Fluent* pour configurer le comportement d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2fab-352">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="a2fab-353">L’API est appelée « Fluent », car elle est souvent utilisée en enchaînant une série d’appels de méthode en une seule instruction.</span><span class="sxs-lookup"><span data-stu-id="a2fab-353">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="a2fab-354">Le [code suivant](/ef/core/modeling/#use-fluent-api-to-configure-a-model) est un exemple de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="a2fab-354">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="a2fab-355">Dans ce tutoriel, l’API Fluent est utilisée uniquement pour le mappage de base de données qui ne peut pas être effectué avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-355">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="a2fab-356">Toutefois, l’API Fluent peut spécifier la plupart des règles de mise en forme, de validation et de mappage pouvant être spécifiées à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-356">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="a2fab-357">Certains attributs, tels que `MinimumLength`, ne peuvent pas être appliqués avec l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a2fab-357">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="a2fab-358">`MinimumLength` ne change pas le schéma. Il applique uniquement une règle de validation de longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="a2fab-358">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="a2fab-359">Certains développeurs préfèrent utiliser exclusivement l’API Fluent afin de conserver des classes d’entité « propres ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-359">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="a2fab-360">Vous pouvez combiner des attributs et l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a2fab-360">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="a2fab-361">Certaines configurations peuvent être effectuées uniquement avec l’API Fluent (spécification d’une clé primaire composite).</span><span class="sxs-lookup"><span data-stu-id="a2fab-361">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="a2fab-362">Certaines autres peuvent être effectuées uniquement avec des attributs (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-362">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="a2fab-363">Voici ce que nous recommandons pour l’utilisation des API Fluent ou des attributs :</span><span class="sxs-lookup"><span data-stu-id="a2fab-363">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="a2fab-364">Choisissez l’une de ces deux approches.</span><span class="sxs-lookup"><span data-stu-id="a2fab-364">Choose one of these two approaches.</span></span>
* <span data-ttu-id="a2fab-365">Dans la mesure du possible, utilisez l’approche choisie de manière cohérente.</span><span class="sxs-lookup"><span data-stu-id="a2fab-365">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="a2fab-366">Certains des attributs utilisés dans ce tutoriel sont utilisés pour :</span><span class="sxs-lookup"><span data-stu-id="a2fab-366">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="a2fab-367">La validation uniquement (par exemple, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-367">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="a2fab-368">La configuration d’EF Core uniquement (par exemple, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-368">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="a2fab-369">La validation et la configuration d’EF Core (par exemple, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-369">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="a2fab-370">Pour plus d’informations sur les attributs et l’API Fluent, consultez [Méthodes de configuration](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="a2fab-370">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="a2fab-371">Diagramme des entités</span><span class="sxs-lookup"><span data-stu-id="a2fab-371">Entity diagram</span></span>

<span data-ttu-id="a2fab-372">L’illustration suivante montre le diagramme que les outils EF Power créent pour le modèle School complet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-372">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramme des entités](complex-data-model/_static/diagram.png)

<span data-ttu-id="a2fab-374">Le diagramme précédent montre :</span><span class="sxs-lookup"><span data-stu-id="a2fab-374">The preceding diagram shows:</span></span>

* <span data-ttu-id="a2fab-375">Plusieurs lignes de relations un-à-plusieurs (1 à \*).</span><span class="sxs-lookup"><span data-stu-id="a2fab-375">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="a2fab-376">La ligne de relation un-à-zéro-ou-un (1 à 0..1) entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-376">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="a2fab-377">La ligne de relation zéro-ou-un-à-plusieurs (0..1 à \*) entre les entités `Instructor` et `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-377">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="a2fab-378">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-378">Seed the database</span></span>

<span data-ttu-id="a2fab-379">Mettez à jour le code dans *Data/DbInitializer.cs* :</span><span class="sxs-lookup"><span data-stu-id="a2fab-379">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="a2fab-380">Le code précédent fournit des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="a2fab-380">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="a2fab-381">La majeure partie de ce code crée des objets d’entités et charge des exemples de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-381">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="a2fab-382">Les exemples de données sont utilisés à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="a2fab-382">The sample data is used for testing.</span></span> <span data-ttu-id="a2fab-383">Consultez `Enrollments` et `CourseAssignments` pour obtenir des exemples de la façon dont des tables de jointure plusieurs-à-plusieurs peuvent être amorcées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-383">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="a2fab-384">Ajouter une migration</span><span class="sxs-lookup"><span data-stu-id="a2fab-384">Add a migration</span></span>

<span data-ttu-id="a2fab-385">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-385">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-386">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2fab-387">Dans PMC, exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="a2fab-387">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="a2fab-388">La commande précédente affiche un avertissement concernant les pertes de données possibles.</span><span class="sxs-lookup"><span data-stu-id="a2fab-388">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="a2fab-389">Si la commande `database update` est exécutée, l’erreur suivante se produit :</span><span class="sxs-lookup"><span data-stu-id="a2fab-389">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="a2fab-390">Dans la section suivante, vous allez voir comment traiter cette erreur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-390">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-391">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a2fab-392">Si vous ajoutez une migration et exécutez la commande `database update`, l’erreur suivante se produit :</span><span class="sxs-lookup"><span data-stu-id="a2fab-392">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="a2fab-393">Dans la section suivante, vous allez découvrir comment éviter cette erreur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-393">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="a2fab-394">Appliquer la migration ou supprimer et recréer</span><span class="sxs-lookup"><span data-stu-id="a2fab-394">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="a2fab-395">Maintenant que vous disposez d’une base de données, vous devez réfléchir à la façon dont vous y apporterez des modifications.</span><span class="sxs-lookup"><span data-stu-id="a2fab-395">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="a2fab-396">Ce tutoriel présente deux autres solutions :</span><span class="sxs-lookup"><span data-stu-id="a2fab-396">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="a2fab-397">[Supprimer et recréer la base de données](#drop).</span><span class="sxs-lookup"><span data-stu-id="a2fab-397">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="a2fab-398">Choisissez cette section si vous utilisez SQLite.</span><span class="sxs-lookup"><span data-stu-id="a2fab-398">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="a2fab-399">[Appliquer la migration à la base de données](#applyexisting)</span><span class="sxs-lookup"><span data-stu-id="a2fab-399">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="a2fab-400">Les instructions de cette section valent uniquement pour SQL Server, **pas pour SQLite**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-400">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="a2fab-401">Les deux options fonctionnent pour SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a2fab-401">Either choice works for SQL Server.</span></span> <span data-ttu-id="a2fab-402">Bien que la méthode d’application de la migration soit plus longue et complexe, il s’agit de l’approche privilégiée pour les environnements de production réels.</span><span class="sxs-lookup"><span data-stu-id="a2fab-402">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="a2fab-403">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-403">Drop and re-create the database</span></span>

<span data-ttu-id="a2fab-404">[Ignorez cette section](#apply-the-migration) si vous utilisez SQL Server et que vous souhaitez effectuer l’approche d’application de la migration dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="a2fab-404">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="a2fab-405">Pour forcer EF Core à créer une base de données, supprimez et mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-405">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a2fab-407">Dans la **console du Gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-407">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="a2fab-408">Supprimez le dossier *Migrations*, puis exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-408">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-409">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-409">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a2fab-410">Ouvrez une fenêtre de commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-410">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="a2fab-411">Le dossier de projet contient le fichier *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-411">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="a2fab-412">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-412">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="a2fab-413">Supprimez le dossier *Migrations*, puis exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-413">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="a2fab-414">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-414">Run the app.</span></span> <span data-ttu-id="a2fab-415">L’exécution de l’application entraîne l’exécution de la méthode `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="a2fab-416">La méthode `DbInitializer.Initialize` remplit la nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-416">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-417">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-417">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2fab-418">Ouvrez la base de données dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="a2fab-418">Open the database in SSOX:</span></span>

* <span data-ttu-id="a2fab-419">Si SSOX était déjà ouvert, cliquez sur le bouton **Actualiser**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-419">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="a2fab-420">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-420">Expand the **Tables** node.</span></span> <span data-ttu-id="a2fab-421">Les tables créées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-421">The created tables are displayed.</span></span>

  ![Tables dans SSOX](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="a2fab-423">Examinez la table **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="a2fab-423">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="a2fab-424">Cliquez avec le bouton droit sur la table **CourseAssignment** et sélectionnez **Afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-424">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="a2fab-425">Vérifiez que la table **CourseAssignment** contient des données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-425">Verify the **CourseAssignment** table contains data.</span></span>

  ![Données CourseAssignment dans SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-427">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-427">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a2fab-428">Utilisez votre outil SQLite pour examiner la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-428">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="a2fab-429">Nouvelles tables et colonnes.</span><span class="sxs-lookup"><span data-stu-id="a2fab-429">New tables and columns.</span></span>
* <span data-ttu-id="a2fab-430">Données amorcées dans des tables, par exemple la table **CourseAssignment**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-430">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="a2fab-431">Appliquer la migration</span><span class="sxs-lookup"><span data-stu-id="a2fab-431">Apply the migration</span></span>

<span data-ttu-id="a2fab-432">Cette section est facultative.</span><span class="sxs-lookup"><span data-stu-id="a2fab-432">This section is optional.</span></span> <span data-ttu-id="a2fab-433">Ces étapes fonctionnent uniquement pour la Base de données locale SQL Server et seulement si vous avez ignoré la section précédente [Supprimer et recréer la base de données](#drop).</span><span class="sxs-lookup"><span data-stu-id="a2fab-433">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="a2fab-434">Quand des migrations sont exécutées avec des données existantes, il peut y avoir des contraintes de clé étrangère qui ne sont pas satisfaites avec les données existantes.</span><span class="sxs-lookup"><span data-stu-id="a2fab-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="a2fab-435">Avec des données de production, vous devez effectuer certaines actions pour migrer les données existantes.</span><span class="sxs-lookup"><span data-stu-id="a2fab-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="a2fab-436">Cette section fournit un exemple de correction des violations de contraintes de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="a2fab-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="a2fab-437">N’effectuez pas ces modifications de code sans une sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="a2fab-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="a2fab-438">N’apportez pas ces modifications au code si vous avez terminé la section précédente [Supprimer et recréer la base de données](#drop).</span><span class="sxs-lookup"><span data-stu-id="a2fab-438">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="a2fab-439">Le fichier *{horodatage}_ComplexDataModel.cs* contient le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="a2fab-440">Le code précédent ajoute une clé étrangère `DepartmentID` non-nullable à la table `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="a2fab-441">La base de données du tutoriel précédent contient des lignes dans `Course` ; cette table ne peut donc pas être mise à jour par des migrations.</span><span class="sxs-lookup"><span data-stu-id="a2fab-441">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="a2fab-442">Pour faire en sorte que la migration `ComplexDataModel` fonctionne avec des données existantes</span><span class="sxs-lookup"><span data-stu-id="a2fab-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="a2fab-443">Modifiez le code pour affecter une valeur par défaut à la nouvelle colonne (`DepartmentID`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="a2fab-444">Créez un département fictif nommé « Temp » assumant la fonction de département par défaut.</span><span class="sxs-lookup"><span data-stu-id="a2fab-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="a2fab-445">Corriger les contraintes de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="a2fab-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="a2fab-446">Dans la classe de migration `ComplexDataModel`, mettez à jour la méthode `Up` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-446">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="a2fab-447">Ouvrez le fichier *{horodatage}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="a2fab-448">Commentez la ligne de code qui ajoute la colonne `DepartmentID` à la table `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="a2fab-449">Ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="a2fab-449">Add the following highlighted code.</span></span> <span data-ttu-id="a2fab-450">Le nouveau code va après le bloc `.CreateTable( name: "Department"` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="a2fab-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span><span class="sxs-lookup"><span data-stu-id="a2fab-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="a2fab-452">Avec les modifications précédentes, les lignes `Course` existantes seront liées au service « Temp » après l’exécution de la méthode `ComplexDataModel.Up`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-452">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="a2fab-453">La façon de gérer la situation présentée ici est simplifiée pour ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="a2fab-453">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="a2fab-454">Une application de production :</span><span class="sxs-lookup"><span data-stu-id="a2fab-454">A production app would:</span></span>

* <span data-ttu-id="a2fab-455">Comprendrait du code ou des scripts pour ajouter des lignes `Department` et des lignes `Course` associées aux nouvelles lignes `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-455">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="a2fab-456">N’utiliserait pas le département « Temp » ou la valeur par défaut pour `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-456">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a2fab-458">Dans la **console du Gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-458">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="a2fab-459">La méthode `DbInitializer.Initialize` étant conçue pour fonctionner uniquement avec une base de données vide, utilisez SSOX pour supprimer toutes les lignes des tables Student et Course.</span><span class="sxs-lookup"><span data-stu-id="a2fab-459">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="a2fab-460">(La suppression en cascade s’occupe de la table Enrollment)</span><span class="sxs-lookup"><span data-stu-id="a2fab-460">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-461">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-461">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a2fab-462">Si vous utilisez la Base de données locale SQL Server base avec Visual Studio Code, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-462">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="a2fab-463">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-463">Run the app.</span></span> <span data-ttu-id="a2fab-464">L’exécution de l’application entraîne l’exécution de la méthode `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-464">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="a2fab-465">La méthode `DbInitializer.Initialize` remplit la nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-465">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2fab-466">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="a2fab-466">Next steps</span></span>

<span data-ttu-id="a2fab-467">Les deux tutoriels suivants montrent comment lire et mettre à jour des données associées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-467">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2fab-468">[Tutoriel précédent](xref:data/ef-rp/migrations)
> [Tutoriel suivant](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="a2fab-468">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a2fab-469">Dans les didacticiels précédents, nous avons travaillé avec un modèle de données de base composé de trois entités.</span><span class="sxs-lookup"><span data-stu-id="a2fab-469">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="a2fab-470">Dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="a2fab-470">In this tutorial:</span></span>

* <span data-ttu-id="a2fab-471">Nous allons ajouter d’autres entités et relations</span><span class="sxs-lookup"><span data-stu-id="a2fab-471">More entities and relationships are added.</span></span>
* <span data-ttu-id="a2fab-472">Nous allons personnaliser le modèle de données en spécifiant des règles de mise en forme, de validation et de mappage de base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-472">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="a2fab-473">Les classes d’entité pour le modèle de données final sont présentées dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-473">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Diagramme des entités](complex-data-model/_static/diagram.png)

<span data-ttu-id="a2fab-475">Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="a2fab-475">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="a2fab-476">Personnaliser le modèle de données avec des attributs</span><span class="sxs-lookup"><span data-stu-id="a2fab-476">Customize the data model with attributes</span></span>

<span data-ttu-id="a2fab-477">Dans cette section, nous allons personnaliser le modèle de données à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-477">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="a2fab-478">Attribut DataType</span><span class="sxs-lookup"><span data-stu-id="a2fab-478">The DataType attribute</span></span>

<span data-ttu-id="a2fab-479">Actuellement, les pages sur les étudiants affichent l’heure et la date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="a2fab-479">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="a2fab-480">En règle générale, les champs de date ne montrent que la date, et non l’heure.</span><span class="sxs-lookup"><span data-stu-id="a2fab-480">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="a2fab-481">Mettez à jour *Models/Student.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-481">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="a2fab-482">L’attribut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) spécifie un type de données qui est plus spécifique que le type intrinsèque de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-482">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="a2fab-483">Ici, seule la date doit être affichée (pas la date et l’heure).</span><span class="sxs-lookup"><span data-stu-id="a2fab-483">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="a2fab-484">L’énumération [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fournit de nombreux types de données, tels que Date, Time, PhoneNumber, Currency, EmailAddress, et ainsi de suite. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type.</span><span class="sxs-lookup"><span data-stu-id="a2fab-484">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="a2fab-485">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="a2fab-485">For example:</span></span>

* <span data-ttu-id="a2fab-486">Le lien `mailto:` est créé automatiquement pour `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-486">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="a2fab-487">Le sélecteur de date est fourni pour `DataType.Date` dans la plupart des navigateurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-487">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="a2fab-488">L’attribut `DataType` émet des attributs HTML 5 `data-` utilisés par les navigateurs HTML 5.</span><span class="sxs-lookup"><span data-stu-id="a2fab-488">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="a2fab-489">Les attributs `DataType` ne fournissent aucune validation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-489">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="a2fab-490">`DataType.Date` ne spécifie pas le format de la date qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a2fab-490">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="a2fab-491">Par défaut, le champ de date est affiché conformément aux formats par défaut basés sur l’objet [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) du serveur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-491">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="a2fab-492">L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :</span><span class="sxs-lookup"><span data-stu-id="a2fab-492">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="a2fab-493">Le paramètre `ApplyFormatInEditMode` spécifie que la mise en forme doit également être appliquée à l’interface utilisateur de modification.</span><span class="sxs-lookup"><span data-stu-id="a2fab-493">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="a2fab-494">Certains champs ne doivent pas utiliser `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-494">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="a2fab-495">Par exemple, le symbole monétaire ne doit généralement pas être affiché dans une zone de texte d’édition.</span><span class="sxs-lookup"><span data-stu-id="a2fab-495">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="a2fab-496">L’attribut `DisplayFormat` peut être utilisé seul.</span><span class="sxs-lookup"><span data-stu-id="a2fab-496">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="a2fab-497">Il est généralement préférable d’utiliser l’attribut `DataType` avec l’attribut `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-497">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="a2fab-498">L’attribut `DataType` transmet la sémantique des données, plutôt que la manière de l’afficher à l’écran.</span><span class="sxs-lookup"><span data-stu-id="a2fab-498">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="a2fab-499">L’attribut `DataType` offre les avantages suivants qui ne sont pas disponibles dans `DisplayFormat` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-499">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="a2fab-500">Le navigateur peut activer des fonctionnalités HTML5</span><span class="sxs-lookup"><span data-stu-id="a2fab-500">The browser can enable HTML5 features.</span></span> <span data-ttu-id="a2fab-501">(par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, une validation des entrées côté client, et ainsi de suite).</span><span class="sxs-lookup"><span data-stu-id="a2fab-501">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="a2fab-502">Par défaut, le navigateur affiche les données à l’aide du format correspondant aux paramètres régionaux.</span><span class="sxs-lookup"><span data-stu-id="a2fab-502">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="a2fab-503">Pour plus d’informations, consultez la [documentation relative au Tag Helper \<input>](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a2fab-503">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="a2fab-504">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-504">Run the app.</span></span> <span data-ttu-id="a2fab-505">Accédez à la page d’index des étudiants.</span><span class="sxs-lookup"><span data-stu-id="a2fab-505">Navigate to the Students Index page.</span></span> <span data-ttu-id="a2fab-506">Les heures ne sont plus affichées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-506">Times are no longer displayed.</span></span> <span data-ttu-id="a2fab-507">Tous les affichages qui utilisent le modèle `Student` affichent la date sans heure.</span><span class="sxs-lookup"><span data-stu-id="a2fab-507">Every view that uses the `Student` model displays the date without time.</span></span>

![Page d’index des étudiants affichant les dates sans les heures](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="a2fab-509">Attribut StringLength</span><span class="sxs-lookup"><span data-stu-id="a2fab-509">The StringLength attribute</span></span>

<span data-ttu-id="a2fab-510">Vous pouvez également spécifier des règles de validation de données et des messages d’erreur de validation à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-510">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="a2fab-511">L’attribut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) spécifie les longueurs minimale et maximale de caractères autorisées dans un champ de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-511">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="a2fab-512">L’attribut `StringLength` fournit également la validation côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-512">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="a2fab-513">La valeur minimale n’a aucun impact sur le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-513">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="a2fab-514">Mettez à jour le modèle `Student` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-514">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="a2fab-515">Le code précédent limite la longueur des noms à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="a2fab-515">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="a2fab-516">L’attribut `StringLength` n’empêche pas un utilisateur d’entrer un espace blanc comme nom.</span><span class="sxs-lookup"><span data-stu-id="a2fab-516">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="a2fab-517">L’attribut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) est utilisé pour appliquer des restrictions à l’entrée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-517">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="a2fab-518">Par exemple, le code suivant exige que le premier caractère soit une majuscule et que les autres caractères soient alphabétiques :</span><span class="sxs-lookup"><span data-stu-id="a2fab-518">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="a2fab-519">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="a2fab-519">Run the app:</span></span>

* <span data-ttu-id="a2fab-520">Accédez à la page Students.</span><span class="sxs-lookup"><span data-stu-id="a2fab-520">Navigate to the Students page.</span></span>
* <span data-ttu-id="a2fab-521">Sélectionnez **Create New** et entrez un nom de plus de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="a2fab-521">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="a2fab-522">Sélectionnez **Create**. La validation côté client affiche un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-522">Select **Create**, client-side validation shows an error message.</span></span>

![Page d’index des étudiants affichant des erreurs de longueur de chaîne](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="a2fab-524">Dans **l’Explorateur d’objets SQL Server** (SSOX), ouvrez le concepteur de tables Student en double-cliquant sur la table **Student**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-524">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Table Students dans SSOX avant les migrations](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="a2fab-526">L’image précédente montre le schéma pour la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-526">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="a2fab-527">Les champs de nom sont de type `nvarchar(MAX)`, car les migrations n’ont pas été exécutées sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-527">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="a2fab-528">Quand nous exécuterons les migrations plus loin dans ce didacticiel, les champs de nom deviendront `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-528">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="a2fab-529">Attribut Column</span><span class="sxs-lookup"><span data-stu-id="a2fab-529">The Column attribute</span></span>

<span data-ttu-id="a2fab-530">Les attributs peuvent contrôler comment les classes et les propriétés sont mappées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-530">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="a2fab-531">Dans cette section, nous utilisons l’attribut `Column` pour mapper le nom de la propriété `FirstMidName` « FirstName » dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-531">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="a2fab-532">Lors de la création de la base de données, les noms des propriétés sur le modèle sont utilisés comme noms de colonne (sauf quand l’attribut `Column` est utilisé).</span><span class="sxs-lookup"><span data-stu-id="a2fab-532">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="a2fab-533">Le modèle `Student` utilise `FirstMidName` pour le champ de prénom, car le champ peut également contenir un deuxième prénom.</span><span class="sxs-lookup"><span data-stu-id="a2fab-533">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="a2fab-534">Mettez à jour *Student.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-534">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="a2fab-535">Avec la modification précédente, `Student.FirstMidName` dans l’application est mappé à la colonne `FirstName` de la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-535">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="a2fab-536">L’ajout de l’attribut `Column` change le modèle sur lequel repose `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-536">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="a2fab-537">Le modèle sur lequel repose le `SchoolContext` ne correspond plus à la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-537">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="a2fab-538">Si vous exécutez l’application avant d’appliquer les migrations, l’exception suivante est générée :</span><span class="sxs-lookup"><span data-stu-id="a2fab-538">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="a2fab-539">Pour mettre à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-539">To update the DB:</span></span>

* <span data-ttu-id="a2fab-540">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-540">Build the project.</span></span>
* <span data-ttu-id="a2fab-541">Ouvrez une fenêtre de commande dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-541">Open a command window in the project folder.</span></span> <span data-ttu-id="a2fab-542">Entrez les commandes suivantes pour créer une migration et mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-542">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-543">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-544">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-544">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="a2fab-545">La commande `migrations add ColumnFirstName` génère le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-545">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="a2fab-546">Cet avertissement est généré, car les champs de nom sont désormais limités à 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="a2fab-546">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="a2fab-547">Si un nom dans la base de données a plus de 50 caractères, tous les caractères au-delà du cinquantième sont perdus.</span><span class="sxs-lookup"><span data-stu-id="a2fab-547">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="a2fab-548">Testez l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-548">Test the app.</span></span>

<span data-ttu-id="a2fab-549">Ouvrez la table Student dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="a2fab-549">Open the Student table in SSOX:</span></span>

![Table Students dans SSOX après les migrations](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="a2fab-551">Avant l’application de la migration, les colonnes de nom étaient de type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="a2fab-551">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="a2fab-552">Les colonnes de nom sont maintenant `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-552">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="a2fab-553">Le nom de la colonne est passé de `FirstMidName` à `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-553">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="a2fab-554">Dans la section suivante, la génération de l’application à certaines étapes génère des erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-554">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="a2fab-555">Les instructions indiquent quand générer l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-555">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="a2fab-556">Mise à jour de l’entité Student</span><span class="sxs-lookup"><span data-stu-id="a2fab-556">Student entity update</span></span>

![Entité Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="a2fab-558">Mettez à jour *Models/Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-558">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="a2fab-559">Attribut Required</span><span class="sxs-lookup"><span data-stu-id="a2fab-559">The Required attribute</span></span>

<span data-ttu-id="a2fab-560">L’attribut `Required` fait des propriétés de nom des champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-560">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="a2fab-561">L’attribut `Required` n’est pas nécessaire pour les types non nullables tels que les types valeur (`DateTime`, `int`, `double` et ainsi de suite).</span><span class="sxs-lookup"><span data-stu-id="a2fab-561">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="a2fab-562">Les types qui n’acceptent pas les valeurs Null sont traités automatiquement comme des champs obligatoires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-562">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="a2fab-563">L’attribut `Required` peut être remplacé par un paramètre de longueur minimale dans l’attribut `StringLength` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-563">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="a2fab-564">Attribut Display</span><span class="sxs-lookup"><span data-stu-id="a2fab-564">The Display attribute</span></span>

<span data-ttu-id="a2fab-565">L’attribut `Display` indique que la légende des zones de texte doit être « First Name », « Last Name », « Full Name » et « Enrollment Date ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-565">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="a2fab-566">Les légendes par défaut ne comportaient pas d’espace entre les mots, par exemple « Lastname ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-566">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="a2fab-567">Propriété calculée FullName</span><span class="sxs-lookup"><span data-stu-id="a2fab-567">The FullName calculated property</span></span>

<span data-ttu-id="a2fab-568">`FullName` est une propriété calculée qui retourne une valeur créée par concaténation de deux autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="a2fab-568">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="a2fab-569">`FullName` ne peut pas être définie. Elle a uniquement un accesseur get.</span><span class="sxs-lookup"><span data-stu-id="a2fab-569">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="a2fab-570">Aucune colonne `FullName` n’est créée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-570">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="a2fab-571">Créer l’entité Instructor</span><span class="sxs-lookup"><span data-stu-id="a2fab-571">Create the Instructor Entity</span></span>

![Entité Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="a2fab-573">Créez *Models/Instructor.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-573">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="a2fab-574">Plusieurs attributs peuvent être sur une seule ligne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-574">Multiple attributes can be on one line.</span></span> <span data-ttu-id="a2fab-575">Les attributs `HireDate` peuvent être écrits comme suit :</span><span class="sxs-lookup"><span data-stu-id="a2fab-575">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="a2fab-576">Propriétés de navigation CourseAssignments et OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="a2fab-576">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="a2fab-577">Les propriétés `CourseAssignments` et `OfficeAssignment` sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-577">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="a2fab-578">Un formateur pouvant animer un nombre quelconque de cours, `CourseAssignments` est défini comme une collection.</span><span class="sxs-lookup"><span data-stu-id="a2fab-578">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="a2fab-579">Si une propriété de navigation contient plusieurs entités :</span><span class="sxs-lookup"><span data-stu-id="a2fab-579">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="a2fab-580">Elle doit être un type de liste où les entrées peuvent être ajoutées, supprimées et mises à jour.</span><span class="sxs-lookup"><span data-stu-id="a2fab-580">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="a2fab-581">Les types de propriétés de navigation sont :</span><span class="sxs-lookup"><span data-stu-id="a2fab-581">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="a2fab-582">Si `ICollection<T>` est spécifié, EF Core crée une collection `HashSet<T>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="a2fab-582">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="a2fab-583">L’entité `CourseAssignment` est expliquée dans la section sur les relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-583">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="a2fab-584">Les règles professionnelles de Contoso University stipulent qu’un formateur peut avoir au plus un bureau.</span><span class="sxs-lookup"><span data-stu-id="a2fab-584">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="a2fab-585">La propriété `OfficeAssignment` contient une seule entité `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-585">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="a2fab-586">`OfficeAssignment` a la valeur Null si aucun bureau n’est affecté.</span><span class="sxs-lookup"><span data-stu-id="a2fab-586">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="a2fab-587">Créer l’entité OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="a2fab-587">Create the OfficeAssignment entity</span></span>

![Entité OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="a2fab-589">Créez *Models/OfficeAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-589">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="a2fab-590">Attribut Key</span><span class="sxs-lookup"><span data-stu-id="a2fab-590">The Key attribute</span></span>

<span data-ttu-id="a2fab-591">L’attribut `[Key]` est utilisé pour identifier une propriété comme clé primaire quand le nom de la propriété est autre que classnameID ou ID.</span><span class="sxs-lookup"><span data-stu-id="a2fab-591">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="a2fab-592">Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-592">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="a2fab-593">Une affectation de bureau existe uniquement relativement au formateur auquel elle est assignée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-593">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="a2fab-594">La clé primaire `OfficeAssignment` est également sa clé étrangère pour l’entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-594">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="a2fab-595">EF Core ne peut pas reconnaître automatiquement `InstructorID` comme clé primaire de `OfficeAssignment`, car :</span><span class="sxs-lookup"><span data-stu-id="a2fab-595">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="a2fab-596">`InstructorID` ne suit pas la convention de nommage ID ou classnameID.</span><span class="sxs-lookup"><span data-stu-id="a2fab-596">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="a2fab-597">Ainsi, l’attribut `Key` est utilisé pour identifier `InstructorID` comme clé primaire :</span><span class="sxs-lookup"><span data-stu-id="a2fab-597">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="a2fab-598">Par défaut, EF Core traite la clé comme n’étant pas générée par la base de données, car la colonne est utilisée pour une relation d’identification.</span><span class="sxs-lookup"><span data-stu-id="a2fab-598">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="a2fab-599">Propriété de navigation Instructor</span><span class="sxs-lookup"><span data-stu-id="a2fab-599">The Instructor navigation property</span></span>

<span data-ttu-id="a2fab-600">La propriété de navigation `OfficeAssignment` pour l’entité `Instructor` est nullable car :</span><span class="sxs-lookup"><span data-stu-id="a2fab-600">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="a2fab-601">Les types référence (tels que les classes) sont nullables.</span><span class="sxs-lookup"><span data-stu-id="a2fab-601">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="a2fab-602">Un formateur peut ne pas avoir d’affectation de bureau.</span><span class="sxs-lookup"><span data-stu-id="a2fab-602">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="a2fab-603">L’entité `OfficeAssignment` a une propriété de navigation `Instructor` non-nullable car :</span><span class="sxs-lookup"><span data-stu-id="a2fab-603">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="a2fab-604">`InstructorID` n'autorise pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="a2fab-604">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="a2fab-605">Une affectation de bureau ne peut pas exister sans un formateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-605">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="a2fab-606">Quand une entité `Instructor` a une entité `OfficeAssignment` associée, chaque entité a une référence à l’autre dans sa propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-606">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="a2fab-607">L’attribut `[Required]` peut être appliqué à la propriété de navigation `Instructor` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-607">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="a2fab-608">Le code précédent spécifie qu’il doit y avoir un formateur associé.</span><span class="sxs-lookup"><span data-stu-id="a2fab-608">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="a2fab-609">Le code précédent n’est pas nécessaire, car la clé étrangère `InstructorID` (qui est également la clé primaire) n'autorise pas les valeurs Null.</span><span class="sxs-lookup"><span data-stu-id="a2fab-609">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="a2fab-610">Modifier l’entité Course</span><span class="sxs-lookup"><span data-stu-id="a2fab-610">Modify the Course Entity</span></span>

![Entité Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="a2fab-612">Mettez à jour *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-612">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="a2fab-613">L’entité `Course` a une propriété de clé étrangère `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-613">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="a2fab-614">`DepartmentID` pointe vers l’entité `Department` associée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-614">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="a2fab-615">L’entité `Course` a une propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-615">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="a2fab-616">EF Core n’exige pas de propriété de clé étrangère pour un modèle de données quand le modèle a une propriété de navigation pour une entité associée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-616">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="a2fab-617">EF Core crée automatiquement des clés étrangères dans la base de données partout où elles sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-617">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="a2fab-618">EF Core crée des [propriétés cachées](/ef/core/modeling/shadow-properties) pour les clés étrangères créées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a2fab-618">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="a2fab-619">Le fait d’avoir la clé étrangère dans le modèle de données peut rendre les mises à jour plus simples et plus efficaces.</span><span class="sxs-lookup"><span data-stu-id="a2fab-619">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="a2fab-620">Par exemple, considérez un modèle où la propriété de clé étrangère `DepartmentID` n’est *pas* incluse.</span><span class="sxs-lookup"><span data-stu-id="a2fab-620">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="a2fab-621">Quand une entité Course est récupérée en vue d’une modification :</span><span class="sxs-lookup"><span data-stu-id="a2fab-621">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="a2fab-622">L’entité `Department` a la valeur Null si elle n’est pas chargée explicitement.</span><span class="sxs-lookup"><span data-stu-id="a2fab-622">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="a2fab-623">Pour mettre à jour l’entité Course, vous devez d’abord récupérer l’entité `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-623">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="a2fab-624">Quand la propriété de clé étrangère `DepartmentID` est incluse dans le modèle de données, il n’est pas nécessaire de récupérer l’entité `Department` avant une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="a2fab-624">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="a2fab-625">Attribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="a2fab-625">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="a2fab-626">L’attribut `[DatabaseGenerated(DatabaseGeneratedOption.None)]` indique que la clé primaire est fournie par l’application plutôt que générée par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-626">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="a2fab-627">Par défaut, EF Core part du principe que les valeurs de clé primaire sont générées par la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-627">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="a2fab-628">Les valeurs de clé primaire générées par la base de données constituent en général la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="a2fab-628">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="a2fab-629">Pour les entités `Course`, l’utilisateur spécifie la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="a2fab-629">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="a2fab-630">Par exemple, un numéro de cours comme une série 1000 pour le département Mathématiques et une série 2000 pour le département Anglais.</span><span class="sxs-lookup"><span data-stu-id="a2fab-630">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="a2fab-631">Vous pouvez aussi utiliser l’attribut `DatabaseGenerated` pour générer des valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="a2fab-631">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="a2fab-632">Par exemple, la base de données peut générer automatiquement un champ de date pour enregistrer la date de création ou de mise à jour d’une ligne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-632">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="a2fab-633">Pour plus d’informations, consultez [Propriétés générées](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="a2fab-633">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a2fab-634">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-634">Foreign key and navigation properties</span></span>

<span data-ttu-id="a2fab-635">Les propriétés de clé étrangère et les propriétés de navigation dans l’entité `Course` reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-635">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="a2fab-636">Un cours étant affecté à un seul département, il y a une clé étrangère `DepartmentID` et une propriété de navigation `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-636">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="a2fab-637">Un cours pouvant avoir un nombre quelconque d’étudiants inscrits, la propriété de navigation `Enrollments` est une collection :</span><span class="sxs-lookup"><span data-stu-id="a2fab-637">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="a2fab-638">Un cours pouvant être animé par plusieurs formateurs, la propriété de navigation `CourseAssignments` est une collection :</span><span class="sxs-lookup"><span data-stu-id="a2fab-638">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="a2fab-639">`CourseAssignment` est expliquée [plus loin](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="a2fab-639">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="a2fab-640">Créer l’entité Department</span><span class="sxs-lookup"><span data-stu-id="a2fab-640">Create the Department entity</span></span>

![Entité Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="a2fab-642">Créez *Models/Department.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-642">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="a2fab-643">Attribut Column</span><span class="sxs-lookup"><span data-stu-id="a2fab-643">The Column attribute</span></span>

<span data-ttu-id="a2fab-644">Précédemment, vous avez utilisé l’attribut `Column` pour changer le mappage des noms de colonne.</span><span class="sxs-lookup"><span data-stu-id="a2fab-644">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="a2fab-645">Dans le code de l’entité `Department`, l’attribut `Column` est utilisé pour changer le mappage des types de données SQL.</span><span class="sxs-lookup"><span data-stu-id="a2fab-645">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="a2fab-646">La colonne `Budget` est définie à l’aide du type monétaire de SQL Server dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-646">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="a2fab-647">Le mappage de colonnes n’est généralement pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="a2fab-647">Column mapping is generally not required.</span></span> <span data-ttu-id="a2fab-648">EF Core choisit en général le type de données SQL Server approprié en fonction du type CLR de la propriété.</span><span class="sxs-lookup"><span data-stu-id="a2fab-648">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="a2fab-649">Le type CLR `decimal` est mappé à un type SQL Server `decimal`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-649">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="a2fab-650">`Budget` étant une valeur monétaire, le type de données money est plus approprié.</span><span class="sxs-lookup"><span data-stu-id="a2fab-650">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a2fab-651">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-651">Foreign key and navigation properties</span></span>

<span data-ttu-id="a2fab-652">Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-652">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="a2fab-653">Un département peut avoir ou ne pas avoir un administrateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-653">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="a2fab-654">Un administrateur est toujours un formateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-654">An administrator is always an instructor.</span></span> <span data-ttu-id="a2fab-655">Ainsi, la propriété `InstructorID` est incluse en tant que clé étrangère de l’entité `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-655">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="a2fab-656">La propriété de navigation se nomme `Administrator`, mais elle contient une entité `Instructor` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-656">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="a2fab-657">Le point d’interrogation (?) dans le code précédent indique que la propriété est nullable.</span><span class="sxs-lookup"><span data-stu-id="a2fab-657">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="a2fab-658">Un département peut avoir de nombreux cours, si bien qu’il existe une propriété de navigation Courses :</span><span class="sxs-lookup"><span data-stu-id="a2fab-658">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="a2fab-659">Remarque : Par convention, EF Core autorise la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs à plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-659">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="a2fab-660">Les suppressions en cascade peuvent engendrer des règles de suppression en cascade circulaires.</span><span class="sxs-lookup"><span data-stu-id="a2fab-660">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="a2fab-661">Les règles de suppression en cascade circulaires provoquent une exception quand une migration est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-661">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="a2fab-662">Par exemple, si la propriété `Department.InstructorID` ne doit pas accepter les valeurs Null :</span><span class="sxs-lookup"><span data-stu-id="a2fab-662">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="a2fab-663">EF Core configure une règle de suppression en cascade pour supprimer le service lorsque l’instructeur est supprimé.</span><span class="sxs-lookup"><span data-stu-id="a2fab-663">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="a2fab-664">La suppression du service lorsque l’instructeur est supprimé n’est pas le comportement souhaité.</span><span class="sxs-lookup"><span data-stu-id="a2fab-664">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="a2fab-665">L’API Fluent suivante définit une règle de restriction au lieu d’une cascade.</span><span class="sxs-lookup"><span data-stu-id="a2fab-665">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="a2fab-666">Le code précédent désactive la suppression en cascade sur la relation formateur-département.</span><span class="sxs-lookup"><span data-stu-id="a2fab-666">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="a2fab-667">Mettre à jour l’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="a2fab-667">Update the Enrollment entity</span></span>

<span data-ttu-id="a2fab-668">Un enregistrement d’inscription correspond à un cours suivi par un étudiant.</span><span class="sxs-lookup"><span data-stu-id="a2fab-668">An enrollment record is for one course taken by one student.</span></span>

![Entité Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="a2fab-670">Mettez à jour *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-670">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="a2fab-671">Propriétés de clé étrangère et de navigation</span><span class="sxs-lookup"><span data-stu-id="a2fab-671">Foreign key and navigation properties</span></span>

<span data-ttu-id="a2fab-672">Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a2fab-672">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="a2fab-673">Un enregistrement d’inscription ne concernant qu’un seul cours, il y a une propriété de clé étrangère `CourseID` et une propriété de navigation `Course` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-673">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="a2fab-674">Un enregistrement d’inscription ne concernant qu’un seul étudiant, il y a une propriété de clé étrangère `StudentID` et une propriété de navigation `Student` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-674">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="a2fab-675">Relations plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="a2fab-675">Many-to-Many Relationships</span></span>

<span data-ttu-id="a2fab-676">Il existe une relation plusieurs-à-plusieurs entre les entités `Student` et `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-676">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="a2fab-677">L’entité `Enrollment` joue le rôle de table de jointure plusieurs-à-plusieurs *avec charge utile* dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-677">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="a2fab-678">« Avec charge utile » signifie que la table `Enrollment` contient des données supplémentaires en plus des clés étrangères pour les tables jointes (dans le cas présent, la clé primaire et `Grade`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-678">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="a2fab-679">L’illustration suivante montre à quoi ressemblent ces relations dans un diagramme d’entité.</span><span class="sxs-lookup"><span data-stu-id="a2fab-679">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="a2fab-680">(Ce diagramme a été généré à l’aide de [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) pour EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="a2fab-680">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="a2fab-681">Sa création ne fait pas partie de ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="a2fab-681">Creating the diagram isn't part of the tutorial.)</span></span>

![Relation plusieurs à plusieurs Student-Course](complex-data-model/_static/student-course.png)

<span data-ttu-id="a2fab-683">Chaque ligne de relation comporte un 1 à une extrémité et un astérisque (\*) à l’autre, ce qui indique une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-683">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="a2fab-684">Si la table `Enrollment` n’incluait pas d’informations de notes, elle aurait uniquement besoin de contenir les deux clés étrangères (`CourseID` et `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-684">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="a2fab-685">Une table de jointure plusieurs-à-plusieurs sans charge utile est parfois appelée « table de jointure pure ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-685">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="a2fab-686">Les entités `Instructor` et `Course` ont une relation plusieurs-à-plusieurs à l’aide d’une table de jointure pure.</span><span class="sxs-lookup"><span data-stu-id="a2fab-686">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="a2fab-687">Remarque : EF 6.x prend en charge les tables de jointure implicites pour les relations plusieurs-à-plusieurs, mais EF Core ne le fait pas.</span><span class="sxs-lookup"><span data-stu-id="a2fab-687">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="a2fab-688">Pour plus d’informations, consultez [Many-to-many relationships in EF Core 2.0 (Relations plusieurs-à-plusieurs dans EF Core 2.0)](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="a2fab-688">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="a2fab-689">Entité CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="a2fab-689">The CourseAssignment entity</span></span>

![Entité CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="a2fab-691">Créez *Models/CourseAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-691">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="a2fab-692">Instructor-Courses</span><span class="sxs-lookup"><span data-stu-id="a2fab-692">Instructor-to-Courses</span></span>

![Instructor-to-Courses m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="a2fab-694">La relation plusieurs-à-plusieurs Instructor-Courses :</span><span class="sxs-lookup"><span data-stu-id="a2fab-694">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="a2fab-695">Nécessite une table de jointure qui doit être représentée par un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="a2fab-695">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="a2fab-696">Est une table de jointure pure (table sans charge utile).</span><span class="sxs-lookup"><span data-stu-id="a2fab-696">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="a2fab-697">Il est courant de nommer une entité de jointure `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-697">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="a2fab-698">Par exemple, la table de jointure Instructor-to-Courses utilisant ce modèle est `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-698">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="a2fab-699">Toutefois, nous vous recommandons d’utiliser un nom qui décrit la relation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-699">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="a2fab-700">Les modèles de données sont simples au début, puis ils augmentent en complexité.</span><span class="sxs-lookup"><span data-stu-id="a2fab-700">Data models start out simple and grow.</span></span> <span data-ttu-id="a2fab-701">Les jointures sans charge utile (tables de jointure pures) évoluent fréquemment pour inclure une charge utile.</span><span class="sxs-lookup"><span data-stu-id="a2fab-701">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="a2fab-702">En commençant par un nom d’entité descriptif, vous n’aurez pas besoin de modifier le nom quand la table de jointure changera.</span><span class="sxs-lookup"><span data-stu-id="a2fab-702">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="a2fab-703">Dans l’idéal, l’entité de jointure aura son propre nom (éventuellement un mot unique) naturel dans le domaine d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="a2fab-703">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="a2fab-704">Par exemple, les livres et les clients pourraient être liés avec une entité de jointure nommée Évaluations.</span><span class="sxs-lookup"><span data-stu-id="a2fab-704">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="a2fab-705">Pour la relation plusieurs-à-plusieurs Instructor-Courses, il vaut mieux utiliser `CourseAssignment` que `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-705">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="a2fab-706">Clé composite</span><span class="sxs-lookup"><span data-stu-id="a2fab-706">Composite key</span></span>

<span data-ttu-id="a2fab-707">Les clés étrangères ne sont pas nullables.</span><span class="sxs-lookup"><span data-stu-id="a2fab-707">FKs are not nullable.</span></span> <span data-ttu-id="a2fab-708">Ensemble, le deux clés étrangères dans `CourseAssignment` (`InstructorID` et `CourseID`) identifient de façon unique chaque ligne de la table `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-708">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="a2fab-709">`CourseAssignment` ne nécessite pas de clé primaire dédiée.</span><span class="sxs-lookup"><span data-stu-id="a2fab-709">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="a2fab-710">Les propriétés `InstructorID` et `CourseID` fonctionnent comme une clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="a2fab-710">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="a2fab-711">Le seul moyen de spécifier des clés primaires composites dans EF Core consiste à faire appel à l’*API Fluent*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-711">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="a2fab-712">La section suivante montre comment configurer la clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="a2fab-712">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="a2fab-713">La clé composite garantit que :</span><span class="sxs-lookup"><span data-stu-id="a2fab-713">The composite key ensures:</span></span>

* <span data-ttu-id="a2fab-714">Plusieurs lignes sont autorisées pour un cours.</span><span class="sxs-lookup"><span data-stu-id="a2fab-714">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="a2fab-715">Plusieurs lignes sont autorisées pour un formateur.</span><span class="sxs-lookup"><span data-stu-id="a2fab-715">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="a2fab-716">Plusieurs lignes pour la même paire formateur-cours ne sont pas autorisées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-716">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="a2fab-717">Comme l’entité de jointure `Enrollment` définit sa propre clé primaire, des doublons de ce type sont possibles.</span><span class="sxs-lookup"><span data-stu-id="a2fab-717">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="a2fab-718">Pour éviter ces doublons :</span><span class="sxs-lookup"><span data-stu-id="a2fab-718">To prevent such duplicates:</span></span>

* <span data-ttu-id="a2fab-719">Ajoutez un index unique sur les champs de clé primaire, ou</span><span class="sxs-lookup"><span data-stu-id="a2fab-719">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="a2fab-720">Configurez `Enrollment` avec une clé primaire composite similaire à `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-720">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="a2fab-721">Pour plus d'informations, consultez [Index](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="a2fab-721">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="a2fab-722">Mettre à jour le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-722">Update the DB context</span></span>

<span data-ttu-id="a2fab-723">Ajoutez le code en surbrillance suivant à *Data/SchoolContext.cs* :</span><span class="sxs-lookup"><span data-stu-id="a2fab-723">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="a2fab-724">Le code précédent ajoute les nouvelles entités et configure la clé primaire composite de l’entité `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-724">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="a2fab-725">Alternative d’API Fluent aux attributs</span><span class="sxs-lookup"><span data-stu-id="a2fab-725">Fluent API alternative to attributes</span></span>

<span data-ttu-id="a2fab-726">La méthode `OnModelCreating` du code précédent utilise l’*API Fluent* pour configurer le comportement d’EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2fab-726">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="a2fab-727">L’API est appelée « Fluent », car elle est souvent utilisée en enchaînant une série d’appels de méthode en une seule instruction.</span><span class="sxs-lookup"><span data-stu-id="a2fab-727">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="a2fab-728">Le [code suivant](/ef/core/modeling/#use-fluent-api-to-configure-a-model) est un exemple de l’API Fluent :</span><span class="sxs-lookup"><span data-stu-id="a2fab-728">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="a2fab-729">Dans ce didacticiel, l’API Fluent est utilisée uniquement pour le mappage de base de données qui ne peut pas être effectué avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-729">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="a2fab-730">Toutefois, l’API Fluent peut spécifier la plupart des règles de mise en forme, de validation et de mappage pouvant être spécifiées à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="a2fab-730">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="a2fab-731">Certains attributs, tels que `MinimumLength`, ne peuvent pas être appliqués avec l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a2fab-731">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="a2fab-732">`MinimumLength` ne change pas le schéma. Il applique uniquement une règle de validation de longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="a2fab-732">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="a2fab-733">Certains développeurs préfèrent utiliser exclusivement l’API Fluent afin de conserver des classes d’entité « propres ».</span><span class="sxs-lookup"><span data-stu-id="a2fab-733">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="a2fab-734">Vous pouvez combiner des attributs et l’API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a2fab-734">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="a2fab-735">Certaines configurations peuvent être effectuées uniquement avec l’API Fluent (spécification d’une clé primaire composite).</span><span class="sxs-lookup"><span data-stu-id="a2fab-735">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="a2fab-736">Certaines autres peuvent être effectuées uniquement avec des attributs (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-736">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="a2fab-737">Voici ce que nous recommandons pour l’utilisation des API Fluent ou des attributs :</span><span class="sxs-lookup"><span data-stu-id="a2fab-737">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="a2fab-738">Choisissez l’une de ces deux approches.</span><span class="sxs-lookup"><span data-stu-id="a2fab-738">Choose one of these two approaches.</span></span>
* <span data-ttu-id="a2fab-739">Dans la mesure du possible, utilisez l’approche choisie de manière cohérente.</span><span class="sxs-lookup"><span data-stu-id="a2fab-739">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="a2fab-740">Certains des attributs utilisés dans ce didacticiel sont utilisés pour :</span><span class="sxs-lookup"><span data-stu-id="a2fab-740">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="a2fab-741">La validation uniquement (par exemple, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-741">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="a2fab-742">La configuration d’EF Core uniquement (par exemple, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-742">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="a2fab-743">La validation et la configuration d’EF Core (par exemple, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-743">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="a2fab-744">Pour plus d’informations sur les attributs et l’API Fluent, consultez [Méthodes de configuration](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="a2fab-744">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="a2fab-745">Diagramme des entités montrant les relations</span><span class="sxs-lookup"><span data-stu-id="a2fab-745">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="a2fab-746">L’illustration suivante montre le diagramme que les outils EF Power créent pour le modèle School complet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-746">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagramme des entités](complex-data-model/_static/diagram.png)

<span data-ttu-id="a2fab-748">Le diagramme précédent montre :</span><span class="sxs-lookup"><span data-stu-id="a2fab-748">The preceding diagram shows:</span></span>

* <span data-ttu-id="a2fab-749">Plusieurs lignes de relations un-à-plusieurs (1 à \*).</span><span class="sxs-lookup"><span data-stu-id="a2fab-749">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="a2fab-750">La ligne de relation un-à-zéro-ou-un (1 à 0..1) entre les entités `Instructor` et `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-750">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="a2fab-751">La ligne de relation zéro-ou-un-à-plusieurs (0..1 à \*) entre les entités `Instructor` et `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-751">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="a2fab-752">Amorcer la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="a2fab-752">Seed the DB with Test Data</span></span>

<span data-ttu-id="a2fab-753">Mettez à jour le code dans *Data/DbInitializer.cs* :</span><span class="sxs-lookup"><span data-stu-id="a2fab-753">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="a2fab-754">Le code précédent fournit des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="a2fab-754">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="a2fab-755">La majeure partie de ce code crée des objets d’entités et charge des exemples de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-755">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="a2fab-756">Les exemples de données sont utilisés à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="a2fab-756">The sample data is used for testing.</span></span> <span data-ttu-id="a2fab-757">Consultez `Enrollments` et `CourseAssignments` pour obtenir des exemples de la façon dont des tables de jointure plusieurs-à-plusieurs peuvent être amorcées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-757">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="a2fab-758">Ajouter une migration</span><span class="sxs-lookup"><span data-stu-id="a2fab-758">Add a migration</span></span>

<span data-ttu-id="a2fab-759">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-759">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-760">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-760">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-761">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-761">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="a2fab-762">La commande précédente affiche un avertissement concernant les pertes de données possibles.</span><span class="sxs-lookup"><span data-stu-id="a2fab-762">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="a2fab-763">Si la commande `database update` est exécutée, l’erreur suivante se produit :</span><span class="sxs-lookup"><span data-stu-id="a2fab-763">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="a2fab-764">Appliquer la migration</span><span class="sxs-lookup"><span data-stu-id="a2fab-764">Apply the migration</span></span>

<span data-ttu-id="a2fab-765">Disposant à présent d’une base de données, vous devez réfléchir à la façon dont vous y apporterez des modifications.</span><span class="sxs-lookup"><span data-stu-id="a2fab-765">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="a2fab-766">Ce tutoriel montre deux approches :</span><span class="sxs-lookup"><span data-stu-id="a2fab-766">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="a2fab-767">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-767">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="a2fab-768">[Appliquer la migration à la base de données](#applyexisting)</span><span class="sxs-lookup"><span data-stu-id="a2fab-768">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="a2fab-769">Bien que cette méthode soit plus longue et complexe, elle constitue l’approche privilégiée pour les environnements de production réels.</span><span class="sxs-lookup"><span data-stu-id="a2fab-769">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="a2fab-770">**Remarque** : Cette section du tutoriel est facultative.</span><span class="sxs-lookup"><span data-stu-id="a2fab-770">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="a2fab-771">Vous pouvez effectuer les étapes de suppression et de recréation et ignorer cette section.</span><span class="sxs-lookup"><span data-stu-id="a2fab-771">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="a2fab-772">Si vous souhaitez suivre les étapes décrites dans cette section, n’effectuez pas les étapes de suppression et de recréation.</span><span class="sxs-lookup"><span data-stu-id="a2fab-772">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="a2fab-773">Supprimer et recréer la base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-773">Drop and re-create the database</span></span>

<span data-ttu-id="a2fab-774">Le code dans le `DbInitializer` mis à jour ajoute des données de valeur initiale pour les nouvelles entités.</span><span class="sxs-lookup"><span data-stu-id="a2fab-774">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="a2fab-775">Pour forcer EF Core à créer une autre base de données, supprimez et mettez à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="a2fab-775">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a2fab-776">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2fab-776">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a2fab-777">Dans la **console du Gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a2fab-777">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="a2fab-778">Exécutez `Get-Help about_EntityFrameworkCore` à partir de la console du Gestionnaire de package pour obtenir des informations d’aide.</span><span class="sxs-lookup"><span data-stu-id="a2fab-778">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a2fab-779">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a2fab-779">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a2fab-780">Ouvrez une fenêtre de commande et accédez au dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="a2fab-780">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="a2fab-781">Le dossier du projet contient le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-781">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="a2fab-782">Entrez ce qui suit dans la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="a2fab-782">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="a2fab-783">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="a2fab-783">Run the app.</span></span> <span data-ttu-id="a2fab-784">L’exécution de l’application entraîne l’exécution de la méthode `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-784">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="a2fab-785">La méthode `DbInitializer.Initialize` remplit la nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-785">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="a2fab-786">Ouvrez la base de données dans SSOX :</span><span class="sxs-lookup"><span data-stu-id="a2fab-786">Open the DB in SSOX:</span></span>

* <span data-ttu-id="a2fab-787">Si SSOX était déjà ouvert, cliquez sur le bouton **Actualiser**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-787">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="a2fab-788">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-788">Expand the **Tables** node.</span></span> <span data-ttu-id="a2fab-789">Les tables créées sont affichées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-789">The created tables are displayed.</span></span>

![Tables dans SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="a2fab-791">Examinez la table **CourseAssignment** :</span><span class="sxs-lookup"><span data-stu-id="a2fab-791">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="a2fab-792">Cliquez avec le bouton droit sur la table **CourseAssignment** et sélectionnez **Afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="a2fab-792">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="a2fab-793">Vérifiez que la table **CourseAssignment** contient des données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-793">Verify the **CourseAssignment** table contains data.</span></span>

![Données CourseAssignment dans SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="a2fab-795">Appliquer la migration à la base de données</span><span class="sxs-lookup"><span data-stu-id="a2fab-795">Apply the migration to the existing database</span></span>

<span data-ttu-id="a2fab-796">Cette section est facultative.</span><span class="sxs-lookup"><span data-stu-id="a2fab-796">This section is optional.</span></span> <span data-ttu-id="a2fab-797">Ces étapes fonctionnent seulement si vous avez ignoré la section [Supprimer et recréer la base de données](#drop) précédente.</span><span class="sxs-lookup"><span data-stu-id="a2fab-797">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="a2fab-798">Quand des migrations sont exécutées avec des données existantes, il peut y avoir des contraintes de clé étrangère qui ne sont pas satisfaites avec les données existantes.</span><span class="sxs-lookup"><span data-stu-id="a2fab-798">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="a2fab-799">Avec des données de production, vous devez effectuer certaines actions pour migrer les données existantes.</span><span class="sxs-lookup"><span data-stu-id="a2fab-799">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="a2fab-800">Cette section fournit un exemple de correction des violations de contraintes de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="a2fab-800">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="a2fab-801">N’effectuez pas ces modifications de code sans une sauvegarde.</span><span class="sxs-lookup"><span data-stu-id="a2fab-801">Don't make these code changes without a backup.</span></span> <span data-ttu-id="a2fab-802">N’effectuez pas ces modifications de code si vous avez terminé la section précédente et mis à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="a2fab-802">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="a2fab-803">Le fichier *{horodatage}_ComplexDataModel.cs* contient le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a2fab-803">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="a2fab-804">Le code précédent ajoute une clé étrangère `DepartmentID` non-nullable à la table `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-804">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="a2fab-805">La base de données du didacticiel précédent contient des lignes dans `Course` ; cette table ne peut donc pas être mise à jour par des migrations.</span><span class="sxs-lookup"><span data-stu-id="a2fab-805">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="a2fab-806">Pour faire en sorte que la migration `ComplexDataModel` fonctionne avec des données existantes</span><span class="sxs-lookup"><span data-stu-id="a2fab-806">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="a2fab-807">Modifiez le code pour affecter une valeur par défaut à la nouvelle colonne (`DepartmentID`).</span><span class="sxs-lookup"><span data-stu-id="a2fab-807">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="a2fab-808">Créez un département fictif nommé « Temp » assumant la fonction de département par défaut.</span><span class="sxs-lookup"><span data-stu-id="a2fab-808">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="a2fab-809">Corriger les contraintes de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="a2fab-809">Fix the foreign key constraints</span></span>

<span data-ttu-id="a2fab-810">Mettez à jour la méthode `Up` de la classe `ComplexDataModel` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-810">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="a2fab-811">Ouvrez le fichier *{horodatage}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="a2fab-811">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="a2fab-812">Commentez la ligne de code qui ajoute la colonne `DepartmentID` à la table `Course`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-812">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="a2fab-813">Ajoutez le code en surbrillance suivant.</span><span class="sxs-lookup"><span data-stu-id="a2fab-813">Add the following highlighted code.</span></span> <span data-ttu-id="a2fab-814">Le nouveau code va après le bloc `.CreateTable( name: "Department"` :</span><span class="sxs-lookup"><span data-stu-id="a2fab-814">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="a2fab-815">Avec les modifications précédentes, les lignes `Course` existantes seront toutes associées au département « Temp » après l’exécution de la méthode `Up` de `ComplexDataModel`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-815">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="a2fab-816">Une application de production :</span><span class="sxs-lookup"><span data-stu-id="a2fab-816">A production app would:</span></span>

* <span data-ttu-id="a2fab-817">Comprendrait du code ou des scripts pour ajouter des lignes `Department` et des lignes `Course` associées aux nouvelles lignes `Department`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-817">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="a2fab-818">N’utiliserait pas le département « Temp » ou la valeur par défaut pour `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="a2fab-818">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="a2fab-819">Le didacticiel suivant traite des données associées.</span><span class="sxs-lookup"><span data-stu-id="a2fab-819">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2fab-820">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a2fab-820">Additional resources</span></span>

* [<span data-ttu-id="a2fab-821">Version YouTube de ce tutoriel(Partie 1)</span><span class="sxs-lookup"><span data-stu-id="a2fab-821">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="a2fab-822">Version YouTube de ce tutoriel(Partie 2)</span><span class="sxs-lookup"><span data-stu-id="a2fab-822">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="a2fab-823">[Précédent](xref:data/ef-rp/migrations)
> [Suivant](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="a2fab-823">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end