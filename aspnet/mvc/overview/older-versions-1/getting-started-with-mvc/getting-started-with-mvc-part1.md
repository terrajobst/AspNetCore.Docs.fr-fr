---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Présentation d’ASP.NET MVC | Documents Microsoft
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC. Créez une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="48258-104">Présentation d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="48258-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="48258-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="48258-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="48258-106">Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="48258-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="48258-107">Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="48258-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="48258-108">Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="48258-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="48258-109">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="48258-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="48258-110">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="48258-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="48258-111">Nous allons en créer votre première Application Web ASP.NET MVC à l’aide [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="48258-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="48258-112">Nous allons effectuer une petite application de liste de films qui nous crée et liste de films.</span><span class="sxs-lookup"><span data-stu-id="48258-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="48258-113">Ce que vous allez générer</span><span class="sxs-lookup"><span data-stu-id="48258-113">What You'll Build</span></span>

<span data-ttu-id="48258-114">Voici deux captures d’écran de l’application que vous allez générer.</span><span class="sxs-lookup"><span data-stu-id="48258-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="48258-115">Vous aurez une table simple des films à l’aide de diverses colonnes.</span><span class="sxs-lookup"><span data-stu-id="48258-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="48258-116">[![Liste de films - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="48258-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="48258-117">Et vous aurez un formulaire de création afin que nous pouvons ajouter films à la liste.</span><span class="sxs-lookup"><span data-stu-id="48258-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="48258-118">[![Créer un film - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="48258-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="48258-119">Vous allez apprendre des compétences</span><span class="sxs-lookup"><span data-stu-id="48258-119">Skills You'll Learn</span></span>

<span data-ttu-id="48258-120">Ce didacticiel, vous allez apprendre les principes fondamentaux de la création d’une Application de Web ASP.NET MVC à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48258-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="48258-121">Vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="48258-121">You'll learn:</span></span>

- <span data-ttu-id="48258-122">Comment créer un nouveau projet ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="48258-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="48258-123">Comment créer une nouvelle base de données avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="48258-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="48258-124">Comment créer des vues et les contrôleurs MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="48258-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="48258-125">Comment récupérer et afficher des données</span><span class="sxs-lookup"><span data-stu-id="48258-125">How to retrieve and display data</span></span>
- <span data-ttu-id="48258-126">Comment modifier des données et activer la validation de données</span><span class="sxs-lookup"><span data-stu-id="48258-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="48258-127">Comment mettre à jour le schéma de base de données</span><span class="sxs-lookup"><span data-stu-id="48258-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="48258-128">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="48258-128">Get Started</span></span>

<span data-ttu-id="48258-129">Commencer par exécuter Visual Web Developer 2010 Express (je vais l’appeler « VWD » par la suite), puis sélectionnez Nouveau projet à partir de l’écran d’accueil.</span><span class="sxs-lookup"><span data-stu-id="48258-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="48258-130">Visual Web Developer est un environnement de développement intégré ou IDE.</span><span class="sxs-lookup"><span data-stu-id="48258-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="48258-131">Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un bus IDE pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="48258-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="48258-132">Il existe une barre d’outils en haut affichant les différentes options disponibles pour vous, ainsi que le menu vous pouvez également avoir utilisé pour sélectionner le fichier | Nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="48258-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="48258-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="48258-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="48258-134">Créer votre première Application</span><span class="sxs-lookup"><span data-stu-id="48258-134">Creating Your First Application</span></span>

<span data-ttu-id="48258-135">Vous pouvez créer des applications à l’aide de Visual Basic ou Visual c#.</span><span class="sxs-lookup"><span data-stu-id="48258-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="48258-136">Pour l’instant, sélectionnez Visual c# sur la gauche, puis choisissez « Application de Web ASP.NET MVC 2 ».</span><span class="sxs-lookup"><span data-stu-id="48258-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="48258-137">Nommez votre projet « Films », puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="48258-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="48258-138">[![Nouveau projet](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="48258-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="48258-139">Dans la partie droite est l’Explorateur de solutions affichant tous les fichiers et dossiers dans votre application.</span><span class="sxs-lookup"><span data-stu-id="48258-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="48258-140">La fenêtre big au milieu est où vous modifiez votre code et passez la majeure partie de votre temps.</span><span class="sxs-lookup"><span data-stu-id="48258-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="48258-141">Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application fonctionne maintenant sans rien faire !</span><span class="sxs-lookup"><span data-stu-id="48258-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="48258-142">Il s’agit d’un simple « Hello World !</span><span class="sxs-lookup"><span data-stu-id="48258-142">This is a simple "Hello World!</span></span> <span data-ttu-id="48258-143">projet, qui est un bon point de départ pour notre application.</span><span class="sxs-lookup"><span data-stu-id="48258-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="48258-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="48258-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="48258-145">Sélectionnez le bouton « lecture » pour la barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="48258-145">Select the "play" button to the toolbar.</span></span>

![Démarrer le débogage](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="48258-147">Il s’agit d’une flèche verte pointant vers la droite qui compiler votre programme et démarrez votre application dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="48258-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="48258-148">*Remarque : Vous pouvez à la place de votre clavier, appuyez sur la touche F5 ou sélectionnez Debug -&gt;démarrer le débogage à partir du menu « Debug ».*</span><span class="sxs-lookup"><span data-stu-id="48258-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="48258-149">Cela entraîne Visual Web Developer pour démarrer un serveur web de développement et d’exécuter notre application web (il n’y aucune configuration ou les étapes manuelles requises pour activer cette option).</span><span class="sxs-lookup"><span data-stu-id="48258-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="48258-150">Puis il lance un navigateur et le configurer pour parcourir la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="48258-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="48258-151">Ci-dessous, notez que la barre d’adresses du navigateur indique « localhost » et pas quelque chose comme exemple.com. C’est parce que localhost pointe toujours sur votre ordinateur local - qui dans ce cas est exécutée dans l’application que nous vient d’être générées.</span><span class="sxs-lookup"><span data-stu-id="48258-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="48258-152">[![Page d’accueil](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="48258-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="48258-153">En dehors de la zone de ce modèle par défaut donne vous deux pages à visiter et une page de connexion de base.</span><span class="sxs-lookup"><span data-stu-id="48258-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="48258-154">Nous allons modifier le fonctionnement de cette application et en savoir un peu ASP.NET MVC dans le processus.</span><span class="sxs-lookup"><span data-stu-id="48258-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="48258-155">Fermez votre navigateur et vous permet de modifier du code.</span><span class="sxs-lookup"><span data-stu-id="48258-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="48258-156">Next</span><span class="sxs-lookup"><span data-stu-id="48258-156">Next</span></span>](getting-started-with-mvc-part2.md)
