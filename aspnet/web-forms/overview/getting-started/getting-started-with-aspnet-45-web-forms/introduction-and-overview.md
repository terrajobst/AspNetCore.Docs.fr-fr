---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Prise en main 4.5 Web Forms ASP.NET et Visual Studio 2013 | Documents Microsoft
author: Erikre
description: Cette série de didacticiels pas à pas, vous allez apprendre les principes fondamentaux de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: a3527b54d1936bc14e32a1828ac3a2be625107ba
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="b0750-103">Prise en main 4.5 Web Forms ASP.NET et Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b0750-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="b0750-104">par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="b0750-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="b0750-105">[Télécharger Wingtip Toys exemple de projet (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger des livres (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0750-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="b0750-106">Cette série de didacticiels pas à pas, vous allez apprendre les principes fondamentaux de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="b0750-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="b0750-107">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="b0750-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="b0750-108">Testez vos connaissances et approfondir les concepts clés en prenant le questionnaire d’ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="b0750-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="b0750-109">Ce test a été spécialement conçu de contenu de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b0750-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="b0750-110">Chaque question dans le questionnaire fournit une explication, ainsi que des liens vers des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="b0750-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="b0750-111">Introduction</span><span class="sxs-lookup"><span data-stu-id="b0750-111">Introduction</span></span>

<span data-ttu-id="b0750-112">Cette série de didacticiels vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET à l’aide de Visual Studio Express 2013 pour le Web et ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b0750-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="b0750-113">L’application que vous allez créer est nommée **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="b0750-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="b0750-114">Il s’agit d’un exemple simplifié d’un site web avant de magasin qui vend des éléments en ligne.</span><span class="sxs-lookup"><span data-stu-id="b0750-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="b0750-115">Cette série de didacticiels met en évidence les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b0750-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="b0750-116">Commentaires sont les bienvenus et nous allons effectuer tout en oeuvre pour mettre à jour cette série de didacticiels en fonction de vos suggestions.</span><span class="sxs-lookup"><span data-stu-id="b0750-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="b0750-117">Projet de téléchargement terminé</span><span class="sxs-lookup"><span data-stu-id="b0750-117">Download completed project</span></span>

<span data-ttu-id="b0750-118">Vous pouvez télécharger un projet c# qui contient le didacticiel terminé.</span><span class="sxs-lookup"><span data-stu-id="b0750-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="b0750-119">[Prise en main 4.5 Web Forms ASP.NET et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="b0750-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="b0750-120">Passez en revue le contenu en prenant le questionnaire ASP.NET Web Forms connexe</span><span class="sxs-lookup"><span data-stu-id="b0750-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="b0750-121">Après avoir terminé ce didacticiel, testez vos connaissances et approfondir les concepts clés en prenant le [ASP.NET Web Forms questionnaire](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="b0750-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="b0750-122">Ce test a été spécialement conçu de contenu de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b0750-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="b0750-123">Chaque question dans le questionnaire fournit une explication, ainsi que des liens vers des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="b0750-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="b0750-124">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="b0750-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="b0750-125">Public</span><span class="sxs-lookup"><span data-stu-id="b0750-125">Audience</span></span>

<span data-ttu-id="b0750-126">Est de cette série de didacticiels destiné aux développeurs expérimentés qui sont nouveaux pour ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="b0750-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="b0750-127">Un développeur intéressé par cette série de didacticiels doit avoir les compétences suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0750-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="b0750-128">Un objet qui connaissent orientée langage de programmation (OOP)</span><span class="sxs-lookup"><span data-stu-id="b0750-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="b0750-129">Familiarisé avec les concepts de développement Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="b0750-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="b0750-130">Familiarisé avec les concepts de base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="b0750-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="b0750-131">Familiarisé avec les concepts de l’architecture multicouche</span><span class="sxs-lookup"><span data-stu-id="b0750-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="b0750-132">Si vous souhaitez parcourir les éléments mentionnés ci-dessus, examinez le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="b0750-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="b0750-133">Prise en main de Visual c#</span><span class="sxs-lookup"><span data-stu-id="b0750-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="b0750-134">[Développement Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="b0750-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="b0750-135">Base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="b0750-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="b0750-136">Architecture multiniveau</span><span class="sxs-lookup"><span data-stu-id="b0750-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="b0750-137">Fonctionnalités de l’application</span><span class="sxs-lookup"><span data-stu-id="b0750-137">Application Features</span></span>

