---
title: Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8
author: rick-anderson
description: Montre comment créer une application Pages Razor à l’aide d’Entity Framework Core.
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 99a8d158c896566c2f6e6c22e4b37b1956e21cbf
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="33fb9-103">Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8</span><span class="sxs-lookup"><span data-stu-id="33fb9-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="33fb9-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33fb9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33fb9-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core 2.0 MVC à l’aide d’Entity Framework (EF) Core 2.0 et Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="33fb9-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="33fb9-106">L’exemple d’application est un site web pour une université Contoso fictive.</span><span class="sxs-lookup"><span data-stu-id="33fb9-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="33fb9-107">Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs.</span><span class="sxs-lookup"><span data-stu-id="33fb9-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="33fb9-108">Cette page est la première d’une série de didacticiels qui expliquent comment générer l’exemple d’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="33fb9-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="33fb9-109">Télécharger ou afficher l’application complète.</span><span class="sxs-lookup"><span data-stu-id="33fb9-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="33fb9-110">[Télécharger les instructions](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="33fb9-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33fb9-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="33fb9-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<span data-ttu-id="33fb9-112">Connaissance des [Pages Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="33fb9-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="33fb9-113">Les programmeurs débutants doivent lire [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) avant de démarrer cette série.</span><span class="sxs-lookup"><span data-stu-id="33fb9-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="33fb9-114">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="33fb9-114">Troubleshooting</span></span>

