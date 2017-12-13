---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "Génération de modèles automatique ASP.NET dans Visual Studio 2013 | Documents Microsoft"
author: tfitzmac
description: "Génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="70f41-103">Génération de modèles automatique ASP.NET dans Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="70f41-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="70f41-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="70f41-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="70f41-105">Génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="70f41-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="70f41-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="70f41-106">Overview</span></span>

<span data-ttu-id="70f41-107">Génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="70f41-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="70f41-108">Visual Studio 2013 inclut des générateurs de code préalablement installés pour les projets MVC et API Web.</span><span class="sxs-lookup"><span data-stu-id="70f41-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="70f41-109">Vous ajoutez la structure à votre projet lorsque vous souhaitez ajouter rapidement du code qui interagit avec les modèles de données.</span><span class="sxs-lookup"><span data-stu-id="70f41-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="70f41-110">À l’aide de la génération de modèles automatique peut réduire la quantité de temps au développement des opérations de données standard dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="70f41-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="70f41-111">Par défaut, Visual Studio 2013 ne prend pas en charge la génération de code pour un projet Web Forms, mais vous pouvez utiliser la structure des Web Forms en ajoutant des dépendances MVC au projet ou installation d’une extension.</span><span class="sxs-lookup"><span data-stu-id="70f41-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="70f41-112">Les deux approches sont indiquées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="70f41-112">Both approaches are shown below.</span></span>

<span data-ttu-id="70f41-113">Visual Studio 2013 Update 2 (actuellement RC) offre la possibilité d’étendre la génération de modèles automatique ASP.NET pour satisfaire les exigences de votre scénario.</span><span class="sxs-lookup"><span data-stu-id="70f41-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="70f41-114">Avec cette fonctionnalité, vous pouvez créer un modèle personnalisé de la structure et l’ajouter à la boîte de dialogue Ajouter une nouvelle structure.</span><span class="sxs-lookup"><span data-stu-id="70f41-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="70f41-115">Dans le modèle personnalisé, vous spécifiez le code qui est généré lors de l’ajout d’un élément généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="70f41-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="70f41-116">Pour plus d’informations, consultez [création d’un Scaffolder personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="70f41-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70f41-117">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="70f41-117">Prerequisites</span></span>

