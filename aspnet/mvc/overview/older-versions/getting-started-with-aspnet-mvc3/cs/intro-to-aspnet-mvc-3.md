---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introduction à ASP.NET MVC 3 (c#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: fe98ad8a9f7891b3bf17d2da840d0c333d96a210
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833202"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="dab82-103">Introduction à ASP.NET MVC 3 (c#)</span><span class="sxs-lookup"><span data-stu-id="dab82-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="dab82-104">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="dab82-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="dab82-105">Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="dab82-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="dab82-106">Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="dab82-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="dab82-107">Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dab82-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="dab82-108">Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="dab82-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="dab82-109">Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="dab82-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="dab82-110">Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :</span><span class="sxs-lookup"><span data-stu-id="dab82-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="dab82-111">Prérequis pour le Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="dab82-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="dab82-112">Mettre à jour des outils ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="dab82-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="dab82-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)</span><span class="sxs-lookup"><span data-stu-id="dab82-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="dab82-114">Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="dab82-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="dab82-115">Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="dab82-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="dab82-116">[Téléchargez la version c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="dab82-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="dab82-117">Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="dab82-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="dab82-118">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="dab82-118">What You'll Build</span></span>

<span data-ttu-id="dab82-119">Vous allez implémenter une application de liste de film simple qui prend en charge la création, la modification et la liste de films à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="dab82-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="dab82-120">Voici deux captures d’écran de l’application que vous allez générer.</span><span class="sxs-lookup"><span data-stu-id="dab82-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="dab82-121">Il comprend une page qui affiche une liste de films à partir d’une base de données :</span><span class="sxs-lookup"><span data-stu-id="dab82-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="dab82-123">L’application vous permet également ajouter, modifier et supprimer des films, mais aussi voir des détails sur les modifications individuelles.</span><span class="sxs-lookup"><span data-stu-id="dab82-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="dab82-124">Tous les scénarios de saisie de données incluent la validation pour vous assurer que les données stockées dans la base de données sont correctes.</span><span class="sxs-lookup"><span data-stu-id="dab82-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="dab82-125">Vous allez apprendre des compétences</span><span class="sxs-lookup"><span data-stu-id="dab82-125">Skills You'll Learn</span></span>

<span data-ttu-id="dab82-126">Voici ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="dab82-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="dab82-127">Comment créer un nouveau projet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dab82-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="dab82-128">La création d’ASP.NET MVC contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="dab82-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="dab82-129">Comment créer une nouvelle base de données à l’aide du modèle Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="dab82-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="dab82-130">Comment récupérer et afficher des données.</span><span class="sxs-lookup"><span data-stu-id="dab82-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="dab82-131">Comment modifier des données et activer la validation de données.</span><span class="sxs-lookup"><span data-stu-id="dab82-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="dab82-132">Prise en main</span><span class="sxs-lookup"><span data-stu-id="dab82-132">Getting Started</span></span>

<span data-ttu-id="dab82-133">Commencez par exécuter Visual Web Developer 2010 Express (« Visual Web Developer » pour faire plus court) et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="dab82-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="dab82-134">Visual Web Developer est un environnement de développement intégré ou IDE.</span><span class="sxs-lookup"><span data-stu-id="dab82-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="dab82-135">Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="dab82-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="dab82-136">Dans Visual Web Developer, il existe une barre d’outils en haut montrant les différentes options disponibles pour vous.</span><span class="sxs-lookup"><span data-stu-id="dab82-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="dab82-137">Il existe également un menu qui fournit une autre façon d’effectuer des tâches dans l’IDE.</span><span class="sxs-lookup"><span data-stu-id="dab82-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="dab82-138">(Par exemple, au lieu de sélectionner **nouveau projet** à partir de la **Démarrer** page, vous pouvez utiliser le menu et sélectionnez **fichier** &gt; **denouveauprojet**.)</span><span class="sxs-lookup"><span data-stu-id="dab82-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="dab82-139">Créer votre première Application</span><span class="sxs-lookup"><span data-stu-id="dab82-139">Creating Your First Application</span></span>

<span data-ttu-id="dab82-140">Vous pouvez créer des applications à l’aide de Visual Basic ou Visual c# comme langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="dab82-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="dab82-141">Sélectionnez Visual c# sur la gauche, puis sélectionnez **Application Web de ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="dab82-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="dab82-142">Nommez votre projet « MvcMovie », puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="dab82-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="dab82-143">(Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="dab82-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="dab82-144">Dans le **nouveau projet ASP.NET MVC 3** boîte de dialogue, sélectionnez **Application Internet**.</span><span class="sxs-lookup"><span data-stu-id="dab82-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="dab82-145">Vérifiez **balisage utiliser HTML5** et laissez **Razor** en tant que le moteur d’affichage par défaut.</span><span class="sxs-lookup"><span data-stu-id="dab82-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="dab82-146">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="dab82-146">Click **OK**.</span></span> <span data-ttu-id="dab82-147">Visual Web Developer a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant !</span><span class="sxs-lookup"><span data-stu-id="dab82-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="dab82-148">Il s’agit d’un simple « Hello World ! »</span><span class="sxs-lookup"><span data-stu-id="dab82-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="dab82-149">projet et il l’un bon point de départ de votre application.</span><span class="sxs-lookup"><span data-stu-id="dab82-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="dab82-150">Dans le menu **Déboguer**, sélectionnez **Démarrer le débogage**.</span><span class="sxs-lookup"><span data-stu-id="dab82-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="dab82-151">Notez que le raccourci clavier pour démarrer le débogage F5.</span><span class="sxs-lookup"><span data-stu-id="dab82-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="dab82-152">F5 provoque Visual Web Developer démarrer un serveur web de développement et d’exécuter votre application web.</span><span class="sxs-lookup"><span data-stu-id="dab82-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="dab82-153">Visual Web Developer lance un navigateur, puis ouvre la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="dab82-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="dab82-154">Notez que la barre d’adresses du navigateur indique `localhost` et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dab82-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="dab82-155">C’est parce que `localhost` pointe toujours vers votre ordinateur local, qui dans ce cas s’exécute l’application que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="dab82-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="dab82-156">Lorsque Visual Web Developer exécute un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="dab82-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="dab82-157">Dans l’image ci-dessous, le numéro de port aléatoire est 43246.</span><span class="sxs-lookup"><span data-stu-id="dab82-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="dab82-158">Lorsque vous exécutez l’application, vous verrez probablement un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="dab82-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="dab82-159">Dès ce modèle par défaut vous donne deux pages à visiter et une page de connexion de base.</span><span class="sxs-lookup"><span data-stu-id="dab82-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="dab82-160">L’étape suivante consiste à modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC dans le processus.</span><span class="sxs-lookup"><span data-stu-id="dab82-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="dab82-161">Fermez votre navigateur et nous allons modifier du code.</span><span class="sxs-lookup"><span data-stu-id="dab82-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dab82-162">Next</span><span class="sxs-lookup"><span data-stu-id="dab82-162">Next</span></span>](adding-a-controller.md)