<span data-ttu-id="33fb9-115">Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous pouvez généralement trouver la solution en comparant votre code à [l’étape terminée](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span><span class="sxs-lookup"><span data-stu-id="33fb9-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span></span> <span data-ttu-id="33fb9-116">Pour obtenir la liste des erreurs courantes et comment les résoudre, consultez [la section Dépannage du dernier didacticiel de la série](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="33fb9-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="33fb9-117">Si vous n’y trouvez pas ce dont vous avez besoin, vous pouvez publier une question sur [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pour [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="33fb9-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="33fb9-118">Cette série de didacticiels s’appuie sur les actions effectuées dans les didacticiels précédents.</span><span class="sxs-lookup"><span data-stu-id="33fb9-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="33fb9-119">Pensez à enregistrer une copie du projet après avoir fini chaque didacticiel.</span><span class="sxs-lookup"><span data-stu-id="33fb9-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="33fb9-120">Si vous rencontrez des problèmes, vous pourrez ainsi recommencer à partir du didacticiel précédent, au lieu de revenir au début.</span><span class="sxs-lookup"><span data-stu-id="33fb9-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="33fb9-121">Vous pouvez également télécharger une [phase terminée](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) et recommencer à l’aide de cette phase terminée.</span><span class="sxs-lookup"><span data-stu-id="33fb9-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="33fb9-122">L’application web Contoso University</span><span class="sxs-lookup"><span data-stu-id="33fb9-122">The Contoso University web app</span></span>

<span data-ttu-id="33fb9-123">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="33fb9-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="33fb9-124">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="33fb9-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="33fb9-125">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="33fb9-125">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index.png)

![Page de modification des étudiants](intro/_static/student-edit.png)

<span data-ttu-id="33fb9-128">Le style de l’interface utilisateur de ce site est proche de ce qui est généré par les modèles prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="33fb9-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="33fb9-129">Le didacticiel est axé sur Core EF avec des pages Razor, et non sur l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="33fb9-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="33fb9-130">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="33fb9-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="33fb9-131">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="33fb9-132">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33fb9-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="33fb9-133">Nommez le projet **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="33fb9-134">Il est important de nommer le projet *ContosoUniversity* afin que les espaces de noms correspondent quand le code est copié/collé.</span><span class="sxs-lookup"><span data-stu-id="33fb9-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="33fb9-135">![Nouvelle application web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="33fb9-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="33fb9-136">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="33fb9-137">![Application web (pages Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="33fb9-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="33fb9-138">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur</span><span class="sxs-lookup"><span data-stu-id="33fb9-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="33fb9-139">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="33fb9-139">Set up the site style</span></span>

<span data-ttu-id="33fb9-140">Quelques changements permettent de définir le menu, la disposition et la page d’accueil du site.</span><span class="sxs-lookup"><span data-stu-id="33fb9-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="33fb9-141">Ouvrez *Pages/_Layout.cshtml* et apportez les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="33fb9-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="33fb9-142">Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="33fb9-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="33fb9-143">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="33fb9-143">There are three occurrences.</span></span>

* <span data-ttu-id="33fb9-144">Ajoutez des entrées de menu pour **Students**, **Courses**, **Instructors** et **Departments**, et supprimez l’entrée de menu **Contact**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="33fb9-145">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="33fb9-145">The changes are highlighted.</span></span> <span data-ttu-id="33fb9-146">(Tout le balisage n’est *pas* affiché.)</span><span class="sxs-lookup"><span data-stu-id="33fb9-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="33fb9-147">Dans *Pages/Index.cshtml*, remplacez le contenu du fichier par le code suivant afin de remplacer le texte sur ASP.NET et MVC par le texte concernant cette application :</span><span class="sxs-lookup"><span data-stu-id="33fb9-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="33fb9-148">Appuyez sur CTRL+F5 pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="33fb9-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="33fb9-149">La page d’accueil s’affiche avec les onglets créés dans les didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="33fb9-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Page d’accueil de Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="33fb9-151">Créer le modèle de données</span><span class="sxs-lookup"><span data-stu-id="33fb9-151">Create the data model</span></span>

<span data-ttu-id="33fb9-152">Créez des classes d’entités pour l’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="33fb9-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="33fb9-153">Commencez par les trois entités suivantes :</span><span class="sxs-lookup"><span data-stu-id="33fb9-153">Start with the following three entities:</span></span>

![Diagramme de modèle de données Cours-Inscription-Étudiant](intro/_static/data-model-diagram.png)

<span data-ttu-id="33fb9-155">Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="33fb9-156">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="33fb9-157">Un étudiant peut s’inscrire à autant de cours qu’il le souhaite.</span><span class="sxs-lookup"><span data-stu-id="33fb9-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="33fb9-158">Un cours peut avoir une quantité illimitée d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="33fb9-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="33fb9-159">Dans les sections suivantes, une classe pour chacune de ces entités est créée.</span><span class="sxs-lookup"><span data-stu-id="33fb9-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="33fb9-160">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="33fb9-160">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

<span data-ttu-id="33fb9-162">Créez un dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="33fb9-162">Create a *Models* folder.</span></span> <span data-ttu-id="33fb9-163">Dans le dossier *Models*, créez un fichier de classe nommé *Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="33fb9-164">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="33fb9-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="33fb9-165">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="33fb9-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="33fb9-166">Dans `classnameID`, `classname` est le nom de la classe, comme `Student` dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="33fb9-166">In `classnameID`, `classname` is the name of the class, such as `Student` in the preceding example.</span></span>

<span data-ttu-id="33fb9-167">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="33fb9-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="33fb9-168">Les propriétés de navigation établissent des liaisons à d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="33fb9-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="33fb9-169">Ici, la propriété `Enrollments` d’un `Student entity` contient toutes les entités `Enrollment` associées à ce `Student`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="33fb9-170">Par exemple, si une ligne Student dans la base de données a deux lignes Enrollment associées, la propriété de navigation `Enrollments` contient ces deux entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="33fb9-171">Une ligne `Enrollment` associée est une ligne qui contient la valeur de clé primaire de cette étudiant dans la colonne `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="33fb9-172">Par exemple, supposez que l’étudiant avec ID=1 a deux lignes dans la table `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="33fb9-173">La table `Enrollment` a deux lignes avec `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="33fb9-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="33fb9-174">`StudentID` est une clé étrangère dans la table `Enrollment` qui spécifie l’étudiant dans la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="33fb9-175">Si une propriété de navigation peut contenir plusieurs entités, la propriété de navigation doit être un type de liste, tel que `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="33fb9-176">Vous pouvez spécifier `ICollection<T>`, ou un type tel que `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="33fb9-177">Quand vous utilisez `ICollection<T>`, EF Core crée une collection `HashSet<T>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="33fb9-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="33fb9-178">Les propriétés de navigation qui contiennent plusieurs entités proviennent de relations plusieurs-à-plusieurs et un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="33fb9-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="33fb9-179">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="33fb9-179">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="33fb9-181">Dans le dossier *Models*, créez un fichier *Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="33fb9-182">La propriété `EnrollmentID` est la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="33fb9-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="33fb9-183">Cette entité utilise le modèle `classnameID` au lieu de `ID` comme l’entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="33fb9-184">En général, les développeurs choisissent un modèle et l’utilisent dans tout le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="33fb9-185">Dans un prochain didacticiel, nous verrons qu’utiliser ID sans classname simplifie l’implémentation de l’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="33fb9-186">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="33fb9-187">Le point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` est nullable.</span><span class="sxs-lookup"><span data-stu-id="33fb9-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="33fb9-188">Une note (Grade) qui a la valeur Null est différente d’une note égale à zéro : la valeur Null signifie qu’une note n’est pas connue ou n’a pas encore été affectée.</span><span class="sxs-lookup"><span data-stu-id="33fb9-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="33fb9-189">La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="33fb9-190">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="33fb9-191">L’entité `Student` diffère de la propriété de navigation `Student.Enrollments`, qui contient plusieurs entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="33fb9-192">La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="33fb9-193">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="33fb9-194">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="33fb9-195">Par exemple, `StudentID` pour la propriété de navigation `Student`, puisque la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="33fb9-196">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="33fb9-197">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="33fb9-198">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="33fb9-198">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="33fb9-200">Dans le dossier *Models*, créez un fichier *Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="33fb9-201">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="33fb9-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="33fb9-202">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="33fb9-203">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="33fb9-204">Créer le contexte de base de données SchoolContext</span><span class="sxs-lookup"><span data-stu-id="33fb9-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="33fb9-205">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="33fb9-206">Le contexte de données est dérivé de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="33fb9-207">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="33fb9-208">Dans ce projet, la classe se nomme `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="33fb9-209">Dans le dossier du projet, créez un dossier nommé *Data*.</span><span class="sxs-lookup"><span data-stu-id="33fb9-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="33fb9-210">Dans le dossier *Data*, créez *SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="33fb9-211">Ce code crée une propriété `DbSet` pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="33fb9-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="33fb9-212">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="33fb9-212">In EF Core terminology:</span></span>

* <span data-ttu-id="33fb9-213">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="33fb9-214">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="33fb9-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="33fb9-215">`DbSet<Enrollment>` et `DbSet<Course>` peut être omis.</span><span class="sxs-lookup"><span data-stu-id="33fb9-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="33fb9-216">EF Core les inclut implicitement, car l’entité `Student` référence l’entité `Enrollment`, et l’entité `Enrollment` référence l’entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="33fb9-217">Pour ce didacticiel, conservez `DbSet<Enrollment>` et `DbSet<Course>` dans le `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="33fb9-218">Quand la base de données est créée, EF Core crée des tables dont les noms sont identiques à ceux de la propriété `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="33fb9-219">Les noms des propriétés pour les collections sont généralement au pluriel (Students plutôt que Student).</span><span class="sxs-lookup"><span data-stu-id="33fb9-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="33fb9-220">Les développeurs ne sont pas tous d’accord sur la nécessité d’utiliser des noms de tables au pluriel.</span><span class="sxs-lookup"><span data-stu-id="33fb9-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="33fb9-221">Pour ces didacticiels, le comportement par défaut est remplacé en spécifiant des noms de tables au singulier dans le DbContext.</span><span class="sxs-lookup"><span data-stu-id="33fb9-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="33fb9-222">Pour spécifier des noms de tables au singulier, ajoutez le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="33fb9-223">Inscrire le contexte avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="33fb9-223">Register the context with dependency injection</span></span>

<span data-ttu-id="33fb9-224">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="33fb9-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="33fb9-225">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="33fb9-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="33fb9-226">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="33fb9-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="33fb9-227">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="33fb9-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="33fb9-228">Pour inscrire `SchoolContext` en tant que service, ouvrez *Startup.cs* et ajoutez les lignes en surbrillance à la méthode `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="33fb9-229">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="33fb9-230">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="33fb9-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="33fb9-231">Ajoutez des instructions `using` pour les espaces de noms `ContosoUniversity.Data` et `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="33fb9-232">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="33fb9-232">Build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="33fb9-233">Ouvrez le fichier *appsettings.json* et ajoutez une chaîne de connexion comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="33fb9-234">La chaîne de connexion précédente utilise `ConnectRetryCount=0` pour empêcher le blocage de [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).</span><span class="sxs-lookup"><span data-stu-id="33fb9-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="33fb9-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="33fb9-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="33fb9-236">La chaîne de connexion spécifie une base de données SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="33fb9-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="33fb9-237">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="33fb9-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="33fb9-238">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="33fb9-238">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="33fb9-239">Par défaut, LocalDB crée des fichiers de base de données *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="33fb9-240">Ajouter du code pour initialiser la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="33fb9-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="33fb9-241">EF Core crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="33fb9-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="33fb9-242">Dans cette section, une méthode *Seed* est écrite afin de la renseigner avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="33fb9-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="33fb9-243">Dans le dossier *Data*, créez un fichier de classe nommé *DbInitializer.cs* et ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="33fb9-244">Le code vérifie s’il existe des étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="33fb9-245">S’il n’y en a pas, la base de données est amorcée avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="33fb9-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="33fb9-246">Il charge les données de test dans des tableaux plutôt que des collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="33fb9-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="33fb9-247">La méthode `EnsureCreated` crée automatiquement la base de données pour le contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="33fb9-248">Si la base de données existe, `EnsureCreated` retourne sans modifier la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="33fb9-249">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="33fb9-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="33fb9-250">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="33fb9-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="33fb9-251">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="33fb9-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="33fb9-252">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="33fb9-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="33fb9-253">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="33fb9-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="33fb9-254">Lors de la première exécution de l’application, la base de données est créée et amorcée avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="33fb9-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="33fb9-255">Quand le modèle de données est mis à jour :</span><span class="sxs-lookup"><span data-stu-id="33fb9-255">When the data model is updated:</span></span>
* <span data-ttu-id="33fb9-256">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-256">Delete the DB.</span></span>
* <span data-ttu-id="33fb9-257">Mettez à jour la méthode seed.</span><span class="sxs-lookup"><span data-stu-id="33fb9-257">Update the seed method.</span></span>
* <span data-ttu-id="33fb9-258">Exécutez l’application, et une nouvelle base de données amorcée est créée.</span><span class="sxs-lookup"><span data-stu-id="33fb9-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="33fb9-259">Dans les didacticiels suivants, la base de données est mise à jour quand le modèle de données change, sans supprimer et recréer la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="33fb9-260">Ajouter des outils de génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="33fb9-260">Add scaffold tooling</span></span>

<span data-ttu-id="33fb9-261">Dans cette section, nous utilisons la console du Gestionnaire de package pour ajouter le package de génération de code web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33fb9-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="33fb9-262">Ce package est nécessaire à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="33fb9-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="33fb9-263">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="33fb9-264">Dans la console du Gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="33fb9-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="33fb9-265">La commande précédente ajoute les packages NuGet au fichier \*.csproj :</span><span class="sxs-lookup"><span data-stu-id="33fb9-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="33fb9-266">Effectuer la génération de modèles automatique pour le modèle</span><span class="sxs-lookup"><span data-stu-id="33fb9-266">Scaffold the model</span></span>

* <span data-ttu-id="33fb9-267">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="33fb9-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="33fb9-268">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="33fb9-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

<span data-ttu-id="33fb9-269">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="33fb9-269">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="33fb9-270">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="33fb9-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="33fb9-271">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="33fb9-271">Build the project.</span></span> <span data-ttu-id="33fb9-272">La build génère des erreurs telles que celle-ci :</span><span class="sxs-lookup"><span data-stu-id="33fb9-272">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="33fb9-273">Remplacez globalement `_context.Student` par `_context.Students` (autrement dit, ajoutez un « s » à `Student`).</span><span class="sxs-lookup"><span data-stu-id="33fb9-273">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="33fb9-274">7 occurrences sont trouvées et mises à jour.</span><span class="sxs-lookup"><span data-stu-id="33fb9-274">7 occurrences are found and updated.</span></span> <span data-ttu-id="33fb9-275">Nous espérons résoudre [ce bogue](https://github.com/aspnet/Scaffolding/issues/633) dans la prochaine version.</span><span class="sxs-lookup"><span data-stu-id="33fb9-275">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="33fb9-276">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="33fb9-276">Test the app</span></span>

<span data-ttu-id="33fb9-277">Exécutez l’application et sélectionnez le lien **Students**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-277">Run the app and select the **Students** link.</span></span> <span data-ttu-id="33fb9-278">Le lien **Students** figure en haut de la page, mais cela dépend de la largeur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="33fb9-278">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="33fb9-279">Si le lien **Students** n’est pas visible, cliquez sur l’icône de navigation dans le coin supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="33fb9-279">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Page d’accueil étroite de Contoso University](intro/_static/home-page-narrow.png)

<span data-ttu-id="33fb9-281">Testez les liens **Create**, **Edit** et **Details**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-281">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="33fb9-282">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="33fb9-282">View the DB</span></span>

<span data-ttu-id="33fb9-283">Quand l’application est lancée, `DbInitializer.Initialize` appelle `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-283">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="33fb9-284">`EnsureCreated` détecte si la base de données existe et en crée une si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="33fb9-284">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="33fb9-285">Si la base de données ne contient aucun étudiant, la méthode `Initialize` en ajoute.</span><span class="sxs-lookup"><span data-stu-id="33fb9-285">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="33fb9-286">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33fb9-286">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="33fb9-287">Dans SSOX, cliquez sur **(localdb)\MSSQLLocalDB > Bases de données > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-287">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="33fb9-288">Développez le nœud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="33fb9-288">Expand the **Tables** node.</span></span>

<span data-ttu-id="33fb9-289">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="33fb9-289">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="33fb9-290">Les fichiers de base de données <em>.mdf</em> et <em>.ldf</em> se trouvent dans le dossier <em>C:\Utilisateurs\\<yourusername></em>.</span><span class="sxs-lookup"><span data-stu-id="33fb9-290">The <em>.mdf</em> and <em>.ldf</em> DB files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="33fb9-291">`EnsureCreated` est appelée au démarrage de l’application, ce qui active le flux de travail suivant :</span><span class="sxs-lookup"><span data-stu-id="33fb9-291">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="33fb9-292">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-292">Delete the DB.</span></span>
* <span data-ttu-id="33fb9-293">Modification du schéma de base de données (par exemple, ajout d’un champ `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="33fb9-293">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="33fb9-294">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="33fb9-294">Run the app.</span></span>

<span data-ttu-id="33fb9-295">`EnsureCreated` crée une base de données avec la colonne `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-295">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="33fb9-296">Conventions</span><span class="sxs-lookup"><span data-stu-id="33fb9-296">Conventions</span></span>

<span data-ttu-id="33fb9-297">La quantité de code écrite pour qu’EF Core crée une base de données complète est minime en raison de l’utilisation des conventions, ou des hypothèses émises par EF Core.</span><span class="sxs-lookup"><span data-stu-id="33fb9-297">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="33fb9-298">Les noms des propriétés `DbSet` sont utilisés comme noms de tables.</span><span class="sxs-lookup"><span data-stu-id="33fb9-298">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="33fb9-299">Pour les entités non référencées par une propriété `DbSet`, les noms des classes d’entités sont utilisés comme noms de tables.</span><span class="sxs-lookup"><span data-stu-id="33fb9-299">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="33fb9-300">Les noms des propriétés d’entités sont utilisés comme noms de colonnes.</span><span class="sxs-lookup"><span data-stu-id="33fb9-300">Entity property names are used for column names.</span></span>

* <span data-ttu-id="33fb9-301">Les propriétés d’entité nommées ID ou classnameID sont reconnues comme propriétés de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="33fb9-301">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="33fb9-302">Une propriété est interprétée comme propriété de clé étrangère si elle se nomme *<navigation property name><primary key property name>* (par exemple `StudentID` pour la propriété de navigation `Student`, puisque la clé primaire de l’entité `Student` est `ID`).</span><span class="sxs-lookup"><span data-stu-id="33fb9-302">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="33fb9-303">Les propriétés de clé étrangère peuvent se nommer *<primary key property name>* (par exemple `EnrollmentID`, puisque la clé primaire de l’entité `Enrollment` est `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="33fb9-303">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="33fb9-304">Le comportement conventionnel peut être remplacé.</span><span class="sxs-lookup"><span data-stu-id="33fb9-304">Conventional behavior can be overridden.</span></span> <span data-ttu-id="33fb9-305">Par exemple, les noms des tables peuvent être spécifiés explicitement, comme indiqué plus haut dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="33fb9-305">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="33fb9-306">Les noms des colonnes peuvent être définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="33fb9-306">The column names can be explicitly set.</span></span> <span data-ttu-id="33fb9-307">Les clés primaires et les clés étrangères peuvent être définies explicitement.</span><span class="sxs-lookup"><span data-stu-id="33fb9-307">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="33fb9-308">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="33fb9-308">Asynchronous code</span></span>

<span data-ttu-id="33fb9-309">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="33fb9-309">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="33fb9-310">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="33fb9-310">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="33fb9-311">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="33fb9-311">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="33fb9-312">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="33fb9-312">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="33fb9-313">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="33fb9-313">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="33fb9-314">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="33fb9-314">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="33fb9-315">Le code asynchrone introduit néanmoins une petite surcharge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="33fb9-315">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="33fb9-316">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="33fb9-316">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="33fb9-317">Dans le code suivant, le mot clé `async`, la valeur de retour `Task<T>`, le mot clé `await` et la méthode `ToListAsync` provoquent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="33fb9-317">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="33fb9-318">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="33fb9-318">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="33fb9-319">Génère des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="33fb9-319">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="33fb9-320">Crée automatiquement l’objet [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="33fb9-320">Automatically create the [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="33fb9-321">Pour plus d’informations, consultez [Type de retour Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="33fb9-321">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="33fb9-322">Le type de retour implicite `Task` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="33fb9-322">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="33fb9-323">Le mot clé `await` fait en sorte que le compilateur fractionne la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="33fb9-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="33fb9-324">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="33fb9-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="33fb9-325">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="33fb9-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="33fb9-326">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="33fb9-327">Voici quelques éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="33fb9-327">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="33fb9-328">Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="33fb9-328">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="33fb9-329">Cela comprend `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`,</span><span class="sxs-lookup"><span data-stu-id="33fb9-329">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="33fb9-330">mais pas les instructions qui ne font que changer un `IQueryable`, telles que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="33fb9-330">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="33fb9-331">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="33fb9-331">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="33fb9-332">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="33fb9-332">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="33fb9-333">Pour plus d’informations sur les méthodes asynchrones dans .NET, consultez [Vue d’ensemble d’async](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="33fb9-333">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="33fb9-334">Dans le didacticiel suivant, nous allons examiner les opérations CRUD de base (créer, lire, mettre à jour, supprimer).</span><span class="sxs-lookup"><span data-stu-id="33fb9-334">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="33fb9-335">Next</span><span class="sxs-lookup"><span data-stu-id="33fb9-335">Next</span></span>](xref:data/ef-rp/crud)
