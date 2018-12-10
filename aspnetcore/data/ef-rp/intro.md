---
title: Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8
author: rick-anderson
description: Montre comment créer une application Pages Razor à l’aide d’Entity Framework Core.
ms.author: riande
ms.custom: seodec18
ms.date: 11/22/2018
uid: data/ef-rp/intro
ms.openlocfilehash: 34c7238b689993245e033625dcd0e728b7c45163
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121698"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="33f01-103">Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8</span><span class="sxs-lookup"><span data-stu-id="33f01-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="33f01-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33f01-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33f01-105">L’exemple d’application web Contoso University montre comment créer une application web Razor Pages ASP.NET Core à l’aide d’Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="33f01-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="33f01-106">L’exemple d’application est un site web pour une université Contoso fictive.</span><span class="sxs-lookup"><span data-stu-id="33f01-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="33f01-107">Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs.</span><span class="sxs-lookup"><span data-stu-id="33f01-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="33f01-108">Cette page est la première d’une série de didacticiels qui expliquent comment générer l’exemple d’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="33f01-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="33f01-109">Télécharger ou afficher l’application complète.</span><span class="sxs-lookup"><span data-stu-id="33f01-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="33f01-110">[Télécharger les instructions](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="33f01-110">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33f01-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="33f01-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33f01-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33f01-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33f01-113">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="33f01-113">.NET Core CLI</span></span>](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

