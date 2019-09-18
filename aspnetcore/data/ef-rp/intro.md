---
title: Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8
author: tdykstra
description: Montre comment créer une application Pages Razor à l’aide d’Entity Framework Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/22/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 107b348b4484301b86eeb5528833914fe4c1eaf7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080932"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="c620d-103">Pages Razor avec Entity Framework Core dans ASP.NET Core - Tutoriel 1 sur 8</span><span class="sxs-lookup"><span data-stu-id="c620d-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="c620d-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c620d-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c620d-105">Il s’agit du premier tutoriel d’une série qui montre comment utiliser Entity Framework (EF) Core dans une application Razor Pages ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an ASP.NET Core Razor Pages app.</span></span> <span data-ttu-id="c620d-106">Dans ces tutoriels, un site web est créé pour une université fictive nommée Contoso.</span><span class="sxs-lookup"><span data-stu-id="c620d-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="c620d-107">Le site comprend des fonctionnalités comme l’admission des étudiants, la création de cours et les affectations des formateurs.</span><span class="sxs-lookup"><span data-stu-id="c620d-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="c620d-108">Télécharger ou afficher l’application complète.</span><span class="sxs-lookup"><span data-stu-id="c620d-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="c620d-109">[Télécharger les instructions](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c620d-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c620d-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="c620d-110">Prerequisites</span></span>

* <span data-ttu-id="c620d-111">Si vous débutez avec Razor Pages, parcourez le tutoriel [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start) avant de commencer celui-ci.</span><span class="sxs-lookup"><span data-stu-id="c620d-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="c620d-114">Moteurs de base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-114">Database engines</span></span>

