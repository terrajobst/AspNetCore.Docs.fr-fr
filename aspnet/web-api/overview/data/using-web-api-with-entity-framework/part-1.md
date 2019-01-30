---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: À l’aide de Web API 2 avec Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel vous apprend aux principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end. Ce didacticiel utilise Entity Framework 6 pour la disposition de données...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236521"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="89283-104">À l’aide de Web API 2 avec Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="89283-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="89283-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="89283-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="89283-106">Ce didacticiel vous enseigne les principes fondamentaux de création d’une application web avec une API Web ASP.NET back-end.</span><span class="sxs-lookup"><span data-stu-id="89283-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="89283-107">Le didacticiel utilise Entity Framework 6 de la couche de données et Knockout.js pour l’application de JavaScript côté client.</span><span class="sxs-lookup"><span data-stu-id="89283-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="89283-108">Le didacticiel montre également comment déployer l’application dans Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="89283-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="89283-109">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="89283-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="89283-110">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="89283-110">Web API 2.1</span></span>
> - <span data-ttu-id="89283-111">Visual Studio 2017 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="89283-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="89283-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="89283-112">Entity Framework 6</span></span>
> - <span data-ttu-id="89283-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="89283-113">.NET 4.7</span></span>
> - <span data-ttu-id="89283-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="89283-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="89283-115">Ce didacticiel utilise l’API Web ASP.NET 2 avec Entity Framework 6 pour créer une application web qui manipule une base de données principale.</span><span class="sxs-lookup"><span data-stu-id="89283-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="89283-116">Voici une capture d’écran de l’application que vous allez créer.</span><span class="sxs-lookup"><span data-stu-id="89283-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="89283-117">L’application utilise une conception d’application à page unique (SPA).</span><span class="sxs-lookup"><span data-stu-id="89283-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="89283-118">« Single-page application » est le terme général qui désigne une application web qui charge une seule page HTML et puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages.</span><span class="sxs-lookup"><span data-stu-id="89283-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="89283-119">Après le chargement de page initial, l’application s’entretient avec le serveur via les demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="89283-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="89283-120">L’AJAX demande de renvoyer les données JSON, que l’application utilise pour mettre à jour de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="89283-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="89283-121">AJAX n’est pas nouveau, mais aujourd'hui, il existe des infrastructures JavaScript qui le rendent plus facile créer et gérer une grande application SPA sophistiquée.</span><span class="sxs-lookup"><span data-stu-id="89283-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="89283-122">Ce didacticiel utilise [Knockout.js](http://knockoutjs.com/), mais vous pouvez utiliser n’importe quel framework de client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="89283-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="89283-123">Voici les principaux blocs de construction pour cette application :</span><span class="sxs-lookup"><span data-stu-id="89283-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="89283-124">ASP.NET MVC crée la page HTML.</span><span class="sxs-lookup"><span data-stu-id="89283-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="89283-125">API Web ASP.NET gère les requêtes AJAX et retourne les données JSON.</span><span class="sxs-lookup"><span data-stu-id="89283-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="89283-126">Knockout.js-lie les éléments HTML pour les données JSON.</span><span class="sxs-lookup"><span data-stu-id="89283-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="89283-127">Entity Framework communique avec la base de données.</span><span class="sxs-lookup"><span data-stu-id="89283-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="89283-128">Consultez cette application en cours d’exécution sur Azure</span><span class="sxs-lookup"><span data-stu-id="89283-128">See this app running on Azure</span></span>

<span data-ttu-id="89283-129">Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ?</span><span class="sxs-lookup"><span data-stu-id="89283-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="89283-130">Vous pouvez déployer une version complète de l’application à votre compte Azure en sélectionnant le bouton suivant.</span><span class="sxs-lookup"><span data-stu-id="89283-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="89283-131">Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure.</span><span class="sxs-lookup"><span data-stu-id="89283-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="89283-132">Si vous n’avez pas déjà un compte, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="89283-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="89283-133">[Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.</span><span class="sxs-lookup"><span data-stu-id="89283-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="89283-134">[Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="89283-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="89283-135">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="89283-135">Create the project</span></span>

<span data-ttu-id="89283-136">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89283-136">Open Visual Studio.</span></span> <span data-ttu-id="89283-137">À partir de la **fichier** menu, sélectionnez **New**, puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="89283-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="89283-138">(Ou sélectionnez **nouveau projet** sur la page de démarrage.)</span><span class="sxs-lookup"><span data-stu-id="89283-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="89283-139">Dans le **nouveau projet** boîte de dialogue, sélectionnez **Web** dans le volet gauche et **Application Web ASP.NET (.NET Framework)** dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="89283-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="89283-140">Nommez le projet **BookService** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="89283-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="89283-141">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **API Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="89283-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="89283-142">Sélectionnez **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="89283-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="89283-143">Configurer les paramètres Azure (facultatifs)</span><span class="sxs-lookup"><span data-stu-id="89283-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="89283-144">Après avoir créé le projet, vous pouvez choisir de déployer dans Azure App Service Web Apps à tout moment.</span><span class="sxs-lookup"><span data-stu-id="89283-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="89283-145">Dans l’Explorateur de solutions, cliquez sur votre projet, puis sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="89283-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="89283-146">Dans la fenêtre qui s’affiche, sélectionnez **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="89283-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="89283-147">Le **choisir une cible de publication** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="89283-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="89283-148">Sélectionnez **Créer un profil**.</span><span class="sxs-lookup"><span data-stu-id="89283-148">Select **Create Profile**.</span></span> <span data-ttu-id="89283-149">La boîte de dialogue **Créer App Service** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="89283-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="89283-150">Acceptez les valeurs par défaut, ou entrez des valeurs différentes pour le nom de l’application, groupe de ressources, qui héberge le plan d’abonnement Azure et région géographique.</span><span class="sxs-lookup"><span data-stu-id="89283-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="89283-151">Sélectionnez **créer une base de données SQL**.</span><span class="sxs-lookup"><span data-stu-id="89283-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="89283-152">Le **configurer SQL Server** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="89283-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="89283-153">Acceptez les valeurs par défaut ou entrez des valeurs différentes.</span><span class="sxs-lookup"><span data-stu-id="89283-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="89283-154">Entrez un **Adminstrator Username** et **mot de passe administrateur** pour votre nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="89283-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="89283-155">Sélectionnez **OK** lorsque vous avez terminé.</span><span class="sxs-lookup"><span data-stu-id="89283-155">Select **OK** when you're done.</span></span> <span data-ttu-id="89283-156">Le **créer App Service** page s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="89283-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="89283-157">Sélectionnez **créer** pour créer votre profil.</span><span class="sxs-lookup"><span data-stu-id="89283-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="89283-158">Un message s’affiche dans le coin inférieur droit, indiquant que le déploiement est en cours.</span><span class="sxs-lookup"><span data-stu-id="89283-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="89283-159">Après quelques instants, le **publier** fenêtre réapparaît.</span><span class="sxs-lookup"><span data-stu-id="89283-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="89283-160">Le profil que vous avez créé pour déployer l’application est désormais disponible.</span><span class="sxs-lookup"><span data-stu-id="89283-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="89283-161">Next</span><span class="sxs-lookup"><span data-stu-id="89283-161">Next</span></span>](part-2.md)
