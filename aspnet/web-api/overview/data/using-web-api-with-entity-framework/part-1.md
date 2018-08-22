---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: À l’aide de Web API 2 avec Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel vous apprend aux principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end. Ce didacticiel utilise Entity Framework 6 pour la disposition de données...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 4abe0e06dfd927765efd8e566584e111cf4117d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830734"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="aa945-104">À l’aide de Web API 2 avec Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="aa945-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="aa945-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aa945-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="aa945-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="aa945-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="aa945-107">Ce didacticiel vous apprend aux principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end.</span><span class="sxs-lookup"><span data-stu-id="aa945-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="aa945-108">Le didacticiel utilise Entity Framework 6 de la couche de données et Knockout.js pour l’application de JavaScript côté client.</span><span class="sxs-lookup"><span data-stu-id="aa945-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="aa945-109">Le didacticiel montre également comment déployer l’application dans Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="aa945-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aa945-110">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="aa945-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="aa945-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="aa945-111">Web API 2.1</span></span>
> - [<span data-ttu-id="aa945-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="aa945-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="aa945-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="aa945-113">Entity Framework 6</span></span>
> - <span data-ttu-id="aa945-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="aa945-114">.NET 4.5</span></span>
> - <span data-ttu-id="aa945-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="aa945-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="aa945-116">Ce didacticiel utilise l’API Web ASP.NET 2 avec Entity Framework 6 pour créer une application web qui manipule une base de données principale.</span><span class="sxs-lookup"><span data-stu-id="aa945-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="aa945-117">Voici une capture d’écran de l’application que vous allez créer.</span><span class="sxs-lookup"><span data-stu-id="aa945-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="aa945-118">L’application utilise une conception d’application à page unique (SPA).</span><span class="sxs-lookup"><span data-stu-id="aa945-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="aa945-119">« Single-page application » est le terme général qui désigne une application web qui charge une seule page HTML et puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages.</span><span class="sxs-lookup"><span data-stu-id="aa945-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="aa945-120">Après le chargement de page initial, l’application s’entretient avec le serveur via les demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="aa945-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="aa945-121">L’AJAX demande de renvoyer les données JSON, que l’application utilise pour mettre à jour de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aa945-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="aa945-122">AJAX n’est pas nouveau, mais aujourd'hui, il existe des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée.</span><span class="sxs-lookup"><span data-stu-id="aa945-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="aa945-123">Ce didacticiel utilise [Knockout.js](http://knockoutjs.com/), mais vous pouvez utiliser n’importe quel framework de client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="aa945-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="aa945-124">Voici les principaux blocs de construction pour cette application :</span><span class="sxs-lookup"><span data-stu-id="aa945-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="aa945-125">ASP.NET MVC crée la page HTML.</span><span class="sxs-lookup"><span data-stu-id="aa945-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="aa945-126">API Web ASP.NET gère les requêtes AJAX et retourne les données JSON.</span><span class="sxs-lookup"><span data-stu-id="aa945-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="aa945-127">Knockout.js-lie les éléments HTML pour les données JSON.</span><span class="sxs-lookup"><span data-stu-id="aa945-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="aa945-128">Entity Framework communique avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="aa945-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="aa945-129">Consultez cette application en cours d’exécution sur Azure</span><span class="sxs-lookup"><span data-stu-id="aa945-129">See this App Running on Azure</span></span>

<span data-ttu-id="aa945-130">Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ?</span><span class="sxs-lookup"><span data-stu-id="aa945-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="aa945-131">Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.</span><span class="sxs-lookup"><span data-stu-id="aa945-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="aa945-132">Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure.</span><span class="sxs-lookup"><span data-stu-id="aa945-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="aa945-133">Si vous n’avez pas déjà un compte, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa945-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="aa945-134">[Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.</span><span class="sxs-lookup"><span data-stu-id="aa945-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="aa945-135">[Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="aa945-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="aa945-136">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="aa945-136">Create the Project</span></span>

<span data-ttu-id="aa945-137">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa945-137">Open Visual Studio.</span></span> <span data-ttu-id="aa945-138">À partir de la **fichier** menu, sélectionnez **New**, puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="aa945-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="aa945-139">(Ou cliquez sur **nouveau projet** sur la page de démarrage.)</span><span class="sxs-lookup"><span data-stu-id="aa945-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="aa945-140">Dans le **nouveau projet** boîte de dialogue, cliquez sur **Web** dans le volet gauche et **Application Web ASP.NET** dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="aa945-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="aa945-141">Nommez le projet BookService et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa945-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="aa945-142">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **API Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="aa945-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="aa945-143">Si vous souhaitez héberger le projet dans un Azure App Service, laissez le **hôte dans le cloud** case cochée.</span><span class="sxs-lookup"><span data-stu-id="aa945-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="aa945-144">Cliquez sur **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="aa945-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="aa945-145">Configurer les paramètres Azure (facultatifs)</span><span class="sxs-lookup"><span data-stu-id="aa945-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="aa945-146">Si vous avez laissé le **hôte dans le Cloud** option activée, Visual Studio vous invite à vous connecter à Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="aa945-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="aa945-147">Après que vous être connecté à Azure, Visual Studio vous invite à configurer l’application web.</span><span class="sxs-lookup"><span data-stu-id="aa945-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="aa945-148">Entrez un nom pour le site, sélectionnez votre abonnement Azure, puis une région géographique.</span><span class="sxs-lookup"><span data-stu-id="aa945-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="aa945-149">Sous **serveur de base de données**, sélectionnez **créer un serveur**.</span><span class="sxs-lookup"><span data-stu-id="aa945-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="aa945-150">Entrez un nom d’utilisateur administrateur et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="aa945-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="aa945-151">Next</span><span class="sxs-lookup"><span data-stu-id="aa945-151">Next</span></span>](part-2.md)
