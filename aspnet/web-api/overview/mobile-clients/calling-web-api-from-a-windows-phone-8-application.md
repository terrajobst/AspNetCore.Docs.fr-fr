---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Appel des API Web à partir d’un Windows Phone 8 d’Application (c#) | Documents Microsoft
author: rmcmurray
description: Créer un scénario complet de bout en bout consistant en une application ASP.NET Web API qui fournit un catalogue de livres à une application Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="fc522-103">Appel d’API Web à partir d’une Application de 8 Windows Phone (c#)</span><span class="sxs-lookup"><span data-stu-id="fc522-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="fc522-104">par [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="fc522-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="fc522-105">Dans ce didacticiel, vous allez apprendre comment créer un scénario complet de bout en bout consistant en une application ASP.NET Web API qui fournit un catalogue de livres à une application Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="fc522-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="fc522-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="fc522-106">Overview</span></span>

<span data-ttu-id="fc522-107">Les services rESTful comme API Web ASP.NET simplifient la création d’applications HTTP pour les développeurs en faisant abstraction de l’architecture pour les applications côté client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="fc522-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="fc522-108">Au lieu de créer un protocole propriétaire basé sur un socket pour la communication, les développeurs Web API souhaitent publier les méthodes HTTP requis pour leur application, (par exemple : GET, POST, PUT, DELETE), et les développeurs d’applications client suffit à consommer les méthodes HTTP qui sont nécessaires à leur application.</span><span class="sxs-lookup"><span data-stu-id="fc522-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="fc522-109">Dans ce didacticiel de bout en bout, vous allez apprendre à utiliser des API Web pour créer les projets suivants :</span><span class="sxs-lookup"><span data-stu-id="fc522-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="fc522-110">Dans le [première partie de ce didacticiel](#STEP1), vous allez créer une application de l’API Web ASP.NET qui prend en charge toutes les opérations de création, lecture, mise à jour et supprimer (CRUD) pour gérer un catalogue de livres.</span><span class="sxs-lookup"><span data-stu-id="fc522-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="fc522-111">Cette application doit utiliser le [exemple de fichier XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) à partir de MSDN.</span><span class="sxs-lookup"><span data-stu-id="fc522-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="fc522-112">Dans le [deuxième partie de ce didacticiel](#STEP2), vous allez créer une application Windows Phone 8 interactive qui extrait les données de votre application d’API Web.</span><span class="sxs-lookup"><span data-stu-id="fc522-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="fc522-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="fc522-113">Prerequisites</span></span>

- <span data-ttu-id="fc522-114">Visual Studio 2013 avec le Kit de développement Windows Phone 8 installé</span><span class="sxs-lookup"><span data-stu-id="fc522-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="fc522-115">Windows 8 ou version ultérieure sur un système 64 bits avec Hyper-V est installé</span><span class="sxs-lookup"><span data-stu-id="fc522-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="fc522-116">Pour obtenir la liste des exigences supplémentaires, consultez le *requise* section sur le [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) page de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="fc522-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="fc522-117">Si vous vous apprêtez à tester la connectivité entre l’API Web et les projets Windows Phone 8 sur votre système local, vous devez suivre les instructions fournies dans le *[la connexion de l’émulateur de 8 Windows Phone pour les Applications de l’API Web sur un ordinateur Local Ordinateur](https://go.microsoft.com/fwlink/?LinkId=324014)* article pour configurer votre environnement de test.</span><span class="sxs-lookup"><span data-stu-id="fc522-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="fc522-118">Étape 1 : Création du projet de librairie d’API Web</span><span class="sxs-lookup"><span data-stu-id="fc522-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="fc522-119">La première étape de ce didacticiel de bout en bout consiste à créer un projet d’API Web qui prend en charge toutes les opérations CRUD ; Notez que vous allez ajouter le projet d’application Windows Phone à cette solution dans [étape 2](#STEP2) de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="fc522-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="fc522-120">Ouvrez **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="fc522-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="fc522-121">Cliquez sur **fichier**, puis **nouveau**, puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="fc522-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="fc522-122">Lorsque le **nouveau projet** boîte de dialogue, développez **installé**, puis **modèles**, puis **Visual C#**, puis **Web**.</span><span class="sxs-lookup"><span data-stu-id="fc522-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="fc522-123">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="fc522-124">Mettez en surbrillance **Application Web ASP.NET**, entrez **BookStore** pour le nom du projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc522-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="fc522-125">Lorsque le **nouveau projet ASP.NET** boîte de dialogue est affichée, sélectionnez le **API Web** modèle, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc522-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="fc522-126">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="fc522-127">Lorsque le projet d’API Web s’ouvre, supprimez le contrôleur de l’exemple du projet :</span><span class="sxs-lookup"><span data-stu-id="fc522-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="fc522-128">Développez le **contrôleurs** dossier dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="fc522-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="fc522-129">Cliquez sur le **le fichier ValuesController.cs** de fichiers, puis cliquez sur **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="fc522-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="fc522-130">Cliquez sur **OK** lorsque vous êtes invité à confirmer la suppression.</span><span class="sxs-lookup"><span data-stu-id="fc522-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="fc522-131">Ajouter un fichier de données XML pour le projet d’API Web ; Ce fichier contient le contenu du catalogue bookstore :</span><span class="sxs-lookup"><span data-stu-id="fc522-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="fc522-132">Avec le bouton droit le **application\_données** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="fc522-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="fc522-133">Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, mettez en surbrillance le **fichier XML** modèle.</span><span class="sxs-lookup"><span data-stu-id="fc522-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="fc522-134">Nommez le fichier **Books.xml**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc522-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="fc522-135">Lorsque le **Books.xml** fichier est ouvert, remplacez le code dans le fichier par le code XML à partir de l’exemple **books.xml** fichier sur MSDN :</span><span class="sxs-lookup"><span data-stu-id="fc522-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="fc522-136">Enregistrez et fermez le fichier XML.</span><span class="sxs-lookup"><span data-stu-id="fc522-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="fc522-137">Ajouter le modèle bookstore au projet d’API Web ; Ce modèle contient la logique de création, lecture, mise à jour et supprimer (CRUD) pour l’application bookstore :</span><span class="sxs-lookup"><span data-stu-id="fc522-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="fc522-138">Avec le bouton droit le **modèles** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="fc522-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="fc522-139">Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc522-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="fc522-140">Lorsque le **BookDetails.cs** fichier est ouvert, remplacez le code dans le fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fc522-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="fc522-141">Enregistrez et fermez le **BookDetails.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="fc522-142">Ajouter le contrôleur bookstore au projet d’API Web :</span><span class="sxs-lookup"><span data-stu-id="fc522-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="fc522-143">Avec le bouton droit le **contrôleurs** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="fc522-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="fc522-144">Lorsque le **ajouter une vue de structure** boîte de dialogue s’affiche, mettez en surbrillance **Web vide - contrôleur 2 d’API**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc522-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="fc522-145">Lorsque le **ajouter un contrôleur** boîte de dialogue s’affiche, nommez le contrôleur **BooksController**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc522-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="fc522-146">Lorsque le **BooksController.cs** fichier est ouvert, remplacez le code dans le fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fc522-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="fc522-147">Enregistrez et fermez le **BooksController.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="fc522-148">Générez l’application API Web pour rechercher les erreurs.</span><span class="sxs-lookup"><span data-stu-id="fc522-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="fc522-149">Étape 2 : Ajout de projet Windows Phone 8 Bookstore catalogue</span><span class="sxs-lookup"><span data-stu-id="fc522-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="fc522-150">Est de l’étape suivante de ce scénario de bout en bout pour créer l’application de catalogue pour Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="fc522-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="fc522-151">Cette application doit utiliser le *Windows Phone Databound application* pour l’interface utilisateur par défaut et qu’il utilisera l’application API Web que vous avez créé dans [étape 1](#STEP1) de ce didacticiel en tant que la source de données.</span><span class="sxs-lookup"><span data-stu-id="fc522-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="fc522-152">Avec le bouton droit le **BookStore** solution dans le dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="fc522-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="fc522-153">Lorsque le **nouveau projet** boîte de dialogue, développez **installé**, puis **Visual C#**, puis **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="fc522-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="fc522-154">Mettez en surbrillance **Windows Phone Databound application**, entrez **BookCatalog** pour le nom, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc522-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="fc522-155">Ajouter le package NuGet de Json.NET à le **BookCatalog** projet :</span><span class="sxs-lookup"><span data-stu-id="fc522-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="fc522-156">Avec le bouton droit **références** pour le **BookCatalog** de projet dans l’Explorateur de solutions, puis cliquez sur **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fc522-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="fc522-157">Lorsque le **gérer les Packages NuGet** boîte de dialogue, développez le **Online** section et mettre en surbrillance **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="fc522-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="fc522-158">Entrez **Json.NET** dans la recherche de champ et cliquez sur l’icône de recherche.</span><span class="sxs-lookup"><span data-stu-id="fc522-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="fc522-159">Mettez en surbrillance **Json.NET** dans les résultats de la recherche, puis cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="fc522-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="fc522-160">Une fois l’installation terminée, cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="fc522-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="fc522-161">Ajouter le **BookDetails** modèle pour le **BookCatalog** projet ; contient un modèle générique de la classe bookstore :</span><span class="sxs-lookup"><span data-stu-id="fc522-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="fc522-162">Avec le bouton droit le **BookCatalog** de projet dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="fc522-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="fc522-163">Nommez le nouveau dossier **modèles**.</span><span class="sxs-lookup"><span data-stu-id="fc522-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="fc522-164">Avec le bouton droit le **modèles** dossier dans l’Explorateur de solutions, puis cliquez sur **ajouter**, puis cliquez sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="fc522-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="fc522-165">Lorsque le **ajouter un nouvel élément** boîte de dialogue s’affiche, nommez le fichier de classe **BookDetails.cs**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc522-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="fc522-166">Lorsque le **BookDetails.cs** fichier est ouvert, remplacez le code dans le fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fc522-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="fc522-167">Enregistrez et fermez le **BookDetails.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="fc522-168">Mise à jour la **MainViewModel.cs** classe pour inclure les fonctionnalités pour communiquer avec l’application BookStore Web API :</span><span class="sxs-lookup"><span data-stu-id="fc522-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="fc522-169">Développez le **ViewModel** dossier dans l’Explorateur de solutions, puis double-cliquez le **MainViewModel.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="fc522-170">Lorsque le **MainViewModel.cs** fichier est ouvert, remplacez le code dans le fichier par le code suivant ; Notez que vous devez mettre à jour la valeur de la `apiUrl` constante avec l’URL réelle de votre API Web :</span><span class="sxs-lookup"><span data-stu-id="fc522-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="fc522-171">Enregistrez et fermez le **MainViewModel.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="fc522-172">Mise à jour la **MainPage.xaml** fichier pour personnaliser le nom de l’application :</span><span class="sxs-lookup"><span data-stu-id="fc522-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="fc522-173">Double-cliquez sur le **MainPage.xaml** fichier dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="fc522-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="fc522-174">Lorsque le **MainPage.xaml** fichier est ouvert, recherchez les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="fc522-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="fc522-175">Remplacez les lignes avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fc522-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="fc522-176">Enregistrez et fermez le **MainPage.xaml** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="fc522-177">Mise à jour la **DetailsPage.xaml** fichier pour personnaliser les éléments affichés :</span><span class="sxs-lookup"><span data-stu-id="fc522-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="fc522-178">Double-cliquez sur le **DetailsPage.xaml** fichier dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="fc522-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="fc522-179">Lorsque le **DetailsPage.xaml** fichier est ouvert, recherchez les lignes de code suivantes :</span><span class="sxs-lookup"><span data-stu-id="fc522-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="fc522-180">Remplacez les lignes avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fc522-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="fc522-181">Enregistrez et fermez le **DetailsPage.xaml** fichier.</span><span class="sxs-lookup"><span data-stu-id="fc522-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="fc522-182">Générez l’application Windows Phone pour rechercher les erreurs.</span><span class="sxs-lookup"><span data-stu-id="fc522-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="fc522-183">Étape 3 : Test de la Solution de bout en bout</span><span class="sxs-lookup"><span data-stu-id="fc522-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="fc522-184">Comme indiqué dans le *conditions préalables* projets de la section de ce didacticiel, lorsque vous testez la connectivité entre l’API Web et Windows Phone 8 sur votre système local, vous devez suivre les instructions fournies dans le *[ La connexion de l’émulateur de 8 Windows Phone pour les Applications de l’API Web sur un ordinateur Local](https://go.microsoft.com/fwlink/?LinkId=324014)* article pour configurer votre environnement de test.</span><span class="sxs-lookup"><span data-stu-id="fc522-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="fc522-185">Une fois que vous avez configuré l’environnement de test, vous devez définir l’application Windows Phone comme projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="fc522-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="fc522-186">Pour ce faire, mettez en surbrillance le **BookCatalog** application dans l’Explorateur de solutions, puis cliquez sur **définir comme projet de démarrage**:</span><span class="sxs-lookup"><span data-stu-id="fc522-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="fc522-187">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-187">Click image to expand</span></span> |

<span data-ttu-id="fc522-188">Lorsque vous appuyez sur F5, Visual Studio démarre à la fois le Windows Phone émulateur, qui affiche un &quot;Patientez&quot; message pendant que les données d’application sont récupérées à partir de votre API Web :</span><span class="sxs-lookup"><span data-stu-id="fc522-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="fc522-189">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-189">Click image to expand</span></span> |

<span data-ttu-id="fc522-190">Si tout est réussie, vous devez voir le catalogue affiché :</span><span class="sxs-lookup"><span data-stu-id="fc522-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="fc522-191">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-191">Click image to expand</span></span> |

<span data-ttu-id="fc522-192">Si vous appuyez sur n’importe quel titre de livre, l’application affiche la description du Registre :</span><span class="sxs-lookup"><span data-stu-id="fc522-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="fc522-193">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-193">Click image to expand</span></span> |

<span data-ttu-id="fc522-194">Si l’application ne peut pas communiquer avec votre API Web, un message d’erreur s’affichera :</span><span class="sxs-lookup"><span data-stu-id="fc522-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="fc522-195">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-195">Click image to expand</span></span> |

<span data-ttu-id="fc522-196">Si vous cliquez sur le message d’erreur, des détails supplémentaires sur l’erreur seront affiche :</span><span class="sxs-lookup"><span data-stu-id="fc522-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="fc522-197">Cliquez sur l’image pour développer</span><span class="sxs-lookup"><span data-stu-id="fc522-197">Click image to expand</span></span>                                                                 |

