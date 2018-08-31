---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous présente les principes de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 8e3ae964dafc73bdf703cd7cbab430bbc99a6188
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336022"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="71f33-103">Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="71f33-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="71f33-104">par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="71f33-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="71f33-105">[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="71f33-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="71f33-106">Cette série de didacticiels pas à pas vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="71f33-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="71f33-107">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="71f33-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  

## <a name="introduction"></a><span data-ttu-id="71f33-108">Introduction</span><span class="sxs-lookup"><span data-stu-id="71f33-108">Introduction</span></span>

<span data-ttu-id="71f33-109">Cette série de didacticiels vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET à l’aide de Visual Studio Express 2013 pour le Web et ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="71f33-109">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="71f33-110">L’application que vous allez créer est nommée **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="71f33-110">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="71f33-111">Il est un exemple simplifié d’un site web avant de magasin qui vend des éléments en ligne.</span><span class="sxs-lookup"><span data-stu-id="71f33-111">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="71f33-112">Cette série de didacticiels met en évidence les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="71f33-112">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="71f33-113">Commentaires sont les bienvenus, et nous allons s’efforcer de mettre à jour cette série de didacticiels en fonction de vos suggestions.</span><span class="sxs-lookup"><span data-stu-id="71f33-113">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="71f33-114">Projet de téléchargement terminé</span><span class="sxs-lookup"><span data-stu-id="71f33-114">Download completed project</span></span>

<span data-ttu-id="71f33-115">Vous pouvez télécharger un projet c# qui contient la fin du didacticiel.</span><span class="sxs-lookup"><span data-stu-id="71f33-115">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="71f33-116">[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="71f33-116">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="71f33-117">Passez en revue le contenu en prenant le questionnaire de Web Forms ASP.NET connexe</span><span class="sxs-lookup"><span data-stu-id="71f33-117">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="71f33-118">Après avoir terminé ce didacticiel, testez vos connaissances et approfondir les concepts clés en prenant le [ASP.NET Web Forms questionnaire](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="71f33-118">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="71f33-119">Ce questionnaire a été spécifiquement conçu de contenu de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="71f33-119">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="71f33-120">Chaque question du test fournit une explication ainsi que des liens vers des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="71f33-120">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="71f33-121">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="71f33-121">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="71f33-122">Public</span><span class="sxs-lookup"><span data-stu-id="71f33-122">Audience</span></span>

<span data-ttu-id="71f33-123">Est de cette série de didacticiels destiné aux développeurs expérimentés qui débutent avec ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="71f33-123">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="71f33-124">Un développeur intéressé à cette série de didacticiels doit avoir les compétences suivantes :</span><span class="sxs-lookup"><span data-stu-id="71f33-124">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="71f33-125">Familiarisés avec un objet orientée langage de programmation (OOP)</span><span class="sxs-lookup"><span data-stu-id="71f33-125">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="71f33-126">Maîtriser les concepts de développement Web (HTML, CSS et JavaScript)</span><span class="sxs-lookup"><span data-stu-id="71f33-126">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="71f33-127">Maîtriser les concepts de base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="71f33-127">Familiar with relational database concepts</span></span>
- <span data-ttu-id="71f33-128">Maîtriser les concepts d’architecture multiniveau</span><span class="sxs-lookup"><span data-stu-id="71f33-128">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="71f33-129">Si vous souhaitez passer en revue les éléments mentionnés ci-dessus, examinez le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="71f33-129">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="71f33-130">Mise en route avec Visual c#</span><span class="sxs-lookup"><span data-stu-id="71f33-130">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="71f33-131">[Développement Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="71f33-131">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="71f33-132">Base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="71f33-132">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="71f33-133">Architecture à plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="71f33-133">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="71f33-134">Fonctionnalités de l’application</span><span class="sxs-lookup"><span data-stu-id="71f33-134">Application Features</span></span>

