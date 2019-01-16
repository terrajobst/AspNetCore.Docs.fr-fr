---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Prise en main 4.7 Web Forms ASP.NET et Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de Microsoft Visual Studio et ASP.NET 4.7
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341550"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="a42d8-103">Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a42d8-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="a42d8-104">[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a42d8-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="a42d8-105">Cette série de didacticiels vous montre comment créer une application Web Forms ASP.NET avec ASP.NET 4.5 et Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a42d8-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="a42d8-106">Introduction</span><span class="sxs-lookup"><span data-stu-id="a42d8-106">Introduction</span></span>

<span data-ttu-id="a42d8-107">Cette série de didacticiels vous guide tout au long de la création d’une application Web Forms ASP.NET à l’aide de Visual Studio 2017 et ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a42d8-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="a42d8-108">Vous allez créer une application nommée **Wingtip Toys** : un site web storefront simplifiée vente d’articles en ligne.</span><span class="sxs-lookup"><span data-stu-id="a42d8-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="a42d8-109">Lors de la série, les nouvelles fonctionnalités de ASP.NET 4.5 sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="a42d8-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="a42d8-110">Public cible</span><span class="sxs-lookup"><span data-stu-id="a42d8-110">Target audience</span></span>

<span data-ttu-id="a42d8-111">Les développeurs qui découvrent les Web Forms ASP.NET sont le public cible de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="a42d8-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="a42d8-112">Vous devez avoir des connaissances dans les domaines suivants :</span><span class="sxs-lookup"><span data-stu-id="a42d8-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="a42d8-113">Programmation orientée objet (OOP) et les langues</span><span class="sxs-lookup"><span data-stu-id="a42d8-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="a42d8-114">Développement Web (HTML, CSS et JavaScript)</span><span class="sxs-lookup"><span data-stu-id="a42d8-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="a42d8-115">bases de données relationnelles</span><span class="sxs-lookup"><span data-stu-id="a42d8-115">Relational databases</span></span>
- <span data-ttu-id="a42d8-116">Architecture multiniveau</span><span class="sxs-lookup"><span data-stu-id="a42d8-116">N-tier architecture</span></span>

<span data-ttu-id="a42d8-117">Pour passer en revue ces zones, envisagez d’étudier le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="a42d8-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="a42d8-118">Mise en route avec Visual c#</span><span class="sxs-lookup"><span data-stu-id="a42d8-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="a42d8-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="a42d8-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="a42d8-120">Base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="a42d8-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="a42d8-121">Architecture à plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="a42d8-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="a42d8-122">Fonctionnalités de l’application</span><span class="sxs-lookup"><span data-stu-id="a42d8-122">Application features</span></span>

<span data-ttu-id="a42d8-123">Les fonctionnalités de Web Form ASP.NET présentées dans cette série incluent :</span><span class="sxs-lookup"><span data-stu-id="a42d8-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="a42d8-124">Le projet d’Application Web (pas le projet de Site Web)</span><span class="sxs-lookup"><span data-stu-id="a42d8-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="a42d8-125">Web Forms</span><span class="sxs-lookup"><span data-stu-id="a42d8-125">Web Forms</span></span>
- <span data-ttu-id="a42d8-126">Pages maîtres, Configuration</span><span class="sxs-lookup"><span data-stu-id="a42d8-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="a42d8-127">Programme d’amorçage</span><span class="sxs-lookup"><span data-stu-id="a42d8-127">Bootstrap</span></span>
- <span data-ttu-id="a42d8-128">Entity Framework Code First, base de données locale</span><span class="sxs-lookup"><span data-stu-id="a42d8-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="a42d8-129">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="a42d8-129">Request Validation</span></span>
- <span data-ttu-id="a42d8-130">Contrôles de données fortement typées</span><span class="sxs-lookup"><span data-stu-id="a42d8-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="a42d8-131">Liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="a42d8-131">Model Binding</span></span>
- <span data-ttu-id="a42d8-132">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="a42d8-132">Data Annotations</span></span>
- <span data-ttu-id="a42d8-133">Fournisseurs de valeurs</span><span class="sxs-lookup"><span data-stu-id="a42d8-133">Value Providers</span></span>
- <span data-ttu-id="a42d8-134">SSL et OAuth</span><span class="sxs-lookup"><span data-stu-id="a42d8-134">SSL and OAuth</span></span>
- <span data-ttu-id="a42d8-135">ASP.NET Identity, Configuration et autorisation</span><span class="sxs-lookup"><span data-stu-id="a42d8-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="a42d8-136">Validation non obstrusive</span><span class="sxs-lookup"><span data-stu-id="a42d8-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="a42d8-137">Routage</span><span class="sxs-lookup"><span data-stu-id="a42d8-137">Routing</span></span>
- <span data-ttu-id="a42d8-138">Gestion des erreurs ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a42d8-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="a42d8-139">Tâches et des scénarios d’application</span><span class="sxs-lookup"><span data-stu-id="a42d8-139">Application scenarios and tasks</span></span>

