---
title: "Ajout d’un modèle à une application de pages Razor dans ASP.NET Core"
author: rick-anderson
description: "Ajout d’un modèle à une application de pages Razor dans ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0ce7693bfdc37d930488304b329dbcd533a5ec1d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="3305b-103">Ajout d’un modèle à une application de pages Razor</span><span class="sxs-lookup"><span data-stu-id="3305b-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="3305b-104">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="3305b-104">Add a data model</span></span>

<span data-ttu-id="3305b-105">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie** > **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="3305b-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="3305b-106">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="3305b-106">Name the folder *Models*.</span></span>

<span data-ttu-id="3305b-107">Cliquez avec le bouton droit sur le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="3305b-107">Right click the *Models* folder.</span></span> <span data-ttu-id="3305b-108">Sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="3305b-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="3305b-109">Nommez la classe **Movie**, puis ajoutez les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="3305b-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="3305b-110">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="3305b-110">Add a database connection string</span></span>

<span data-ttu-id="3305b-111">Ajoutez une chaîne de connexion au fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3305b-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="3305b-112">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="3305b-112">Register the database context</span></span>

<span data-ttu-id="3305b-113">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3305b-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="3305b-114">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="3305b-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="3305b-115">Ajouter un outil de génération de modèles automatique et effectuer la migration initiale</span><span class="sxs-lookup"><span data-stu-id="3305b-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="3305b-116">Dans cette section, effectuez les tâches suivantes à l’aide de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="3305b-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="3305b-117">Ajoutez le package de génération de code web Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3305b-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="3305b-118">Ce package est nécessaire à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="3305b-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="3305b-119">Ajoutez une migration initiale.</span><span class="sxs-lookup"><span data-stu-id="3305b-119">Add an initial migration.</span></span>
* <span data-ttu-id="3305b-120">Mettez à jour la base de données avec la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="3305b-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="3305b-121">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="3305b-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu Console du Gestionnaire de package](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3305b-123">Dans la console du gestionnaire de package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="3305b-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="3305b-124">La commande `Install-Package` installe les outils nécessaires à l’exécution du moteur de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="3305b-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="3305b-125">La commande `Add-Migration` génère le code nécessaire à la création du schéma de base de données initial.</span><span class="sxs-lookup"><span data-stu-id="3305b-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="3305b-126">Le schéma est basé sur le modèle spécifié dans le fichier `DbContext` (dans *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="3305b-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="3305b-127">L’argument `Initial` est utilisé pour nommer les migrations.</span><span class="sxs-lookup"><span data-stu-id="3305b-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="3305b-128">Vous pouvez utiliser n’importe quel nom, mais par convention, choisissez un nom qui décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="3305b-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="3305b-129">Pour plus d’informations, consultez [Présentation des migrations](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="3305b-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="3305b-130">La commande `Update-Database` exécute la méthode `Up` dans le fichier *Migrations/\<horodatage>_InitialCreate.cs*, ce qui entraîne la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="3305b-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="3305b-131">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="3305b-131">Test the app</span></span>

* <span data-ttu-id="3305b-132">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="3305b-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="3305b-133">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="3305b-133">Test the **Create** link.</span></span>

 ![Créer une page](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="3305b-135">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="3305b-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="3305b-136">Si vous obtenez une exception SQL, vérifiez que vous avez exécuté les migrations et mis à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="3305b-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="3305b-137">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="3305b-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3305b-138">[Précédent : Bien démarrer](xref:tutorials/razor-pages/razor-pages-start)
[Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="3305b-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
