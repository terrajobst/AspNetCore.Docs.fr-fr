---
title: Déploiement continu pour Azure avec Visual Studio et Git avec ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="7e9cc-103">Déploiement continu pour Azure avec Visual Studio et Git avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e9cc-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="7e9cc-104">Par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="7e9cc-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="7e9cc-105">Ce didacticiel montre comment créer une application de web ASP.NET Core à l’aide de Visual Studio et le déployer vers Azure App Service à partir de Visual Studio à l’aide d’un déploiement continu.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="7e9cc-106">Consultez aussi [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), qui montre comment configurer un workflow de livraison continue pour [Azure App Service](/azure/app-service/app-service-web-overview) en utilisant Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="7e9cc-107">Azure livraison continue dans Team Services simplifie la configuration d’un pipeline de déploiement fiable pour publier des mises à jour pour les applications hébergées dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="7e9cc-108">Le pipeline peut être configuré à partir du portail Azure pour générer, exécuter des tests, déployer vers un emplacement intermédiaire, puis déployer en production.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="7e9cc-109">Pour effectuer ce didacticiel, un compte Microsoft Azure est requis.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="7e9cc-110">Pour obtenir un compte, [activer les abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) ou [s’inscrire à une évaluation gratuite](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="7e9cc-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e9cc-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="7e9cc-111">Prerequisites</span></span>

