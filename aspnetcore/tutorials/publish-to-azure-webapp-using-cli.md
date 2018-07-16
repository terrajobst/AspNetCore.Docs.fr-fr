---
title: Publier une application ASP.NET Core sur Azure avec des outils en ligne de commande
author: camsoper
description: Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126958"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="b974a-103">Publier une application ASP.NET Core sur Azure avec des outils en ligne de commande</span><span class="sxs-lookup"><span data-stu-id="b974a-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="b974a-104">Auteur : [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b974a-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="b974a-105">Ce didacticiel vous montrera comment générer et déployer une application ASP.NET Core sur Microsoft Azure App Service à l’aide d’outils en ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b974a-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="b974a-106">Quand vous aurez terminé, vous disposerez d’une application web Razor Pages créée dans ASP.NET Core et hébergée en tant qu’application web Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b974a-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="b974a-107">Ce didacticiel est écrit avec des outils en ligne de commande Windows, mais il peut également s’appliquer à des environnements macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="b974a-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="b974a-108">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="b974a-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b974a-109">créer un site web Azure App Service à l’aide de l’interface Azure CLI ;</span><span class="sxs-lookup"><span data-stu-id="b974a-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b974a-110">déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="b974a-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b974a-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b974a-111">Prerequisites</span></span>

<span data-ttu-id="b974a-112">Pour suivre ce didacticiel, vous aurez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b974a-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="b974a-113">Un [abonnement Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="b974a-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="b974a-114">Un client de ligne de commande [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="b974a-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="b974a-115">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="b974a-115">Create a web app</span></span>

<span data-ttu-id="b974a-116">Créez un répertoire pour l’application web, créez une application Razor Pages ASP.NET Core, puis exécutez le site web en local.</span><span class="sxs-lookup"><span data-stu-id="b974a-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b974a-117">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="b974a-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="b974a-118">Autre</span><span class="sxs-lookup"><span data-stu-id="b974a-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Sortie de la ligne de commande](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="b974a-120">Testez l’application en accédant à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b974a-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Le site web en cours d’exécution en local](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="b974a-122">Créer l’instance Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b974a-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="b974a-123">À l’aide [d’Azure Cloud Shell](/azure/cloud-shell/quickstart), créez un groupe de ressources, un plan App Service et une application web App Service.</span><span class="sxs-lookup"><span data-stu-id="b974a-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="b974a-124">Avant le déploiement, définissez les identifiants de déploiement au niveau du compte à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b974a-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="b974a-125">Une URL de déploiement est nécessaire pour déployer l’application à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="b974a-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="b974a-126">Récupérez l’URL ainsi.</span><span class="sxs-lookup"><span data-stu-id="b974a-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="b974a-127">Notez l’URL qui s’affiche ; elle se termine par `.git`.</span><span class="sxs-lookup"><span data-stu-id="b974a-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="b974a-128">Elle est utilisée dans l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="b974a-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="b974a-129">Déployer l’application à l’aide de Git</span><span class="sxs-lookup"><span data-stu-id="b974a-129">Deploy the app using Git</span></span>

<span data-ttu-id="b974a-130">Vous pouvez maintenant effectuer le déploiement avec Git à partir de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b974a-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="b974a-131">Il est possible d’ignorer sans risque les avertissements de Git sur les fins de ligne.</span><span class="sxs-lookup"><span data-stu-id="b974a-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b974a-132">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="b974a-132">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="b974a-133">Autre</span><span class="sxs-lookup"><span data-stu-id="b974a-133">Other</span></span>](#tab/other)

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

<span data-ttu-id="b974a-134">Git vous demande les identifiants de déploiement définis précédemment.</span><span class="sxs-lookup"><span data-stu-id="b974a-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="b974a-135">Après l’authentification, l’application sera envoyée sur l’emplacement distant, générée et déployée.</span><span class="sxs-lookup"><span data-stu-id="b974a-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Sortie du déploiement Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="b974a-137">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="b974a-137">Test the app</span></span>

<span data-ttu-id="b974a-138">Testez l’application en accédant à `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="b974a-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="b974a-139">Pour afficher l’adresse dans Cloud Shell (ou Azure CLI), utilisez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b974a-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![L’application en cours d’exécution dans Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="b974a-141">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="b974a-141">Clean up</span></span>

<span data-ttu-id="b974a-142">Lorsque vous avez terminé de tester l’application et d’inspecter le code et les ressources, supprimez l’application web et le plan en supprimant le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="b974a-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="b974a-143">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="b974a-143">Next steps</span></span>

<span data-ttu-id="b974a-144">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="b974a-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b974a-145">créer un site web Azure App Service à l’aide de l’interface Azure CLI ;</span><span class="sxs-lookup"><span data-stu-id="b974a-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b974a-146">déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="b974a-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="b974a-147">Apprenez maintenant à utiliser la ligne de commande pour déployer une application web existante qui utilise CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="b974a-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b974a-148">Déployer sur Azure en ligne de commande avec .NET Core</span><span class="sxs-lookup"><span data-stu-id="b974a-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
