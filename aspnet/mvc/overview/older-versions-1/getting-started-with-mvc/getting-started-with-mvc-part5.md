---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: L’accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Créer une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 556eed77cbd9e81c0a2a1334bb0a8ee56abafd34
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814999"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="9d08e-104">L’accès aux données de votre modèle à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="9d08e-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="9d08e-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="9d08e-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="9d08e-106">Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9d08e-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="9d08e-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="9d08e-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="9d08e-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="9d08e-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="9d08e-109">Dans cette section, nous allons créer une nouvelle classe MoviesController, et écrire du code qui Récupère nos données de film et l’affiche dans le navigateur à l’aide d’un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="9d08e-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="9d08e-110">Cliquez avec le bouton droit sur le dossier Controllers et effectuer une nouvelle MoviesController.</span><span class="sxs-lookup"><span data-stu-id="9d08e-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="9d08e-111">[![Ajouter un contrôleur](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9d08e-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="9d08e-112">Cela créera un nouveau fichier « MoviesController.cs » en dessous de notre dossier \Controllers au sein de notre projet.</span><span class="sxs-lookup"><span data-stu-id="9d08e-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="9d08e-113">Nous allons mettre à jour le MovieController pour récupérer la liste de films à partir de notre base de données nouvellement rempli.</span><span class="sxs-lookup"><span data-stu-id="9d08e-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="9d08e-114">Nous exécutons une requête LINQ, afin que nous récupérons uniquement films publiées après l’été 1984.</span><span class="sxs-lookup"><span data-stu-id="9d08e-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="9d08e-115">Nous avons besoin d’un modèle de vue pour afficher cette liste de films de retour, par conséquent, avec le bouton droit dans la méthode et sélectionnez Ajouter une vue pour la créer.</span><span class="sxs-lookup"><span data-stu-id="9d08e-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="9d08e-116">Dans la boîte de dialogue Ajouter une vue, nous allons indiquer que nous passons une liste&lt;Movies.Models.Movie&gt; à notre modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="9d08e-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="9d08e-117">Contrairement aux fois précédentes nous utilisé la boîte de dialogue Ajouter une vue et que vous avez choisi de créer un modèle « Vide », cette fois, que nous allons indiquer que nous voulons Visual Studio automatiquement « structurer » un modèle de vue pour nous avec du contenu par défaut.</span><span class="sxs-lookup"><span data-stu-id="9d08e-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="9d08e-118">Pour cela, nous allons sélectionner l’élément « List » dans le menu « contenu de liste déroulante de l’affichage.</span><span class="sxs-lookup"><span data-stu-id="9d08e-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="9d08e-119">N’oubliez pas, lorsque vous avez créé une nouvelle classe, vous aurez besoin pour compiler votre application pour qu’elle s’affiche dans la boîte de dialogue Vue ajouter.</span><span class="sxs-lookup"><span data-stu-id="9d08e-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Ajouter une vue](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="9d08e-121">Cliquez sur Ajouter et le système génère automatiquement le code pour nous qui affiche notre liste de films pour une vue.</span><span class="sxs-lookup"><span data-stu-id="9d08e-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="9d08e-122">Il s’agit d’un bon moment pour modifier le &lt;h2&gt; titre à quelque chose comme « My Movie List » comme nous l’avons fait précédemment avec la vue de Hello World.</span><span class="sxs-lookup"><span data-stu-id="9d08e-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="9d08e-123">[![Films - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9d08e-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="9d08e-124">Exécutez votre application et visitez /Movies dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="9d08e-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="9d08e-125">Maintenant, nous avons récupéré des données à partir de la base de données à l’aide d’une requête de base à l’intérieur du contrôleur et renvoyé les données à une vue qui connaît les films.</span><span class="sxs-lookup"><span data-stu-id="9d08e-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="9d08e-126">Cette vue puis tourne via la liste de films et crée une table de données pour nous.</span><span class="sxs-lookup"><span data-stu-id="9d08e-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="9d08e-127">[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="9d08e-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="9d08e-128">Nous n’implémentation modifier, détails et supprimer des fonctionnalités avec cette application -, ce qui nous évite les liens par défaut que le modèle de structure créé pour nous.</span><span class="sxs-lookup"><span data-stu-id="9d08e-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="9d08e-129">Ouvrez le fichier /Movies/Index.aspx et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="9d08e-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="9d08e-130">Voici le code source pour ce que notre modèle de vue mis à jour doit ressembler à une fois que nous apporter ces modifications :</span><span class="sxs-lookup"><span data-stu-id="9d08e-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="9d08e-131">Il consiste à créer des liens que nous ne devons, donc nous allons les supprimer pour cet exemple.</span><span class="sxs-lookup"><span data-stu-id="9d08e-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="9d08e-132">Nous conserverons notre créer un nouveau lien, car il s’agit suivant !</span><span class="sxs-lookup"><span data-stu-id="9d08e-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="9d08e-133">Voici à quoi ressemble notre application avec cette colonne supprimée.</span><span class="sxs-lookup"><span data-stu-id="9d08e-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="9d08e-134">[![Liste de films - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="9d08e-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="9d08e-135">Nous disposons désormais d’une simple liste de nos données de film.</span><span class="sxs-lookup"><span data-stu-id="9d08e-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="9d08e-136">Toutefois, si nous cliquons sur le lien « Créer nouveau », nous obtenons une erreur car il n’est pas connecté !</span><span class="sxs-lookup"><span data-stu-id="9d08e-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="9d08e-137">Nous allons implémenter une méthode d’Action de créer et activer un utilisateur à entrer de nouveaux films dans notre base de données.</span><span class="sxs-lookup"><span data-stu-id="9d08e-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d08e-138">[Précédent](getting-started-with-mvc-part4.md)
> [Suivant](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="9d08e-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