<span data-ttu-id="b0750-138">Les fonctionnalités d’ASP.NET Web Form présentées dans cette série sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0750-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="b0750-139">Le projet d’Application Web (pas le projet de Site Web)</span><span class="sxs-lookup"><span data-stu-id="b0750-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="b0750-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="b0750-140">Web Forms</span></span>
- <span data-ttu-id="b0750-141">Pages maîtres, Configuration</span><span class="sxs-lookup"><span data-stu-id="b0750-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="b0750-142">programme d’amorçage</span><span class="sxs-lookup"><span data-stu-id="b0750-142">Bootstrap</span></span>
- <span data-ttu-id="b0750-143">Entity Framework Code First, base de données locale</span><span class="sxs-lookup"><span data-stu-id="b0750-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="b0750-144">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="b0750-144">Request Validation</span></span>
- <span data-ttu-id="b0750-145">Fortement typé, les contrôles de données de modèle de liaison, les Annotations de données et que la valeur de fournisseurs</span><span class="sxs-lookup"><span data-stu-id="b0750-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="b0750-146">Le protocole SSL et OAuth</span><span class="sxs-lookup"><span data-stu-id="b0750-146">SSL and OAuth</span></span>
- <span data-ttu-id="b0750-147">Identité ASP.NET, Configuration et autorisation</span><span class="sxs-lookup"><span data-stu-id="b0750-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="b0750-148">Validation non obstrusive</span><span class="sxs-lookup"><span data-stu-id="b0750-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="b0750-149">Routage</span><span class="sxs-lookup"><span data-stu-id="b0750-149">Routing</span></span>
- <span data-ttu-id="b0750-150">Gestion des erreurs de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b0750-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="b0750-151">Tâches et les scénarios d’application</span><span class="sxs-lookup"><span data-stu-id="b0750-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="b0750-152">Les tâches décrites dans cette série sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0750-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="b0750-153">Création, la révision et le nouveau projet en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="b0750-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="b0750-154">Création de la structure de base de données</span><span class="sxs-lookup"><span data-stu-id="b0750-154">Creating the database structure</span></span>
- <span data-ttu-id="b0750-155">L’initialisation et l’amorçage de la base de données</span><span class="sxs-lookup"><span data-stu-id="b0750-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="b0750-156">Personnalisation de l’interface utilisateur à l’aide de styles, de graphiques et d’une page maître</span><span class="sxs-lookup"><span data-stu-id="b0750-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="b0750-157">Ajout de pages et de navigation</span><span class="sxs-lookup"><span data-stu-id="b0750-157">Adding pages and navigation</span></span>
- <span data-ttu-id="b0750-158">Affichage des détails de menu et les données de produit</span><span class="sxs-lookup"><span data-stu-id="b0750-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="b0750-159">Création d’un panier d’achat</span><span class="sxs-lookup"><span data-stu-id="b0750-159">Creating a shopping cart</span></span>
- <span data-ttu-id="b0750-160">Prise en charge SSL Ajout et OAuth</span><span class="sxs-lookup"><span data-stu-id="b0750-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="b0750-161">Ajout d’une méthode de paiement</span><span class="sxs-lookup"><span data-stu-id="b0750-161">Adding a payment method</span></span>
- <span data-ttu-id="b0750-162">Y compris un rôle d’administrateur et un utilisateur à l’application</span><span class="sxs-lookup"><span data-stu-id="b0750-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="b0750-163">Restreindre l’accès à des pages spécifiques et de dossier</span><span class="sxs-lookup"><span data-stu-id="b0750-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="b0750-164">Téléchargement d’un fichier à l’application web</span><span class="sxs-lookup"><span data-stu-id="b0750-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="b0750-165">Implémenter la validation d’entrée</span><span class="sxs-lookup"><span data-stu-id="b0750-165">Implementing input validation</span></span>
- <span data-ttu-id="b0750-166">Inscription des itinéraires pour l’application web</span><span class="sxs-lookup"><span data-stu-id="b0750-166">Registering routes for the web application</span></span>
- <span data-ttu-id="b0750-167">Implémentation de la gestion des erreurs et journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="b0750-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="b0750-168">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="b0750-168">Overview</span></span>