<span data-ttu-id="a42d8-140">Tâches de la série de didacticiels :</span><span class="sxs-lookup"><span data-stu-id="a42d8-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="a42d8-141">Création, révision et un nouveau projet en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="a42d8-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="a42d8-142">Création d’une structure de base de données</span><span class="sxs-lookup"><span data-stu-id="a42d8-142">Creating a database structure</span></span>
- <span data-ttu-id="a42d8-143">L’initialisation et l’amorçage d’une base de données</span><span class="sxs-lookup"><span data-stu-id="a42d8-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="a42d8-144">Personnalisation de l’interface utilisateur avec des styles, des graphiques et une page maître</span><span class="sxs-lookup"><span data-stu-id="a42d8-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="a42d8-145">Ajout de pages et la navigation</span><span class="sxs-lookup"><span data-stu-id="a42d8-145">Adding pages and navigation</span></span>
- <span data-ttu-id="a42d8-146">Affichage des détails de menu et des données de produit</span><span class="sxs-lookup"><span data-stu-id="a42d8-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="a42d8-147">Création d’un panier d’achat</span><span class="sxs-lookup"><span data-stu-id="a42d8-147">Creating a shopping cart</span></span>
- <span data-ttu-id="a42d8-148">Prise en charge SSL Ajout et OAuth</span><span class="sxs-lookup"><span data-stu-id="a42d8-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="a42d8-149">Ajout d’une méthode de paiement</span><span class="sxs-lookup"><span data-stu-id="a42d8-149">Adding a payment method</span></span>
- <span data-ttu-id="a42d8-150">Y compris un rôle d’administrateur et un utilisateur à l’application</span><span class="sxs-lookup"><span data-stu-id="a42d8-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="a42d8-151">Restreindre l’accès à des pages spécifiques et de dossier</span><span class="sxs-lookup"><span data-stu-id="a42d8-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="a42d8-152">Téléchargement d’un fichier à l’application web</span><span class="sxs-lookup"><span data-stu-id="a42d8-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="a42d8-153">Implémentation de la validation des entrées</span><span class="sxs-lookup"><span data-stu-id="a42d8-153">Implementing input validation</span></span>
- <span data-ttu-id="a42d8-154">Inscription des itinéraires pour l’application web</span><span class="sxs-lookup"><span data-stu-id="a42d8-154">Registering routes for the web application</span></span>
- <span data-ttu-id="a42d8-155">Implémentation de la gestion des erreurs et journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="a42d8-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="a42d8-156">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a42d8-156">Overview</span></span>

<span data-ttu-id="a42d8-157">Cette série de didacticiels est destinée à une personne connaissant les concepts de programmation, mais aucun nouveau Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a42d8-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="a42d8-158">Si vous êtes déjà familiarisé avec ASP.NET Web Forms, cette série peut toujours vous aider à en savoir plus sur les nouvelles fonctionnalités de ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a42d8-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="a42d8-159">Pour les lecteurs êtes pas familiarisés avec les concepts de programmation et ASP.NET Web Forms, consultez les didacticiels de Web Forms supplémentaires fournies dans le [mise en route](../../../index.md) section sur le site Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a42d8-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="a42d8-160">Le fournies dans cette série de didacticiels ASP.NET 4.5 inclut les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="a42d8-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="a42d8-161">Une interface utilisateur simple pour créer des projets qui offre [prise en charge de nombreuses infrastructures ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).</span><span class="sxs-lookup"><span data-stu-id="a42d8-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="a42d8-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), une disposition, thèmes et framework de conception réactive.</span><span class="sxs-lookup"><span data-stu-id="a42d8-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="a42d8-163">[ASP.NET Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures ASP.NET et fonctionne avec des logiciels autres que IIS d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="a42d8-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="a42d8-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a42d8-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="a42d8-165">Une mise à jour vers Entity Framework, ce qui vous permet :</span><span class="sxs-lookup"><span data-stu-id="a42d8-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="a42d8-166">Récupérer et manipuler des données en tant qu’objets fortement typés</span><span class="sxs-lookup"><span data-stu-id="a42d8-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="a42d8-167">Accéder aux données de façon asynchrone</span><span class="sxs-lookup"><span data-stu-id="a42d8-167">Access data asynchronously</span></span>
  - <span data-ttu-id="a42d8-168">Gérer les défaillances temporaires de connexion</span><span class="sxs-lookup"><span data-stu-id="a42d8-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="a42d8-169">Instructions de journalisation SQL</span><span class="sxs-lookup"><span data-stu-id="a42d8-169">Log SQL statements</span></span>

<span data-ttu-id="a42d8-170">Pour une liste complète des fonctionnalités ASP.NET 4.5, consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="a42d8-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="a42d8-171">L’exemple d’application Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="a42d8-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="a42d8-172">Les captures d’écran suivantes proviennent de l’application Web Forms ASP.NET que vous créez dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="a42d8-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="a42d8-173">Lorsque vous exécutez l’application dans Visual Studio, la page d’accueil web suivante s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a42d8-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys - page par défaut](introduction-and-overview/_static/image1.png)

<span data-ttu-id="a42d8-175">Vous pouvez enregistrer en tant qu’un nouvel utilisateur, ou connectez-vous en tant qu’un utilisateur existant.</span><span class="sxs-lookup"><span data-stu-id="a42d8-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="a42d8-176">Le volet de navigation supérieur propose des liens vers les catégories de produits et de leurs produits à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a42d8-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="a42d8-177">Si vous sélectionnez **produits**, tous les produits disponibles sont affichés.</span><span class="sxs-lookup"><span data-stu-id="a42d8-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys - produits](introduction-and-overview/_static/image2.png)

