---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduction à ASP.NET MVC | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: ee39cf78835767d73bd9a1d4c9a2c4ca75aca5fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808861"
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="ac12a-104">Introduction à ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ac12a-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="ac12a-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="ac12a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ac12a-106">Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="ac12a-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="ac12a-107">Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ac12a-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="ac12a-108">Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ac12a-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="ac12a-109">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="ac12a-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="ac12a-110">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="ac12a-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="ac12a-111">Assurons-nous que notre première Application Web ASP.NET MVC à l’aide [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="ac12a-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="ac12a-112">Nous allons rendre une petite application de liste de films qui sera créons et liste de films.</span><span class="sxs-lookup"><span data-stu-id="ac12a-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="ac12a-113">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="ac12a-113">What You'll Build</span></span>

<span data-ttu-id="ac12a-114">Voici deux captures d’écran de l’application que vous allez générer.</span><span class="sxs-lookup"><span data-stu-id="ac12a-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="ac12a-115">Vous aurez une table simple de films avec diverses colonnes.</span><span class="sxs-lookup"><span data-stu-id="ac12a-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="ac12a-116">[![Liste de films - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ac12a-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="ac12a-117">Et vous aurez un formulaire de création afin de pouvoir ajouter à la liste de films.</span><span class="sxs-lookup"><span data-stu-id="ac12a-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="ac12a-118">[![Créer un film - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ac12a-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="ac12a-119">Vous allez apprendre des compétences</span><span class="sxs-lookup"><span data-stu-id="ac12a-119">Skills You'll Learn</span></span>

<span data-ttu-id="ac12a-120">Ce didacticiel vous apprend les notions de base de la création d’une Application de Web ASP.NET MVC à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac12a-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="ac12a-121">Vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="ac12a-121">You'll learn:</span></span>

- <span data-ttu-id="ac12a-122">Comment créer un nouveau projet ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ac12a-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="ac12a-123">Comment créer une nouvelle base de données avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="ac12a-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="ac12a-124">Comment créer des vues et contrôleurs de MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ac12a-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="ac12a-125">Comment récupérer et afficher des données</span><span class="sxs-lookup"><span data-stu-id="ac12a-125">How to retrieve and display data</span></span>
- <span data-ttu-id="ac12a-126">Comment modifier des données et activer la validation de données</span><span class="sxs-lookup"><span data-stu-id="ac12a-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="ac12a-127">Comment mettre à jour le schéma de base de données</span><span class="sxs-lookup"><span data-stu-id="ac12a-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="ac12a-128">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="ac12a-128">Get Started</span></span>

<span data-ttu-id="ac12a-129">Commencez par exécuter Visual Web Developer 2010 Express (je l’appellerai « VWD » par la suite) et sélectionnez Nouveau projet à partir de l’écran d’accueil.</span><span class="sxs-lookup"><span data-stu-id="ac12a-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="ac12a-130">Visual Web Developer est un IDE ou l’environnement de développement intégré.</span><span class="sxs-lookup"><span data-stu-id="ac12a-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="ac12a-131">Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="ac12a-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="ac12a-132">Il existe une barre d’outils en haut montrant les différentes options disponibles pour vous, ainsi que du menu vous pouvez également avoir utilisé pour sélectionner le fichier | Nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="ac12a-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="ac12a-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ac12a-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="ac12a-134">Créer votre première Application</span><span class="sxs-lookup"><span data-stu-id="ac12a-134">Creating Your First Application</span></span>

<span data-ttu-id="ac12a-135">Vous pouvez créer des applications à l’aide de Visual Basic ou Visual c#.</span><span class="sxs-lookup"><span data-stu-id="ac12a-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="ac12a-136">Pour l’instant, sélectionnez Visual c# sur la gauche, puis choisissez « Application Web de ASP.NET MVC 2. »</span><span class="sxs-lookup"><span data-stu-id="ac12a-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="ac12a-137">Nommez votre projet « Movies » et cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="ac12a-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="ac12a-138">[![Nouveau projet](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ac12a-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="ac12a-139">Dans la partie droite est l’Explorateur de solutions affichant tous les fichiers et dossiers dans votre application.</span><span class="sxs-lookup"><span data-stu-id="ac12a-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="ac12a-140">La fenêtre big au milieu est où vous modifiez votre code et la plupart de votre temps.</span><span class="sxs-lookup"><span data-stu-id="ac12a-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="ac12a-141">Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant !</span><span class="sxs-lookup"><span data-stu-id="ac12a-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="ac12a-142">Il s’agit d’un simple « Hello World !</span><span class="sxs-lookup"><span data-stu-id="ac12a-142">This is a simple "Hello World!</span></span> <span data-ttu-id="ac12a-143">projet et que c’est un bon point de départ pour notre application.</span><span class="sxs-lookup"><span data-stu-id="ac12a-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="ac12a-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ac12a-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="ac12a-145">Sélectionnez le bouton « lecture » à la barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="ac12a-145">Select the "play" button to the toolbar.</span></span>

![Démarrer le débogage](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="ac12a-147">Il s’agit d’une flèche verte pointant vers la droite qui compilera votre programme et démarrer votre application dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="ac12a-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="ac12a-148">*Remarque : Vous pouvez à la place appuyez sur F5 de votre clavier, ou sélectionnez débogage -&gt;démarrer le débogage à partir du menu « Debug ».*</span><span class="sxs-lookup"><span data-stu-id="ac12a-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="ac12a-149">Ainsi, Visual Web Developer démarrer un serveur web de développement et d’exécuter notre application web (il n’existe aucune configuration ou les étapes manuelles requises pour activer cette option).</span><span class="sxs-lookup"><span data-stu-id="ac12a-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="ac12a-150">Puis il lance un navigateur et configurez-le pour parcourir la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="ac12a-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="ac12a-151">Ci-dessous, notez que la barre d’adresses du navigateur indique « localhost » et pas quelque chose comme example.com.</span><span class="sxs-lookup"><span data-stu-id="ac12a-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="ac12a-152">C’est parce que localhost pointe toujours vers votre propre ordinateur local - qui dans ce cas s’exécute l’application que nous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="ac12a-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="ac12a-153">[![Page d’accueil](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="ac12a-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="ac12a-154">Prêt à l’emploi ce modèle par défaut donne vous deux pages à visiter et une page de connexion de base.</span><span class="sxs-lookup"><span data-stu-id="ac12a-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="ac12a-155">Nous allons modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC dans le processus.</span><span class="sxs-lookup"><span data-stu-id="ac12a-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="ac12a-156">Fermez votre navigateur et vous permet de modifier du code.</span><span class="sxs-lookup"><span data-stu-id="ac12a-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ac12a-157">Next</span><span class="sxs-lookup"><span data-stu-id="ac12a-157">Next</span></span>](getting-started-with-mvc-part2.md)