<span data-ttu-id="b0750-169">Si vous débutez avec ASP.NET Web Forms, mais êtes familiarisé avec les concepts de programmation, vous avez le droit didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b0750-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="b0750-170">Si vous êtes déjà familiarisé avec ASP.NET Web Forms, vous pouvez bénéficier de cette série de didacticiels par les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b0750-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="b0750-171">Si vous n’êtes pas familiarisé avec les concepts de programmation et les Web Forms ASP.NET, consultez les didacticiels supplémentaires fournies dans les formulaires Web [mise en route](../../../index.md) section sur le site Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b0750-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="b0750-172">Spécifique au **dernière** ASP.NET 4.5 fonctionnalités fournies dans les formulaires Web cette série de didacticiels inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b0750-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="b0750-173">Une interface utilisateur simple pour la création de projets à cette offre [prennent en charge plusieurs infrastructures d’ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).</span><span class="sxs-lookup"><span data-stu-id="b0750-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="b0750-174">[Programme d’amorçage](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), une infrastructure de mise en page et des thèmes qui fournit des fonctionnalités de conception et des thèmes réactives.</span><span class="sxs-lookup"><span data-stu-id="b0750-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="b0750-175">[ASP.NET Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures d’ASP.NET et fonctionne avec le logiciel autres qu’IIS d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="b0750-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="b0750-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), une mise à jour à Entity Framework qui vous permet de récupérez et manipuler des données de manière fortement typée d’objets, accéder aux données de façon asynchrone, gérer les défaillances temporaires de connexion et consigner les instructions SQL.</span><span class="sxs-lookup"><span data-stu-id="b0750-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="b0750-177">Pour obtenir une liste complète des fonctionnalités ASP.NET 4.5, consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="b0750-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="b0750-178">L’exemple d’Application Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="b0750-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="b0750-179">Les captures d’écran suivantes fournissent un aperçu rapide de l’application ASP.NET Web forms que vous allez créer dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b0750-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="b0750-180">Lorsque vous exécutez l’application à partir de Visual Studio Express 2013 pour le Web, vous verrez la page d’accueil web suivantes.</span><span class="sxs-lookup"><span data-stu-id="b0750-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - page par défaut](introduction-and-overview/_static/image1.png)

<span data-ttu-id="b0750-182">Vous pouvez inscrire comme nouvel utilisateur, ou en tant qu’un utilisateur existant.</span><span class="sxs-lookup"><span data-stu-id="b0750-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="b0750-183">Navigation est fournie en haut de chaque catégorie de produits en récupérant les produits disponibles à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b0750-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="b0750-184">En sélectionnant le lien de produits, vous serez en mesure d’afficher une liste de tous les produits disponibles.</span><span class="sxs-lookup"><span data-stu-id="b0750-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - produits](introduction-and-overview/_static/image2.png)

<span data-ttu-id="b0750-186">Vous pouvez également voir les détails des produits individuels en sélectionnant un des produits répertoriés.</span><span class="sxs-lookup"><span data-stu-id="b0750-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - détails du produit](introduction-and-overview/_static/image3.png)

<span data-ttu-id="b0750-188">En tant qu’utilisateur, vous pouvez inscrire et se connecter à l’aide de la fonctionnalité par défaut du modèle Web Forms.</span><span class="sxs-lookup"><span data-stu-id="b0750-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="b0750-189">Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant.</span><span class="sxs-lookup"><span data-stu-id="b0750-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="b0750-190">En outre, vous pouvez vous connecter en tant que l’administrateur pour ajouter et supprimer des produits à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b0750-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - connectez-vous](introduction-and-overview/_static/image4.png)

<span data-ttu-id="b0750-192">Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et extraction avec PayPal.</span><span class="sxs-lookup"><span data-stu-id="b0750-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="b0750-193">Notez que cet exemple d’application est conçue pour fonctionner avec un sandbox de développement de PayPal.</span><span class="sxs-lookup"><span data-stu-id="b0750-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="b0750-194">Aucune transaction money réelle n’a lieu.</span><span class="sxs-lookup"><span data-stu-id="b0750-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - panier d’achat](introduction-and-overview/_static/image5.png)

