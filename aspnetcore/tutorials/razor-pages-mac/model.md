---
title: "Ajout d’un modèle à une application de pages Razor avec Visual Studio pour Mac"
author: rick-anderson
description: "Ajout d’un modèle à une application de pages Razor dans ASP.NET Core à l’aide de Visual Studio pour Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: f4fb4fc3402c866fa9f956341c06be34ca9f4763
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="c5c98-103">Ajout d’un modèle à une application de pages Razor dans ASP.NET Core avec Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c5c98-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c5c98-104">Ajouter un modèle de données</span><span class="sxs-lookup"><span data-stu-id="c5c98-104">Add a data model</span></span>

* <span data-ttu-id="c5c98-105">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **RazorPagesMovie**, puis sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="c5c98-106">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="c5c98-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="c5c98-107">Cliquez avec le bouton droit sur le dossier *Models*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="c5c98-108">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="c5c98-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="c5c98-109">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="c5c98-110">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="c5c98-111">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="c5c98-112">Cliquez avec le bouton droit sur une ligne ondulée rouge, par exemple `MovieContext` à la ligne `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="c5c98-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="c5c98-113">Sélectionnez **Correctif rapide > using RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="c5c98-114">Visual Studio ajoute l’instruction using.</span><span class="sxs-lookup"><span data-stu-id="c5c98-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="c5c98-115">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="c5c98-115">Build the project to verify you don't have any errors.</span></span>

![Créer une page](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="c5c98-117">Packages NuGet Entity Framework Core pour les migrations</span><span class="sxs-lookup"><span data-stu-id="c5c98-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="c5c98-118">Les outils EF de l’interface de ligne de commande (CLI) sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="c5c98-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="c5c98-119">Cliquez sur le lien [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) pour obtenir le numéro de version à utiliser.</span><span class="sxs-lookup"><span data-stu-id="c5c98-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="c5c98-120">Pour installer ce package, ajoutez-le à la collection `DotNetCliToolReference` dans le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c5c98-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="c5c98-121">**Remarque :** Vous devez installer ce package en modifiant le fichier *.csproj*. Vous ne pouvez pas utiliser la commande `install-package` ou le GUI (interface graphique utilisateur) du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="c5c98-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="c5c98-122">Pour modifier un fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="c5c98-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="c5c98-123">Sélectionnez **Fichier** > **Ouvrir**, puis sélectionnez le fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c5c98-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="c5c98-124">Sélectionnez **Options**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-124">Select **Options**.</span></span>
* <span data-ttu-id="c5c98-125">Affectez à **Ouvrir avec** la valeur **Éditeur de code source**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-125">Change **Open with** to **Source Code Editor**.</span></span>

![Modifier le fichier csproj](model/csproj.png)

<span data-ttu-id="c5c98-127">Ajoutez la référence d’outil de `Microsoft.EntityFrameworkCore.Tools.DotNet` au deuxième **\<ItemGroup>** :</span><span class="sxs-lookup"><span data-stu-id="c5c98-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="c5c98-128">Les numéros de version indiqués dans le code suivant étaient corrects au moment où ils ont été écrits.</span><span class="sxs-lookup"><span data-stu-id="c5c98-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="c5c98-129">Ajouter les fichiers Pages/Movies au projet</span><span class="sxs-lookup"><span data-stu-id="c5c98-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="c5c98-130">Dans Visual Studio, cliquez avec le bouton droit sur le dossier *Pages*, puis sélectionnez **Ajouter > Ajouter un dossier existant**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="c5c98-131">Sélectionnez le dossier *Movies*.</span><span class="sxs-lookup"><span data-stu-id="c5c98-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="c5c98-132">Dans la boîte de dialogue *Choisir les fichiers à inclure dans le projet*, sélectionnez **Inclure tout**.</span><span class="sxs-lookup"><span data-stu-id="c5c98-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="c5c98-133">Le prochain didacticiel décrit les fichiers créés par la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="c5c98-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c5c98-134">[Précédent : Bien démarrer](xref:tutorials/razor-pages-mac/razor-pages-start)
[Suivant : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="c5c98-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