<span data-ttu-id="71f33-135">Les fonctionnalités de Web Form ASP.NET présentées dans cette série incluent :</span><span class="sxs-lookup"><span data-stu-id="71f33-135">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="71f33-136">Le projet d’Application Web (pas le projet de Site Web)</span><span class="sxs-lookup"><span data-stu-id="71f33-136">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="71f33-137">Web Forms</span><span class="sxs-lookup"><span data-stu-id="71f33-137">Web Forms</span></span>
- <span data-ttu-id="71f33-138">Pages maîtres, Configuration</span><span class="sxs-lookup"><span data-stu-id="71f33-138">Master Pages, Configuration</span></span>
- <span data-ttu-id="71f33-139">Programme d’amorçage</span><span class="sxs-lookup"><span data-stu-id="71f33-139">Bootstrap</span></span>
- <span data-ttu-id="71f33-140">Entity Framework Code First, base de données locale</span><span class="sxs-lookup"><span data-stu-id="71f33-140">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="71f33-141">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="71f33-141">Request Validation</span></span>
- <span data-ttu-id="71f33-142">Contrôles de données fortement typés liaison, les Annotations de données, de modèle et que la valeur des fournisseurs</span><span class="sxs-lookup"><span data-stu-id="71f33-142">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="71f33-143">SSL et OAuth</span><span class="sxs-lookup"><span data-stu-id="71f33-143">SSL and OAuth</span></span>
- <span data-ttu-id="71f33-144">ASP.NET Identity, Configuration et autorisation</span><span class="sxs-lookup"><span data-stu-id="71f33-144">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="71f33-145">Validation non obstrusive</span><span class="sxs-lookup"><span data-stu-id="71f33-145">Unobtrusive Validation</span></span>
- <span data-ttu-id="71f33-146">Routage</span><span class="sxs-lookup"><span data-stu-id="71f33-146">Routing</span></span>
- <span data-ttu-id="71f33-147">Gestion des erreurs ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71f33-147">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="71f33-148">Tâches et des scénarios d’application</span><span class="sxs-lookup"><span data-stu-id="71f33-148">Application Scenarios and Tasks</span></span>

<span data-ttu-id="71f33-149">Tâches décrites dans cette série sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="71f33-149">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="71f33-150">Création, la révision et le nouveau projet en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="71f33-150">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="71f33-151">Création de la structure de base de données</span><span class="sxs-lookup"><span data-stu-id="71f33-151">Creating the database structure</span></span>
- <span data-ttu-id="71f33-152">L’initialisation et l’amorçage de la base de données</span><span class="sxs-lookup"><span data-stu-id="71f33-152">Initializing and seeding the database</span></span>
- <span data-ttu-id="71f33-153">Personnalisation de l’interface utilisateur à l’aide de styles, de graphiques et d’une page maître</span><span class="sxs-lookup"><span data-stu-id="71f33-153">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="71f33-154">Ajout de pages et la navigation</span><span class="sxs-lookup"><span data-stu-id="71f33-154">Adding pages and navigation</span></span>
- <span data-ttu-id="71f33-155">Affichage des détails de menu et des données de produit</span><span class="sxs-lookup"><span data-stu-id="71f33-155">Displaying menu details and product data</span></span>
- <span data-ttu-id="71f33-156">Création d’un panier d’achat</span><span class="sxs-lookup"><span data-stu-id="71f33-156">Creating a shopping cart</span></span>
- <span data-ttu-id="71f33-157">Prise en charge SSL Ajout et OAuth</span><span class="sxs-lookup"><span data-stu-id="71f33-157">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="71f33-158">Ajout d’une méthode de paiement</span><span class="sxs-lookup"><span data-stu-id="71f33-158">Adding a payment method</span></span>
- <span data-ttu-id="71f33-159">Y compris un rôle d’administrateur et un utilisateur à l’application</span><span class="sxs-lookup"><span data-stu-id="71f33-159">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="71f33-160">Restreindre l’accès à des pages spécifiques et de dossier</span><span class="sxs-lookup"><span data-stu-id="71f33-160">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="71f33-161">Téléchargement d’un fichier à l’application web</span><span class="sxs-lookup"><span data-stu-id="71f33-161">Uploading a file to the web application</span></span>
- <span data-ttu-id="71f33-162">Implémentation de la validation des entrées</span><span class="sxs-lookup"><span data-stu-id="71f33-162">Implementing input validation</span></span>
- <span data-ttu-id="71f33-163">Inscription des itinéraires pour l’application web</span><span class="sxs-lookup"><span data-stu-id="71f33-163">Registering routes for the web application</span></span>
- <span data-ttu-id="71f33-164">Implémentation de la gestion des erreurs et journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="71f33-164">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="71f33-165">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="71f33-165">Overview</span></span>