<span data-ttu-id="a42d8-179">Si vous sélectionnez un produit spécifique, les détails du produit sont affichés.</span><span class="sxs-lookup"><span data-stu-id="a42d8-179">If you select a specific product, product details are displayed.</span></span>


![Wingtip Toys - détails du produit](introduction-and-overview/_static/image3.png)

<span data-ttu-id="a42d8-181">En tant qu’utilisateur, vous pouvez inscrire et se connecter avec des fonctionnalités par défaut de modèle Web Forms.</span><span class="sxs-lookup"><span data-stu-id="a42d8-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="a42d8-182">Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant.</span><span class="sxs-lookup"><span data-stu-id="a42d8-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="a42d8-183">En outre, vous pouvez vous connecter en tant que l’administrateur pour ajouter et supprimer des produits à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="a42d8-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - connectez-vous](introduction-and-overview/_static/image4.png)

<span data-ttu-id="a42d8-185">Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et de validation avec PayPal.</span><span class="sxs-lookup"><span data-stu-id="a42d8-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="a42d8-186">L’exemple d’application est conçu pour fonctionner dans un sandbox du développeur de PayPal.</span><span class="sxs-lookup"><span data-stu-id="a42d8-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="a42d8-187">Aucune transaction money réelle n’a lieu.</span><span class="sxs-lookup"><span data-stu-id="a42d8-187">No actual money transaction takes place.</span></span>

![Wingtip Toys - panier d’achat](introduction-and-overview/_static/image5.png)

