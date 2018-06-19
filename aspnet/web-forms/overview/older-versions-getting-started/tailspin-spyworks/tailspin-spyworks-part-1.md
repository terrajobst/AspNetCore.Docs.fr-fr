---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Partie 1 : Fichier -> Nouveau projet | Documents Microsoft'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 1 couvre la vue d’ensemble et le fichier/nouveau projet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892463"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="9b3a5-104">Partie 1 : Fichier -> Nouveau projet</span><span class="sxs-lookup"><span data-stu-id="9b3a5-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="9b3a5-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9b3a5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9b3a5-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9b3a5-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9b3a5-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9b3a5-109">Partie 1 couvre la vue d’ensemble et le fichier/nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="9b3a5-110">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="9b3a5-110">Overview</span></span>

<span data-ttu-id="9b3a5-111">Ce didacticiel est une introduction à Web Forms d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="9b3a5-112">Nous allons commencer lentement, afin de l’expérience de développement de niveau web débutant est OK.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="9b3a5-113">L’application que nous allons créer est un magasin en ligne simple.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="9b3a5-114">Les visiteurs peuvent parcourir les produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="9b3a5-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="9b3a5-115">Ils peuvent afficher un produit unique et l’ajouter à leur panier d’achat :</span><span class="sxs-lookup"><span data-stu-id="9b3a5-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="9b3a5-116">Ils peuvent consulter leur panier d’achat, suppression de tous les éléments qu’ils ne voulez plus que :</span><span class="sxs-lookup"><span data-stu-id="9b3a5-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="9b3a5-117">Procéder à la validation les invite à</span><span class="sxs-lookup"><span data-stu-id="9b3a5-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="9b3a5-118">Après le classement, ils voient un écran de confirmation simple :</span><span class="sxs-lookup"><span data-stu-id="9b3a5-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="9b3a5-119">Nous allons commencer par créer un nouveau projet ASP.NET WebForms dans Visual Studio 2010, et nous allons ajouter progressivement les fonctionnalités pour créer une application fonctionnelle complète.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="9b3a5-120">Avant cela, nous aborderons accès de base de données, les affichages de listes et de la grille, pages de mise à jour des données, validation des données, à l’aide de pages maîtres pour la mise en page cohérente, AJAX, validation, l’appartenance des utilisateurs et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="9b3a5-121">Peut suivre la procédure étape par étape, ou vous pouvez télécharger l’application terminée [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="9b3a5-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="9b3a5-122">Vous pouvez utiliser Visual Studio 2010 ou le libre Visual Web Developer 2010 à partir de [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="9b3a5-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="9b3a5-123">Pour générer l’application, vous pouvez utiliser SQL Server soit libre SQL Server Express pour héberger la base de données.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="9b3a5-124">Fichier / nouveau projet</span><span class="sxs-lookup"><span data-stu-id="9b3a5-124">File / New Project</span></span>

<span data-ttu-id="9b3a5-125">Nous allons commencer en sélectionnant le nouveau projet à partir du menu fichier dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="9b3a5-126">De la boîte de dialogue Nouveau projet s’affiche.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="9b3a5-127">Nous allons sélectionner Visual C# / modèles Web groupe situé à gauche, puis choisissez le modèle « Application de Web ASP.NET » dans la colonne centrale.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="9b3a5-128">Nommez votre projet TailspinSpyworks et appuyez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="9b3a5-129">Cela va créer notre projet.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-129">This will create our project.</span></span> <span data-ttu-id="9b3a5-130">Examinons les dossiers qui sont inclus dans notre application dans l’Explorateur de solutions sur le côté droit.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="9b3a5-131">La Solution vide n’est pas vide, il ajoute une structure de dossiers de base :</span><span class="sxs-lookup"><span data-stu-id="9b3a5-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="9b3a5-132">Notez les conventions implémentées par le modèle de projet par défaut ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="9b3a5-133">Le dossier « Compte » implémente une interface utilisateur de base pour ASP. Sous-système de l’appartenance du réseau.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="9b3a5-134">Le dossier « Scripts » sert de référentiel pour les fichiers JavaScript côté client et les fichiers .js de jQuery principaux sont disponibles par défaut.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="9b3a5-135">Le dossier « Styles » est utilisé pour organiser nos éléments visuels de site web (feuilles de Style CSS)</span><span class="sxs-lookup"><span data-stu-id="9b3a5-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="9b3a5-136">Lorsque vous appuyez sur F5 pour exécuter votre application et de restituer la page default.aspx nous consultez les rubriques suivantes.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="9b3a5-137">Notre première amélioration d’application seront pour remplacer le fichier Style.css dans le modèle Web Forms par défaut avec les classes CSS et les fichiers d’image associée qui restituera l’asthetics visual que nous souhaitons pour notre application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="9b3a5-138">Après cela, notre page default.aspx restitue comme suit.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="9b3a5-139">Notez les liens de l’image en haut à droite de la page et les éléments de menu qui ont été ajoutés à la page maître.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="9b3a5-140">Uniquement les liens de la « connexion » et « Compte » pointent vers les pages qui existent (générés par le modèle par défaut) et le reste des pages que nous créons notre application, nous allons implémenter.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="9b3a5-141">Nous allons également de déplacer la Page maître dans le répertoire de Styles.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="9b3a5-142">S’il s’agit uniquement d’une préférence il peut simplifier les choses un peu si vous décidez que notre application « personnalisable » dans le futur.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="9b3a5-143">Une fois cela que nous aurons besoin pour modifier la page maître références dans tous les fichiers .aspx générés par la valeur par défaut des pages Web Forms d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b3a5-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9b3a5-144">Next</span><span class="sxs-lookup"><span data-stu-id="9b3a5-144">Next</span></span>](tailspin-spyworks-part-2.md)