<span data-ttu-id="70f41-118">Pour utiliser la génération de modèles automatique ASP.NET, vous devez :</span><span class="sxs-lookup"><span data-stu-id="70f41-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="70f41-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="70f41-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="70f41-120">Outils de développement Web (partie de l’installation de Visual Studio 2013 par défaut)</span><span class="sxs-lookup"><span data-stu-id="70f41-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="70f41-121">Infrastructures de Web ASP.NET et outils 2013 (partie de l’installation de Visual Studio 2013 par défaut)</span><span class="sxs-lookup"><span data-stu-id="70f41-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="70f41-122">Ajouter un élément généré automatiquement pour MVC ou l’API Web</span><span class="sxs-lookup"><span data-stu-id="70f41-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="70f41-123">Pour ajouter une structure, cliquez sur le projet ou un dossier du projet, puis sélectionnez **ajouter** – **un nouvel élément structuré**, comme illustré dans l’image suivante.</span><span class="sxs-lookup"><span data-stu-id="70f41-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Ajouter un élément de la vue de structure](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="70f41-125">À partir de la **ajouter une vue de structure** fenêtre, sélectionnez le type de structure à ajouter.</span><span class="sxs-lookup"><span data-stu-id="70f41-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Sélectionnez le type de vue de structure](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="70f41-127">Le **ajouter un contrôleur** fenêtre vous donne la possibilité de sélectionner des options pour générer le contrôleur, notamment si vous souhaitez utiliser les nouvelles fonctionnalités asynchrones à partir de l’Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="70f41-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Ajouter un contrôleur](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="70f41-129">Les classes appropriées et les pages sont créés pour votre scénario.</span><span class="sxs-lookup"><span data-stu-id="70f41-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="70f41-130">Par exemple, l’illustration suivante montre le contrôleur MVC et les vues qui ont été créés via la structure d’une classe de modèle nommée films.</span><span class="sxs-lookup"><span data-stu-id="70f41-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Les fichiers créés](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="70f41-132">Ajouter un élément généré automatiquement pour Web Forms</span><span class="sxs-lookup"><span data-stu-id="70f41-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="70f41-133">Pour ajouter la structure qui génère du code de Web Forms, vous devez installer une extension de Visual Studio ou ajouter des dépendances MVC.</span><span class="sxs-lookup"><span data-stu-id="70f41-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="70f41-134">Les deux approches sont indiquées ci-dessous, mais vous devez uniquement effectuer l’une de ces approches.</span><span class="sxs-lookup"><span data-stu-id="70f41-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="70f41-135">Extension de génération de modèles automatique de Web Forms</span><span class="sxs-lookup"><span data-stu-id="70f41-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="70f41-136">Vous pouvez installer une extension Visual Studio qui vous permettent d’utiliser la structure avec un projet Web Forms.</span><span class="sxs-lookup"><span data-stu-id="70f41-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="70f41-137">Dans Visual Studio, sélectionnez **outils** , puis **Extensions et mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="70f41-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="70f41-138">À partir de cette boîte de dialogue Rechercher la galerie Visual Studio pour **la structure de Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="70f41-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![installer les formulaires web génération de modèles automatique](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="70f41-140">Pour plus d’informations, consultez [la structure de Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="70f41-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="70f41-141">Dépendances MVC</span><span class="sxs-lookup"><span data-stu-id="70f41-141">MVC Dependencies</span></span>

<span data-ttu-id="70f41-142">Pour ajouter des dépendances MVC, sélectionnez **ajouter** - **un nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="70f41-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="70f41-143">Dans la fenêtre Ajouter une vue de structure, sélectionnez **dépendances MVC**, comme illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="70f41-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![ajouter des dépendances MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="70f41-145">Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète.</span><span class="sxs-lookup"><span data-stu-id="70f41-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="70f41-146">Si vous sélectionnez Minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet.</span><span class="sxs-lookup"><span data-stu-id="70f41-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="70f41-147">Si vous sélectionnez l’option complet, les dépendances minimale sont ajoutés, ainsi que les fichiers de contenu requis pour un projet MVC.</span><span class="sxs-lookup"><span data-stu-id="70f41-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="70f41-148">Pour utiliser facilement la génération de modèles automatique, sélectionnez dépendances complètes.</span><span class="sxs-lookup"><span data-stu-id="70f41-148">To easily use scaffolding, select Full dependencies.</span></span>

![Sélectionnez les dépendances complètes](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="70f41-150">Après avoir ajouté les dépendances, vous verrez un **readme.txt** fichier.</span><span class="sxs-lookup"><span data-stu-id="70f41-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="70f41-151">Suivez attentivement les instructions de ce fichier pour vous assurer que votre projet fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="70f41-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="70f41-152">Lorsque vous avez terminé les étapes décrites dans le fichier readme.txt, vous pouvez ajouter un nouvel élément structuré comme indiqué dans la section précédente sur MVC et API Web.</span><span class="sxs-lookup"><span data-stu-id="70f41-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="70f41-153">Les vues de généré automatiquement et d’un contrôleur ne fonctionnent pas correctement au sein de votre projet.</span><span class="sxs-lookup"><span data-stu-id="70f41-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="70f41-154">Didacticiels</span><span class="sxs-lookup"><span data-stu-id="70f41-154">Tutorials</span></span>

<span data-ttu-id="70f41-155">Pour créer un scaffolder personnalisé, consultez [création d’un Scaffolder personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="70f41-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="70f41-156">Pour personnaliser les fichiers générés, consultez [comment personnaliser les fichiers générés à partir de la boîte de dialogue Nouvel élément structuré](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="70f41-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="70f41-157">Pour obtenir un exemple d’utilisation de la structure avec **développement Database First**, consultez [EF Database First avec ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="70f41-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="70f41-158">Pour obtenir un exemple d’utilisation de la structure dans un **MVC** projet d’équipe, consultez [mise en route avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="70f41-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="70f41-159">Pour obtenir un exemple d’utilisation de la structure dans un **API Web** projet d’équipe, consultez [créer une API REST avec le routage d’attribut dans l’API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="70f41-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
