---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: "Partie 1 : Vue d’ensemble et fichier -> Nouveau projet | Documents Microsoft"
author: jongalloway
description: "Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC. Partie 1 traite de vue d’ensemble et de fichier -> Nouveau projet."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1e3373a21c7d1766cfad390a7ba68b1363d8d895
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="03f27-104">Partie 1 : Vue d’ensemble et fichier -> Nouveau projet</span><span class="sxs-lookup"><span data-stu-id="03f27-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="03f27-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="03f27-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="03f27-106">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="03f27-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="03f27-107">Le magasin de musique MVC est une implémentation de magasin exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, authentification de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="03f27-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="03f27-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03f27-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="03f27-109">Partie 1 couvre la vue d’ensemble et fichier -&gt;nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="03f27-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="03f27-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="03f27-110">Overview</span></span>

<span data-ttu-id="03f27-111">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Web Developer pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="03f27-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="03f27-112">Nous allons commencer lentement, afin de l’expérience de développement de niveau web débutant est OK.</span><span class="sxs-lookup"><span data-stu-id="03f27-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="03f27-113">L’application que nous allons créer est un magasin de musique simple.</span><span class="sxs-lookup"><span data-stu-id="03f27-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="03f27-114">Il existe trois éléments principaux à l’application : achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="03f27-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="03f27-115">Les visiteurs peuvent parcourir des Albums par Genre :</span><span class="sxs-lookup"><span data-stu-id="03f27-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="03f27-116">Ils peuvent afficher un seul album et l’ajouter à leur panier d’achat :</span><span class="sxs-lookup"><span data-stu-id="03f27-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="03f27-117">Ils peuvent consulter leur panier d’achat, suppression de tous les éléments qu’ils ne voulez plus que :</span><span class="sxs-lookup"><span data-stu-id="03f27-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="03f27-118">Procéder à la validation les invite à se connecter ou s’inscrire pour un compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="03f27-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="03f27-119">Après avoir créé un compte, il peut effectuer l’ordre en remplissant les informations d’expédition et de paiement.</span><span class="sxs-lookup"><span data-stu-id="03f27-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="03f27-120">Pour simplifier les choses, nous exécutons une promotion incroyable : tout est gratuit s’il a entré un code de promotion « Gratuit » !</span><span class="sxs-lookup"><span data-stu-id="03f27-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="03f27-121">Après le classement, ils voient un écran de confirmation simple :</span><span class="sxs-lookup"><span data-stu-id="03f27-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="03f27-122">En plus des pages du client-faceing, nous également générer une section d’administrateur qui affiche une liste d’albums à partir de laquelle les administrateurs peuvent créer, modifier et supprimer des albums :</span><span class="sxs-lookup"><span data-stu-id="03f27-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="03f27-123">1. Fichier -&gt; nouveau projet</span><span class="sxs-lookup"><span data-stu-id="03f27-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="03f27-124">L’installation du logiciel</span><span class="sxs-lookup"><span data-stu-id="03f27-124">Installing the software</span></span>

