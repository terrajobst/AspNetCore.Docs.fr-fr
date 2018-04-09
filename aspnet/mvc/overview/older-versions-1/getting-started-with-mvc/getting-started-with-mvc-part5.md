---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: L’accès aux données de votre modèle à partir d’un contrôleur | Documents Microsoft
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC. Créez une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="00856-104">L’accès aux données de votre modèle à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="00856-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="00856-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="00856-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="00856-106">Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00856-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="00856-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="00856-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="00856-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="00856-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="00856-109">Dans cette section, nous allons créer une nouvelle classe MoviesController et écrire du code qui extrait les données de notre vidéo et l’affiche au navigateur à l’aide d’un modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="00856-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="00856-110">Cliquez avec le bouton droit sur le dossier contrôleurs et effectuer une nouvelle MoviesController.</span><span class="sxs-lookup"><span data-stu-id="00856-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="00856-111">[![Ajouter un contrôleur](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="00856-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="00856-112">Cela crée un nouveau fichier « MoviesController.cs » sous le dossier \Controllers dans notre projet.</span><span class="sxs-lookup"><span data-stu-id="00856-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="00856-113">Nous allons mettre à jour de la MovieController pour récupérer la liste de films à partir de notre base de données qui vient d’être rempli.</span><span class="sxs-lookup"><span data-stu-id="00856-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="00856-114">Nous exécutons une requête LINQ, afin que nous récupérer uniquement les films publiés après l’été 1984.</span><span class="sxs-lookup"><span data-stu-id="00856-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="00856-115">Nous avons besoin d’un modèle d’affichage pour rendre cette liste de films précédent : avec le bouton droit dans la méthode et sélectionnez Ajouter une vue de le créer.</span><span class="sxs-lookup"><span data-stu-id="00856-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="00856-116">Dans la boîte de dialogue Ajouter une vue nous allons indiquer que nous passons une liste&lt;Movies.Models.Movie&gt; à notre modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="00856-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="00856-117">Contrairement aux fois précédentes nous utilisé la boîte de dialogue Ajouter une vue et que vous avez choisi de créer un modèle « Vide », cette fois, que nous allons indiquer que nous souhaitons automatiquement « structurez » un modèle d’affichage pour nous avec du contenu par défaut de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="00856-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="00856-118">Nous allons le faire en sélectionnant l’élément « List » dans le menu « contenu de liste déroulante de l’affichage.</span><span class="sxs-lookup"><span data-stu-id="00856-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="00856-119">N’oubliez pas, lorsque vous avez créé une nouvelle classe, vous devez compiler votre application pour qu’elle s’affiche dans la boîte de dialogue Vue ajouter.</span><span class="sxs-lookup"><span data-stu-id="00856-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Ajouter une vue](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="00856-121">Cliquez sur Ajouter, et le système génère automatiquement le code pour nous qui affiche la liste de films pour une vue.</span><span class="sxs-lookup"><span data-stu-id="00856-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="00856-122">Il est judicieux de modifier le &lt;h2&gt; titre à quelque chose comme « Ma liste de films » comme nous l’avons fait précédemment avec la vue Hello World.</span><span class="sxs-lookup"><span data-stu-id="00856-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="00856-123">[![Films - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="00856-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="00856-124">Exécutez votre application, vous accédez à /Movies dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="00856-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="00856-125">Maintenant, nous avons récupéré des données à partir de la base de données à l’aide d’une requête de base à l’intérieur du contrôleur et renvoyé les données à une vue qui connaît les films.</span><span class="sxs-lookup"><span data-stu-id="00856-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="00856-126">Cette vue, puis passe via la liste de films et crée une table de données pour nous.</span><span class="sxs-lookup"><span data-stu-id="00856-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="00856-127">[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="00856-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="00856-128">Nous ne sera pas implémenter les détails, modifier et supprimer des fonctionnalités avec cette application - qui nous évite les liens de valeur par défaut créé par le modèle de vue de structure pour nous.</span><span class="sxs-lookup"><span data-stu-id="00856-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="00856-129">Ouvrez le fichier /Movies/Index.aspx et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="00856-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="00856-130">Voici le code source pour que notre modèle mis à jour de la vue doit ressembler à une fois que nous avons apporté ces modifications :</span><span class="sxs-lookup"><span data-stu-id="00856-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="00856-131">Elle consiste à créer des liens ne sont pas nécessaires, nous allons supprimer les pour cet exemple.</span><span class="sxs-lookup"><span data-stu-id="00856-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="00856-132">Nous allons conserver notre créer un nouveau lien, car il s’agit suivant !</span><span class="sxs-lookup"><span data-stu-id="00856-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="00856-133">Voici ce que notre application avec cette colonne supprimée.</span><span class="sxs-lookup"><span data-stu-id="00856-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="00856-134">[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="00856-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="00856-135">Nous disposons désormais d’une liste simple de nos données de film.</span><span class="sxs-lookup"><span data-stu-id="00856-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="00856-136">Toutefois, si vous cliquez sur le lien « Nouvel », nous obtenons une erreur car il n’est pas connecté !</span><span class="sxs-lookup"><span data-stu-id="00856-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="00856-137">Nous allons implémenter une méthode d’Action de création et permettre à un utilisateur à entrer de nouveaux films dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="00856-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00856-138">[Précédent](getting-started-with-mvc-part4.md)
> [Suivant](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="00856-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
