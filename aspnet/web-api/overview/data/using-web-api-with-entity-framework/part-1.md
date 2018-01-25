---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: "À l’aide des API Web 2 avec Entity Framework 6 | Documents Microsoft"
author: MikeWasson
description: "Ce didacticiel, vous allez apprendre les principes fondamentaux de la création d’une application web avec une API Web ASP.NET back-end. Ce didacticiel utilise Entity Framework 6 pour la disposition de données..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: cceefa128f90b4c3e23dd31119f44e6ffc55f46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="5a855-104">À l’aide des API Web 2 avec Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5a855-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="5a855-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5a855-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5a855-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="5a855-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="5a855-107">Ce didacticiel, vous allez apprendre les principes fondamentaux de la création d’une application web avec une API Web ASP.NET back-end.</span><span class="sxs-lookup"><span data-stu-id="5a855-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="5a855-108">Le didacticiel utilise Entity Framework 6 pour la couche données et Knockout.js pour l’application JavaScript côté client.</span><span class="sxs-lookup"><span data-stu-id="5a855-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="5a855-109">Le didacticiel montre également comment déployer l’application sur Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="5a855-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5a855-110">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5a855-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5a855-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="5a855-111">Web API 2.1</span></span>
> - [<span data-ttu-id="5a855-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="5a855-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="5a855-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5a855-113">Entity Framework 6</span></span>
> - <span data-ttu-id="5a855-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5a855-114">.NET 4.5</span></span>
> - <span data-ttu-id="5a855-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="5a855-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="5a855-116">Ce didacticiel utilise ASP.NET Web API 2 avec Entity Framework 6 pour créer une application web qui manipule une base de données principale.</span><span class="sxs-lookup"><span data-stu-id="5a855-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="5a855-117">Voici une capture d’écran de l’application que vous allez créer.</span><span class="sxs-lookup"><span data-stu-id="5a855-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="5a855-118">L’application utilise une conception d’application de page unique (SPA).</span><span class="sxs-lookup"><span data-stu-id="5a855-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="5a855-119">« Seule page application » est le terme général pour une application web qui charge une page HTML unique, puis met à jour la page dynamiquement, au lieu de charger les nouvelles pages.</span><span class="sxs-lookup"><span data-stu-id="5a855-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="5a855-120">Après le chargement de page initial, l’application communique avec le serveur via les requêtes AJAX.</span><span class="sxs-lookup"><span data-stu-id="5a855-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="5a855-121">Le AJAX demande renvoyer les données JSON, que l’application utilise pour mettre à jour l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5a855-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="5a855-122">AJAX n’est pas nouveau, mais aujourd'hui des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée.</span><span class="sxs-lookup"><span data-stu-id="5a855-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="5a855-123">Ce didacticiel utilise [Knockout.js](http://knockoutjs.com/), mais vous pouvez utiliser n’importe quelle infrastructure de client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a855-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="5a855-124">Voici les principaux blocs de construction pour cette application :</span><span class="sxs-lookup"><span data-stu-id="5a855-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="5a855-125">ASP.NET MVC crée la page HTML.</span><span class="sxs-lookup"><span data-stu-id="5a855-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="5a855-126">API Web ASP.NET gère les requêtes AJAX et retourne les données JSON.</span><span class="sxs-lookup"><span data-stu-id="5a855-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="5a855-127">Knockout.js-lie les éléments HTML pour les données JSON.</span><span class="sxs-lookup"><span data-stu-id="5a855-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="5a855-128">Entity Framework communique avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="5a855-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="5a855-129">Consultez cette application en cours d’exécution sur Azure</span><span class="sxs-lookup"><span data-stu-id="5a855-129">See this App Running on Azure</span></span>

<span data-ttu-id="5a855-130">Vous voulez voir le site terminé en cours d’exécution comme une application web dynamique ?</span><span class="sxs-lookup"><span data-stu-id="5a855-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="5a855-131">Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.</span><span class="sxs-lookup"><span data-stu-id="5a855-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="5a855-132">Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure.</span><span class="sxs-lookup"><span data-stu-id="5a855-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="5a855-133">Si vous n’avez pas déjà un compte, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="5a855-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="5a855-134">[Ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous obtenez des crédits que vous pouvez utiliser pour tester les services Azure payants et même après qu’ils soient utilisés jusqu'à, vous pouvez conserver le compte et libérer de l’utilisation des services Azure.</span><span class="sxs-lookup"><span data-stu-id="5a855-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="5a855-135">[Activer les abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="5a855-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="5a855-136">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="5a855-136">Create the Project</span></span>

<span data-ttu-id="5a855-137">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5a855-137">Open Visual Studio.</span></span> <span data-ttu-id="5a855-138">À partir de la **fichier** menu, sélectionnez **nouveau**, puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="5a855-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="5a855-139">(Ou cliquez sur **nouveau projet** sur la page de démarrage.)</span><span class="sxs-lookup"><span data-stu-id="5a855-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="5a855-140">Dans le **nouveau projet** boîte de dialogue, cliquez sur **Web** dans le volet gauche et **Application Web ASP.NET** dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="5a855-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="5a855-141">Nommez le projet BookService et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a855-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="5a855-142">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **API Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="5a855-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="5a855-143">Si vous souhaitez héberger le projet dans un Service d’applications Azure, laissez le **hôte dans le cloud** case activée.</span><span class="sxs-lookup"><span data-stu-id="5a855-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="5a855-144">Cliquez sur **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="5a855-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="5a855-145">Configurer les paramètres Azure (facultatifs)</span><span class="sxs-lookup"><span data-stu-id="5a855-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="5a855-146">Si vous avez laissé le **hôte du Cloud** option activée, Visual Studio vous invite à vous connecter à Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="5a855-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="5a855-147">Après que vous être connecté à Azure, Visual Studio vous invite à configurer l’application web.</span><span class="sxs-lookup"><span data-stu-id="5a855-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="5a855-148">Entrez un nom pour le site de votre abonnement Azure et sélectionnez une région géographique.</span><span class="sxs-lookup"><span data-stu-id="5a855-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="5a855-149">Sous **serveur de base de données**, sélectionnez **créer un nouveau serveur**.</span><span class="sxs-lookup"><span data-stu-id="5a855-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="5a855-150">Entrez un nom d’utilisateur administrateur et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="5a855-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[<span data-ttu-id="5a855-151">Next</span><span class="sxs-lookup"><span data-stu-id="5a855-151">Next</span></span>](part-2.md)