<span data-ttu-id="03f27-125">Ce didacticiel commence par créer un nouveau projet ASP.NET MVC 3 à l’aide de la libre Visual Web Developer 2010 Express (qui est disponible), puis nous allons ajouter progressivement des fonctionnalités pour créer une application fonctionnelle complète.</span><span class="sxs-lookup"><span data-stu-id="03f27-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="03f27-126">Avant cela, nous aborderons accès de base de données, les scénarios de validation de formulaire, validation des données, à l’aide de pages maîtres pour la mise en page cohérente, à l’aide d’AJAX pour les mises à jour de la page et la validation, connexion de l’utilisateur et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="03f27-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="03f27-127">Peut suivre la procédure étape par étape, ou vous pouvez télécharger l’application terminée à partir de [magasin de musique MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="03f27-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="03f27-128">Vous pouvez utiliser Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (une version gratuite de Visual Studio 2010) pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="03f27-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="03f27-129">Nous utiliserons SQL Server Compact (également gratuit) pour héberger la base de données.</span><span class="sxs-lookup"><span data-stu-id="03f27-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="03f27-130">Avant de commencer, assurez-vous que vous avez installé les composants requis répertoriés ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="03f27-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="03f27-131">[Configuration requise de visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="03f27-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="03f27-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="03f27-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="03f27-133">[SQL Server Compact 4.0 -], y compris la prise en charge runtime et outils</span><span class="sxs-lookup"><span data-stu-id="03f27-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="03f27-134">Création d’un projet ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="03f27-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="03f27-135">Nous allons commencer en sélectionnant « Nouveau projet » dans le menu fichier dans Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="03f27-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="03f27-136">De la boîte de dialogue Nouveau projet s’affiche.</span><span class="sxs-lookup"><span data-stu-id="03f27-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="03f27-137">Nous allons sélectionner le Visual c# -&gt; modèles Web groupe situé à gauche, puis choisissez le modèle de « Application Web ASP.NET MVC 3 » dans la colonne centrale.</span><span class="sxs-lookup"><span data-stu-id="03f27-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="03f27-138">Nommez votre projet MvcMusicStore et appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="03f27-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="03f27-139">Cette action affiche une boîte de dialogue secondaire qui vous permet de définir des paramètres spécifiques de MVC pour notre projet.</span><span class="sxs-lookup"><span data-stu-id="03f27-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="03f27-140">Sélectionnez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="03f27-140">Select the following:</span></span>

<span data-ttu-id="03f27-141">Modèle de projet, sélectionnez vide</span><span class="sxs-lookup"><span data-stu-id="03f27-141">Project Template - select Empty</span></span>

<span data-ttu-id="03f27-142">Moteur d’affichage - Sélectionnez Razor</span><span class="sxs-lookup"><span data-stu-id="03f27-142">View Engine - select Razor</span></span>

<span data-ttu-id="03f27-143">Utiliser des balises sémantiques de HTML5 - activée</span><span class="sxs-lookup"><span data-stu-id="03f27-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="03f27-144">Vérifiez que vos paramètres sont indiquées ci-dessous, puis appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="03f27-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="03f27-145">Cela va créer notre projet.</span><span class="sxs-lookup"><span data-stu-id="03f27-145">This will create our project.</span></span> <span data-ttu-id="03f27-146">Examinons les dossiers qui ont été ajoutés à notre application dans l’Explorateur de solutions sur le côté droit.</span><span class="sxs-lookup"><span data-stu-id="03f27-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="03f27-147">Le modèle vide MVC 3 n’est pas vide, il ajoute une structure de dossiers de base :</span><span class="sxs-lookup"><span data-stu-id="03f27-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="03f27-148">ASP.NET MVC utilise des conventions d’affectation de noms de base pour les noms de dossier :</span><span class="sxs-lookup"><span data-stu-id="03f27-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="03f27-149">**Dossier**</span><span class="sxs-lookup"><span data-stu-id="03f27-149">**Folder**</span></span> | <span data-ttu-id="03f27-150">**Fonction**</span><span class="sxs-lookup"><span data-stu-id="03f27-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="03f27-151">**/ Contrôleurs**</span><span class="sxs-lookup"><span data-stu-id="03f27-151">**/Controllers**</span></span> | <span data-ttu-id="03f27-152">Contrôleurs de répondent à l’entrée à partir du navigateur, décider quoi faire avec ce dernier et retourner une réponse à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="03f27-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="03f27-153">**/ Vues**</span><span class="sxs-lookup"><span data-stu-id="03f27-153">**/Views**</span></span> | <span data-ttu-id="03f27-154">Vues contiennent nos modèles de l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="03f27-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="03f27-155">**/ Modèles**</span><span class="sxs-lookup"><span data-stu-id="03f27-155">**/Models**</span></span> | <span data-ttu-id="03f27-156">Les modèles contiennent et manipulent des données</span><span class="sxs-lookup"><span data-stu-id="03f27-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="03f27-157">**/ Contenu**</span><span class="sxs-lookup"><span data-stu-id="03f27-157">**/Content**</span></span> | <span data-ttu-id="03f27-158">Ce dossier conserve nos images, CSS et tout autre contenu statique</span><span class="sxs-lookup"><span data-stu-id="03f27-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="03f27-159">**Scripts**</span><span class="sxs-lookup"><span data-stu-id="03f27-159">**/Scripts**</span></span> | <span data-ttu-id="03f27-160">Ce dossier conserve nos fichiers JavaScript</span><span class="sxs-lookup"><span data-stu-id="03f27-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="03f27-161">Ces dossiers sont inclus même dans une application vide ASP.NET MVC parce que l’infrastructure ASP.NET MVC par défaut utilise une approche « convention avant la configuration » et fait des hypothèses par défaut basés sur les conventions d’affectation de noms de dossier.</span><span class="sxs-lookup"><span data-stu-id="03f27-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="03f27-162">Par exemple, les contrôleurs de recherchent des vues dans le dossier vues par défaut sans avoir à spécifier explicitement ceci dans votre code.</span><span class="sxs-lookup"><span data-stu-id="03f27-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="03f27-163">Les conventions par défaut ne conserverez réduit la quantité de code, vous devez écrire, et peut également faciliter par d’autres développeurs à comprendre votre projet.</span><span class="sxs-lookup"><span data-stu-id="03f27-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="03f27-164">Nous allons expliquer ces conventions plus que nous créons notre application.</span><span class="sxs-lookup"><span data-stu-id="03f27-164">We'll explain these conventions more as we build our application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="03f27-165">Next</span><span class="sxs-lookup"><span data-stu-id="03f27-165">Next</span></span>](mvc-music-store-part-2.md)
