---
title: Ajouter un modèle à une application de pages Razor ASP.NET Core avec Visual Studio pour Mac
author: rick-anderson
description: Découvrez comment ajouter un modèle à une application de pages Razor dans ASP.NET Core à l’aide de Visual Studio pour Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 97bc9f14b8d6da958a7f587e54a37d2d0e0aabd4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483663"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="befd4-103">Ajouter un modèle à une application de pages Razor ASP.NET Core avec Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="befd4-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="befd4-104">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="befd4-104">Add a data model</span></span>

* <span data-ttu-id="befd4-105">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="befd4-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="befd4-106">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="befd4-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="befd4-107">Cliquez avec le bouton droit sur le dossier *Models*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="befd4-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="befd4-108">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="befd4-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="befd4-109">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="befd4-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="befd4-110">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="befd4-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="befd4-111">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="befd4-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="befd4-112">Cliquez avec le bouton droit sur une ligne ondulée rouge, par exemple `MovieContext` à la ligne `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="befd4-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="befd4-113">Sélectionnez **Correctif rapide > using RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="befd4-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="befd4-114">Visual Studio ajoute l’instruction using.</span><span class="sxs-lookup"><span data-stu-id="befd4-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="befd4-115">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="befd4-115">Build the project to verify you don't have any errors.</span></span>

![Créer une page](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="befd4-117">Packages NuGet Entity Framework Core pour les migrations</span><span class="sxs-lookup"><span data-stu-id="befd4-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="befd4-118">Les outils EF de l’interface de ligne de commande (CLI) sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="befd4-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="befd4-119">Cliquez sur le lien [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) pour obtenir le numéro de version à utiliser.</span><span class="sxs-lookup"><span data-stu-id="befd4-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="befd4-120">Pour installer ce package, ajoutez-le à la collection `DotNetCliToolReference` dans le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="befd4-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="befd4-121">**Remarque :** Vous devez installer ce package en modifiant le fichier *.csproj*. Vous ne pouvez pas utiliser la commande `install-package` ou le GUI (interface graphique utilisateur) du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="befd4-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="befd4-122">Pour modifier un fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="befd4-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="befd4-123">Sélectionnez **Fichier** > **Ouvrir**, puis sélectionnez le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="befd4-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="befd4-124">Sélectionnez **Options**.</span><span class="sxs-lookup"><span data-stu-id="befd4-124">Select **Options**.</span></span>
* <span data-ttu-id="befd4-125">Affectez à **Ouvrir avec** la valeur **Éditeur de code source**.</span><span class="sxs-lookup"><span data-stu-id="befd4-125">Change **Open with** to **Source Code Editor**.</span></span>

![Modifier le fichier csproj](model/csproj.png)

<span data-ttu-id="befd4-127">Ajoutez la référence d’outil de `Microsoft.EntityFrameworkCore.Tools.DotNet` au deuxième **\<ItemGroup>**  :</span><span class="sxs-lookup"><span data-stu-id="befd4-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="befd4-128">Les numéros de version indiqués dans le code suivant étaient corrects au moment où ils ont été écrits.</span><span class="sxs-lookup"><span data-stu-id="befd4-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="befd4-129">Ajouter les fichiers Pages/Movies au projet</span><span class="sxs-lookup"><span data-stu-id="befd4-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="befd4-130">Dans Visual Studio, cliquez avec le bouton droit sur le dossier *Pages*, puis sélectionnez **Ajouter > Ajouter un dossier existant**.</span><span class="sxs-lookup"><span data-stu-id="befd4-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="befd4-131">Sélectionnez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="befd4-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="befd4-132">Dans la boîte de dialogue *Choisir les fichiers à inclure dans le projet*, sélectionnez **Tout inclure**.</span><span class="sxs-lookup"><span data-stu-id="befd4-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="befd4-133">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="befd4-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="befd4-134">[Précédent : Bien démarrer](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="befd4-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
