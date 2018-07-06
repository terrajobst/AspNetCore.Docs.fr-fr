---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Partie 1 : Fichier -> Nouveau projet | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 1 couvre la vue d’ensemble et fichier/nouveau projet.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1c8ba561745c9c02b5513e326b4739ec97338393
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825685"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="78a9c-104">Partie 1 : Fichier -> Nouveau projet</span><span class="sxs-lookup"><span data-stu-id="78a9c-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="78a9c-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="78a9c-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="78a9c-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="78a9c-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="78a9c-107">Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="78a9c-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="78a9c-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="78a9c-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="78a9c-109">Partie 1 couvre la vue d’ensemble et fichier/nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="78a9c-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="78a9c-110">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="78a9c-110">Overview</span></span>

<span data-ttu-id="78a9c-111">Ce didacticiel est une introduction à Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="78a9c-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="78a9c-112">Nous allons commencer lentement, afin de l’expérience de développement de niveau web pour débutants OK.</span><span class="sxs-lookup"><span data-stu-id="78a9c-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="78a9c-113">L’application que nous allons créer est un magasin en ligne simple.</span><span class="sxs-lookup"><span data-stu-id="78a9c-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="78a9c-114">Les visiteurs peuvent parcourir les produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="78a9c-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="78a9c-115">Ils peuvent afficher un produit unique et l’ajouter à leur panier d’achat :</span><span class="sxs-lookup"><span data-stu-id="78a9c-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="78a9c-116">Ils peuvent consulter leur panier, supprimer des éléments qu’ils ne voulez plus de :</span><span class="sxs-lookup"><span data-stu-id="78a9c-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="78a9c-117">Continuer à extraire les invite à</span><span class="sxs-lookup"><span data-stu-id="78a9c-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="78a9c-118">Après avoir commandé, ils voient un écran de confirmation simple :</span><span class="sxs-lookup"><span data-stu-id="78a9c-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="78a9c-119">Nous allons commencer en créant un nouveau projet Web Forms ASP.NET dans Visual Studio 2010, et nous ajouterons progressivement des fonctionnalités pour créer une application fonctionnelle complète.</span><span class="sxs-lookup"><span data-stu-id="78a9c-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="78a9c-120">Tout au long du processus, nous traiterons accès de base de données, les affichages de listes et de la grille, pages de mise à jour de données, validation des données, à l’aide de pages maîtres pour la mise en page cohérente, AJAX, validation, l’appartenance utilisateur et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="78a9c-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="78a9c-121">Vous pouvez suivre la procédure étape par étape, ou vous pouvez télécharger l’application terminée à partir de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="78a9c-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="78a9c-122">Vous pouvez utiliser Visual Studio 2010 ou le gratuit Visual Web Developer 2010 à partir de [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="78a9c-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="78a9c-123">Pour générer l’application, vous pouvez utiliser SQL Server ou gratuit SQL Server Express pour héberger la base de données.</span><span class="sxs-lookup"><span data-stu-id="78a9c-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="78a9c-124">Fichier / nouveau projet</span><span class="sxs-lookup"><span data-stu-id="78a9c-124">File / New Project</span></span>

<span data-ttu-id="78a9c-125">Nous allons commencer en sélectionnant le nouveau projet dans le menu fichier dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78a9c-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="78a9c-126">Ceci fait apparaître la boîte de dialogue Nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="78a9c-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="78a9c-127">Nous allons sélectionner Visual C# / modèles Web regrouper sur la gauche, puis choisissez le modèle « ASP.NET Web Application » dans la colonne centrale.</span><span class="sxs-lookup"><span data-stu-id="78a9c-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="78a9c-128">Nommez votre projet TailspinSpyworks et appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="78a9c-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="78a9c-129">Cela crée notre projet.</span><span class="sxs-lookup"><span data-stu-id="78a9c-129">This will create our project.</span></span> <span data-ttu-id="78a9c-130">Jetons un œil au niveau des dossiers qui sont inclus dans notre application dans l’Explorateur de solutions sur le côté droit.</span><span class="sxs-lookup"><span data-stu-id="78a9c-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="78a9c-131">La Solution vide n’est pas complètement vide, il ajoute une structure de dossiers de base :</span><span class="sxs-lookup"><span data-stu-id="78a9c-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="78a9c-132">Notez les conventions implémentées par le modèle de projet ASP.NET 4 par défaut.</span><span class="sxs-lookup"><span data-stu-id="78a9c-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="78a9c-133">Le dossier « Compte » implémente une interface utilisateur de base pour ASP. Sous-système de l’appartenance du NET.</span><span class="sxs-lookup"><span data-stu-id="78a9c-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="78a9c-134">Le dossier « Scripts » sert le référentiel pour les fichiers de JavaScript côté client et les fichiers .js de jQuery core sont disponibles par défaut.</span><span class="sxs-lookup"><span data-stu-id="78a9c-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="78a9c-135">Le dossier « Styles » est utilisé pour organiser notre site web des éléments visuels (feuilles de Style CSS)</span><span class="sxs-lookup"><span data-stu-id="78a9c-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="78a9c-136">Lorsque vous appuyez sur F5 pour exécuter notre application et de restituer la page default.aspx, nous voyons ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="78a9c-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="78a9c-137">Notre première amélioration d’application sera pour remplacer le fichier Style.css à partir du modèle de Web Forms par défaut avec les classes CSS et les fichiers d’image associée qui restituera l’asthetics visual que nous souhaitons pour notre application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="78a9c-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="78a9c-138">Après cela, notre page default.aspx affiche sous cette forme.</span><span class="sxs-lookup"><span data-stu-id="78a9c-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="78a9c-139">Notez que les liens de l’image dans la partie supérieure droite de la page et les éléments de menu qui ont été ajoutés à la page maître.</span><span class="sxs-lookup"><span data-stu-id="78a9c-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="78a9c-140">Uniquement les liens de « connexion » et « Compte » pointent vers les pages qui existent (générés par le modèle par défaut) et le reste des pages, nous allons implémenter que nous construisons notre application.</span><span class="sxs-lookup"><span data-stu-id="78a9c-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="78a9c-141">Nous allons également déplacer la Page maître dans le répertoire de Styles.</span><span class="sxs-lookup"><span data-stu-id="78a9c-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="78a9c-142">Bien que ce soit uniquement une préférence il peut rendre les choses un peu plus facile si nous décidons pour que notre application « personnalisable » à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="78a9c-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="78a9c-143">Une fois cela que nous devons modifier la page maître références dans tous les fichiers .aspx générés par la valeur par défaut des pages Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="78a9c-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="78a9c-144">Next</span><span class="sxs-lookup"><span data-stu-id="78a9c-144">Next</span></span>](tailspin-spyworks-part-2.md)
