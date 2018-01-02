---
title: Pages Razor avec Entity Framework Core - didacticiel 1 de 8
author: rick-anderson
description: "Montre comment créer une application de Pages Razor à l’aide d’Entity Framework Core"
keywords: Didacticiel de ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: acbd987438edeea13f29547dc471f9a211e87b04
ms.sourcegitcommit: 1de159820a572c08955ee77cc8b1caa3d7aa938c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="e569e-104">Prise en main de Pages Razor et Entity Framework Core, à l’aide de Visual Studio (1 / 8)</span><span class="sxs-lookup"><span data-stu-id="e569e-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="e569e-105">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e569e-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e569e-106">L’exemple d’application web de Contoso University montre comment créer des applications web ASP.NET Core 2.0 MVC à l’aide d’Entity Framework (EF) 2.0 et Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e569e-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="e569e-107">L’exemple d’application est un site web pour une université fictif de Contoso.</span><span class="sxs-lookup"><span data-stu-id="e569e-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="e569e-108">Il inclut des fonctionnalités telles que leur admission d’étudiant, la création de cours et les affectations de formateur.</span><span class="sxs-lookup"><span data-stu-id="e569e-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="e569e-109">Cette page est la première d’une série de didacticiels qui expliquent comment générer l’exemple d’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="e569e-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="e569e-110">Télécharger ou afficher l’application terminée.</span><span class="sxs-lookup"><span data-stu-id="e569e-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="e569e-111">[Les instructions de téléchargement](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="e569e-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e569e-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e569e-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="e569e-113">Vous êtes familiarisé avec [Pages Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="e569e-113">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="e569e-114">Les programmeurs doivent terminer [prise en main Pages Razor](xref:tutorials/razor-pages/razor-pages-start) avant de démarrer cette série.</span><span class="sxs-lookup"><span data-stu-id="e569e-114">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e569e-115">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="e569e-115">Troubleshooting</span></span>

<span data-ttu-id="e569e-116">Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous trouverez généralement la solution en comparant votre code pour le [terminé la phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) ou [projet achevé](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="e569e-116">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="e569e-117">Pour obtenir la liste des erreurs courantes et comment les résoudre, consultez [la section de dépannage du didacticiel dernière dans la série](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="e569e-117">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="e569e-118">Si vous ne trouvez pas ce dont vous avez besoin il, vous pouvez publier une question dans StackOverflow.com pour [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="e569e-118">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="e569e-119">Cette série de didacticiels s’appuie sur l’action effectuée dans les didacticiels antérieures.</span><span class="sxs-lookup"><span data-stu-id="e569e-119">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="e569e-120">Pensez à enregistrer une copie du projet après chaque didacticiel réussite.</span><span class="sxs-lookup"><span data-stu-id="e569e-120">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="e569e-121">Si vous rencontrez des problèmes, vous pouvez recommencer à partir du didacticiel précédent, au lieu de revenir au début.</span><span class="sxs-lookup"><span data-stu-id="e569e-121">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="e569e-122">Vous pouvez également télécharger un [terminé la phase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) et tout recommencer à l’aide de l’étape terminée.</span><span class="sxs-lookup"><span data-stu-id="e569e-122">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="e569e-123">L’application web de Contoso University</span><span class="sxs-lookup"><span data-stu-id="e569e-123">The Contoso University web app</span></span>

<span data-ttu-id="e569e-124">L’application générée dans ces didacticiels est un site web de base university.</span><span class="sxs-lookup"><span data-stu-id="e569e-124">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="e569e-125">Les utilisateurs peuvent afficher et mettre à jour des étudiants, les cours et les informations de formateur.</span><span class="sxs-lookup"><span data-stu-id="e569e-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="e569e-126">Voici quelques exemples d’écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e569e-126">Here are a few of the screens created in the tutorial.</span></span>

![Page d’Index les étudiants](intro/_static/students-index.png)

![Page de modification des étudiants](intro/_static/student-edit.png)

<span data-ttu-id="e569e-129">Le style de l’interface utilisateur de ce site est proche de ce qui est généré par les modèles prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="e569e-129">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="e569e-130">Le didacticiel focus est sur Core EF comportant des Pages Razor, pas l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e569e-130">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="e569e-131">Créer une application de web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="e569e-131">Create a Razor Pages web app</span></span>

* <span data-ttu-id="e569e-132">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e569e-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e569e-133">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e569e-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="e569e-134">Nommez le projet **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="e569e-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="e569e-135">Il est important de nommer le projet *ContosoUniversity* afin que les espaces de noms corresponde à lorsque le code est copiées/collées.</span><span class="sxs-lookup"><span data-stu-id="e569e-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="e569e-136">![Nouvelle application web ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="e569e-136">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="e569e-137">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="e569e-137">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="e569e-138">![Application web (pages Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="e569e-138">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="e569e-139">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur</span><span class="sxs-lookup"><span data-stu-id="e569e-139">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="e569e-140">Définir le style de site</span><span class="sxs-lookup"><span data-stu-id="e569e-140">Set up the site style</span></span>

<span data-ttu-id="e569e-141">Quelques modifications définissent le menu du site, la disposition et la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="e569e-141">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="e569e-142">Ouvrez *Pages/_Layout.cshtml* et apportez les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="e569e-142">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="e569e-143">Remplacer chaque occurrence de « ContosoUniversity » par « Contoso université. »</span><span class="sxs-lookup"><span data-stu-id="e569e-143">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="e569e-144">Il existe trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="e569e-144">There are three occurrences.</span></span>

* <span data-ttu-id="e569e-145">Ajouter des entrées de menu pour **étudiants**, **cours**, **instructeurs**, et **départements**et supprimer la **Contact** entrée de menu.</span><span class="sxs-lookup"><span data-stu-id="e569e-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="e569e-146">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="e569e-146">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="e569e-147">Dans *Pages/Index.cshtml*, remplacez le contenu du fichier par le code suivant pour remplacer le texte sur ASP.NET et MVC avec du texte sur cette application :</span><span class="sxs-lookup"><span data-stu-id="e569e-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="e569e-148">Appuyez sur CTRL+F5 pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="e569e-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="e569e-149">La page d’accueil s’affiche avec les onglets créés dans les didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="e569e-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Page d’accueil de l’université Contoso](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="e569e-151">Créer le modèle de données</span><span class="sxs-lookup"><span data-stu-id="e569e-151">Create the data model</span></span>

<span data-ttu-id="e569e-152">Créer des classes d’entité pour l’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="e569e-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="e569e-153">Démarrer avec les trois entités suivantes :</span><span class="sxs-lookup"><span data-stu-id="e569e-153">Start with the following three entities:</span></span>

![Diagramme de modèle de données de l’étudiant de l’inscription de cours](intro/_static/data-model-diagram.png)

<span data-ttu-id="e569e-155">Il existe une relation un-à-plusieurs entre `Student` et `Enrollment` entités.</span><span class="sxs-lookup"><span data-stu-id="e569e-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="e569e-156">Il existe une relation un-à-plusieurs entre `Course` et `Enrollment` entités.</span><span class="sxs-lookup"><span data-stu-id="e569e-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="e569e-157">Un étudiant peut inscrire dans n’importe quel nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="e569e-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="e569e-158">Un cours peut avoir n’importe quel nombre d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="e569e-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="e569e-159">Dans les sections suivantes, une classe pour chacune de ces entités est créée.</span><span class="sxs-lookup"><span data-stu-id="e569e-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="e569e-160">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="e569e-160">The Student entity</span></span>

![Diagramme de l’entité étudiant](intro/_static/student-entity.png)

<span data-ttu-id="e569e-162">Créer un *modèles* dossier.</span><span class="sxs-lookup"><span data-stu-id="e569e-162">Create a *Models* folder.</span></span> <span data-ttu-id="e569e-163">Dans le *modèles* dossier, créez un fichier de classe nommé *Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="e569e-164">Le `ID` propriété devient la colonne de clé primaire de la table de base de données (DB) qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="e569e-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="e569e-165">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="e569e-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="e569e-166">Le `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="e569e-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="e569e-167">Propriétés de navigation se lier à d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="e569e-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="e569e-168">Dans ce cas, le `Enrollments` propriété d’un `Student entity` contient toutes les `Enrollment` entités qui sont associées à cette `Student`.</span><span class="sxs-lookup"><span data-stu-id="e569e-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="e569e-169">Par exemple, si une ligne de l’étudiant dans la base de données sont deux en relation avec l’inscription, le `Enrollments` propriété de navigation contient ces deux `Enrollment` entités.</span><span class="sxs-lookup"><span data-stu-id="e569e-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="e569e-170">Un `Enrollment` ligne est une ligne qui contient la valeur de clé primaire qui student dans les `StudentID` colonne.</span><span class="sxs-lookup"><span data-stu-id="e569e-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="e569e-171">Par exemple, supposons que le stagiaire avec l’ID = 1 a deux lignes la `Enrollment` table.</span><span class="sxs-lookup"><span data-stu-id="e569e-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="e569e-172">Le `Enrollment` table comporte deux lignes avec `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="e569e-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="e569e-173">`StudentID`est une clé étrangère dans la `Enrollment` table qui spécifie l’étudiant dans la `Student` table.</span><span class="sxs-lookup"><span data-stu-id="e569e-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="e569e-174">Si une propriété de navigation peut contenir plusieurs entités, la propriété de navigation doit être du type de la liste, tel que `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="e569e-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="e569e-175">`ICollection<T>`peut être spécifié, ou un type tel que `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="e569e-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="e569e-176">Lorsque `ICollection<T>` est utilisé, de EF Core crée un `HashSet<T>` collection par défaut.</span><span class="sxs-lookup"><span data-stu-id="e569e-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="e569e-177">Les propriétés de navigation qui contiennent plusieurs entités proviennent des relations plusieurs-à-plusieurs et un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="e569e-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="e569e-178">L’entité de l’inscription</span><span class="sxs-lookup"><span data-stu-id="e569e-178">The Enrollment entity</span></span>

![Diagramme de l’entité d’inscription](intro/_static/enrollment-entity.png)

<span data-ttu-id="e569e-180">Dans le *modèles* dossier, créez *Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="e569e-181">Le `EnrollmentID` propriété est la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="e569e-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="e569e-182">Cette entité utilise le `classnameID` de modèle au lieu de `ID` comme le `Student` entité.</span><span class="sxs-lookup"><span data-stu-id="e569e-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="e569e-183">En général, les développeurs choisir un modèle et l’utiliser dans tout le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="e569e-184">Dans un prochain didacticiel à l’aide de code sans classname est indiqué pour le rendre plus facile à implémenter l’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="e569e-185">Le `Grade` propriété est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="e569e-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="e569e-186">Le point d’interrogation après la `Grade` déclaration de type indique que le `Grade` propriété est nullable.</span><span class="sxs-lookup"><span data-stu-id="e569e-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="e569e-187">Un niveau qui a la valeur null est différent de zéro une note : null signifie une note n’est pas connu ou n’a pas encore été affectée.</span><span class="sxs-lookup"><span data-stu-id="e569e-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="e569e-188">Le `StudentID` propriété est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="e569e-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="e569e-189">Un `Enrollment` entité est associée à un `Student` entité, par conséquent, la propriété contienne un seul `Student` entité.</span><span class="sxs-lookup"><span data-stu-id="e569e-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="e569e-190">Le `Student` entité diffère de la `Student.Enrollments` propriété de navigation, qui contient plusieurs `Enrollment` entités.</span><span class="sxs-lookup"><span data-stu-id="e569e-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="e569e-191">Le `CourseID` propriété est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="e569e-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="e569e-192">Un `Enrollment` entité est associée à un `Course` entité.</span><span class="sxs-lookup"><span data-stu-id="e569e-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="e569e-193">EF Core interprète une propriété comme une clé étrangère s’il est nommé `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="e569e-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="e569e-194">Par exemple,`StudentID` pour le `Student` propriété de navigation, dans la mesure où le `Student` la clé primaire de l’entité est `ID`.</span><span class="sxs-lookup"><span data-stu-id="e569e-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="e569e-195">Propriétés de clé étrangère peuvent également être nommées `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="e569e-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="e569e-196">Par exemple, `CourseID` depuis le `Course` la clé primaire de l’entité est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="e569e-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="e569e-197">L’entité de cours</span><span class="sxs-lookup"><span data-stu-id="e569e-197">The Course entity</span></span>

![Diagramme d’entité de cours](intro/_static/course-entity.png)

<span data-ttu-id="e569e-199">Dans le *modèles* dossier, créez *Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="e569e-200">Le `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="e569e-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="e569e-201">A `Course` entité peut être associée à un nombre quelconque de `Enrollment` entités.</span><span class="sxs-lookup"><span data-stu-id="e569e-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="e569e-202">Le `DatabaseGenerated` attribut permet à l’application spécifier la clé primaire plutôt que d’avoir de la base de données de sa génération.</span><span class="sxs-lookup"><span data-stu-id="e569e-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="e569e-203">Créer le contexte de base de données SchoolContext</span><span class="sxs-lookup"><span data-stu-id="e569e-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="e569e-204">La classe principale qui coordonne les fonctionnalités principales d’EF pour un modèle de données spécifiée est la classe de contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="e569e-205">Le contexte de données est dérivé de `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e569e-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="e569e-206">Le contexte de données spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="e569e-207">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="e569e-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="e569e-208">Dans le dossier du projet, créez un dossier nommé *données*.</span><span class="sxs-lookup"><span data-stu-id="e569e-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="e569e-209">Dans le *données* créer de dossier *SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="e569e-210">Ce code crée un `DbSet` propriété pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="e569e-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="e569e-211">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="e569e-211">In EF Core terminology:</span></span>

* <span data-ttu-id="e569e-212">En général, une entité correspond à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="e569e-213">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="e569e-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e569e-214">`DbSet<Enrollment>`et `DbSet<Course>` peut être omis.</span><span class="sxs-lookup"><span data-stu-id="e569e-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="e569e-215">EF Core inclut les implicitement, car le `Student` références d’entité le `Enrollment` entité et le `Enrollment` références d’entité le `Course` entité.</span><span class="sxs-lookup"><span data-stu-id="e569e-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="e569e-216">Pour ce didacticiel, conservez `DbSet<Enrollment>` et `DbSet<Course>` dans le `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="e569e-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="e569e-217">Lorsque la base de données est créé, EF Core crée des tables qui ont des noms identique à la `DbSet` les noms de propriété.</span><span class="sxs-lookup"><span data-stu-id="e569e-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="e569e-218">Les noms de propriété pour les collections sont en général au pluriel (étudiants plutôt qu’étudiant).</span><span class="sxs-lookup"><span data-stu-id="e569e-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="e569e-219">Les développeurs sont en désaccord sur si les noms de table doivent être au pluriel.</span><span class="sxs-lookup"><span data-stu-id="e569e-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="e569e-220">Pour ces didacticiels, le comportement par défaut est substitué en spécifiant des noms de table unique dans le DbContext.</span><span class="sxs-lookup"><span data-stu-id="e569e-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="e569e-221">Pour spécifier les noms de table au singulier, ajoutez le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="e569e-222">Enregistrer le contexte avec l’injection de dépendance</span><span class="sxs-lookup"><span data-stu-id="e569e-222">Register the context with dependency injection</span></span>

<span data-ttu-id="e569e-223">ASP.NET Core inclut [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e569e-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e569e-224">Services (par exemple, le contexte de la base de données EF Core) sont enregistrés avec l’injection de dépendance au cours du démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e569e-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="e569e-225">Composants qui requièrent ces services (tels que les Pages Razor) sont fournis à ces services via les paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="e569e-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e569e-226">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e569e-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="e569e-227">Pour inscrire `SchoolContext` en tant que service, ouvrez *Startup.cs*et ajoutez les lignes en surbrillance vers le `ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="e569e-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="e569e-228">Le nom de la chaîne de connexion est passé dans le contexte en appelant une méthode sur un `DbContextOptionsBuilder` objet.</span><span class="sxs-lookup"><span data-stu-id="e569e-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="e569e-229">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir de la *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="e569e-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="e569e-230">Ajouter `using` instructions pour `ContosoUniversity.Data` et `Microsoft.EntityFrameworkCore` espaces de noms.</span><span class="sxs-lookup"><span data-stu-id="e569e-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="e569e-231">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="e569e-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="e569e-232">Ouvrez le *appsettings.json* et ajoutez une chaîne de connexion comme indiqué dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="e569e-233">La chaîne de connexion précédente utilise `ConnectRetryCount=0` pour empêcher [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) blocage.</span><span class="sxs-lookup"><span data-stu-id="e569e-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="e569e-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e569e-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e569e-235">La chaîne de connexion spécifie une base de données SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e569e-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="e569e-236">LocalDB est une version légère de SQL Server Express Database Engine et est prévu pour le développement d’applications, pas les fins de production.</span><span class="sxs-lookup"><span data-stu-id="e569e-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="e569e-237">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="e569e-237">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="e569e-238">Par défaut, LocalDB crée *.mdf* les fichiers de base de données dans le `C:/Users/<user>` active.</span><span class="sxs-lookup"><span data-stu-id="e569e-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="e569e-239">Ajoutez du code pour initialiser la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="e569e-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="e569e-240">EF Core crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="e569e-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="e569e-241">Dans cette section, un *Seed* méthode est écrite à remplir avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="e569e-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="e569e-242">Dans le *données* dossier, créez un nouveau fichier de classe nommé *DbInitializer.cs* et ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="e569e-243">Le code vérifie s’il existe des étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="e569e-244">S’il n’y a aucun élève dans la base de données, la base de données est amorcé avec les données de test.</span><span class="sxs-lookup"><span data-stu-id="e569e-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="e569e-245">Il charge les données de test dans les tableaux plutôt que `List<T>` collections pour optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="e569e-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="e569e-246">Le `EnsureCreated` méthode crée automatiquement la base de données pour le contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="e569e-247">Si la base de données existe, `EnsureCreated` retourne sans modifier la base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="e569e-248">Dans *Program.cs*, modifiez le `Main` méthode pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e569e-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="e569e-249">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="e569e-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e569e-250">Appelez la méthode de valeur initiale, en lui passant le contexte.</span><span class="sxs-lookup"><span data-stu-id="e569e-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e569e-251">Supprimer le contexte lorsque la méthode de la valeur initiale est terminée.</span><span class="sxs-lookup"><span data-stu-id="e569e-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="e569e-252">Le code suivant illustre la mise à jour *Program.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="e569e-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="e569e-253">La première fois que l’application est exécutée, la base de données est créée et amorcée avec les données de test.</span><span class="sxs-lookup"><span data-stu-id="e569e-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="e569e-254">Lorsque le modèle de données est mise à jour :</span><span class="sxs-lookup"><span data-stu-id="e569e-254">When the data model is updated:</span></span>
* <span data-ttu-id="e569e-255">Supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-255">Delete the DB.</span></span>
* <span data-ttu-id="e569e-256">Mettre à jour la méthode de valeur initiale.</span><span class="sxs-lookup"><span data-stu-id="e569e-256">Update the seed method.</span></span>
* <span data-ttu-id="e569e-257">Exécuter l’application et une nouvelle base de données amorcée est créé.</span><span class="sxs-lookup"><span data-stu-id="e569e-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="e569e-258">Dans les didacticiels suivants, la base de données est mise à jour lorsque le modèle de données change, sans supprimer et recréer la base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="e569e-259">Ajouter des outils de la vue de structure</span><span class="sxs-lookup"><span data-stu-id="e569e-259">Add scaffold tooling</span></span>

<span data-ttu-id="e569e-260">Dans cette section, la Console Gestionnaire de Package (PMC) est utilisé pour ajouter le package de la génération de code web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e569e-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="e569e-261">Ce package est nécessaire à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="e569e-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="e569e-262">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="e569e-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="e569e-263">Dans Package Manager Console (PMC), entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e569e-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="e569e-264">La commande précédente ajoute les packages NuGet dans le fichier *.csproj :</span><span class="sxs-lookup"><span data-stu-id="e569e-264">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="e569e-265">Structure du modèle</span><span class="sxs-lookup"><span data-stu-id="e569e-265">Scaffold the model</span></span>

* <span data-ttu-id="e569e-266">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e569e-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e569e-267">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e569e-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="e569e-268">Si l’erreur suivante est générée :</span><span class="sxs-lookup"><span data-stu-id="e569e-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="e569e-269">Exécutez de nouveau la commande et laisser un commentaire au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="e569e-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="e569e-270">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="e569e-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="e569e-271">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="e569e-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="e569e-272">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="e569e-272">Build the project.</span></span> <span data-ttu-id="e569e-273">La build génère des erreurs comme suit :</span><span class="sxs-lookup"><span data-stu-id="e569e-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="e569e-274">Modifier globalement `_context.Student` à `_context.Students` (autrement dit, ajoutez un « s » pour `Student`).</span><span class="sxs-lookup"><span data-stu-id="e569e-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="e569e-275">7 occurrences sont trouvées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e569e-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="e569e-276">Nous espérons résoudre [ce bogue](https://github.com/aspnet/Scaffolding/issues/633) dans la prochaine version.</span><span class="sxs-lookup"><span data-stu-id="e569e-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="e569e-277">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="e569e-277">Test the app</span></span>

<span data-ttu-id="e569e-278">Exécutez l’application et sélectionnez le **étudiants** lien.</span><span class="sxs-lookup"><span data-stu-id="e569e-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="e569e-279">En fonction de la largeur du navigateur, le **étudiants** lien s’affiche en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="e569e-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="e569e-280">Si le **étudiants** lien n’est pas visible, cliquez sur l’icône de navigation dans le coin supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="e569e-280">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Page d’accueil Contoso University étroit](intro/_static/home-page-narrow.png)

<span data-ttu-id="e569e-282">Test du **créer**, **modifier**, et **détails** des liens.</span><span class="sxs-lookup"><span data-stu-id="e569e-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="e569e-283">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="e569e-283">View the DB</span></span>

<span data-ttu-id="e569e-284">Lorsque l’application est lancée, `DbInitializer.Initialize` appelle `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="e569e-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="e569e-285">`EnsureCreated`détecte si la base de données existe et qu’il crée un si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e569e-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="e569e-286">S’il n’y aucun élève dans la base de données, le `Initialize` méthode ajoute les étudiants.</span><span class="sxs-lookup"><span data-stu-id="e569e-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="e569e-287">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir de la **vue** menu dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e569e-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="e569e-288">Dans SSOX, cliquez sur **(localdb) \MSSQLLocalDB > bases de données > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="e569e-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="e569e-289">Développez le **Tables** nœud.</span><span class="sxs-lookup"><span data-stu-id="e569e-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="e569e-290">Avec le bouton droit le **Student** de table et cliquez sur **des données d’affichage** pour afficher les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="e569e-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="e569e-291">Le *.mdf* et *.ldf* les fichiers de base de données se trouvent dans le *C:\Users\\ <yourusername>*  dossier.</span><span class="sxs-lookup"><span data-stu-id="e569e-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="e569e-292">`EnsureCreated`est appelé sur le démarrage de l’application, ce qui permet le flux de travail suivant :</span><span class="sxs-lookup"><span data-stu-id="e569e-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="e569e-293">Supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-293">Delete the DB.</span></span>
* <span data-ttu-id="e569e-294">Modifiez le schéma de base de données (par exemple, ajouter un `EmailAddress` champ).</span><span class="sxs-lookup"><span data-stu-id="e569e-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="e569e-295">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="e569e-295">Run the app.</span></span>

<span data-ttu-id="e569e-296">`EnsureCreated`Crée une base de données avec la`EmailAddress` colonne.</span><span class="sxs-lookup"><span data-stu-id="e569e-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="e569e-297">Conventions</span><span class="sxs-lookup"><span data-stu-id="e569e-297">Conventions</span></span>

<span data-ttu-id="e569e-298">La quantité de code écrit dans l’ordre pour les principaux EF créer une base de données complète est minime en raison de l’utilisation des conventions ou les hypothèses qu’EF Core.</span><span class="sxs-lookup"><span data-stu-id="e569e-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="e569e-299">Les noms des `DbSet` propriétés sont utilisées comme noms de tables.</span><span class="sxs-lookup"><span data-stu-id="e569e-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="e569e-300">Pour les entités non référencées par un `DbSet` propriété, classe d’entité noms sont utilisés comme noms de tables.</span><span class="sxs-lookup"><span data-stu-id="e569e-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="e569e-301">Noms de propriété d’entité sont utilisées pour les noms de colonne.</span><span class="sxs-lookup"><span data-stu-id="e569e-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="e569e-302">Propriétés de l’entité qui sont nommées ID ou classnameID sont reconnues comme propriétés de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="e569e-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="e569e-303">Une propriété est interprétée comme une propriété de clé étrangère s’il est nommé  *<navigation property name> <primary key property name>*  (par exemple, `StudentID` pour le `Student` propriété de navigation depuis la `Student` la clé primaire de l’entité est `ID`).</span><span class="sxs-lookup"><span data-stu-id="e569e-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="e569e-304">Propriétés de clé étrangère peuvent être nommées  *<primary key property name>*  (par exemple, `EnrollmentID` depuis le `Enrollment` la clé primaire de l’entité est `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="e569e-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="e569e-305">Un comportement conventionnel peut être remplacé.</span><span class="sxs-lookup"><span data-stu-id="e569e-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="e569e-306">Par exemple, les noms de table peuvent être explicitement spécifiés, comme indiqué précédemment dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e569e-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="e569e-307">Les noms de colonne peuvent être définis explicitement.</span><span class="sxs-lookup"><span data-stu-id="e569e-307">The column names can be explicitly set.</span></span> <span data-ttu-id="e569e-308">Clés primaires et étrangères peut être défini explicitement.</span><span class="sxs-lookup"><span data-stu-id="e569e-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="e569e-309">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="e569e-309">Asynchronous code</span></span>

<span data-ttu-id="e569e-310">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="e569e-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="e569e-311">Un serveur web a un nombre limité de threads disponibles, et dans les situations de forte charge tous les threads disponibles peuvent être en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="e569e-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="e569e-312">Lorsque cela se produit, le serveur ne peut pas traiter de nouvelles demandes jusqu'à ce que les threads ne sont pas libérées.</span><span class="sxs-lookup"><span data-stu-id="e569e-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="e569e-313">Avec le code synchrone, plusieurs threads peuvent être bloqués dans pendant qu’ils ne sont pas réellement effectuer aucun travail car ils sont en attente pour les e/s.</span><span class="sxs-lookup"><span data-stu-id="e569e-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="e569e-314">Avec le code asynchrone, lorsqu’un processus est en attente d’e/s, son thread est libéré pour le serveur à utiliser pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="e569e-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="e569e-315">Par conséquent, code asynchrone permet d’utiliser plus efficacement les ressources serveur et le serveur est activé pour gérer plus de trafic sans délai.</span><span class="sxs-lookup"><span data-stu-id="e569e-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="e569e-316">Code asynchrone introduit une petite quantité de charge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e569e-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="e569e-317">Dans les situations de faible trafic, le gain de performances est négligeable, lors de la pour les cas de trafic élevé, l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="e569e-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="e569e-318">Dans le code suivant, le `async` (mot clé), `Task<T>` valeur de retour, `await` (mot clé), et `ToListAsync` méthode rendre le code à exécuter de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e569e-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="e569e-319">Le `async` mot clé indique au compilateur de :</span><span class="sxs-lookup"><span data-stu-id="e569e-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="e569e-320">Génèrent des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="e569e-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="e569e-321">Créer automatiquement le [tâche](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objet qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="e569e-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="e569e-322">Pour plus d’informations, consultez [Type de retour de tâche](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="e569e-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="e569e-323">Type de retour implicite `Task` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="e569e-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="e569e-324">Le `await` (mot clé), le compilateur fractionner la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="e569e-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="e569e-325">La première partie se termine par l’opération qui est démarrée en mode asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e569e-325">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="e569e-326">La deuxième partie est placée dans une méthode de rappel qui est appelée lorsque l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="e569e-326">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="e569e-327">`ToListAsync`est la version asynchrone de la `ToList` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="e569e-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="e569e-328">Éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="e569e-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="e569e-329">Uniquement les instructions qui génèrent des requêtes ou des commandes à envoyer à la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="e569e-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="e569e-330">Qui inclut, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, et `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="e569e-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="e569e-331">Il n’inclut pas d’instructions qui modifient simplement un `IQueryable`, tel que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="e569e-331">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="e569e-332">Un contexte EF n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="e569e-332">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="e569e-333">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (tels que la pagination) utilisent async si celles-ci appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="e569e-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="e569e-334">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [vue d’ensemble de Async](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="e569e-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="e569e-335">Dans le didacticiel suivant, CRUD de base (créer, lire, mettre à jour, supprimer) les opérations.</span><span class="sxs-lookup"><span data-stu-id="e569e-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e569e-336">Next</span><span class="sxs-lookup"><span data-stu-id="e569e-336">Next</span></span>](xref:data/ef-rp/crud)
