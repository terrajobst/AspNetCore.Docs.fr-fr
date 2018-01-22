---
title: "Publier une application ASP.NET Core sur Azure à l’aide d’outils en ligne de commande | Microsoft Docs"
description: "Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git."
services: multiple
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 4797260f95443954e86aae1614140c0caa5ca8bd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="c5c64-103">Déployer une application ASP.NET Core sur Azure App Service en ligne de commande</span><span class="sxs-lookup"><span data-stu-id="c5c64-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="c5c64-104">Auteur : [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="c5c64-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="c5c64-105">Ce didacticiel vous montrera comment générer et déployer une application ASP.NET Core sur Microsoft Azure App Service à l’aide d’outils en ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="c5c64-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="c5c64-106">Lorsque vous aurez terminé, vous disposerez d’une application web créée dans ASP.NET MVC Core et hébergée comme une application Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="c5c64-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="c5c64-107">Ce didacticiel est écrit avec des outils en ligne de commande Windows, mais il peut également s’appliquer à des environnements macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="c5c64-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="c5c64-108">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="c5c64-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c5c64-109">créer un site web Azure App Service à l’aide de l’interface Azure CLI ;</span><span class="sxs-lookup"><span data-stu-id="c5c64-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c5c64-110">déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="c5c64-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5c64-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="c5c64-111">Prerequisites</span></span>

<span data-ttu-id="c5c64-112">Pour suivre ce didacticiel, vous aurez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c5c64-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="c5c64-113">Un [abonnement Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="c5c64-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="c5c64-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5c64-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="c5c64-115">Un client de ligne de commande [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="c5c64-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="c5c64-116">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="c5c64-116">Create a web application</span></span>

<span data-ttu-id="c5c64-117">Créez un nouveau répertoire pour l’application web, créez une nouvelle application ASP.NET Core MVC, puis exécutez le site web en local.</span><span class="sxs-lookup"><span data-stu-id="c5c64-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c5c64-118">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="c5c64-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="c5c64-119">Autre</span><span class="sxs-lookup"><span data-stu-id="c5c64-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Sortie de la ligne de commande](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="c5c64-121">Testez l’application en accédant à http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="c5c64-121">Test the application by browsing to http://localhost:5000.</span></span>

![Le site web en cours d’exécution en local](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="c5c64-123">Créer l’instance Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c5c64-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="c5c64-124">À l’aide [d’Azure Cloud Shell](/azure/cloud-shell/quickstart), créez un groupe de ressources, un plan App Service et une application web App Service.</span><span class="sxs-lookup"><span data-stu-id="c5c64-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="c5c64-125">Avant le déploiement, définissez les identifiants de déploiement au niveau du compte à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c5c64-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="c5c64-126">Une URL de déploiement est nécessaire pour déployer l’application à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="c5c64-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="c5c64-127">Récupérez l’URL ainsi.</span><span class="sxs-lookup"><span data-stu-id="c5c64-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="c5c64-128">Notez l’URL qui s’affiche ; elle se termine par `.git`.</span><span class="sxs-lookup"><span data-stu-id="c5c64-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="c5c64-129">Elle est utilisée dans l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="c5c64-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="c5c64-130">Déployer l’application avec Git</span><span class="sxs-lookup"><span data-stu-id="c5c64-130">Deploy the application using Git</span></span>

<span data-ttu-id="c5c64-131">Vous pouvez maintenant effectuer le déploiement avec Git à partir de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c5c64-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="c5c64-132">Il est possible d’ignorer sans risque les avertissements de Git sur les fins de ligne.</span><span class="sxs-lookup"><span data-stu-id="c5c64-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c5c64-133">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="c5c64-133">Windows</span></span>](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="c5c64-134">Autre</span><span class="sxs-lookup"><span data-stu-id="c5c64-134">Other</span></span>](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

<span data-ttu-id="c5c64-135">Git vous demande les identifiants de déploiement définis précédemment.</span><span class="sxs-lookup"><span data-stu-id="c5c64-135">Git will prompt for the deployment credentials that were set earlier.</span></span>  <span data-ttu-id="c5c64-136">Après l’authentification, l’application sera envoyée sur l’emplacement distant, générée et déployée.</span><span class="sxs-lookup"><span data-stu-id="c5c64-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Sortie du déploiement Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="c5c64-138">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="c5c64-138">Test the application</span></span>

<span data-ttu-id="c5c64-139">Testez l’application en accédant à `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c5c64-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="c5c64-140">Pour afficher l’adresse dans Cloud Shell (ou Azure CLI), utilisez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c5c64-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![L’application en cours d’exécution dans Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="c5c64-142">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="c5c64-142">Clean up</span></span>

<span data-ttu-id="c5c64-143">Lorsque vous avez terminé de tester l’application et d’inspecter le code et les ressources, supprimez l’application web et le plan en supprimant le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="c5c64-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="c5c64-144">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c5c64-144">Next steps</span></span>

<span data-ttu-id="c5c64-145">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="c5c64-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c5c64-146">créer un site web Azure App Service à l’aide de l’interface Azure CLI ;</span><span class="sxs-lookup"><span data-stu-id="c5c64-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c5c64-147">déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="c5c64-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="c5c64-148">Apprenez maintenant à utiliser la ligne de commande pour déployer une application web existante qui utilise CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="c5c64-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c5c64-149">Déployer sur Azure en ligne de commande avec .NET Core</span><span class="sxs-lookup"><span data-stu-id="c5c64-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