<span data-ttu-id="7e9cc-112">Ce didacticiel suppose que les logiciels suivants sont installés :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="7e9cc-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e9cc-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="7e9cc-114">[Git](https://git-scm.com/downloads) pour Windows</span><span class="sxs-lookup"><span data-stu-id="7e9cc-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="7e9cc-115">Créer une application web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e9cc-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="7e9cc-116">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="7e9cc-117">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="7e9cc-118">Sélectionnez le modèle de projet **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="7e9cc-119">Il apparaît sous **Installé** > **Modèles** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="7e9cc-120">Attribuez un nom au projet `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="7e9cc-121">Sélectionnez le **nouveau référentiel Git de créer** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Boîte de dialogue Nouveau projet](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="7e9cc-123">Dans la boîte de dialogue **Nouveau projet ASP.NET Core**, sélectionnez le modèle ASP.NET Core **Vide**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Boîte de dialogue Nouveau projet ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="7e9cc-125">La version la plus récente du .NET Core est 2.0.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="7e9cc-126">Exécution de l’application web localement</span><span class="sxs-lookup"><span data-stu-id="7e9cc-126">Running the web app locally</span></span>

1. <span data-ttu-id="7e9cc-127">Une fois que Visual Studio a terminé la création de l’application, exécutez l’application en sélectionnant **Déboguer** > **Démarrer le débogage**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="7e9cc-128">En guise d’alternative, appuyez sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="7e9cc-129">L’initialisation de Visual Studio et de la nouvelle application peut prendre un certain temps.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="7e9cc-130">Une fois terminé, le navigateur affiche l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-130">Once it's complete, the browser shows the running app.</span></span>

   ![Fenêtre de navigateur montrant l’application en cours d’exécution qui affiche « Hello World! »](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="7e9cc-132">Après avoir vérifié l’application Web en cours d’exécution, fermez le navigateur et sélectionnez l’icône « Arrêter le débogage » dans la barre d’outils de Visual Studio pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="7e9cc-133">Créer une application web dans le portail Azure</span><span class="sxs-lookup"><span data-stu-id="7e9cc-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="7e9cc-134">La procédure suivante crée une application web dans le portail Azure :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="7e9cc-135">Se connecter à la [portail Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e9cc-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="7e9cc-136">Sélectionnez **nouveau** en haut à gauche de l’interface du portail.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="7e9cc-137">Sélectionnez **Web + Mobile** > **application Web**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portail Microsoft Azure : Nouveau bouton Web + Mobile sous Place de marché - Bouton Application Web sous Applications à la une](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="7e9cc-139">Dans le panneau **Application web**, entrez une valeur unique pour le **Nom App Service**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Panneau Application web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="7e9cc-141">Le **nom du Service application** nom doit être unique.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="7e9cc-142">Le portail applique cette règle lorsque le nom est fourni.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="7e9cc-143">Si vous en fournissant une valeur différente, remplacez cette valeur pour chaque occurrence de **SampleWebAppDemo** dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="7e9cc-144">Dans le panneau **Application web**, sélectionnez un **Plan/emplacement App Service** existant ou créez-en un.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="7e9cc-145">Si vous créez un nouveau plan, sélectionnez le niveau de tarification, d’emplacement et d’autres options.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="7e9cc-146">Pour plus d’informations sur les plans de Service d’applications, consultez [vue d’ensemble approfondie des plans de Service d’applications Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="7e9cc-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="7e9cc-147">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-147">Select **Create**.</span></span> <span data-ttu-id="7e9cc-148">Windows Azure configurer et démarrer l’application web.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-148">Azure will provision and start the web app.</span></span>

   ![Portail Azure : Panneau Exemple d’application web Demo 01 Essentials](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="7e9cc-150">Activer la publication Git pour la nouvelle application web</span><span class="sxs-lookup"><span data-stu-id="7e9cc-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="7e9cc-151">GIT est un système de contrôle de version distribuée qui peut être utilisé pour déployer une application web de Service d’applications Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="7e9cc-152">Code d’application Web est stocké dans un référentiel Git local et le code est déployé sur Azure en envoyant à un référentiel distant.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="7e9cc-153">Connectez-vous à la [portail Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e9cc-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="7e9cc-154">Sélectionnez **des Services d’application** pour afficher la liste des services d’application associé à l’abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="7e9cc-155">Sélectionnez l’application web créée dans la section précédente de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="7e9cc-156">Dans le panneau **Déploiement**, sélectionnez **Options de déploiement** > **Choisir une source** > **Référentiel Git local**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Panneau Paramètres : Panneau Source de déploiement : Panneau Choisir une source](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="7e9cc-158">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-158">Select **OK**.</span></span>

1. <span data-ttu-id="7e9cc-159">Si les informations d’identification de déploiement pour la publication d’une application web ou autre application de Service d’applications n’ont pas été précédemment configurées, vous pouvez les configurer maintenant :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="7e9cc-160">Sélectionnez **paramètres** > **informations d’identification de déploiement**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="7e9cc-161">Le **définir les informations d’identification de déploiement** panneau s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="7e9cc-162">Créez un nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-162">Create a user name and password.</span></span> <span data-ttu-id="7e9cc-163">Enregistrer le mot de passe pour une utilisation ultérieure lorsque vous configurez Git.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="7e9cc-164">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-164">Select **Save**.</span></span>

1. <span data-ttu-id="7e9cc-165">Dans le **Web App** panneau, sélectionnez **paramètres** > **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="7e9cc-166">L’URL du référentiel Git distant à déployer sur est affiché sous **URL GIT**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="7e9cc-167">Copiez la valeur de **URL Git** pour l’utiliser plus tard dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portail Azure : Panneau Propriétés de l’application](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="7e9cc-169">Publier l’application web vers Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7e9cc-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="7e9cc-170">Dans cette section, créez un référentiel Git local à l’aide de Visual Studio et par envoi de données à partir de ce référentiel vers Azure pour déployer l’application web.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="7e9cc-171">Les étapes impliquées sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-171">The steps involved include the following:</span></span>

* <span data-ttu-id="7e9cc-172">Ajoutez le paramètre de référentiel distant à l’aide de la valeur d’URL de GIT, donc le référentiel local peut être déployé sur Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="7e9cc-173">Valider les modifications du projet.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-173">Commit project changes.</span></span>
* <span data-ttu-id="7e9cc-174">Push des modifications de projet à partir du référentiel local vers le référentiel distant sur Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="7e9cc-175">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Solution 'SampleWebAppDemo'** et sélectionnez **Valider**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="7e9cc-176">Le **Team Explorer** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-176">The **Team Explorer** is displayed.</span></span>

   ![Onglet Connexion de Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="7e9cc-178">Dans **Team Explorer**, sélectionnez **Accueil** (icône de maison) > **Paramètres** > **Paramètres du dépôt**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="7e9cc-179">Dans le **distantes** section de la **paramètres du référentiel**, sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="7e9cc-180">Le **ajouter distant** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="7e9cc-181">Définissez le **Nom** de l’élément distant sur **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="7e9cc-182">Définir la valeur de **Fetch** à la **URL Git** qui est copié à partir de Azure précédemment dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="7e9cc-183">Notez qu’il s’agit de l’URL qui se termine par **.git**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Boîte de dialogue Modifier un élément distant](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="7e9cc-185">En guise d’alternative, spécifiez le référentiel distant à partir de la **fenêtre commande** en ouvrant le **fenêtre commande**, la modification au répertoire du projet et en entrant la commande.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="7e9cc-186">Exemple :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="7e9cc-187">Sélectionnez **Accueil** (icône de maison) > **Paramètres** > **Paramètres globaux**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="7e9cc-188">Confirmez que le nom et adresse de messagerie sont définis.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="7e9cc-189">Sélectionnez **mise à jour** si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="7e9cc-190">Sélectionnez **Accueil** > **Modifications** pour revenir à la vue **Modifications**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="7e9cc-191">Entrez un message de validation, tel que **initiale Push #1** et sélectionnez **validation**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="7e9cc-192">Cette action crée un *validation* localement.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-192">This action creates a *commit* locally.</span></span>

   ![Onglet Connexion de Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="7e9cc-194">En guise d’alternative, écrire les modifications à partir de la **fenêtre commande** en ouvrant le **fenêtre commande**, la modification au répertoire du projet et en entrant les commandes git.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="7e9cc-195">Exemple :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="7e9cc-196">Sélectionnez **Accueil** > **Synchroniser** > **Actions** > **Ouvrir l’invite de commandes**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="7e9cc-197">L’invite de commandes s’ouvre dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="7e9cc-198">Entrez la commande suivante dans la fenêtre Commande :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="7e9cc-199">Entrez Azure **informations d’identification de déploiement** mot de passe créé précédemment dans Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="7e9cc-200">Cette commande démarre le processus de l’envoi de fichiers de projet local vers Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="7e9cc-201">La sortie de la commande ci-dessus se termine par un message que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="7e9cc-202">Si la collaboration sur le projet est requise, envisagez d’envoi au [GitHub](https://github.com) avant d’installer sur Azure.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="7e9cc-203">Vérifier le déploiement actif</span><span class="sxs-lookup"><span data-stu-id="7e9cc-203">Verify the Active Deployment</span></span>

<span data-ttu-id="7e9cc-204">Vérifiez que le transfert d’application web à partir de l’environnement local vers Azure a réussi.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="7e9cc-205">Dans le [Azure Portal](https://portal.azure.com), sélectionnez l’application web.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="7e9cc-206">Sélectionnez **déploiement** > **options de déploiement**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-206">Select **Deployment** > **Deployment options**.</span></span>

![Portail Azure : Panneau Paramètres : Panneau Déploiements montrant la réussite du déploiement](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="7e9cc-208">Exécuter l’application dans Azure</span><span class="sxs-lookup"><span data-stu-id="7e9cc-208">Run the app in Azure</span></span>

<span data-ttu-id="7e9cc-209">Maintenant que l’application web est déployée sur Azure, exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="7e9cc-210">Cela peut être accompli de deux manières :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="7e9cc-211">Dans le portail Azure, recherchez le panneau des applications web pour l’application web.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="7e9cc-212">Sélectionnez **Parcourir** pour l’afficher dans le navigateur par défaut.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="7e9cc-213">Ouvrez un navigateur et entrez l’URL de l’application web.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="7e9cc-214">Exemple : `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="7e9cc-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="7e9cc-215">Mettre à jour de l’application web et publier à nouveau</span><span class="sxs-lookup"><span data-stu-id="7e9cc-215">Update the web app and republish</span></span>

