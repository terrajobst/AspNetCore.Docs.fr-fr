---
title: Ajouter un modèle à une application de pages Razor ASP.NET Core avec Visual Studio Code
author: rick-anderson
description: Découvrez comment ajouter un modèle à une application de pages Razor dans ASP.NET Core à l’aide de Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244721"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="6643d-103">Ajouter un modèle à une application de pages Razor ASP.NET Core avec Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6643d-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6643d-104">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="6643d-104">Add a data model</span></span>

* <span data-ttu-id="6643d-105">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="6643d-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6643d-106">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6643d-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="6643d-107">Ajoutez le code suivant au fichier *Models/Movie.cs* :</span><span class="sxs-lookup"><span data-stu-id="6643d-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="6643d-108">Package NuGet Entity Framework Core pour SQLite</span><span class="sxs-lookup"><span data-stu-id="6643d-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="6643d-109">À partir de la ligne de commande, exécutez la commande CLI .NET Core suivante :</span><span class="sxs-lookup"><span data-stu-id="6643d-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="6643d-110">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6643d-110">Register the database context</span></span>

<span data-ttu-id="6643d-111">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6643d-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="6643d-112">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="6643d-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6643d-113">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="6643d-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="6643d-114">Effectuer la génération de modèles automatique pour le modèle Movie</span><span class="sxs-lookup"><span data-stu-id="6643d-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="6643d-115">Ouvrez une fenêtre Commande dans le répertoire de projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="6643d-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6643d-116">**Pour Windows**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="6643d-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6643d-117">**Pour macOS et Linux**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="6643d-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="6643d-118">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="6643d-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6643d-119">[Précédent : Bien démarrer](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="6643d-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
