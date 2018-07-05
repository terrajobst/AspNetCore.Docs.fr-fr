---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Création du chapitre téléchargements pour le MVC EF 5 4 didacticiels | Microsoft Docs
author: Rick-Anderson
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6f1a28a2703fa543430d0210cc7792cb19439136
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379918"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="c2ff8-103">Création du chapitre téléchargements pour le MVC EF 5 4 didacticiels</span><span class="sxs-lookup"><span data-stu-id="c2ff8-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="c2ff8-104">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c2ff8-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="c2ff8-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="c2ff8-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="c2ff8-106">L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de l’Entity Framework 5 Code First et Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="c2ff8-107">Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="c2ff8-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="c2ff8-108">Créer les téléchargements du chapitre</span><span class="sxs-lookup"><span data-stu-id="c2ff8-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="c2ff8-109">Téléchargez et décompressez le fichier zip d’exemple de projet.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="c2ff8-110">Dans le package de téléchargement décompressé, vous trouverez les fichiers zip supplémentaires, une pour la fin de chaque chapitre.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="c2ff8-111">Cliquez avec le bouton droit sur le fichier zip de votre choix, cliquez sur **propriétés**, puis cliquez sur le **Unblock** bouton.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="c2ff8-112">Décompressez le fichier.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-112">Unzip the file.</span></span>
4. <span data-ttu-id="c2ff8-113">Double-cliquez sur le *CUx.sln* fichier pour lancer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="c2ff8-114">À partir de la **outils** menu, cliquez sur **Library Package Manager**, puis **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="c2ff8-115">Dans le Package Manager de la console, cliquez sur **restaurer**.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="c2ff8-116">Quittez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="c2ff8-117">Redémarrez Visual Studio, en ouvrant le fichier de solution que vous avez fermé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="c2ff8-118">Dans le Package Manager de la console, entrez le `Update-Database` commande :</span><span class="sxs-lookup"><span data-stu-id="c2ff8-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="c2ff8-119">Si vous obtenez l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="c2ff8-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="c2ff8-120">*Le terme « Update-Database » n’est pas reconnu comme le nom de l’applet de commande, fonction, fichier de script ou programme exécutable. Vérifiez l’orthographe du nom, ou si un chemin d’accès a été inclus, vérifiez que le chemin d’accès est correct et réessayez.*</span><span class="sxs-lookup"><span data-stu-id="c2ff8-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="c2ff8-121">Quittez et redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="c2ff8-122">Chaque migration s’exécute, puis la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="c2ff8-123">Vous pouvez maintenant exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c2ff8-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="c2ff8-124">Précédent</span><span class="sxs-lookup"><span data-stu-id="c2ff8-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