<span data-ttu-id="c620d-115">Les instructions Visual Studio utilisent la [Base de données locale SQL Server](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), version de SQL Server Express qui s’exécute uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="c620d-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="c620d-116">Les instructions Visual Studio Code utilisent [SQLite](https://www.sqlite.org/), moteur de base de données multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="c620d-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="c620d-117">Si vous choisissez d’utiliser SQLite, téléchargez et installez un outil tiers pour la gestion et l’affichage d’une base de données SQLite, comme [Browser for SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="c620d-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c620d-118">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="c620d-118">Troubleshooting</span></span>

<span data-ttu-id="c620d-119">Si vous rencontrez un problème que vous ne pouvez pas résoudre, comparez votre code au [projet terminé](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="c620d-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="c620d-120">Un bon moyen d’obtenir de l’aide est de poster une question sur StackOverflow.com en utilisant le [mot-clé ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou le [mot-clé EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="c620d-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="c620d-121">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="c620d-121">The sample app</span></span>

<span data-ttu-id="c620d-122">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="c620d-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="c620d-123">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="c620d-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="c620d-124">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c620d-124">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index30.png)

![Page de modification des étudiants](intro/_static/student-edit30.png)

<span data-ttu-id="c620d-127">Le style de l’interface utilisateur de ce site repose sur les modèles de projet intégrés.</span><span class="sxs-lookup"><span data-stu-id="c620d-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="c620d-128">Le tutoriel traite essentiellement de l’utilisation d’EF Core, et non de la façon de personnaliser l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c620d-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="c620d-129">Suivez le lien en haut de la page pour obtenir le code source du projet terminé.</span><span class="sxs-lookup"><span data-stu-id="c620d-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="c620d-130">Le dossier *cu30* contient le code de la version 3.0 d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="c620d-131">Les fichiers qui reflètent l’état du code pour les tutoriels 1-7 se trouvent dans le dossier *cu30snapshots*.</span><span class="sxs-lookup"><span data-stu-id="c620d-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c620d-133">Pour exécuter l’application après avoir téléchargé le projet terminé :</span><span class="sxs-lookup"><span data-stu-id="c620d-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="c620d-134">Supprimez trois fichiers et un dossier dont le nom contient *SQLite*.</span><span class="sxs-lookup"><span data-stu-id="c620d-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="c620d-135">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="c620d-135">Build the project.</span></span>
* <span data-ttu-id="c620d-136">Dans la console du Gestionnaire de package (PMC), exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c620d-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="c620d-137">Exécutez le projet pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c620d-139">Pour exécuter l’application après avoir téléchargé le projet terminé :</span><span class="sxs-lookup"><span data-stu-id="c620d-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="c620d-140">Supprimez *ContosoUniversity.csproj*, puis renommez *ContosoUniversitySQLite.csproj* en *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c620d-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="c620d-141">Supprimez *Startup.cs* et renommez *StartupSQLite.cs* en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c620d-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="c620d-142">Supprimez *appSettings.json* et renommez *appSettingsSQLite.json* en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c620d-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="c620d-143">Supprimez le dossier *Migrations* et renommez *MigrationsSQL* en *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="c620d-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="c620d-144">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="c620d-144">Build the project.</span></span>
* <span data-ttu-id="c620d-145">Dans une invite de commandes, exécutez la commande suivante dans le dossier du projet :</span><span class="sxs-lookup"><span data-stu-id="c620d-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  dotnet ef database update
  ```

* <span data-ttu-id="c620d-146">Dans votre outil SQLite, exécutez l’instruction SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="c620d-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="c620d-147">Exécutez le projet pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="c620d-148">Créer le projet d’application web</span><span class="sxs-lookup"><span data-stu-id="c620d-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c620d-150">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="c620d-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c620d-151">Sélectionnez **Nouvelle application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c620d-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c620d-152">Nommez le projet *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="c620d-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="c620d-153">Il est important d’utiliser ce nom exact, en respectant l’utilisation des majuscules, de sorte que les espaces de noms correspondent au moment où le code est copié et collé.</span><span class="sxs-lookup"><span data-stu-id="c620d-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="c620d-154">Sélectionnez **.NET Core** et **ASP.NET Core 3.0** dans les listes déroulantes, puis **Application web**.</span><span class="sxs-lookup"><span data-stu-id="c620d-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c620d-156">Dans un terminal, accédez au dossier dans lequel le dossier de projet doit être créé.</span><span class="sxs-lookup"><span data-stu-id="c620d-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="c620d-157">Exécutez les commandes suivantes pour créer un projet Razor Pages et `cd` dans le nouveau dossier de projet :</span><span class="sxs-lookup"><span data-stu-id="c620d-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="c620d-158">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="c620d-158">Set up the site style</span></span>

<span data-ttu-id="c620d-159">Configurez l’en-tête, le pied de page et le menu du site en mettant à jour *Pages/Shared/_Layout.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="c620d-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="c620d-160">Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="c620d-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="c620d-161">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="c620d-161">There are three occurrences.</span></span>

* <span data-ttu-id="c620d-162">Supprimez les entrées de menu **Home** et **Privacy** et ajoutez les entrées **About**, **Students**, **Courses**, **Instructors** et **Departments**.</span><span class="sxs-lookup"><span data-stu-id="c620d-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="c620d-163">Les modifications apparaissent en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="c620d-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="c620d-164">Dans *Pages/Index.cshtml*, remplacez le contenu du fichier par le code suivant de façon à remplacer le texte relatif à ASP.NET Core par le texte se rapportant à cette application :</span><span class="sxs-lookup"><span data-stu-id="c620d-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="c620d-165">Exécutez l’application pour vérifier que la page d’accueil (« Home ») s’affiche.</span><span class="sxs-lookup"><span data-stu-id="c620d-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="c620d-166">Modèle de données</span><span class="sxs-lookup"><span data-stu-id="c620d-166">The data model</span></span>

<span data-ttu-id="c620d-167">Les sections suivantes créent un modèle de données :</span><span class="sxs-lookup"><span data-stu-id="c620d-167">The following sections create a data model:</span></span>

![Diagramme de modèle de données Cours-Inscription-Étudiant](intro/_static/data-model-diagram.png)

<span data-ttu-id="c620d-169">Un étudiant peut s’inscrire à un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="c620d-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="c620d-170">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="c620d-170">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

* <span data-ttu-id="c620d-172">Créez un dossier *Models* dans le dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="c620d-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="c620d-173">Créez *Models/Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="c620d-174">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="c620d-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="c620d-175">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="c620d-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="c620d-176">L’autre nom reconnu automatiquement de la clé primaire de classe `Student` est `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="c620d-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="c620d-177">La propriété `Enrollments` est une [propriété de navigation](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="c620d-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="c620d-178">Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="c620d-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="c620d-179">Dans ce cas, la propriété `Enrollments` d’une entité `Student` contient toutes les entités `Enrollment` associées à cet étudiant.</span><span class="sxs-lookup"><span data-stu-id="c620d-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="c620d-180">Par exemple, si une ligne Student dans la base de données est associée à deux lignes Enrollment, la propriété de navigation `Enrollments` contient ces deux entités Enrollment.</span><span class="sxs-lookup"><span data-stu-id="c620d-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="c620d-181">Dans la base de données, une ligne Enrollment est associée à une ligne Student si sa colonne StudentID contient la valeur d’ID de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="c620d-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="c620d-182">Par exemple, supposez qu’une ligne Student présente un ID égal à 1.</span><span class="sxs-lookup"><span data-stu-id="c620d-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="c620d-183">Les lignes Enrollment associées auront un StudentID égal à 1.</span><span class="sxs-lookup"><span data-stu-id="c620d-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="c620d-184">StudentID est une *clé étrangère* dans la table Enrollment.</span><span class="sxs-lookup"><span data-stu-id="c620d-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="c620d-185">La propriété `Enrollments` est définie en tant que `ICollection<Enrollment>`, car plusieurs entités Enrollment associées peuvent exister.</span><span class="sxs-lookup"><span data-stu-id="c620d-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="c620d-186">Vous pouvez utiliser d’autres types de collection, comme `List<Enrollment>` ou `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="c620d-187">Quand vous utilisez `ICollection<Enrollment>`, EF Core crée une collection `HashSet<Enrollment>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="c620d-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="c620d-188">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="c620d-188">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="c620d-190">Créez *Models/Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="c620d-191">La propriété `EnrollmentID` est la clé primaire ; cette entité utilise le modèle `classnameID` à la place de `ID` par lui-même.</span><span class="sxs-lookup"><span data-stu-id="c620d-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="c620d-192">Pour un modèle de données de production, choisissez un modèle et utilisez-le systématiquement.</span><span class="sxs-lookup"><span data-stu-id="c620d-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="c620d-193">Ce tutoriel utilise les deux pour montrer qu’ils fonctionnent tous les deux.</span><span class="sxs-lookup"><span data-stu-id="c620d-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="c620d-194">L’utilisation de `ID` sans `classname` facilite l’implémentation de certaines modifications du modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="c620d-195">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="c620d-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="c620d-196">La présence du point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` [accepte les valeurs Null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="c620d-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="c620d-197">Une note (Grade) de valeur Null est différente d’une note zéro : la valeur Null signifie que la note n’est pas connue ou qu’elle n’a pas encore été attribuée.</span><span class="sxs-lookup"><span data-stu-id="c620d-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="c620d-198">La propriété `StudentID` est une clé étrangère et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="c620d-199">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="c620d-200">La propriété `CourseID` est une clé étrangère et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="c620d-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="c620d-201">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="c620d-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="c620d-202">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="c620d-203">Par exemple, `StudentID` est la clé étrangère pour la propriété de navigation `Student`, car la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="c620d-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="c620d-204">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="c620d-205">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c620d-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="c620d-206">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="c620d-206">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="c620d-208">Créez *Models/Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="c620d-209">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="c620d-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c620d-210">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="c620d-211">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="c620d-212">Générez le projet pour vérifier qu’il n’y a pas d’erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="c620d-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="c620d-213">Générer automatiquement des modèles de pages Student</span><span class="sxs-lookup"><span data-stu-id="c620d-213">Scaffold Student pages</span></span>

<span data-ttu-id="c620d-214">Dans cette section, vous allez utiliser l’outil de génération de modèles automatique ASP.NET Core pour générer :</span><span class="sxs-lookup"><span data-stu-id="c620d-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="c620d-215">Une classe de *contexte* EF Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-215">An EF Core *context* class.</span></span> <span data-ttu-id="c620d-216">Le contexte est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données déterminé.</span><span class="sxs-lookup"><span data-stu-id="c620d-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c620d-217">Il dérive de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c620d-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="c620d-218">Les pages Razor qui gèrent les opérations Create, Read, Update et Delete (CRUD) pour l’entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c620d-220">Créez un dossier *Students* dans le dossier *Pages*.</span><span class="sxs-lookup"><span data-stu-id="c620d-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="c620d-221">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Students*, puis sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="c620d-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c620d-222">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)** > **AJOUTER**.</span><span class="sxs-lookup"><span data-stu-id="c620d-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="c620d-223">Dans la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)**  :</span><span class="sxs-lookup"><span data-stu-id="c620d-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="c620d-224">Dans la liste déroulante **Classe de modèle**, sélectionnez **Student (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="c620d-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="c620d-225">Dans la ligne **Classe du contexte de données**, sélectionnez le signe **+** (plus).</span><span class="sxs-lookup"><span data-stu-id="c620d-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="c620d-226">Remplacez le nom du contexte de données *ContosoUniversity.Models.ContosoUniversityContext* par *ContosoUniversity.Data.SchoolContext*.</span><span class="sxs-lookup"><span data-stu-id="c620d-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="c620d-227">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c620d-227">Select **Add**.</span></span>

<span data-ttu-id="c620d-228">Les packages suivants sont automatiquement installés :</span><span class="sxs-lookup"><span data-stu-id="c620d-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c620d-230">Exécutez les commandes CLI .NET Core suivantes pour installer les packages NuGet nécessaires :</span><span class="sxs-lookup"><span data-stu-id="c620d-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Tools --version 3.0.0-*
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
  dotnet add package Microsoft.Extensions.Logging.Debug --version 3.0.0-*
  ```

  <span data-ttu-id="c620d-231">Le package Microsoft.VisualStudio.Web.CodeGeneration.Design est requis pour la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="c620d-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="c620d-232">Bien que l’application ne soit pas appelée à utiliser SQL Server, l’outil de génération de modèles automatique a besoin du package SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c620d-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="c620d-233">Créez un dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="c620d-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="c620d-234">Exécutez la commande suivante pour installer l’[outil de génération de modèles automatique aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="c620d-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
  ```

* <span data-ttu-id="c620d-235">Exécutez la commande suivante pour générer automatiquement des modèles de pages Student.</span><span class="sxs-lookup"><span data-stu-id="c620d-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="c620d-236">**Sur Windows**</span><span class="sxs-lookup"><span data-stu-id="c620d-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="c620d-237">**Sur macOS ou Linux**</span><span class="sxs-lookup"><span data-stu-id="c620d-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="c620d-238">Si vous rencontrez un problème à l’étape précédente, générez le projet et recommencez l’étape de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="c620d-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="c620d-239">Le processus de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="c620d-239">The scaffolding process:</span></span>

* <span data-ttu-id="c620d-240">Crée les pages Razor suivantes dans le dossier *Pages/Students* :</span><span class="sxs-lookup"><span data-stu-id="c620d-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="c620d-241">*Create.cshtml* et *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c620d-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="c620d-242">*Delete.cshtml* et *Delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c620d-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="c620d-243">*Details.cshtml* et *Details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c620d-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="c620d-244">*Edit.cshtml* et *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c620d-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="c620d-245">*Index.cshtml* et *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="c620d-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="c620d-246">Crée *Data/SchoolContext. cs*.</span><span class="sxs-lookup"><span data-stu-id="c620d-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="c620d-247">Ajoute le contexte à l’injection de dépendances dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c620d-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="c620d-248">Ajoute une chaîne de connexion de base de données à *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c620d-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="c620d-249">Chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c620d-251">La chaîne de connexion spécifie [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="c620d-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="c620d-252">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="c620d-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="c620d-253">Par défaut, la Base de données locale crée des fichiers *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c620d-255">Modifiez la chaîne de connexion de sorte qu’elle pointe vers un fichier de base de données SQLite nommé *CU.db* :</span><span class="sxs-lookup"><span data-stu-id="c620d-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="c620d-256">Mettre à jour la classe du contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-256">Update the database context class</span></span>

<span data-ttu-id="c620d-257">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données déterminé est la classe du contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="c620d-258">Le contexte est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c620d-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c620d-259">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="c620d-260">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c620d-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="c620d-261">Mettez à jour *SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="c620d-262">Le code en surbrillance crée une propriété [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="c620d-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="c620d-263">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="c620d-263">In EF Core terminology:</span></span>

* <span data-ttu-id="c620d-264">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="c620d-265">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="c620d-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c620d-266">Comme un jeu d’entités contient plusieurs entités, les propriétés DBSet doivent être des noms au pluriel.</span><span class="sxs-lookup"><span data-stu-id="c620d-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="c620d-267">Comme l’outil de génération de modèles automatique a créé un DBSet `Student`, cette étape le remplace par le pluriel `Students`.</span><span class="sxs-lookup"><span data-stu-id="c620d-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="c620d-268">Pour que le code Razor Pages corresponde au nouveau nom DBSet, apportez une modification globale à l’ensemble du projet en remplaçant `_context.Student` par `_context.Students`.</span><span class="sxs-lookup"><span data-stu-id="c620d-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="c620d-269">Il y a 8 occurrences.</span><span class="sxs-lookup"><span data-stu-id="c620d-269">There are 8 occurrences.</span></span>

<span data-ttu-id="c620d-270">Générez le projet pour vérifier qu’il n’y a pas d’erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="c620d-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="c620d-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c620d-271">Startup.cs</span></span>

<span data-ttu-id="c620d-272">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c620d-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c620d-273">Certains services (comme le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c620d-274">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="c620d-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c620d-275">Le code de constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le tutoriel.</span><span class="sxs-lookup"><span data-stu-id="c620d-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c620d-276">L’outil de génération de modèles automatique a inscrit automatiquement la classe du contexte dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="c620d-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c620d-278">Dans `ConfigureServices`, les lignes en surbrillance ont été ajoutées par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="c620d-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c620d-280">Dans `ConfigureServices`, vérifiez que le code ajouté par l’outil de génération de modèles automatique appelle `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="c620d-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="c620d-281">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c620d-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c620d-282">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c620d-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c620d-283">Créer la base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-283">Create the database</span></span>

<span data-ttu-id="c620d-284">Mettez à jour *Program.cs* pour créer la base de données si elle n’existe pas :</span><span class="sxs-lookup"><span data-stu-id="c620d-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="c620d-285">La méthode [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) n’effectue aucune action s’il existe une base de données pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="c620d-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="c620d-286">S’il n’existe pas de base de données, elle crée la base de données et le schéma.</span><span class="sxs-lookup"><span data-stu-id="c620d-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="c620d-287">`EnsureCreated` active le workflow suivant pour gérer les modifications du modèle de données :</span><span class="sxs-lookup"><span data-stu-id="c620d-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="c620d-288">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-288">Delete the database.</span></span> <span data-ttu-id="c620d-289">Toutes les données existantes sont perdues.</span><span class="sxs-lookup"><span data-stu-id="c620d-289">Any existing data is lost.</span></span>
* <span data-ttu-id="c620d-290">Modifiez le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-290">Change the data model.</span></span> <span data-ttu-id="c620d-291">Par exemple, ajoutez un champ `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="c620d-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="c620d-292">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-292">Run the app.</span></span>
* <span data-ttu-id="c620d-293">`EnsureCreated` crée une base de données avec le nouveau schéma.</span><span class="sxs-lookup"><span data-stu-id="c620d-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="c620d-294">Ce workflow fonctionne bien à un stade précoce du développement, quand le schéma évolue rapidement, aussi longtemps que vous n’avez pas besoin de conserver les données.</span><span class="sxs-lookup"><span data-stu-id="c620d-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="c620d-295">La situation est différente quand les données qui ont été entrées dans la base de données doivent être conservées.</span><span class="sxs-lookup"><span data-stu-id="c620d-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="c620d-296">Dans ce cas, procédez à des migrations.</span><span class="sxs-lookup"><span data-stu-id="c620d-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="c620d-297">Plus tard dans cette série de tutoriels, vous supprimerez la base de données créée par `EnsureCreated` et procéderez à des migrations.</span><span class="sxs-lookup"><span data-stu-id="c620d-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="c620d-298">Une base de données créée par `EnsureCreated` ne peut pas être mise à jour via des migrations.</span><span class="sxs-lookup"><span data-stu-id="c620d-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="c620d-299">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="c620d-299">Test the app</span></span>

* <span data-ttu-id="c620d-300">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-300">Run the app.</span></span>
* <span data-ttu-id="c620d-301">Sélectionnez le lien **Students**, puis **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="c620d-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="c620d-302">Testez les liens Edit, Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="c620d-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="c620d-303">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-303">Seed the database</span></span>

<span data-ttu-id="c620d-304">La méthode `EnsureCreated` crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="c620d-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="c620d-305">Cette section ajoute du code qui remplit la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="c620d-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="c620d-306">Créez *Data/DbInitializer.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="c620d-307">Le code vérifie si des étudiants figurent dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="c620d-308">S’il n’y a pas d’étudiants, il ajoute des données de test à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="c620d-309">Il crée les données de test dans des tableaux et non dans des collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="c620d-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="c620d-310">Dans *Program.cs*, remplacez l’appel `EnsureCreated` par un appel `DbInitializer.Initialize` :</span><span class="sxs-lookup"><span data-stu-id="c620d-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c620d-312">Arrêtez l’application si elle est en cours d’exécution et exécutez la commande suivante dans la **Console du gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="c620d-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c620d-314">Arrêtez l’application si elle est en cours d’exécution, puis supprimez le fichier *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="c620d-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="c620d-315">Redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-315">Restart the app.</span></span>

* <span data-ttu-id="c620d-316">Sélectionnez la page Students pour examiner les données amorcées.</span><span class="sxs-lookup"><span data-stu-id="c620d-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="c620d-317">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c620d-319">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c620d-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="c620d-320">Dans SSOX, sélectionnez **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="c620d-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="c620d-321">Le nom de la base de données est généré à partir du nom de contexte indiqué précédemment, ainsi que d’un tiret et d’un GUID.</span><span class="sxs-lookup"><span data-stu-id="c620d-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="c620d-322">Développez le noeud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="c620d-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="c620d-323">Avec le bouton droit sur la table **Student**, cliquez sur **des données d’affichage** pour afficher les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="c620d-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="c620d-324">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher le code** pour voir comment le modèle `Student` est mappé au schéma de la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c620d-326">Utilisez votre outil SQLite pour examiner le schéma de base de données et les données amorcées.</span><span class="sxs-lookup"><span data-stu-id="c620d-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="c620d-327">Le fichier de base de données est nommé *CU.db* et se trouve dans le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="c620d-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="c620d-328">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="c620d-328">Asynchronous code</span></span>

<span data-ttu-id="c620d-329">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="c620d-330">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="c620d-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="c620d-331">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="c620d-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="c620d-332">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="c620d-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="c620d-333">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="c620d-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="c620d-334">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="c620d-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="c620d-335">Le code asynchrone introduit une petite quantité de charge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="c620d-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="c620d-336">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="c620d-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="c620d-337">Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="c620d-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="c620d-338">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="c620d-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="c620d-339">Génèrer des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="c620d-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="c620d-340">Crée l’objet [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="c620d-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="c620d-341">Le type de retour `Task<T>` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="c620d-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="c620d-342">Le (mot clé) `await`, le compilateur fractionner la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="c620d-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="c620d-343">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="c620d-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="c620d-344">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="c620d-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="c620d-345">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="c620d-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="c620d-346">Les éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="c620d-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="c620d-347">Seules les instructions qui provoquent l’envoi de requêtes ou de commandes vers la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="c620d-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="c620d-348">Cela inclut `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` et `SaveChangesAsync`,</span><span class="sxs-lookup"><span data-stu-id="c620d-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="c620d-349">Il n’inclut pas les instructions qui modifient simplement un `IQueryable`, tel que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="c620d-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="c620d-350">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="c620d-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="c620d-351">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="c620d-352">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="c620d-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c620d-353">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c620d-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c620d-354">Tutoriel suivant</span><span class="sxs-lookup"><span data-stu-id="c620d-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c620d-355">L’exemple d’application web Contoso University montre comment créer une application web Razor Pages ASP.NET Core à l’aide d’Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="c620d-356">L’exemple d’application est un site web pour une université Contoso fictive.</span><span class="sxs-lookup"><span data-stu-id="c620d-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="c620d-357">Il inclut des fonctionnalités telles que l'admission d’étudiant, la création de cours et les affectations de formateur.</span><span class="sxs-lookup"><span data-stu-id="c620d-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="c620d-358">Cette page est la première d’une série de didacticiels qui expliquent comment générer l’exemple d’application pour l'université de Contoso.</span><span class="sxs-lookup"><span data-stu-id="c620d-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="c620d-359">Télécharger ou afficher l’application complète.</span><span class="sxs-lookup"><span data-stu-id="c620d-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="c620d-360">[Télécharger les instructions](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c620d-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c620d-361">Prérequis</span><span class="sxs-lookup"><span data-stu-id="c620d-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="c620d-364">Connaissance des [Pages Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="c620d-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="c620d-365">Les programmeurs débutants doivent lire [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) avant de démarrer cette série.</span><span class="sxs-lookup"><span data-stu-id="c620d-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c620d-366">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="c620d-366">Troubleshooting</span></span>

<span data-ttu-id="c620d-367">Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous pouvez généralement trouver la solution en comparant votre code au [projet terminé](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="c620d-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="c620d-368">Un bon moyen d’obtenir de l’aide est de publier une question sur [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) pour [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) ou [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="c620d-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="c620d-369">L’application web Contoso University</span><span class="sxs-lookup"><span data-stu-id="c620d-369">The Contoso University web app</span></span>

<span data-ttu-id="c620d-370">L’application générée dans ces didacticiels est le site web de base d’une université.</span><span class="sxs-lookup"><span data-stu-id="c620d-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="c620d-371">Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs.</span><span class="sxs-lookup"><span data-stu-id="c620d-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="c620d-372">Voici quelques-uns des écrans créés dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c620d-372">Here are a few of the screens created in the tutorial.</span></span>

![Page d’index des étudiants](intro/_static/students-index.png)

![Page de modification des étudiants](intro/_static/student-edit.png)

<span data-ttu-id="c620d-375">Le style de l’interface utilisateur de ce site est proche de ce qui est généré par les modèles prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="c620d-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="c620d-376">Le didacticiel est axé sur Core EF avec des pages Razor, et non sur l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c620d-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="c620d-377">Créer l’application web Razor Pages ContosoUniversity</span><span class="sxs-lookup"><span data-stu-id="c620d-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c620d-379">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="c620d-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c620d-380">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c620d-381">Nommez le projet **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="c620d-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="c620d-382">Il est important de nommer le projet *ContosoUniversity* afin que les espaces de noms correspondent quand le code est copié/collé.</span><span class="sxs-lookup"><span data-stu-id="c620d-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="c620d-383">Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="c620d-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="c620d-384">Pour les images des étapes précédentes, consultez [Créer une application web Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="c620d-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="c620d-385">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="c620d-387">Configurer le style du site</span><span class="sxs-lookup"><span data-stu-id="c620d-387">Set up the site style</span></span>

<span data-ttu-id="c620d-388">Quelques changements permettent de définir le menu, la disposition et la page d’accueil du site.</span><span class="sxs-lookup"><span data-stu-id="c620d-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="c620d-389">Mettez à jour *Pages/Shared/_Layout.cshtml* avec les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="c620d-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="c620d-390">Remplacez chaque occurrence de « ContosoUniversity » par « Contoso University ».</span><span class="sxs-lookup"><span data-stu-id="c620d-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="c620d-391">Il y a trois occurrences.</span><span class="sxs-lookup"><span data-stu-id="c620d-391">There are three occurrences.</span></span>

* <span data-ttu-id="c620d-392">Ajoutez des entrées de menu pour **Students**, **Courses**, **Instructors** et **Departments**, et supprimez l’entrée de menu **Contact**.</span><span class="sxs-lookup"><span data-stu-id="c620d-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="c620d-393">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="c620d-393">The changes are highlighted.</span></span> <span data-ttu-id="c620d-394">(Tout le balisage n’est *pas* affiché.)</span><span class="sxs-lookup"><span data-stu-id="c620d-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="c620d-395">Dans *Pages/Index.cshtml*, remplacez le contenu du fichier par le code suivant afin de remplacer le texte sur ASP.NET et MVC par le texte concernant cette application :</span><span class="sxs-lookup"><span data-stu-id="c620d-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="c620d-396">Créer le modèle de données</span><span class="sxs-lookup"><span data-stu-id="c620d-396">Create the data model</span></span>

<span data-ttu-id="c620d-397">Créez des classes d’entités pour l’application Contoso University.</span><span class="sxs-lookup"><span data-stu-id="c620d-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="c620d-398">Commencez par les trois entités suivantes :</span><span class="sxs-lookup"><span data-stu-id="c620d-398">Start with the following three entities:</span></span>

![Diagramme de modèle de données Cours-Inscription-Étudiant](intro/_static/data-model-diagram.png)

<span data-ttu-id="c620d-400">Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="c620d-401">Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="c620d-402">Un étudiant peut s’inscrire à autant de cours qu’il le souhaite.</span><span class="sxs-lookup"><span data-stu-id="c620d-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="c620d-403">Un cours peut avoir une quantité illimitée d’élèves inscrits.</span><span class="sxs-lookup"><span data-stu-id="c620d-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="c620d-404">Dans les sections suivantes, une classe pour chacune de ces entités est créée.</span><span class="sxs-lookup"><span data-stu-id="c620d-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="c620d-405">L’entité Student</span><span class="sxs-lookup"><span data-stu-id="c620d-405">The Student entity</span></span>

![Diagramme de l’entité Student](intro/_static/student-entity.png)

<span data-ttu-id="c620d-407">Créez un dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="c620d-407">Create a *Models* folder.</span></span> <span data-ttu-id="c620d-408">Dans le dossier *Models*, créez un fichier de classe nommé *Student.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="c620d-409">La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe.</span><span class="sxs-lookup"><span data-stu-id="c620d-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="c620d-410">Par défaut, EF Core interprète une propriété nommée `ID` ou `classnameID` comme clé primaire.</span><span class="sxs-lookup"><span data-stu-id="c620d-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="c620d-411">Dans `classnameID`, `classname` est le nom de la classe.</span><span class="sxs-lookup"><span data-stu-id="c620d-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="c620d-412">L’autre clé primaire reconnue automatiquement est `StudentID` dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="c620d-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="c620d-413">La propriété `Enrollments` est une [propriété de navigation](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="c620d-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="c620d-414">Les propriétés de navigation établissent des liaisons à d’autres entités qui sont associées à cette entité.</span><span class="sxs-lookup"><span data-stu-id="c620d-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="c620d-415">Ici, la propriété `Enrollments` d’un `Student entity` contient toutes les entités `Enrollment` associées à ce `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="c620d-416">Par exemple, si une ligne Student dans la base de données a deux lignes Enrollment associées, la propriété de navigation `Enrollments` contient ces deux entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="c620d-417">Une ligne `Enrollment` associée est une ligne qui contient la valeur de clé primaire de cette étudiant dans la colonne `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="c620d-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="c620d-418">Par exemple, supposez que l’étudiant avec ID=1 a deux lignes dans la table `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="c620d-419">La table `Enrollment` a deux lignes avec `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="c620d-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="c620d-420">`StudentID` est une clé étrangère dans la table `Enrollment` qui spécifie l’étudiant dans la table `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="c620d-421">Si une propriété de navigation peut contenir plusieurs entités, la propriété de navigation doit être un type de liste, tel que `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="c620d-422">Vous pouvez spécifier `ICollection<T>`, ou un type tel que `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="c620d-423">Quand vous utilisez `ICollection<T>`, EF Core crée une collection `HashSet<T>` par défaut.</span><span class="sxs-lookup"><span data-stu-id="c620d-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="c620d-424">Les propriétés de navigation qui contiennent plusieurs entités proviennent de relations plusieurs-à-plusieurs et un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="c620d-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="c620d-425">L’entité Enrollment</span><span class="sxs-lookup"><span data-stu-id="c620d-425">The Enrollment entity</span></span>

![Diagramme de l’entité Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="c620d-427">Dans le dossier *Models*, créez un fichier *Enrollment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="c620d-428">La propriété `EnrollmentID` est la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="c620d-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="c620d-429">Cette entité utilise le modèle `classnameID` au lieu de `ID` comme l’entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="c620d-430">En général, les développeurs choisissent un modèle et l’utilisent dans tout le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="c620d-431">Dans un prochain didacticiel, nous verrons qu’utiliser ID sans classname simplifie l’implémentation de l’héritage dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="c620d-432">La propriété `Grade` est un `enum`.</span><span class="sxs-lookup"><span data-stu-id="c620d-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="c620d-433">Le point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` est nullable.</span><span class="sxs-lookup"><span data-stu-id="c620d-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="c620d-434">Une note (Grade) qui a la valeur Null est différente d’une note égale à zéro : la valeur Null signifie qu’une note n’est pas connue ou n’a pas encore été affectée.</span><span class="sxs-lookup"><span data-stu-id="c620d-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="c620d-435">La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="c620d-436">Une entité `Enrollment` est associée à une entité `Student`. Par conséquent, la propriété contient une seule entité `Student`.</span><span class="sxs-lookup"><span data-stu-id="c620d-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="c620d-437">L’entité `Student` diffère de la propriété de navigation `Student.Enrollments`, qui contient plusieurs entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="c620d-438">La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`.</span><span class="sxs-lookup"><span data-stu-id="c620d-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="c620d-439">Une entité `Enrollment` est associée à une entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="c620d-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="c620d-440">EF Core interprète une propriété en tant que clé étrangère si elle se nomme `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="c620d-441">Par exemple, `StudentID` pour la propriété de navigation `Student`, puisque la clé primaire de l’entité `Student` est `ID`.</span><span class="sxs-lookup"><span data-stu-id="c620d-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="c620d-442">Les propriétés de clé étrangère peuvent également se nommer `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="c620d-443">Par exemple, `CourseID` puisque la clé primaire de l’entité `Course` est `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c620d-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="c620d-444">L’entité Course</span><span class="sxs-lookup"><span data-stu-id="c620d-444">The Course entity</span></span>

![Diagramme de l’entité Course](intro/_static/course-entity.png)

<span data-ttu-id="c620d-446">Dans le dossier *Models*, créez un fichier *Course.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="c620d-447">La propriété `Enrollments` est une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="c620d-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c620d-448">Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c620d-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="c620d-449">L’attribut `DatabaseGenerated` permet à l’application de spécifier la clé primaire, plutôt que de la faire générer par la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="c620d-450">Générer automatiquement le modèle d’étudiant</span><span class="sxs-lookup"><span data-stu-id="c620d-450">Scaffold the student model</span></span>

<span data-ttu-id="c620d-451">Dans cette section, le modèle d’étudiant est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="c620d-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="c620d-452">Autrement dit, l’outil de génération de modèles automatique génère des pages pour les opérations de création (Create), de lecture (Read), de mise à jour (Update) et de suppression (Delete) (CRUD) pour le modèle d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="c620d-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="c620d-453">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="c620d-453">Build the project.</span></span>
* <span data-ttu-id="c620d-454">Créez le dossier *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="c620d-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c620d-456">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *Pages/Students* > **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="c620d-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c620d-457">Dans la boîte de dialogue **Ajouter un modèle automatique**, sélectionnez **Pages Razor avec Entity Framework (CRUD)** > **AJOUTER**.</span><span class="sxs-lookup"><span data-stu-id="c620d-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="c620d-458">Renseignez la boîte de dialogue **Pages Razor avec Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="c620d-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="c620d-459">Dans la liste déroulante **Classe de modèle**, sélectionnez **Student (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="c620d-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="c620d-460">Dans la ligne **Classe de contexte de données**, sélectionnez le signe **+** (plus) et remplacez le nom généré par **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="c620d-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="c620d-461">Dans la liste déroulante **Classe de contexte de données**, sélectionnez **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="c620d-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="c620d-462">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c620d-462">Select **Add**.</span></span>

![Boîte de dialogue CRUD](intro/_static/s1.png)

<span data-ttu-id="c620d-464">Consultez [Générer automatiquement le modèle de film](xref:tutorials/razor-pages/model#scaffold-the-movie-model) si vous rencontrez un problème à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="c620d-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c620d-466">Exécutez les commandes suivantes pour générer automatiquement le modèle d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="c620d-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="c620d-467">Le processus de génération de modèles automatique a créé et changé les fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="c620d-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="c620d-468">Fichiers créés</span><span class="sxs-lookup"><span data-stu-id="c620d-468">Files created</span></span>

* <span data-ttu-id="c620d-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="c620d-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="c620d-470">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="c620d-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="c620d-471">Mises à jour du fichier</span><span class="sxs-lookup"><span data-stu-id="c620d-471">File updates</span></span>

* <span data-ttu-id="c620d-472">*Startup.cs* : Les changements apportés à ce fichier sont détaillés dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="c620d-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="c620d-473">*appsettings.json* : La chaîne de connexion utilisée pour se connecter à une base de données locale est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="c620d-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="c620d-474">Examiner le contexte inscrit avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="c620d-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="c620d-475">ASP.NET Core comprend [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c620d-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c620d-476">Des services (tels que le contexte de base de données EF Core) sont inscrits avec l’injection de dépendances au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c620d-477">Ces services sont affectés aux composants qui les nécessitent (par exemple les Pages Razor) par le biais de paramètres de constructeur.</span><span class="sxs-lookup"><span data-stu-id="c620d-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c620d-478">Le code du constructeur qui obtient une instance de contexte de base de données est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c620d-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c620d-479">L’outil de génération de modèles automatique a créé automatiquement un contexte de base de données et l’a inscrit dans le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="c620d-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="c620d-480">Examinez la méthode `ConfigureServices` dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c620d-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="c620d-481">La ligne en surbrillance a été ajoutée par l’outil de génération de modèles automatique :</span><span class="sxs-lookup"><span data-stu-id="c620d-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="c620d-482">Le nom de la chaîne de connexion est transmis au contexte en appelant une méthode sur un objet [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c620d-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c620d-483">Pour le développement local, le [système de configuration ASP.NET Core](xref:fundamentals/configuration/index) lit la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c620d-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="c620d-484">Mettre à jour la méthode Main</span><span class="sxs-lookup"><span data-stu-id="c620d-484">Update main</span></span>

<span data-ttu-id="c620d-485">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="c620d-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="c620d-486">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="c620d-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="c620d-487">Appelez [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="c620d-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="c620d-488">Supprimez le contexte une fois la méthode `EnsureCreated` exécutée.</span><span class="sxs-lookup"><span data-stu-id="c620d-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="c620d-489">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="c620d-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="c620d-490">`EnsureCreated` garantit que la base de données existe pour le contexte.</span><span class="sxs-lookup"><span data-stu-id="c620d-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="c620d-491">Si elle existe, aucune action n’est effectuée.</span><span class="sxs-lookup"><span data-stu-id="c620d-491">If it exists, no action is taken.</span></span> <span data-ttu-id="c620d-492">Si elle n’existe pas, la base de données et tous ses schéma sont créés.</span><span class="sxs-lookup"><span data-stu-id="c620d-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="c620d-493">`EnsureCreated` n’utilise pas de migrations pour créer la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="c620d-494">Une base de données créée avec `EnsureCreated` ne peut pas être mise à jour à l’aide de migrations par la suite.</span><span class="sxs-lookup"><span data-stu-id="c620d-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="c620d-495">`EnsureCreated` est appelée au démarrage de l’application, ce qui active le flux de travail suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="c620d-496">Supprimez la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-496">Delete the DB.</span></span>
* <span data-ttu-id="c620d-497">Modifier le schéma de base de données (par exemple, ajouter un `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="c620d-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="c620d-498">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c620d-498">Run the app.</span></span>
* <span data-ttu-id="c620d-499">`EnsureCreated` crée une base de données avec la colonne `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="c620d-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="c620d-500">`EnsureCreated` est pratique au début du développement quand le schéma évolue rapidement.</span><span class="sxs-lookup"><span data-stu-id="c620d-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="c620d-501">Plus loin dans le tutoriel, la base de données est supprimée et les migrations sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="c620d-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="c620d-502">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="c620d-502">Test the app</span></span>

<span data-ttu-id="c620d-503">Exécutez l’application et acceptez la politique de cookies.</span><span class="sxs-lookup"><span data-stu-id="c620d-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="c620d-504">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="c620d-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="c620d-505">Vous pouvez en savoir plus sur la politique de cookies à la section [Prise en charge du règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="c620d-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="c620d-506">Sélectionnez le lien **Students**, puis **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="c620d-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="c620d-507">Testez les liens Edit, Details et Delete.</span><span class="sxs-lookup"><span data-stu-id="c620d-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="c620d-508">Examiner le contexte de base de données SchoolContext</span><span class="sxs-lookup"><span data-stu-id="c620d-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="c620d-509">La classe principale qui coordonne les fonctionnalités d’EF Core pour un modèle de données spécifié est la classe de contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="c620d-510">Le contexte de données est dérivé de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c620d-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c620d-511">Il spécifie les entités qui sont incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="c620d-512">Dans ce projet, la classe est nommée `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c620d-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="c620d-513">Mettez à jour *SchoolContext.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="c620d-514">Le code en surbrillance crée une propriété [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) pour chaque jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="c620d-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="c620d-515">Dans la terminologie d’EF Core :</span><span class="sxs-lookup"><span data-stu-id="c620d-515">In EF Core terminology:</span></span>

* <span data-ttu-id="c620d-516">Un jeu d’entités correspond généralement à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="c620d-517">Une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="c620d-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c620d-518">`DbSet<Enrollment>` et `DbSet<Course>` peuvent être omis.</span><span class="sxs-lookup"><span data-stu-id="c620d-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="c620d-519">EF Core les inclut implicitement, car l’entité `Student` référence l’entité `Enrollment`, et l’entité `Enrollment` référence l’entité `Course`.</span><span class="sxs-lookup"><span data-stu-id="c620d-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="c620d-520">Pour ce didacticiel, conservez `DbSet<Enrollment>` et `DbSet<Course>` dans le `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c620d-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="c620d-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="c620d-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c620d-522">La chaîne de connexion spécifie [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="c620d-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="c620d-523">LocalDB est une version allégée du moteur de base de données SQL Server Express. Elle est destinée au développement d’applications, et non à une utilisation en production.</span><span class="sxs-lookup"><span data-stu-id="c620d-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="c620d-524">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="c620d-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c620d-525">Par défaut, LocalDB crée des fichiers de base de données *.mdf* dans le répertoire `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="c620d-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="c620d-526">Ajouter du code pour initialiser la base de données avec des données de test</span><span class="sxs-lookup"><span data-stu-id="c620d-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="c620d-527">EF Core crée une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="c620d-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="c620d-528">Dans cette section, une méthode `Initialize` est écrite pour la remplir avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="c620d-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="c620d-529">Dans le dossier *Data*, créez un fichier de classe nommé *DbInitializer.cs* et ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c620d-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="c620d-530">Remarque : Le code précédent utilise `Models` pour l’espace de noms (`namespace ContosoUniversity.Models`) au lieu de `Data`.</span><span class="sxs-lookup"><span data-stu-id="c620d-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="c620d-531">`Models` est cohérent avec le code généré par l’outil de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="c620d-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="c620d-532">Pour plus d’informations, consultez [ce problème de génération de modèles automatique GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="c620d-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="c620d-533">Le code vérifie s’il existe des étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="c620d-534">S’il n’y en a pas, la base de données est initialisée avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="c620d-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="c620d-535">Il charge les données de test dans les tableaux plutôt que dans les collections `List<T>` afin d’optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="c620d-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="c620d-536">La méthode `EnsureCreated` crée automatiquement la base de données pour le contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="c620d-537">Si la base de données existe, `EnsureCreated` retourne sans modifier la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="c620d-538">Dans *Program.cs*, modifiez la méthode `Main` pour appeler `Initialize` :</span><span class="sxs-lookup"><span data-stu-id="c620d-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c620d-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c620d-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c620d-540">Arrêtez l’application si elle est en cours d’exécution et exécutez la commande suivante dans la **Console du gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="c620d-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c620d-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c620d-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c620d-542">Arrêtez l’application si elle est en cours d’exécution, puis supprimez le fichier *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="c620d-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="c620d-543">Afficher la base de données</span><span class="sxs-lookup"><span data-stu-id="c620d-543">View the DB</span></span>

<span data-ttu-id="c620d-544">Le nom de la base de données est généré à partir du nom de contexte indiqué précédemment, ainsi que d’un tiret et d’un GUID.</span><span class="sxs-lookup"><span data-stu-id="c620d-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="c620d-545">Il sera donc « SchoolContext-{GUID} ».</span><span class="sxs-lookup"><span data-stu-id="c620d-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="c620d-546">Le GUID est différent pour chaque utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c620d-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="c620d-547">Ouvrez **l’Explorateur d’objets SQL Server** (SSOX) à partir du menu **Affichage** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c620d-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="c620d-548">Dans SSOX, cliquez sur **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="c620d-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="c620d-549">Développez le noeud **Tables**.</span><span class="sxs-lookup"><span data-stu-id="c620d-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="c620d-550">Cliquez avec le bouton droit sur la table **Student** et cliquez sur **Afficher les données** pour voir les colonnes créées et les lignes insérées dans la table.</span><span class="sxs-lookup"><span data-stu-id="c620d-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="c620d-551">Code asynchrone</span><span class="sxs-lookup"><span data-stu-id="c620d-551">Asynchronous code</span></span>

<span data-ttu-id="c620d-552">La programmation asynchrone est le mode par défaut pour ASP.NET Core et EF Core.</span><span class="sxs-lookup"><span data-stu-id="c620d-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="c620d-553">Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés.</span><span class="sxs-lookup"><span data-stu-id="c620d-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="c620d-554">Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés.</span><span class="sxs-lookup"><span data-stu-id="c620d-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="c620d-555">Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent.</span><span class="sxs-lookup"><span data-stu-id="c620d-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="c620d-556">Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes.</span><span class="sxs-lookup"><span data-stu-id="c620d-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="c620d-557">Il permet ainsi d’utiliser les ressources serveur plus efficacement, et le serveur peut gérer plus de trafic sans retard.</span><span class="sxs-lookup"><span data-stu-id="c620d-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="c620d-558">Le code asynchrone introduit une petite quantité de charge au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="c620d-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="c620d-559">Dans les situations de faible trafic, le gain de performances est négligeable, tandis qu’en cas de trafic élevé l’amélioration potentielle des performances est importante.</span><span class="sxs-lookup"><span data-stu-id="c620d-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="c620d-560">Dans le code suivant, le mot clé [async](/dotnet/csharp/language-reference/keywords/async), la valeur renvoyée `Task<T>`, le mot clé `await` et la méthode `ToListAsync` déclenchent l’exécution asynchrone du code.</span><span class="sxs-lookup"><span data-stu-id="c620d-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="c620d-561">Le mot clé `async` fait en sorte que le compilateur :</span><span class="sxs-lookup"><span data-stu-id="c620d-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="c620d-562">Génèrer des rappels pour les parties du corps de méthode.</span><span class="sxs-lookup"><span data-stu-id="c620d-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="c620d-563">Crée automatiquement l’objet [Task](/dotnet/api/system.threading.tasks.task) qui est retourné.</span><span class="sxs-lookup"><span data-stu-id="c620d-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="c620d-564">Pour plus d’informations, consultez [Type de retour Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="c620d-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="c620d-565">Le type de retour implicite `Task` représente le travail en cours.</span><span class="sxs-lookup"><span data-stu-id="c620d-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="c620d-566">Le (mot clé) `await`, le compilateur fractionner la méthode en deux parties.</span><span class="sxs-lookup"><span data-stu-id="c620d-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="c620d-567">La première partie se termine par l’opération qui est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="c620d-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="c620d-568">La seconde partie est placée dans une méthode de rappel qui est appelée quand l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="c620d-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="c620d-569">`ToListAsync` est la version asynchrone de la méthode d’extension `ToList`.</span><span class="sxs-lookup"><span data-stu-id="c620d-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="c620d-570">Les éléments à connaître lors de l’écriture de code asynchrone qui utilise EF Core :</span><span class="sxs-lookup"><span data-stu-id="c620d-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="c620d-571">Uniquement les instructions qui génèrent des requêtes ou des commandes à envoyer à la base de données sont exécutées de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="c620d-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="c620d-572">Ceci inclut, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, et `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="c620d-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="c620d-573">Il n’inclut pas les instructions qui modifient simplement un `IQueryable`, tel que `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="c620d-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="c620d-574">Un contexte EF Core n’est pas thread-safe : n’essayez pas d’effectuer plusieurs opérations en parallèle.</span><span class="sxs-lookup"><span data-stu-id="c620d-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="c620d-575">Pour tirer parti des avantages de performances du code asynchrone, vérifiez que les packages de bibliothèque (par exemple pour la pagination) utilisent le mode asynchrone s’ils appellent des méthodes EF Core qui envoient des requêtes à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c620d-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="c620d-576">Pour plus d’informations sur la programmation asynchrone dans .NET, consultez [Vue d’ensemble d’Async](/dotnet/standard/async) et [Programmation asynchrone avec async et await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="c620d-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="c620d-577">Dans le didacticiel suivant, les opérations CRUD de base (créer, lire, mettre à jour, supprimer).</span><span class="sxs-lookup"><span data-stu-id="c620d-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="c620d-578">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c620d-578">Additional resources</span></span>

* [<span data-ttu-id="c620d-579">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="c620d-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="c620d-580">Next</span><span class="sxs-lookup"><span data-stu-id="c620d-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