<span data-ttu-id="b0750-196">PayPal confirmera votre compte, l’ordre et les informations de paiement.</span><span class="sxs-lookup"><span data-stu-id="b0750-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="b0750-198">Après le retour à partir de PayPal, vous pouvez consulter et finaliser votre commande.</span><span class="sxs-lookup"><span data-stu-id="b0750-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - vérification de la commande](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="b0750-200">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b0750-200">Prerequisites</span></span>

<span data-ttu-id="b0750-201">Avant de commencer, assurez-vous d’avoir les logiciels suivants installés sur votre ordinateur :</span><span class="sxs-lookup"><span data-stu-id="b0750-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="b0750-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="b0750-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="b0750-203">Le .NET Framework est installé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b0750-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="b0750-204">Cette série de didacticiels utilise Microsoft Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="b0750-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="b0750-205">Pour terminer cette série de didacticiels, vous pouvez utiliser Microsoft Visual Studio Express 2013 pour le Web ou Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b0750-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b0750-206">Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web seront souvent être appelées en tant que Visual Studio tout au long de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b0750-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="b0750-207">Si vous avez déjà installée une version de Visual Studio, le processus d’installation installe Visual Studio 2013 ou Microsoft Visual Studio Express 2013 pour le Web en regard de la version existante.</span><span class="sxs-lookup"><span data-stu-id="b0750-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="b0750-208">Les sites que vous avez créé dans les versions antérieures peuvent être ouverts dans Visual Studio 2013 et continuent à ouvrir dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="b0750-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b0750-209">Cette procédure pas à pas suppose que vous avez sélectionné le *développement Web* collection de paramètres de la première fois que vous avez démarré Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0750-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="b0750-210">Pour plus d’informations, consultez [Comment : sélectionner des paramètres d’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0750-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="b0750-211">Télécharger l’exemple d’Application</span><span class="sxs-lookup"><span data-stu-id="b0750-211">Download the Sample Application</span></span>

<span data-ttu-id="b0750-212">Après avoir installé les composants requis, vous êtes prêt à commencer à créer le projet Web est présenté dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b0750-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="b0750-213">Si vous souhaitez **éventuellement** exécuter l’exemple d’application qui crée de cette série de didacticiels, vous pouvez le télécharger depuis le site MSDN Samples.</span><span class="sxs-lookup"><span data-stu-id="b0750-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="b0750-214">Ce téléchargement contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b0750-214">This download contains the following:</span></span>

- <span data-ttu-id="b0750-215">L’exemple d’application dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="b0750-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="b0750-216">Les ressources utilisées pour créer l’exemple d’application dans le *WingtipToys-composants* dossier dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="b0750-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="b0750-217">Téléchargez le fichier à partir du site d’exemples MSDN :</span><span class="sxs-lookup"><span data-stu-id="b0750-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="b0750-218">[Prise en main 4.5 Web Forms ASP.NET et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="b0750-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="b0750-219">Le téléchargement est un <em>.zip</em> fichier.</span><span class="sxs-lookup"><span data-stu-id="b0750-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="b0750-220">Pour afficher le projet terminé cette série de didacticiels crée, recherchez et sélectionnez le <em>c#</em>dossier dans le <em>.zip</em> fichier.</span><span class="sxs-lookup"><span data-stu-id="b0750-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="b0750-221">Enregistrer le <em>c#</em> dossier dans le dossier que vous utilisez pour travailler avec les projets Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b0750-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="b0750-222">Par défaut, le dossier de projets Visual Studio 2013 est le suivant :</span><span class="sxs-lookup"><span data-stu-id="b0750-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="b0750-223"><strong>C:\Users&#92;</strong><strong><em>&lt;nom d’utilisateur&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="b0750-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="b0750-224">Renommer le ***c#*** dossier ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="b0750-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="b0750-225">Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de renommer le *c#* dossier *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="b0750-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="b0750-226">Pour exécuter le projet terminé, ouvrez le *WingtipToys* et double-cliquez sur le *WingtipToys.sln* fichier.</span><span class="sxs-lookup"><span data-stu-id="b0750-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="b0750-227">Visual Studio 2013 s’ouvre le projet.</span><span class="sxs-lookup"><span data-stu-id="b0750-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="b0750-228">Ensuite, cliquez sur le *Default.aspx* dans la fenêtre de l’Explorateur de solutions, cliquez sur Afficher dans le navigateur dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="b0750-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="b0750-229">Commentaires et prise en charge de didacticiel</span><span class="sxs-lookup"><span data-stu-id="b0750-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="b0750-230">Utilisez la section Q et r incluse avec le [prise en main de Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) exemple (c#) pour toute question ou tout commentaire.</span><span class="sxs-lookup"><span data-stu-id="b0750-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="b0750-231">Commentaires sur cette série de didacticiels sont les bienvenus et lorsque la mise à jour de cette série de didacticiels tous les efforts sont effectués pour tenir des corrections de compte ou des suggestions d’amélioration est fournies dans les commentaires du didacticiels.</span><span class="sxs-lookup"><span data-stu-id="b0750-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="b0750-232">Lorsqu’une erreur se produit pendant le développement, ou si le site Web ne fonctionne pas correctement, les messages d’erreur peuvent donner des indices complexes à la source du problème ou ne peuvent pas expliquer comment la corriger.</span><span class="sxs-lookup"><span data-stu-id="b0750-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="b0750-233">Pour résoudre certains problèmes courants, vous pouvez également utiliser le [forums ASP.NET](https://forums.asp.net/) ou la section Q et r inclus avec le [prise en main de Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) exemple.</span><span class="sxs-lookup"><span data-stu-id="b0750-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="b0750-234">Si vous obtenez un message d’erreur, ou quelque chose ne fonctionne pas lorsque vous parcourez les didacticiels, veillez à consulter les emplacements ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b0750-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b0750-235">Next</span><span class="sxs-lookup"><span data-stu-id="b0750-235">Next</span></span>](create-the-project.md)