<span data-ttu-id="7e9cc-216">Après avoir apporté des modifications du code local, republiez :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="7e9cc-217">Dans **l’Explorateur de solutions** de Visual Studio, ouvrez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="7e9cc-218">Dans la méthode `Configure`, modifiez la méthode `Response.WriteAsync` comme suit :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="7e9cc-219">Enregistrer les modifications apportées à *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="7e9cc-220">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Solution 'SampleWebAppDemo'** et sélectionnez **Valider**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="7e9cc-221">Le **Team Explorer** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="7e9cc-222">Entrez un message de validation, tel que `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="7e9cc-223">Appuyez sur le bouton **Valider** pour valider les modifications du projet.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="7e9cc-224">Sélectionnez **Accueil** > **Synchroniser** > **Actions** > **Envoyer (push)**.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="7e9cc-225">En guise d’alternative, envoyer les modifications à partir de la **fenêtre commande** en ouvrant le **fenêtre commande**, la modification au répertoire du projet et en entrant une commande de git.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="7e9cc-226">Exemple :</span><span class="sxs-lookup"><span data-stu-id="7e9cc-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="7e9cc-227">Afficher l’application web mise à jour dans Azure</span><span class="sxs-lookup"><span data-stu-id="7e9cc-227">View the updated web app in Azure</span></span>

<span data-ttu-id="7e9cc-228">Afficher l’application web mis à jour en sélectionnant **Parcourir** depuis le panneau des applications web dans le portail Azure ou en ouvrant un navigateur et entrez l’URL de l’application web.</span><span class="sxs-lookup"><span data-stu-id="7e9cc-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="7e9cc-229">Exemple : `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="7e9cc-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e9cc-230">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7e9cc-230">Additional resources</span></span>

* [<span data-ttu-id="7e9cc-231">Utiliser VSTS pour générer et publier à une application Web Azure avec un déploiement continu</span><span class="sxs-lookup"><span data-stu-id="7e9cc-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="7e9cc-232">Projet Kudu</span><span class="sxs-lookup"><span data-stu-id="7e9cc-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