<span data-ttu-id="33f01-114">Connaissance des [Pages Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="33f01-114">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="33f01-115">Les programmeurs débutants doivent lire [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) avant de démarrer cette série.</span><span class="sxs-lookup"><span data-stu-id="33f01-115">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="33f01-116">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="33f01-116">Troubleshooting</span></span>

<span data-ttu-id="33f01-117">Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous pouvez généralement trouver la solution en comparant votre code au [projet terminé](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="33f01-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="33f01-118">Un bon moyen d’obtenir de l’aide est de publier une question sur [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pour [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="33f01-118">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="33f01-119">L’application web Contoso University</span><span class="sxs-lookup"><span data-stu-id="33f01-119">The Contoso University web app</span></span>

<span data-ttu-id="33f01-120">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="33f01-120">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="33f01-121">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="33f01-121">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="33f01-122">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="33f01-122">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index.png)

![Page de modification des étudiants](intro/_static/student-edit.png)

<span data-ttu-id="33f01-125">Le style de l’interface utilisateur de ce site est proche de ce qui est généré par les modèles prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="33f01-125">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="33f01-126">Le didacticiel est axé sur Core EF avec des pages Razor, et non sur l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="33f01-126">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="33f01-127">Créer l’application web Razor Pages ContosoUniversity</span><span class="sxs-lookup"><span data-stu-id="33f01-127">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33f01-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33f01-128">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="33f01-129">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="33f01-129">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="33f01-130">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33f01-130">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="33f01-131">Nommez le projet **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="33f01-131">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="33f01-132">Il est important de nommer le projet *ContosoUniversity* afin que les espaces de noms correspondent quand le code est copié/collé.</span><span class="sxs-lookup"><span data-stu-id="33f01-132">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="33f01-133">Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="33f01-133">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="33f01-134">Pour les images des étapes précédentes, consultez [Créer une application web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span><span class="sxs-lookup"><span data-stu-id="33f01-134">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="33f01-135">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="33f01-135">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33f01-136">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="33f01-136">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="33f01-137">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="33f01-137">Set up the site style</span></span>

<span data-ttu-id="33f01-138">Quelques changements permettent de définir le menu, la disposition et la page d’accueil du site.</span><span class="sxs-lookup"><span data-stu-id="33f01-138">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="33f01-139">Mettez à jour *Pages/Shared/_Layout.cshtml* avec les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="33f01-139">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="33f01-140">Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="33f01-140">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="33f01-141">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="33f01-141">There are three occurrences.</span></span>

* <span data-ttu-id="33f01-142">Ajoutez des entrées de menu pour **Students**, **Courses**, **Instructors** et **Departments**, et supprimez l’entrée de menu **Contact**.</span><span class="sxs-lookup"><span data-stu-id="33f01-142">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="33f01-143">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="33f01-143">The changes are highlighted.</span></span> <span data-ttu-id="33f01-144">(Tout le balisage n’est *pas* affiché.)</span><span class="sxs-lookup"><span data-stu-id="33f01-144">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="33f01-145">Dans *Pages/Index.cshtml*, remplacez le contenu du fichier par le code suivant afin de remplacer le texte sur ASP.NET et MVC par le texte concernant cette application :</span><span class="sxs-lookup"><span data-stu-id="33f01-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="33f01-146">Créer le modèle de données</span><span class="sxs-lookup"><span data-stu-id="33f01-146">Create the data model</span></span>

<span data-ttu-id="33f01-147">Créez des classes d’entités pour l’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="33f01-147">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="33f01-148">Commencez par les trois entités suivantes :</span><span class="sxs-lookup"><span data-stu-id="33f01-148">Start with the following three entities:</span></span>

![Diagramme de modèle de données Cours-Inscription-Étudiant](intro/_static/data-model-diagram.png)

<span data-ttu-id="33f01-150">Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33f01-150">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="33f01-151">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33f01-151">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="33f01-152">Un étudiant peut s’inscrire à autant de cours qu’il le souhaite.</span><span class="sxs-lookup"><span data-stu-id="33f01-152">A student can enroll in any number of courses.</span></span> <span data-ttu-id="33f01-153">Un cours peut avoir une quantité illimitée d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="33f01-153">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="33f01-154">Dans les sections suivantes, une classe pour chacune de ces entités est créée.</span><span class="sxs-lookup"><span data-stu-id="33f01-154">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="33f01-155">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="33f01-155">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

<span data-ttu-id="33f01-157">Créez un dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="33f01-157">Create a *Models* folder.</span></span> <span data-ttu-id="33f01-158">Dans le dossier *Models*, créez un fichier de classe nommé *Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33f01-158">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="33f01-159">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="33f01-159">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="33f01-160">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="33f01-160">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="33f01-161">Dans `classnameID`, `classname` est le nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="33f01-161">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="33f01-162">L’autre clé primaire reconnue automatiquement est `StudentID` dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="33f01-162">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="33f01-163">La propriété `Enrollments` est une [propriété de navigation](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="33f01-163">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="33f01-164">Les propriétés de navigation établissent des liaisons à d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="33f01-164">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="33f01-165">Ici, la propriété `Enrollments` d’un `Student entity` contient toutes les entités `Enrollment` associées à ce `Student`.</span><span class="sxs-lookup"><span data-stu-id="33f01-165">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="33f01-166">Par exemple, si une ligne Student dans la base de données a deux lignes Enrollment associées, la propriété de navigation `Enrollments` contient ces deux entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33f01-166">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="33f01-167">Une ligne `Enrollment` associée est une ligne qui contient la valeur de clé primaire de cette étudiant dans la colonne `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="33f01-167">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="33f01-168">Par exemple, supposez que l’étudiant avec ID=1 a deux lignes dans la table `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33f01-168">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="33f01-169">La table `Enrollment` a deux lignes avec `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="33f01-169">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="33f01-170">`StudentID` est une clé étrangère dans la table `Enrollment` qui spécifie l’étudiant dans la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="33f01-170">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="33f01-171">Si une propriété de navigation peut contenir plusieurs entités, la propriété de navigation doit être un type de liste, tel que `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="33f01-171">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="33f01-172">Vous pouvez spécifier `ICollection<T>`, ou un type tel que `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="33f01-172">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="33f01-173">Quand vous utilisez `ICollection<T>`, EF Core crée une collection `HashSet<T>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="33f01-173">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="33f01-174">Les propriétés de navigation qui contiennent plusieurs entités proviennent de relations plusieurs-à-plusieurs et un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="33f01-174">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="33f01-175">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="33f01-175">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="33f01-177">Dans le dossier *Models*, créez un fichier *Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33f01-177">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="33f01-178">La propriété `EnrollmentID` est la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="33f01-178">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="33f01-179">Cette entité utilise le modèle `classnameID` au lieu de `ID` comme l’entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="33f01-179">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="33f01-180">En général, les développeurs choisissent un modèle et l’utilisent dans tout le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-180">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="33f01-181">Dans un prochain didacticiel, nous verrons qu’utiliser ID sans classname simplifie l’implémentation de l’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-181">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="33f01-182">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="33f01-182">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="33f01-183">Le point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` est nullable.</span><span class="sxs-lookup"><span data-stu-id="33f01-183">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="33f01-184">Une note (Grade) qui a la valeur Null est différente d’une note égale à zéro : la valeur Null signifie qu’une note n’est pas connue ou n’a pas encore été affectée.</span><span class="sxs-lookup"><span data-stu-id="33f01-184">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="33f01-185">La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="33f01-185">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="33f01-186">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="33f01-186">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="33f01-187">L’entité `Student` diffère de la propriété de navigation `Student.Enrollments`, qui contient plusieurs entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33f01-187">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="33f01-188">La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="33f01-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="33f01-189">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="33f01-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="33f01-190">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="33f01-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="33f01-191">Par exemple, `StudentID` pour la propriété de navigation `Student`, puisque la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="33f01-191">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="33f01-192">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="33f01-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="33f01-193">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="33f01-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="33f01-194">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="33f01-194">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="33f01-196">Dans le dossier *Models*, créez un fichier *Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33f01-196">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="33f01-197">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="33f01-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="33f01-198">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33f01-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="33f01-199">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="33f01-200">Générer automatiquement le modèle d’étudiant</span><span class="sxs-lookup"><span data-stu-id="33f01-200">Scaffold the student model</span></span>

<span data-ttu-id="33f01-201">Dans cette section, le modèle d’étudiant est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="33f01-201">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="33f01-202">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création (Create), de lecture (Read), de mise à jour (Update) et de suppression (Delete) (CRUD) pour le modèle d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="33f01-202">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="33f01-203">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="33f01-203">Build the project.</span></span>
* <span data-ttu-id="33f01-204">Créez le dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="33f01-204">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="33f01-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33f01-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="33f01-206">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Students* > **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="33f01-206">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="33f01-207">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)** > **AJOUTER**.</span><span class="sxs-lookup"><span data-stu-id="33f01-207">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="33f01-208">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="33f01-208">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="33f01-209">Dans la liste déroulante **Classe de modèle**, sélectionnez **Student (ContosoUniversity.Models)**.</span><span class="sxs-lookup"><span data-stu-id="33f01-209">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="33f01-210">Dans la ligne **Classe de contexte de données**, sélectionnez le signe **+** (plus) et remplacez le nom généré par **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="33f01-210">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="33f01-211">Dans la liste déroulante **Classe de contexte de données**, sélectionnez **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="33f01-211">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="33f01-212">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="33f01-212">Select **Add**.</span></span>

![Boîte de dialogue CRUD](intro/_static/s1.png)

<span data-ttu-id="33f01-214">Consultez [Générer automatiquement le modèle de film](xref:tutorials/razor-pages/model#scaffold-the-movie-model) si vous rencontrez un problème à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="33f01-214">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="33f01-215">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="33f01-215">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="33f01-216">Exécutez les commandes suivantes pour générer automatiquement le modèle d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="33f01-216">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="33f01-217">Le processus de génération de modèles automatique a créé et changé les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="33f01-217">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="33f01-218">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="33f01-218">Files created</span></span>

* <span data-ttu-id="33f01-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="33f01-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="33f01-220">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="33f01-220">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="33f01-221">Mises à jour du fichier</span><span class="sxs-lookup"><span data-stu-id="33f01-221">File updates</span></span>

* <span data-ttu-id="33f01-222">*Startup.cs* : Les changements apportés à ce fichier sont détaillés dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="33f01-222">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="33f01-223">*appSettings.JSON* : La chaîne de connexion utilisée pour se connecter à une base de données locale est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="33f01-223">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="33f01-224">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="33f01-224">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="33f01-225">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="33f01-225">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="33f01-226">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="33f01-226">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="33f01-227">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="33f01-227">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="33f01-228">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="33f01-228">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="33f01-229">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="33f01-229">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="33f01-230">Examinez la méthode `ConfigureServices` dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="33f01-230">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="33f01-231">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="33f01-231">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="33f01-232">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="33f01-232">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="33f01-233">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="33f01-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="33f01-234">Mettre à jour la méthode Main</span><span class="sxs-lookup"><span data-stu-id="33f01-234">Update main</span></span>

<span data-ttu-id="33f01-235">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="33f01-235">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="33f01-236">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="33f01-236">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="33f01-237">Appelez [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="33f01-237">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="33f01-238">Supprimez le contexte une fois la méthode `EnsureCreated` exécutée.</span><span class="sxs-lookup"><span data-stu-id="33f01-238">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="33f01-239">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="33f01-239">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="33f01-240">`EnsureCreated` garantit que la base de données existe pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="33f01-240">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="33f01-241">Si elle existe, aucune action n’est effectuée.</span><span class="sxs-lookup"><span data-stu-id="33f01-241">If it exists, no action is taken.</span></span> <span data-ttu-id="33f01-242">Si elle n’existe pas, la base de données et tous ses schéma sont créés.</span><span class="sxs-lookup"><span data-stu-id="33f01-242">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="33f01-243">`EnsureCreated` n’utilise pas de migrations pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-243">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="33f01-244">Une base de données créée avec `EnsureCreated` ne peut pas être mise à jour à l’aide de migrations par la suite.</span><span class="sxs-lookup"><span data-stu-id="33f01-244">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="33f01-245">`EnsureCreated` est appelée au démarrage de l’application, ce qui active le flux de travail suivant :</span><span class="sxs-lookup"><span data-stu-id="33f01-245">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="33f01-246">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-246">Delete the DB.</span></span>
* <span data-ttu-id="33f01-247">Modification du schéma de base de données (par exemple, ajout d’un champ `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="33f01-247">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="33f01-248">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="33f01-248">Run the app.</span></span>
* <span data-ttu-id="33f01-249">`EnsureCreated` crée une base de données avec la colonne `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="33f01-249">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="33f01-250">`EnsureCreated` est pratique au début du développement quand le schéma évolue rapidement.</span><span class="sxs-lookup"><span data-stu-id="33f01-250">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="33f01-251">Plus loin dans le tutoriel, la base de données est supprimée et les migrations sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="33f01-251">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="33f01-252">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="33f01-252">Test the app</span></span>

<span data-ttu-id="33f01-253">Exécutez l’application et acceptez la politique de cookies.</span><span class="sxs-lookup"><span data-stu-id="33f01-253">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="33f01-254">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="33f01-254">This app doesn't keep personal information.</span></span> <span data-ttu-id="33f01-255">Vous pouvez en savoir plus sur la politique de cookies à la section [Prise en charge du règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="33f01-255">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="33f01-256">Sélectionnez le lien **Students**, puis **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="33f01-256">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="33f01-257">Testez les liens Edit, Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="33f01-257">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="33f01-258">Examiner le contexte de base de données SchoolContext</span><span class="sxs-lookup"><span data-stu-id="33f01-258">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="33f01-259">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-259">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="33f01-260">Le contexte de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="33f01-260">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="33f01-261">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-261">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="33f01-262">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="33f01-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="33f01-263">Mettez à jour *SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33f01-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="33f01-264">Le code en surbrillance crée une propriété [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="33f01-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="33f01-265">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="33f01-265">In EF Core terminology:</span></span>

* <span data-ttu-id="33f01-266">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-266">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="33f01-267">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="33f01-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="33f01-268">`DbSet<Enrollment>` et `DbSet<Course>` peuvent être omis.</span><span class="sxs-lookup"><span data-stu-id="33f01-268">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="33f01-269">EF Core les inclut implicitement, car l’entité `Student` référence l’entité `Enrollment`, et l’entité `Enrollment` référence l’entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="33f01-269">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="33f01-270">Pour ce didacticiel, conservez `DbSet<Enrollment>` et `DbSet<Course>` dans le `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="33f01-270">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="33f01-271">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="33f01-271">SQL Server Express LocalDB</span></span>

<span data-ttu-id="33f01-272">La chaîne de connexion spécifie [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="33f01-272">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="33f01-273">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="33f01-273">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="33f01-274">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="33f01-274">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="33f01-275">Par défaut, LocalDB crée des fichiers de base de données *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="33f01-275">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="33f01-276">Ajouter du code pour initialiser la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="33f01-276">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="33f01-277">EF Core crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="33f01-277">EF Core creates an empty DB.</span></span> <span data-ttu-id="33f01-278">Dans cette section, une méthode `Initialize` est écrite pour la remplir avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="33f01-278">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="33f01-279">Dans le dossier *Data*, créez un fichier de classe nommé *DbInitializer.cs* et ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33f01-279">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="33f01-280">Le code vérifie s’il existe des étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-280">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="33f01-281">S’il n’y en a pas, la base de données est initialisée avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="33f01-281">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="33f01-282">Il charge les données de test dans les tableaux plutôt que dans les collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="33f01-282">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="33f01-283">La méthode `EnsureCreated` crée automatiquement la base de données pour le contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-283">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="33f01-284">Si la base de données existe, `EnsureCreated` retourne sans modifier la base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-284">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="33f01-285">Dans *Program.cs*, modifiez la méthode `Main` pour appeler `Initialize` :</span><span class="sxs-lookup"><span data-stu-id="33f01-285">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="33f01-286">Supprimez les enregistrements d’étudiants et redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="33f01-286">Delete any student records and restart the app.</span></span> <span data-ttu-id="33f01-287">Si la base de données n’est pas initialisée, définissez un point d’arrêt dans `Initialize` pour diagnostiquer le problème.</span><span class="sxs-lookup"><span data-stu-id="33f01-287">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="33f01-288">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="33f01-288">View the DB</span></span>

<span data-ttu-id="33f01-289">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33f01-289">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="33f01-290">Dans SSOX, cliquez sur **(localdb)\MSSQLLocalDB > Bases de données > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="33f01-290">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="33f01-291">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="33f01-291">Expand the **Tables** node.</span></span>

<span data-ttu-id="33f01-292">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="33f01-292">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="33f01-293">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="33f01-293">Asynchronous code</span></span>

<span data-ttu-id="33f01-294">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="33f01-294">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="33f01-295">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="33f01-295">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="33f01-296">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="33f01-296">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="33f01-297">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="33f01-297">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="33f01-298">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="33f01-298">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="33f01-299">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="33f01-299">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="33f01-300">Le code asynchrone introduit néanmoins une petite surcharge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="33f01-300">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="33f01-301">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="33f01-301">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="33f01-302">Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="33f01-302">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="33f01-303">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="33f01-303">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="33f01-304">Génère des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="33f01-304">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="33f01-305">Crée automatiquement l’objet [Task](/dotnet/api/system.threading.tasks.task) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="33f01-305">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="33f01-306">Pour plus d’informations, consultez [Type de retour Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="33f01-306">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="33f01-307">Le type de retour implicite `Task` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="33f01-307">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="33f01-308">Le mot clé `await` fait en sorte que le compilateur fractionne la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="33f01-308">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="33f01-309">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="33f01-309">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="33f01-310">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="33f01-310">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="33f01-311">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="33f01-311">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="33f01-312">Voici quelques éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="33f01-312">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="33f01-313">Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="33f01-313">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="33f01-314">Cela comprend `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`,</span><span class="sxs-lookup"><span data-stu-id="33f01-314">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="33f01-315">mais pas les instructions qui ne font que changer un `IQueryable`, telles que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="33f01-315">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="33f01-316">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="33f01-316">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="33f01-317">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="33f01-317">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="33f01-318">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="33f01-318">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="33f01-319">Dans le didacticiel suivant, nous allons examiner les opérations CRUD de base (créer, lire, mettre à jour, supprimer).</span><span class="sxs-lookup"><span data-stu-id="33f01-319">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="33f01-320">Next</span><span class="sxs-lookup"><span data-stu-id="33f01-320">Next</span></span>](xref:data/ef-rp/crud)