<span data-ttu-id="71f33-166">Si vous débutez avec ASP.NET Web Forms, mais êtes familiarisé avec les concepts de programmation, vous avez le didacticiel de droite.</span><span class="sxs-lookup"><span data-stu-id="71f33-166">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="71f33-167">Si vous êtes déjà familiarisé avec ASP.NET Web Forms, vous pouvez bénéficier de cette série de didacticiels par les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="71f33-167">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="71f33-168">Si vous n’êtes pas familiarisé avec les concepts de programmation et ASP.NET Web Forms, consultez les didacticiels supplémentaires fournies dans les formulaires Web [mise en route](../../../index.md) section sur le site Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71f33-168">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="71f33-169">Spécifique au **dernière** ASP.NET 4.5 fonctionnalités fournies dans les formulaires Web cette série de didacticiels inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="71f33-169">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="71f33-170">Une interface utilisateur simple pour la création de projets de cette offre [prennent en charge plusieurs infrastructures ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).</span><span class="sxs-lookup"><span data-stu-id="71f33-170">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="71f33-171">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), une infrastructure de mise en page et des thèmes qui fournit des fonctionnalités de création et de thèmes réactives.</span><span class="sxs-lookup"><span data-stu-id="71f33-171">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="71f33-172">[ASP.NET Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures ASP.NET et fonctionne avec des logiciels autres que IIS d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="71f33-172">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="71f33-173">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), une mise à jour Entity Framework qui vous permet de récupérez et manipuler des données de manière fortement typée d’objets, accéder aux données de façon asynchrone, gérer les défaillances temporaires de connexion et consigner les instructions SQL.</span><span class="sxs-lookup"><span data-stu-id="71f33-173">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="71f33-174">Pour obtenir une liste complète des fonctionnalités de ASP.NET 4.5, consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="71f33-174">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="71f33-175">L’exemple d’Application Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="71f33-175">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="71f33-176">Les captures d’écran suivantes fournissent un aperçu rapide de l’application Web forms ASP.NET que vous allez créer dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="71f33-176">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="71f33-177">Lorsque vous exécutez l’application à partir de Visual Studio Express 2013 pour le Web, vous verrez la page d’accueil web suivantes.</span><span class="sxs-lookup"><span data-stu-id="71f33-177">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - page par défaut](introduction-and-overview/_static/image1.png)

<span data-ttu-id="71f33-179">Vous pouvez enregistrer en tant qu’un nouvel utilisateur, ou connectez-vous en tant qu’un utilisateur existant.</span><span class="sxs-lookup"><span data-stu-id="71f33-179">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="71f33-180">Navigation est fournie en haut de chaque catégorie de produits en récupérant les produits disponibles à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="71f33-180">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="71f33-181">En sélectionnant le lien de produits, vous serez en mesure de voir une liste de tous les produits disponibles.</span><span class="sxs-lookup"><span data-stu-id="71f33-181">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - produits](introduction-and-overview/_static/image2.png)

<span data-ttu-id="71f33-183">Vous pouvez également voir les détails du produit individuels en sélectionnant un des produits répertoriés.</span><span class="sxs-lookup"><span data-stu-id="71f33-183">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - détails du produit](introduction-and-overview/_static/image3.png)

<span data-ttu-id="71f33-185">En tant qu’utilisateur, vous pouvez inscrire et connectez-vous à l’aide de la fonctionnalité par défaut du modèle Web Forms.</span><span class="sxs-lookup"><span data-stu-id="71f33-185">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="71f33-186">Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant.</span><span class="sxs-lookup"><span data-stu-id="71f33-186">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="71f33-187">En outre, vous pouvez vous connecter en tant que l’administrateur pour ajouter et supprimer des produits à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="71f33-187">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - connectez-vous](introduction-and-overview/_static/image4.png)

<span data-ttu-id="71f33-189">Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et de validation avec PayPal.</span><span class="sxs-lookup"><span data-stu-id="71f33-189">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="71f33-190">Notez que cet exemple d’application est conçue pour fonctionner avec sandbox du développeur de PayPal.</span><span class="sxs-lookup"><span data-stu-id="71f33-190">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="71f33-191">Aucune transaction argent n’aura lieu.</span><span class="sxs-lookup"><span data-stu-id="71f33-191">No actual money transaction will take place.</span></span>

![Wingtip Toys - panier d’achat](introduction-and-overview/_static/image5.png)

