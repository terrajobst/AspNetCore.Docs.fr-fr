---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Ajout d’une Validation pour le modèle | Documents Microsoft
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC. Créez une application web simple qui lit et écrit à partir d’une base de données.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871946"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="19ce9-104">Ajout d’une Validation pour le modèle</span><span class="sxs-lookup"><span data-stu-id="19ce9-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="19ce9-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="19ce9-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="19ce9-106">Il s’agit d’un didacticiel débutant qui présente les notions de base d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="19ce9-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="19ce9-107">Vous allez créer une application web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="19ce9-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="19ce9-108">Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.</span><span class="sxs-lookup"><span data-stu-id="19ce9-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="19ce9-109">Dans cette section, nous allons implémenter la prise en charge nécessaire pour activer la validation d’entrée de l’application.</span><span class="sxs-lookup"><span data-stu-id="19ce9-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="19ce9-110">Nous allons vous assurer que notre contenu de la base de données est toujours correct et fournir des messages d’erreur utiles aux utilisateurs finaux lorsqu’ils essayez et entrer des données de film qui ne sont pas valides.</span><span class="sxs-lookup"><span data-stu-id="19ce9-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="19ce9-111">Nous allons commencer en ajoutant un peu logique de validation à la classe de film.</span><span class="sxs-lookup"><span data-stu-id="19ce9-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="19ce9-112">Cliquez avec le bouton droit sur le dossier de modèle et sélectionnez Ajouter une classe.</span><span class="sxs-lookup"><span data-stu-id="19ce9-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="19ce9-113">Nom de votre classe de film.</span><span class="sxs-lookup"><span data-stu-id="19ce9-113">Name your class Movie.</span></span>

<span data-ttu-id="19ce9-114">Lorsque nous avons créé le modèle d’entité de film précédemment, l’IDE créé une classe de film.</span><span class="sxs-lookup"><span data-stu-id="19ce9-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="19ce9-115">En fait, la partie de la classe de film peut être dans un fichier et partie dans un autre.</span><span class="sxs-lookup"><span data-stu-id="19ce9-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="19ce9-116">Il s’agit d’une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="19ce9-116">This is called a Partial Class.</span></span> <span data-ttu-id="19ce9-117">Nous allons étendre la classe de films à partir d’un autre fichier.</span><span class="sxs-lookup"><span data-stu-id="19ce9-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="19ce9-118">Nous allons créer une classe partielle de film qui pointe vers une classe « associé » avec certains attributs qui vous donne des indicateurs de validation pour le système.</span><span class="sxs-lookup"><span data-stu-id="19ce9-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="19ce9-119">Nous allons marquer le titre et le prix que nécessaire et également imposer que le prix soit dans une certaine plage.</span><span class="sxs-lookup"><span data-stu-id="19ce9-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="19ce9-120">Cliquez avec le bouton droit sur le dossier Modèles et sélectionnez Ajouter une classe.</span><span class="sxs-lookup"><span data-stu-id="19ce9-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="19ce9-121">Nom de votre classe film et cliquez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="19ce9-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="19ce9-122">Voici ce que notre partielle film classe ressemble.</span><span class="sxs-lookup"><span data-stu-id="19ce9-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="19ce9-123">Exécuter à nouveau votre application et essayez de saisir un film avec un prix supérieures à 100.</span><span class="sxs-lookup"><span data-stu-id="19ce9-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="19ce9-124">Vous obtiendrez une erreur après avoir soumis le formulaire.</span><span class="sxs-lookup"><span data-stu-id="19ce9-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="19ce9-125">L’erreur est interceptée côté serveur et se produit une fois que le formulaire est publié.</span><span class="sxs-lookup"><span data-stu-id="19ce9-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="19ce9-126">Notez comment les programmes d’assistance HTML intégrés de ASP.NET MVC ont été assez intelligents pour afficher le message d’erreur et de conserver les valeurs pour nous dans les éléments de la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="19ce9-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="19ce9-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="19ce9-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="19ce9-128">Cela fonctionne très bien, mais il peut être intéressant si nous pourrions l’utilisateur sur la côté client, immédiatement, avant que le serveur obtient impliqué.</span><span class="sxs-lookup"><span data-stu-id="19ce9-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="19ce9-129">Nous allons activer une validation côté client avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="19ce9-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="19ce9-130">Ajout d’une Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="19ce9-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="19ce9-131">Étant donné que certains attributs de validation est déjà dans notre classe vidéo, nous devez simplement ajouter quelques fichiers JavaScript à notre Create.aspx afficher le modèle et ajouter une ligne de code pour activer la validation côté client puisse avoir lieu.</span><span class="sxs-lookup"><span data-stu-id="19ce9-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="19ce9-132">À partir de VWD accéder notre dossier vues/film et ouvrez Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="19ce9-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="19ce9-133">Ouvrez le dossier de Scripts dans l’Explorateur de solutions et faites glisser les scripts suivants trois à dans le &lt;head&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="19ce9-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="19ce9-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="19ce9-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="19ce9-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="19ce9-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="19ce9-136">Vous souhaitez que ces fichiers de script à apparaître dans cet ordre.</span><span class="sxs-lookup"><span data-stu-id="19ce9-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="19ce9-137">En outre, ajoutez la ligne unique ci-dessus le Html.BeginForm :</span><span class="sxs-lookup"><span data-stu-id="19ce9-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="19ce9-138">Voici le code indiqué dans l’IDE.</span><span class="sxs-lookup"><span data-stu-id="19ce9-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="19ce9-139">[![Films - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="19ce9-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="19ce9-140">Exécuter votre application et consultez à nouveau /Movies/Create et cliquez sur créer sans entrer de données.</span><span class="sxs-lookup"><span data-stu-id="19ce9-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="19ce9-141">Les messages d’erreur apparaissent immédiatement sans la page flash que nous associons à l’envoi de données complètement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19ce9-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="19ce9-142">Il s’agit, car ASP.NET MVC est maintenant valider l’entrée sur les deux le client (à l’aide de JavaScript) et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="19ce9-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="19ce9-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="19ce9-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="19ce9-144">Cette recherche bon !</span><span class="sxs-lookup"><span data-stu-id="19ce9-144">This is looking good!</span></span> <span data-ttu-id="19ce9-145">Vous allez maintenant ajouter une colonne supplémentaire à la base de données.</span><span class="sxs-lookup"><span data-stu-id="19ce9-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="19ce9-146">[Précédent](getting-started-with-mvc-part6.md)
> [Suivant](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="19ce9-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
