---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: "Partie 1 : Vue d’ensemble et création du projet | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 47af34c72f1959756f5d68e0e80052e700c7b19c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="babd1-102">Partie 1 : Vue d’ensemble et création du projet</span><span class="sxs-lookup"><span data-stu-id="babd1-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="babd1-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="babd1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="babd1-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="babd1-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="babd1-105">Entity Framework est une infrastructure de mappage objet/relationnel.</span><span class="sxs-lookup"><span data-stu-id="babd1-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="babd1-106">Il mappe les objets de domaine dans votre code à des entités dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="babd1-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="babd1-107">Dans la plupart des cas, il est inutile à vous soucier de la couche de base de données, car l’Entity Framework prend en charge pour vous.</span><span class="sxs-lookup"><span data-stu-id="babd1-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="babd1-108">Votre code manipule les objets et les modifications sont conservées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="babd1-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="babd1-109">À propos du didacticiel</span><span class="sxs-lookup"><span data-stu-id="babd1-109">About the Tutorial</span></span>

<span data-ttu-id="babd1-110">Dans ce didacticiel, vous allez créer une application simple de magasin.</span><span class="sxs-lookup"><span data-stu-id="babd1-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="babd1-111">Il existe deux éléments principaux à l’application.</span><span class="sxs-lookup"><span data-stu-id="babd1-111">There are two main parts to the application.</span></span> <span data-ttu-id="babd1-112">Les utilisateurs normaux peuvent afficher les produits et créer des commandes :</span><span class="sxs-lookup"><span data-stu-id="babd1-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="babd1-113">Les administrateurs peuvent créer, supprimer ou modifier des produits :</span><span class="sxs-lookup"><span data-stu-id="babd1-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="babd1-114">Vous allez apprendre des compétences</span><span class="sxs-lookup"><span data-stu-id="babd1-114">Skills You'll Learn</span></span>

<span data-ttu-id="babd1-115">Voici ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="babd1-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="babd1-116">L’utilisation d’Entity Framework avec l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="babd1-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="babd1-117">L’utilisation de knockout.js pour créer un interface utilisateur du client dynamique.</span><span class="sxs-lookup"><span data-stu-id="babd1-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="babd1-118">Comment utiliser l’authentification par formulaire avec l’API Web pour authentifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="babd1-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="babd1-119">Bien que ce didacticiel est autonome, vous pouvez souhaiter lisez d’abord les didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="babd1-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="babd1-120">Votre première API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="babd1-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="babd1-121">Création d’une API Web qui prend en charge les opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="babd1-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="babd1-122">Connaissance de [ASP.NET MVC](../../../../mvc/index.md) s’avère également utile.</span><span class="sxs-lookup"><span data-stu-id="babd1-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="babd1-123">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="babd1-123">Overview</span></span>

<span data-ttu-id="babd1-124">À un niveau élevé, voici l’architecture de l’application :</span><span class="sxs-lookup"><span data-stu-id="babd1-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="babd1-125">ASP.NET MVC génère les pages HTML pour le client.</span><span class="sxs-lookup"><span data-stu-id="babd1-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="babd1-126">API Web ASP.NET expose les opérations CRUD sur les données (products et orders).</span><span class="sxs-lookup"><span data-stu-id="babd1-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="babd1-127">Entity Framework traduit en c# modèles utilisés par l’API Web dans des entités de base de données.</span><span class="sxs-lookup"><span data-stu-id="babd1-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="babd1-128">Le diagramme suivant illustre la façon dont les objets de domaine sont représentées de différentes couches de l’application : la couche de base de données, le modèle objet et enfin le format de transmission, ce qui est utilisé pour transmettre des données au client via le protocole HTTP.</span><span class="sxs-lookup"><span data-stu-id="babd1-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="babd1-129">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="babd1-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="babd1-130">Vous pouvez créer le projet du didacticiel à l’aide de Visual Web Developer Express ou la version complète de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="babd1-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="babd1-131">À partir de la **Démarrer** , cliquez sur **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="babd1-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="babd1-132">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="babd1-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="babd1-133">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="babd1-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="babd1-134">Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="babd1-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="babd1-135">Nommez le projet « ProductStore » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="babd1-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="babd1-136">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Application Internet** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="babd1-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="babd1-137">Le modèle « Application Internet » crée une application ASP.NET MVC qui prend en charge l’authentification par formulaire.</span><span class="sxs-lookup"><span data-stu-id="babd1-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="babd1-138">Si vous exécutez l’application maintenant, elle contient déjà des fonctionnalités :</span><span class="sxs-lookup"><span data-stu-id="babd1-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="babd1-139">Nouveaux utilisateurs peuvent inscrire en cliquant sur le lien « Enregistrer » dans le coin supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="babd1-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="babd1-140">Les utilisateurs inscrits peuvent se connecter en cliquant sur le lien « Connectez-vous ».</span><span class="sxs-lookup"><span data-stu-id="babd1-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="babd1-141">Informations d’appartenance sont conservées dans une base de données est créé automatiquement.</span><span class="sxs-lookup"><span data-stu-id="babd1-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="babd1-142">Pour plus d’informations sur l’authentification par formulaire dans ASP.NET MVC, consultez [procédure pas à pas : à l’aide de l’authentification par formulaire dans ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="babd1-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="babd1-143">Mettre à jour le fichier CSS</span><span class="sxs-lookup"><span data-stu-id="babd1-143">Update the CSS File</span></span>

<span data-ttu-id="babd1-144">Cette étape est esthétique, mais il effectuera les pages de rendu, comme les captures d’écran précédentes.</span><span class="sxs-lookup"><span data-stu-id="babd1-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="babd1-145">Dans l’Explorateur de solutions, développez le dossier de contenu et d’ouvrir le fichier Site.css.</span><span class="sxs-lookup"><span data-stu-id="babd1-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="babd1-146">Ajoutez les styles CSS suivants :</span><span class="sxs-lookup"><span data-stu-id="babd1-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

>[!div class="step-by-step"]
[<span data-ttu-id="babd1-147">Next</span><span class="sxs-lookup"><span data-stu-id="babd1-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