<span data-ttu-id="71f33-193">PayPal confirmera votre compte, l’ordre et les informations de paiement.</span><span class="sxs-lookup"><span data-stu-id="71f33-193">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="71f33-195">Après le renvoi de PayPal, vous pouvez consulter et terminer votre commande.</span><span class="sxs-lookup"><span data-stu-id="71f33-195">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - ordre révision](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="71f33-197">Prérequis</span><span class="sxs-lookup"><span data-stu-id="71f33-197">Prerequisites</span></span>

<span data-ttu-id="71f33-198">Avant de commencer, assurez-vous que vous avez installé sur votre ordinateur les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="71f33-198">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="71f33-199">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="71f33-199">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="71f33-200">Le .NET Framework est installé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="71f33-200">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="71f33-201">Cette série de didacticiels utilise Microsoft Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="71f33-201">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="71f33-202">Vous pouvez utiliser soit Microsoft Visual Studio Express 2013 pour le Web ou Microsoft Visual Studio 2013 pour terminer cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="71f33-202">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="71f33-203">Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web seront souvent être appelé Visual Studio tout au long de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="71f33-203">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="71f33-204">Si vous avez déjà installée une version de Visual Studio, le processus d’installation installe Visual Studio 2013 ou Microsoft Visual Studio Express 2013 pour le Web en regard de la version existante.</span><span class="sxs-lookup"><span data-stu-id="71f33-204">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="71f33-205">Les sites que vous avez créé dans les versions antérieures peuvent être ouverts dans Visual Studio 2013 et continuent à ouvrir dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="71f33-205">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="71f33-206">Cette procédure pas à pas suppose que vous avez choisi le *développement Web* collection de paramètres de la première fois que vous avez démarré Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71f33-206">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="71f33-207">Pour plus d’informations, consultez [Comment : sélectionner des paramètres d’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="71f33-207">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="71f33-208">Télécharger l’exemple d’Application</span><span class="sxs-lookup"><span data-stu-id="71f33-208">Download the Sample Application</span></span>

<span data-ttu-id="71f33-209">Après avoir installé les composants requis, vous êtes prêt à commencer à créer le projet Web est présenté dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="71f33-209">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="71f33-210">Si vous souhaitez **éventuellement** exécuter l’exemple d’application qui crée cette série de didacticiels, vous pouvez le télécharger à partir du site d’exemples MSDN.</span><span class="sxs-lookup"><span data-stu-id="71f33-210">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="71f33-211">Ce téléchargement contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="71f33-211">This download contains the following:</span></span>

- <span data-ttu-id="71f33-212">L’exemple d’application dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="71f33-212">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="71f33-213">Les ressources utilisées pour créer l’exemple d’application dans le *WingtipToys-ressources* dossier dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="71f33-213">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="71f33-214">Téléchargez le fichier à partir du site d’exemples de MSDN :</span><span class="sxs-lookup"><span data-stu-id="71f33-214">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="71f33-215">[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="71f33-215">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="71f33-216">Le téléchargement est un <em>.zip</em> fichier.</span><span class="sxs-lookup"><span data-stu-id="71f33-216">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="71f33-217">Pour voir le projet terminé qui crée cette série de didacticiels, recherchez et sélectionnez le <em>c#</em>dossier dans le <em>.zip</em> fichier.</span><span class="sxs-lookup"><span data-stu-id="71f33-217">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="71f33-218">Enregistrer le <em>c#</em> dossier dans le dossier que vous utilisez pour travailler avec des projets Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="71f33-218">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="71f33-219">Par défaut, le dossier de projets Visual Studio 2013 est le suivant :</span><span class="sxs-lookup"><span data-stu-id="71f33-219">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="71f33-220"><strong>C:\Users&#92;</strong><strong><em>&lt;nom d’utilisateur&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="71f33-220"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="71f33-221">Renommer le ***c#*** dossier ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="71f33-221">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="71f33-222">Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de renommer le *c#* dossier *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="71f33-222">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="71f33-223">Pour exécuter le projet terminé, ouvrez le *WingtipToys* dossier et double-cliquez sur le *WingtipToys.sln* fichier.</span><span class="sxs-lookup"><span data-stu-id="71f33-223">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="71f33-224">Visual Studio 2013 s’ouvre le projet.</span><span class="sxs-lookup"><span data-stu-id="71f33-224">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="71f33-225">Ensuite, cliquez sur le *Default.aspx* fichier dans la fenêtre Explorateur de solutions et cliquez sur Afficher dans le navigateur dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="71f33-225">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="71f33-226">Commentaires et didacticiel prise en charge</span><span class="sxs-lookup"><span data-stu-id="71f33-226">Tutorial Support and Comments</span></span>

<span data-ttu-id="71f33-227">Utilisez la section de questions-réponses incluse avec le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) exemple (c#) pour vos questions ou commentaires.</span><span class="sxs-lookup"><span data-stu-id="71f33-227">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="71f33-228">Commentaires sur cette série de didacticiels sont les bienvenus, et lorsque la mise à jour de cette série de didacticiels chaque effort sera fait tenir des corrections de compte ou des suggestions d’améliorations qui sont fournies dans les commentaires de didacticiel.</span><span class="sxs-lookup"><span data-stu-id="71f33-228">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="71f33-229">Quand une erreur se produit pendant le développement, ou si le site Web ne s’exécute pas correctement, les messages d’erreur peuvent donner des indices complexes à la source du problème ou ne peuvent pas expliquer comment la corriger.</span><span class="sxs-lookup"><span data-stu-id="71f33-229">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="71f33-230">Pour vous aider avec quelques scénarios courants de problème, vous pouvez également utiliser le [forums ASP.NET](https://forums.asp.net/) ou la section de questions-réponses inclus avec le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) exemple.</span><span class="sxs-lookup"><span data-stu-id="71f33-230">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="71f33-231">Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez les didacticiels, veillez à vérifier les emplacements ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="71f33-231">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="71f33-232">Next</span><span class="sxs-lookup"><span data-stu-id="71f33-232">Next</span></span>](create-the-project.md)
