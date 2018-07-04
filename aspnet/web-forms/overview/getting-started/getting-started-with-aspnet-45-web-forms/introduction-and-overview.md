---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous présente les principes de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 55e7a5e8c0c7df9597f8597cba49d33da23e9159
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365404"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="85146-103">Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85146-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="85146-104">par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="85146-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="85146-105">[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="85146-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="85146-106">Cette série de didacticiels pas à pas vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="85146-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="85146-107">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="85146-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="85146-108">Testez vos connaissances et approfondir les concepts clés en prenant le questionnaire d’ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="85146-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="85146-109">Ce questionnaire a été spécifiquement conçu de contenu de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85146-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="85146-110">Chaque question du test fournit une explication ainsi que des liens vers des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="85146-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="85146-111">Introduction</span><span class="sxs-lookup"><span data-stu-id="85146-111">Introduction</span></span>

<span data-ttu-id="85146-112">Cette série de didacticiels vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET à l’aide de Visual Studio Express 2013 pour le Web et ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="85146-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="85146-113">L’application que vous allez créer est nommée **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="85146-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="85146-114">Il est un exemple simplifié d’un site web avant de magasin qui vend des éléments en ligne.</span><span class="sxs-lookup"><span data-stu-id="85146-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="85146-115">Cette série de didacticiels met en évidence les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="85146-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="85146-116">Commentaires sont les bienvenus, et nous allons s’efforcer de mettre à jour cette série de didacticiels en fonction de vos suggestions.</span><span class="sxs-lookup"><span data-stu-id="85146-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="85146-117">Projet de téléchargement terminé</span><span class="sxs-lookup"><span data-stu-id="85146-117">Download completed project</span></span>

<span data-ttu-id="85146-118">Vous pouvez télécharger un projet c# qui contient la fin du didacticiel.</span><span class="sxs-lookup"><span data-stu-id="85146-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="85146-119">[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="85146-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="85146-120">Passez en revue le contenu en prenant le questionnaire de Web Forms ASP.NET connexe</span><span class="sxs-lookup"><span data-stu-id="85146-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="85146-121">Après avoir terminé ce didacticiel, testez vos connaissances et approfondir les concepts clés en prenant le [ASP.NET Web Forms questionnaire](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="85146-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="85146-122">Ce questionnaire a été spécifiquement conçu de contenu de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85146-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="85146-123">Chaque question du test fournit une explication ainsi que des liens vers des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="85146-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="85146-124">Web Forms ASP.NET questionnaire</span><span class="sxs-lookup"><span data-stu-id="85146-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="85146-125">Public</span><span class="sxs-lookup"><span data-stu-id="85146-125">Audience</span></span>

<span data-ttu-id="85146-126">Est de cette série de didacticiels destiné aux développeurs expérimentés qui débutent avec ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="85146-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="85146-127">Un développeur intéressé à cette série de didacticiels doit avoir les compétences suivantes :</span><span class="sxs-lookup"><span data-stu-id="85146-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="85146-128">Familiarisés avec un objet orientée langage de programmation (OOP)</span><span class="sxs-lookup"><span data-stu-id="85146-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="85146-129">Maîtriser les concepts de développement Web (HTML, CSS et JavaScript)</span><span class="sxs-lookup"><span data-stu-id="85146-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="85146-130">Maîtriser les concepts de base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="85146-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="85146-131">Maîtriser les concepts d’architecture multiniveau</span><span class="sxs-lookup"><span data-stu-id="85146-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="85146-132">Si vous souhaitez passer en revue les éléments mentionnés ci-dessus, examinez le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="85146-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="85146-133">Mise en route avec Visual c#</span><span class="sxs-lookup"><span data-stu-id="85146-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="85146-134">[Développement Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="85146-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="85146-135">Base de données relationnelle</span><span class="sxs-lookup"><span data-stu-id="85146-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="85146-136">Architecture à plusieurs niveaux</span><span class="sxs-lookup"><span data-stu-id="85146-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="85146-137">Fonctionnalités de l’application</span><span class="sxs-lookup"><span data-stu-id="85146-137">Application Features</span></span>

<span data-ttu-id="85146-138">Les fonctionnalités de Web Form ASP.NET présentées dans cette série incluent :</span><span class="sxs-lookup"><span data-stu-id="85146-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="85146-139">Le projet d’Application Web (pas le projet de Site Web)</span><span class="sxs-lookup"><span data-stu-id="85146-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="85146-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="85146-140">Web Forms</span></span>
- <span data-ttu-id="85146-141">Pages maîtres, Configuration</span><span class="sxs-lookup"><span data-stu-id="85146-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="85146-142">Programme d’amorçage</span><span class="sxs-lookup"><span data-stu-id="85146-142">Bootstrap</span></span>
- <span data-ttu-id="85146-143">Entity Framework Code First, base de données locale</span><span class="sxs-lookup"><span data-stu-id="85146-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="85146-144">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="85146-144">Request Validation</span></span>
- <span data-ttu-id="85146-145">Contrôles de données fortement typés liaison, les Annotations de données, de modèle et que la valeur des fournisseurs</span><span class="sxs-lookup"><span data-stu-id="85146-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="85146-146">SSL et OAuth</span><span class="sxs-lookup"><span data-stu-id="85146-146">SSL and OAuth</span></span>
- <span data-ttu-id="85146-147">ASP.NET Identity, Configuration et autorisation</span><span class="sxs-lookup"><span data-stu-id="85146-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="85146-148">Validation non obstrusive</span><span class="sxs-lookup"><span data-stu-id="85146-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="85146-149">Routage</span><span class="sxs-lookup"><span data-stu-id="85146-149">Routing</span></span>
- <span data-ttu-id="85146-150">Gestion des erreurs ASP.NET</span><span class="sxs-lookup"><span data-stu-id="85146-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="85146-151">Tâches et des scénarios d’application</span><span class="sxs-lookup"><span data-stu-id="85146-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="85146-152">Tâches décrites dans cette série sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="85146-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="85146-153">Création, la révision et le nouveau projet en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="85146-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="85146-154">Création de la structure de base de données</span><span class="sxs-lookup"><span data-stu-id="85146-154">Creating the database structure</span></span>
- <span data-ttu-id="85146-155">L’initialisation et l’amorçage de la base de données</span><span class="sxs-lookup"><span data-stu-id="85146-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="85146-156">Personnalisation de l’interface utilisateur à l’aide de styles, de graphiques et d’une page maître</span><span class="sxs-lookup"><span data-stu-id="85146-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="85146-157">Ajout de pages et la navigation</span><span class="sxs-lookup"><span data-stu-id="85146-157">Adding pages and navigation</span></span>
- <span data-ttu-id="85146-158">Affichage des détails de menu et des données de produit</span><span class="sxs-lookup"><span data-stu-id="85146-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="85146-159">Création d’un panier d’achat</span><span class="sxs-lookup"><span data-stu-id="85146-159">Creating a shopping cart</span></span>
- <span data-ttu-id="85146-160">Prise en charge SSL Ajout et OAuth</span><span class="sxs-lookup"><span data-stu-id="85146-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="85146-161">Ajout d’une méthode de paiement</span><span class="sxs-lookup"><span data-stu-id="85146-161">Adding a payment method</span></span>
- <span data-ttu-id="85146-162">Y compris un rôle d’administrateur et un utilisateur à l’application</span><span class="sxs-lookup"><span data-stu-id="85146-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="85146-163">Restreindre l’accès à des pages spécifiques et de dossier</span><span class="sxs-lookup"><span data-stu-id="85146-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="85146-164">Téléchargement d’un fichier à l’application web</span><span class="sxs-lookup"><span data-stu-id="85146-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="85146-165">Implémentation de la validation des entrées</span><span class="sxs-lookup"><span data-stu-id="85146-165">Implementing input validation</span></span>
- <span data-ttu-id="85146-166">Inscription des itinéraires pour l’application web</span><span class="sxs-lookup"><span data-stu-id="85146-166">Registering routes for the web application</span></span>
- <span data-ttu-id="85146-167">Implémentation de la gestion des erreurs et journalisation des erreurs</span><span class="sxs-lookup"><span data-stu-id="85146-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="85146-168">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="85146-168">Overview</span></span>

<span data-ttu-id="85146-169">Si vous débutez avec ASP.NET Web Forms, mais êtes familiarisé avec les concepts de programmation, vous avez le didacticiel de droite.</span><span class="sxs-lookup"><span data-stu-id="85146-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="85146-170">Si vous êtes déjà familiarisé avec ASP.NET Web Forms, vous pouvez bénéficier de cette série de didacticiels par les nouvelles fonctionnalités disponibles dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="85146-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="85146-171">Si vous n’êtes pas familiarisé avec les concepts de programmation et ASP.NET Web Forms, consultez les didacticiels supplémentaires fournies dans les formulaires Web [mise en route](../../../index.md) section sur le site Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="85146-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="85146-172">Spécifique au **dernière** ASP.NET 4.5 fonctionnalités fournies dans les formulaires Web cette série de didacticiels inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="85146-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="85146-173">Une interface utilisateur simple pour la création de projets de cette offre [prennent en charge plusieurs infrastructures ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).</span><span class="sxs-lookup"><span data-stu-id="85146-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="85146-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), une infrastructure de mise en page et des thèmes qui fournit des fonctionnalités de création et de thèmes réactives.</span><span class="sxs-lookup"><span data-stu-id="85146-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="85146-175">[ASP.NET Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.NET qui fonctionne de la même dans toutes les infrastructures ASP.NET et fonctionne avec des logiciels autres que IIS d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="85146-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="85146-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), une mise à jour Entity Framework qui vous permet de récupérez et manipuler des données de manière fortement typée d’objets, accéder aux données de façon asynchrone, gérer les défaillances temporaires de connexion et consigner les instructions SQL.</span><span class="sxs-lookup"><span data-stu-id="85146-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="85146-177">Pour obtenir une liste complète des fonctionnalités de ASP.NET 4.5, consultez [ASP.NET et Web Tools pour Visual Studio 2013 Release Notes de publication](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="85146-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="85146-178">L’exemple d’Application Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="85146-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="85146-179">Les captures d’écran suivantes fournissent un aperçu rapide de l’application Web forms ASP.NET que vous allez créer dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85146-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="85146-180">Lorsque vous exécutez l’application à partir de Visual Studio Express 2013 pour le Web, vous verrez la page d’accueil web suivantes.</span><span class="sxs-lookup"><span data-stu-id="85146-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - page par défaut](introduction-and-overview/_static/image1.png)

<span data-ttu-id="85146-182">Vous pouvez enregistrer en tant qu’un nouvel utilisateur, ou connectez-vous en tant qu’un utilisateur existant.</span><span class="sxs-lookup"><span data-stu-id="85146-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="85146-183">Navigation est fournie en haut de chaque catégorie de produits en récupérant les produits disponibles à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="85146-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="85146-184">En sélectionnant le lien de produits, vous serez en mesure de voir une liste de tous les produits disponibles.</span><span class="sxs-lookup"><span data-stu-id="85146-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - produits](introduction-and-overview/_static/image2.png)

<span data-ttu-id="85146-186">Vous pouvez également voir les détails du produit individuels en sélectionnant un des produits répertoriés.</span><span class="sxs-lookup"><span data-stu-id="85146-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - détails du produit](introduction-and-overview/_static/image3.png)

<span data-ttu-id="85146-188">En tant qu’utilisateur, vous pouvez inscrire et connectez-vous à l’aide de la fonctionnalité par défaut du modèle Web Forms.</span><span class="sxs-lookup"><span data-stu-id="85146-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="85146-189">Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant.</span><span class="sxs-lookup"><span data-stu-id="85146-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="85146-190">En outre, vous pouvez vous connecter en tant que l’administrateur pour ajouter et supprimer des produits à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="85146-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - connectez-vous](introduction-and-overview/_static/image4.png)

<span data-ttu-id="85146-192">Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et de validation avec PayPal.</span><span class="sxs-lookup"><span data-stu-id="85146-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="85146-193">Notez que cet exemple d’application est conçue pour fonctionner avec sandbox du développeur de PayPal.</span><span class="sxs-lookup"><span data-stu-id="85146-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="85146-194">Aucune transaction argent n’aura lieu.</span><span class="sxs-lookup"><span data-stu-id="85146-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - panier d’achat](introduction-and-overview/_static/image5.png)

<span data-ttu-id="85146-196">PayPal confirmera votre compte, l’ordre et les informations de paiement.</span><span class="sxs-lookup"><span data-stu-id="85146-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="85146-198">Après le renvoi de PayPal, vous pouvez consulter et terminer votre commande.</span><span class="sxs-lookup"><span data-stu-id="85146-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - ordre révision](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="85146-200">Prérequis</span><span class="sxs-lookup"><span data-stu-id="85146-200">Prerequisites</span></span>

<span data-ttu-id="85146-201">Avant de commencer, assurez-vous que vous avez installé sur votre ordinateur les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="85146-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="85146-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="85146-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="85146-203">Le .NET Framework est installé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="85146-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="85146-204">Cette série de didacticiels utilise Microsoft Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="85146-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="85146-205">Vous pouvez utiliser soit Microsoft Visual Studio Express 2013 pour le Web ou Microsoft Visual Studio 2013 pour terminer cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85146-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="85146-206">Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web seront souvent être appelé Visual Studio tout au long de cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85146-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="85146-207">Si vous avez déjà installée une version de Visual Studio, le processus d’installation installe Visual Studio 2013 ou Microsoft Visual Studio Express 2013 pour le Web en regard de la version existante.</span><span class="sxs-lookup"><span data-stu-id="85146-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="85146-208">Les sites que vous avez créé dans les versions antérieures peuvent être ouverts dans Visual Studio 2013 et continuent à ouvrir dans les versions précédentes.</span><span class="sxs-lookup"><span data-stu-id="85146-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="85146-209">Cette procédure pas à pas suppose que vous avez choisi le *développement Web* collection de paramètres de la première fois que vous avez démarré Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="85146-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="85146-210">Pour plus d’informations, consultez [Comment : sélectionner des paramètres d’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="85146-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="85146-211">Télécharger l’exemple d’Application</span><span class="sxs-lookup"><span data-stu-id="85146-211">Download the Sample Application</span></span>

<span data-ttu-id="85146-212">Après avoir installé les composants requis, vous êtes prêt à commencer à créer le projet Web est présenté dans cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="85146-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="85146-213">Si vous souhaitez **éventuellement** exécuter l’exemple d’application qui crée cette série de didacticiels, vous pouvez le télécharger à partir du site d’exemples MSDN.</span><span class="sxs-lookup"><span data-stu-id="85146-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="85146-214">Ce téléchargement contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="85146-214">This download contains the following:</span></span>

- <span data-ttu-id="85146-215">L’exemple d’application dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="85146-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="85146-216">Les ressources utilisées pour créer l’exemple d’application dans le *WingtipToys-ressources* dossier dans le *WingtipToys* dossier.</span><span class="sxs-lookup"><span data-stu-id="85146-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="85146-217">Téléchargez le fichier à partir du site d’exemples de MSDN :</span><span class="sxs-lookup"><span data-stu-id="85146-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="85146-218">[Bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="85146-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="85146-219">Le téléchargement est un <em>.zip</em> fichier.</span><span class="sxs-lookup"><span data-stu-id="85146-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="85146-220">Pour voir le projet terminé qui crée cette série de didacticiels, recherchez et sélectionnez le <em>c#</em>dossier dans le <em>.zip</em> fichier.</span><span class="sxs-lookup"><span data-stu-id="85146-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="85146-221">Enregistrer le <em>c#</em> dossier dans le dossier que vous utilisez pour travailler avec des projets Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="85146-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="85146-222">Par défaut, le dossier de projets Visual Studio 2013 est le suivant :</span><span class="sxs-lookup"><span data-stu-id="85146-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="85146-223"><strong>C:\Users&#92;</strong><strong><em>&lt;nom d’utilisateur&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="85146-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="85146-224">Renommer le ***c#*** dossier ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="85146-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="85146-225">Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de renommer le *c#* dossier *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="85146-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="85146-226">Pour exécuter le projet terminé, ouvrez le *WingtipToys* dossier et double-cliquez sur le *WingtipToys.sln* fichier.</span><span class="sxs-lookup"><span data-stu-id="85146-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="85146-227">Visual Studio 2013 s’ouvre le projet.</span><span class="sxs-lookup"><span data-stu-id="85146-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="85146-228">Ensuite, cliquez sur le *Default.aspx* fichier dans la fenêtre Explorateur de solutions et cliquez sur Afficher dans le navigateur dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="85146-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="85146-229">Commentaires et didacticiel prise en charge</span><span class="sxs-lookup"><span data-stu-id="85146-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="85146-230">Utilisez la section de questions-réponses incluse avec le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) exemple (c#) pour vos questions ou commentaires.</span><span class="sxs-lookup"><span data-stu-id="85146-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="85146-231">Commentaires sur cette série de didacticiels sont les bienvenus, et lorsque la mise à jour de cette série de didacticiels chaque effort sera fait tenir des corrections de compte ou des suggestions d’améliorations qui sont fournies dans les commentaires de didacticiel.</span><span class="sxs-lookup"><span data-stu-id="85146-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="85146-232">Quand une erreur se produit pendant le développement, ou si le site Web ne s’exécute pas correctement, les messages d’erreur peuvent donner des indices complexes à la source du problème ou ne peuvent pas expliquer comment la corriger.</span><span class="sxs-lookup"><span data-stu-id="85146-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="85146-233">Pour vous aider avec quelques scénarios courants de problème, vous pouvez également utiliser le [forums ASP.NET](https://forums.asp.net/) ou la section de questions-réponses inclus avec le [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) exemple.</span><span class="sxs-lookup"><span data-stu-id="85146-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="85146-234">Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez les didacticiels, veillez à vérifier les emplacements ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="85146-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="85146-235">Next</span><span class="sxs-lookup"><span data-stu-id="85146-235">Next</span></span>](create-the-project.md)