<span data-ttu-id="a42d8-189">PayPal confirme vos informations de compte, de commande et de paiement.</span><span class="sxs-lookup"><span data-stu-id="a42d8-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="a42d8-191">Après le renvoi de PayPal, vous pouvez consulter et terminer votre commande.</span><span class="sxs-lookup"><span data-stu-id="a42d8-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - ordre révision](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="a42d8-193">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a42d8-193">Prerequisites</span></span>

<span data-ttu-id="a42d8-194">Avant de commencer, assurez-vous que le logiciel suivant est installé sur votre ordinateur :</span><span class="sxs-lookup"><span data-stu-id="a42d8-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="a42d8-195">[Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a42d8-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="a42d8-196">Le .NET Framework est installé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a42d8-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="a42d8-197">Cette série de didacticiels utilise Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="a42d8-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="a42d8-198">Vous pouvez utiliser qu’ou Microsoft Visual Studio 2017 pour terminer cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="a42d8-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="a42d8-199">Notez les points suivants concernant Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="a42d8-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="a42d8-200">Microsoft Visual Studio 2017 et Microsoft Visual Studio Community 2017 sont appelés *Visual Studio* tout au long de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="a42d8-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="a42d8-201">Visual Studio 2017 est installé en regard les anciennes versions déjà installées.</span><span class="sxs-lookup"><span data-stu-id="a42d8-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="a42d8-202">Les sites créés dans les versions antérieures peuvent être ouverts dans Visual Studio 2017 et continuent à ouvrir dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="a42d8-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="a42d8-203">La première fois que vous avez démarré Visual Studio, il est supposé que vous avez sélectionné le *développement Web* paramètres.</span><span class="sxs-lookup"><span data-stu-id="a42d8-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="a42d8-204">Pour plus d'informations, voir [Procédure : Sélectionnez les paramètres d’environnement de développement de Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="a42d8-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="a42d8-205">Après avoir installé les composants requis, vous êtes prêt à commencer à créer le projet Web présenté dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="a42d8-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="a42d8-206">Télécharger l’exemple d’application</span><span class="sxs-lookup"><span data-stu-id="a42d8-206">Download the sample application</span></span>

 <span data-ttu-id="a42d8-207">Vous pouvez télécharger l’exemple terminé applicatiion à tout moment à partir du site d’exemples MSDN :</span><span class="sxs-lookup"><span data-stu-id="a42d8-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="a42d8-208">[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="a42d8-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="a42d8-209">Ce téléchargement comprend les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="a42d8-209">This download has the following items:</span></span>

- <span data-ttu-id="a42d8-210">L’exemple d’application dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="a42d8-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="a42d8-211">Les ressources utilisées pour créer l’exemple d’application dans le *WingtipToys-ressources* dossier dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="a42d8-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="a42d8-212">Le téléchargement est un *.zip* fichier.</span><span class="sxs-lookup"><span data-stu-id="a42d8-212">The download is a *.zip* file.</span></span> <span data-ttu-id="a42d8-213">Pour voir le projet terminé qui crée cette série de didacticiels, recherchez et sélectionnez le *C#* dossier dans le fichier .zip.</span><span class="sxs-lookup"><span data-stu-id="a42d8-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="a42d8-214">Enregistrer le C# vers le dossier vous permet de travailler avec les projets Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a42d8-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="a42d8-215">Par défaut, le dossier de projets Visual Studio 2017 est :</span><span class="sxs-lookup"><span data-stu-id="a42d8-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="a42d8-216"><strong>C:\Users&#92;</strong><strong><em>&lt;nom d’utilisateur&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="a42d8-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="a42d8-217">Renommer le ***c#*** dossier ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="a42d8-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="a42d8-218">Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de renommer le *c#* dossier *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="a42d8-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="a42d8-219">Pour exécuter le projet terminé, ouvrez le *WingtipToys* dossier et double-cliquez sur le *WingtipToys.sln* fichier.</span><span class="sxs-lookup"><span data-stu-id="a42d8-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="a42d8-220">Visual Studio 2017 ouvre le projet.</span><span class="sxs-lookup"><span data-stu-id="a42d8-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="a42d8-221">Ensuite, cliquez sur le *Default.aspx* fichier **l’Explorateur de solutions** et sélectionnez **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="a42d8-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="a42d8-222">Participez à un questionnaire de Web Forms ASP.NET pour passer en revue le contenu</span><span class="sxs-lookup"><span data-stu-id="a42d8-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="a42d8-223">À l’issue de la série de didacticiels, participez à un questionnaire pour tester vos connaissances et de renforcer les concepts clés.</span><span class="sxs-lookup"><span data-stu-id="a42d8-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="a42d8-224">Chaque question fournit une explication et les liens vers des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="a42d8-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="a42d8-225">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="a42d8-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="a42d8-226">Commentaires et didacticiel prise en charge</span><span class="sxs-lookup"><span data-stu-id="a42d8-226">Tutorial support and comments</span></span>

<span data-ttu-id="a42d8-227">Pour les questions et vos commentaires, utilisez la section de questions-réponses incluse sur le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) exemple de page.</span><span class="sxs-lookup"><span data-stu-id="a42d8-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="a42d8-228">Commentaires sur cette série de didacticiels sont les bienvenus.</span><span class="sxs-lookup"><span data-stu-id="a42d8-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="a42d8-229">Lorsque la mise à jour de cette série de didacticiels, tout est mis en œuvre à prendre en compte des corrections ou des suggestions d’améliorations.</span><span class="sxs-lookup"><span data-stu-id="a42d8-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="a42d8-230">Si une erreur se produit, les messages d’erreur correspondante peut entraîner une confus, sans aucune bonne explication sur la façon de résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="a42d8-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="a42d8-231">Pour une aide, vous pouvez vérifier le [forums ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="a42d8-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="a42d8-232">Une autre bonne source est la section de questions-réponses dans le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) exemple de page.</span><span class="sxs-lookup"><span data-stu-id="a42d8-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="a42d8-233">Next</span><span class="sxs-lookup"><span data-stu-id="a42d8-233">Next</span></span>](create-the-project.md)
