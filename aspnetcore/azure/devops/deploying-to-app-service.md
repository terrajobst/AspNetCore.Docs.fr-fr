---
title: Déployer une application dans App Service - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Déployer une application ASP.NET Core sur Azure App Service, la première étape pour DevOps avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657744"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="b3f19-103">Déployer une application dans App Service</span><span class="sxs-lookup"><span data-stu-id="b3f19-103">Deploy an app to App Service</span></span>

<span data-ttu-id="b3f19-104">[Azure App service](/azure/app-service/) est la plateforme d’hébergement Web d’Azure.</span><span class="sxs-lookup"><span data-stu-id="b3f19-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="b3f19-105">Déploiement d’une application web sur Azure App Service peut être effectuée manuellement ou par un processus automatisé.</span><span class="sxs-lookup"><span data-stu-id="b3f19-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="b3f19-106">Cette section du guide décrit les méthodes de déploiement qui peuvent être déclenchées manuellement ou par script à l’aide de la ligne de commande, ou déclenchement manuellement à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3f19-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="b3f19-107">Dans cette section, vous allez effectuer les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b3f19-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="b3f19-108">Téléchargez et générez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-108">Download and build the sample app.</span></span>
* <span data-ttu-id="b3f19-109">Créer une application Web Azure App Service à l’aide d’Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b3f19-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="b3f19-110">Déployer l’exemple d’application dans Azure à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="b3f19-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="b3f19-111">Déployer une modification dans l’application à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3f19-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="b3f19-112">Ajouter un emplacement intermédiaire à l’application web.</span><span class="sxs-lookup"><span data-stu-id="b3f19-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="b3f19-113">Déployer une mise à jour dans l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="b3f19-114">Échanger les emplacements intermédiaires et de production.</span><span class="sxs-lookup"><span data-stu-id="b3f19-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="b3f19-115">Télécharger et tester l’application</span><span class="sxs-lookup"><span data-stu-id="b3f19-115">Download and test the app</span></span>

<span data-ttu-id="b3f19-116">L’application utilisée dans ce guide est une application ASP.NET Core pré-générée, un [lecteur de flux simple](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="b3f19-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="b3f19-117">Il s’agit d’une application Razor Pages qui utilise l’API `Microsoft.SyndicationFeed.ReaderWriter` pour récupérer un flux RSS/Atom et afficher les éléments d’actualité dans une liste.</span><span class="sxs-lookup"><span data-stu-id="b3f19-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="b3f19-118">N’hésitez pas à examiner le code, mais il est important de comprendre qu’il n’a rien de spécial à cette application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="b3f19-119">Il est simplement une application ASP.NET Core simple à titre d’illustration.</span><span class="sxs-lookup"><span data-stu-id="b3f19-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="b3f19-120">À partir d’un interpréteur de commandes, vous pouvez télécharger le code, générez le projet et exécutez-le comme suit.</span><span class="sxs-lookup"><span data-stu-id="b3f19-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="b3f19-121">*Remarque : les utilisateurs Linux/macOS doivent apporter les modifications appropriées pour les chemins d’accès, par exemple, à l’aide de la barre oblique (`/`) au lieu de la barre oblique inverse (`\`).*</span><span class="sxs-lookup"><span data-stu-id="b3f19-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="b3f19-122">Clonez le code dans un dossier sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b3f19-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="b3f19-123">Remplacez votre dossier de travail par le dossier *simple-Feed-Reader* qui a été créé.</span><span class="sxs-lookup"><span data-stu-id="b3f19-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="b3f19-124">Restaurer les packages et générez la solution.</span><span class="sxs-lookup"><span data-stu-id="b3f19-124">Restore the packages, and build the solution.</span></span>

    ```dotnetcli
    dotnet build
    ```

4. <span data-ttu-id="b3f19-125">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-125">Run the app.</span></span>

    ```dotnetcli
    dotnet run
    ```

    ![La commande dotnet run a réussi](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="b3f19-127">Ouvrez un navigateur et accédez à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b3f19-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="b3f19-128">L’application vous permet de taper ou coller un URL de flux de syndication et afficher une liste d’éléments d’actualités.</span><span class="sxs-lookup"><span data-stu-id="b3f19-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![L’application Affichage du contenu d’un flux RSS](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="b3f19-130">Une fois que vous êtes satisfait du bon fonctionnement de l’application, arrêtez-la en appuyant sur **Ctrl**+**C** dans l’interface de commande.</span><span class="sxs-lookup"><span data-stu-id="b3f19-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="b3f19-131">Créer l’application Web Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b3f19-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="b3f19-132">Pour déployer l’application, vous devez créer une [application Web](/azure/app-service/app-service-web-overview)App service.</span><span class="sxs-lookup"><span data-stu-id="b3f19-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="b3f19-133">Après la création de l’application Web, vous allez déployer à partir de votre ordinateur local à l’aide de Git à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="b3f19-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="b3f19-134">Connectez-vous à [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="b3f19-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="b3f19-135">Remarque : Lorsque vous connectez pour la première fois, Cloud Shell vous invite à créer un compte de stockage pour les fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="b3f19-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="b3f19-136">Acceptez les valeurs par défaut ou fournissez un nom unique.</span><span class="sxs-lookup"><span data-stu-id="b3f19-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="b3f19-137">Utiliser Cloud Shell pour les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="b3f19-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="b3f19-138">a.</span><span class="sxs-lookup"><span data-stu-id="b3f19-138">a.</span></span> <span data-ttu-id="b3f19-139">Déclarez une variable pour stocker le nom de votre application web.</span><span class="sxs-lookup"><span data-stu-id="b3f19-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="b3f19-140">Le nom doit être unique à utiliser dans l’URL par défaut.</span><span class="sxs-lookup"><span data-stu-id="b3f19-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="b3f19-141">L’utilisation de la fonction bash `$RANDOM` pour construire le nom garantit l’unicité et les résultats au format `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="b3f19-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="b3f19-142">b.</span><span class="sxs-lookup"><span data-stu-id="b3f19-142">b.</span></span> <span data-ttu-id="b3f19-143">Créez un groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="b3f19-143">Create a resource group.</span></span> <span data-ttu-id="b3f19-144">Groupes de ressources fournissent un moyen pour agréger les ressources Azure pour être gérés en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="b3f19-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="b3f19-145">La commande `az` appelle la [Azure CLI](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="b3f19-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="b3f19-146">L’interface CLI peut être exécuté localement, mais l’utiliser dans l’interpréteur de gagner du temps et configuration.</span><span class="sxs-lookup"><span data-stu-id="b3f19-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="b3f19-147">c.</span><span class="sxs-lookup"><span data-stu-id="b3f19-147">c.</span></span> <span data-ttu-id="b3f19-148">Créer un plan App Service dans le niveau S1.</span><span class="sxs-lookup"><span data-stu-id="b3f19-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="b3f19-149">Un plan App Service est un regroupement d’applications web qui partagent le même niveau tarifaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="b3f19-150">Le niveau S1 n’est pas gratuit, mais il est requis pour la fonctionnalité d’emplacements intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="b3f19-151">d.</span><span class="sxs-lookup"><span data-stu-id="b3f19-151">d.</span></span> <span data-ttu-id="b3f19-152">Créer la ressource d’application web à l’aide du plan App Service dans le même groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="b3f19-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="b3f19-153">e.</span><span class="sxs-lookup"><span data-stu-id="b3f19-153">e.</span></span> <span data-ttu-id="b3f19-154">Définir les informations d’identification de déploiement.</span><span class="sxs-lookup"><span data-stu-id="b3f19-154">Set the deployment credentials.</span></span> <span data-ttu-id="b3f19-155">Ces informations d’identification de déploiement s’appliquent à toutes les applications web dans votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="b3f19-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="b3f19-156">N’utilisez pas les caractères spéciaux dans le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b3f19-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="b3f19-157">f.</span><span class="sxs-lookup"><span data-stu-id="b3f19-157">f.</span></span> <span data-ttu-id="b3f19-158">Configurez l’application Web pour accepter les déploiements à partir du Git local et afficher l' *URL de déploiement git*.</span><span class="sxs-lookup"><span data-stu-id="b3f19-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="b3f19-159">**Notez cette URL pour référence ultérieure**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="b3f19-160">g.</span><span class="sxs-lookup"><span data-stu-id="b3f19-160">g.</span></span> <span data-ttu-id="b3f19-161">Affichez l’URL de l' *application Web*.</span><span class="sxs-lookup"><span data-stu-id="b3f19-161">Display the *web app URL*.</span></span> <span data-ttu-id="b3f19-162">Accédez à cette URL pour voir l’application web vide.</span><span class="sxs-lookup"><span data-stu-id="b3f19-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="b3f19-163">**Notez cette URL pour référence ultérieure**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="b3f19-164">À l’aide d’un interpréteur de commandes sur votre ordinateur local, accédez au dossier du projet de l’application Web (par exemple, `.\simple-feed-reader\SimpleFeedReader`).</span><span class="sxs-lookup"><span data-stu-id="b3f19-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="b3f19-165">Exécutez les commandes suivantes pour configurer Git pour pousser vers l’URL de déploiement :</span><span class="sxs-lookup"><span data-stu-id="b3f19-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="b3f19-166">a.</span><span class="sxs-lookup"><span data-stu-id="b3f19-166">a.</span></span> <span data-ttu-id="b3f19-167">Ajoutez l’URL distante dans le référentiel local.</span><span class="sxs-lookup"><span data-stu-id="b3f19-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="b3f19-168">b.</span><span class="sxs-lookup"><span data-stu-id="b3f19-168">b.</span></span> <span data-ttu-id="b3f19-169">Envoyez la branche *principale* locale à la branche *principale* d' *Azure-prod* Remote.</span><span class="sxs-lookup"><span data-stu-id="b3f19-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="b3f19-170">Vous êtes invité pour les informations d’identification de déploiement que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="b3f19-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="b3f19-171">Observez la sortie dans l’interface de commande.</span><span class="sxs-lookup"><span data-stu-id="b3f19-171">Observe the output in the command shell.</span></span> <span data-ttu-id="b3f19-172">Azure génère l’application ASP.NET Core à distance.</span><span class="sxs-lookup"><span data-stu-id="b3f19-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="b3f19-173">Dans un navigateur, accédez à l' *URL de l’application Web* et notez que l’application a été générée et déployée.</span><span class="sxs-lookup"><span data-stu-id="b3f19-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="b3f19-174">Des modifications supplémentaires peuvent être validées dans le référentiel Git local avec `git commit`.</span><span class="sxs-lookup"><span data-stu-id="b3f19-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="b3f19-175">Ces modifications sont transmises à Azure avec la commande `git push` précédente.</span><span class="sxs-lookup"><span data-stu-id="b3f19-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="b3f19-176">Déploiement avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3f19-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="b3f19-177">*Remarque : cette section s’applique uniquement à Windows. Les utilisateurs de Linux et macOS doivent apporter la modification décrite à l’étape 2 ci-dessous. Enregistrez le fichier et validez la modification dans le référentiel local avec `git commit`. Enfin, envoyez la modification avec `git push`, comme dans la première section.*</span><span class="sxs-lookup"><span data-stu-id="b3f19-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="b3f19-178">L’application a déjà été déployée à partir de l’interface de commande.</span><span class="sxs-lookup"><span data-stu-id="b3f19-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="b3f19-179">Nous allons utiliser les outils intégrés de Visual Studio pour déployer une mise à jour à l’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="b3f19-180">Dans les coulisses, Visual Studio effectue la même opération en tant que l’outils en ligne de commande, mais au sein de l’interface familière de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3f19-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="b3f19-181">Ouvrez *SimpleFeedReader. sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3f19-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="b3f19-182">Dans Explorateur de solutions, ouvrez *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b3f19-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="b3f19-183">Remplacez `<h2>Simple Feed Reader</h2>` par `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="b3f19-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="b3f19-184">Appuyez sur **Ctrl**+**MAJ**+**B** pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="b3f19-185">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Capture d’écran montrant le clic droit, publier](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="b3f19-187">Visual Studio peut créer une ressource App Service, mais cette mise à jour sera publiée sur le déploiement existant.</span><span class="sxs-lookup"><span data-stu-id="b3f19-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="b3f19-188">Dans la boîte de dialogue **choisir une cible de publication** , sélectionnez **app service** dans la liste à gauche, puis sélectionnez **Sélectionner existant**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="b3f19-189">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-189">Click **Publish**.</span></span>
6. <span data-ttu-id="b3f19-190">Dans la boîte de dialogue **app service** , vérifiez que le compte Microsoft ou professionnel utilisé pour créer votre abonnement Azure s’affiche dans l’angle supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="b3f19-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="b3f19-191">Si elle n’est pas le cas, cliquez sur la liste déroulante et ajoutez-le.</span><span class="sxs-lookup"><span data-stu-id="b3f19-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="b3f19-192">Confirmez que l' **abonnement** Azure correct est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b3f19-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="b3f19-193">Pour **Afficher**, sélectionnez **groupe de ressources**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="b3f19-194">Développez le groupe de ressources **AzureTutorial** , puis sélectionnez l’application Web existante.</span><span class="sxs-lookup"><span data-stu-id="b3f19-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="b3f19-195">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-195">Click **OK**.</span></span>

    ![Boîte de dialogue Publier le Service application capture d’écran](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="b3f19-197">Visual Studio génère et déploie l’application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="b3f19-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="b3f19-198">Accédez à l’URL de l’application web.</span><span class="sxs-lookup"><span data-stu-id="b3f19-198">Browse to the web app URL.</span></span> <span data-ttu-id="b3f19-199">Vérifiez que la modification de l’élément de `<h2>` est active.</span><span class="sxs-lookup"><span data-stu-id="b3f19-199">Validate that the `<h2>` element modification is live.</span></span>

![L’application avec le titre a été modifié](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="b3f19-201">Emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="b3f19-201">Deployment slots</span></span>

<span data-ttu-id="b3f19-202">Emplacements de déploiement prend en charge la mise en lots de modifications sans affecter l’application en cours d’exécution en production.</span><span class="sxs-lookup"><span data-stu-id="b3f19-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="b3f19-203">Une fois que la version intermédiaire de l’application est validée par une équipe d’assurance qualité, la production et emplacements intermédiaires peuvent être échangés.</span><span class="sxs-lookup"><span data-stu-id="b3f19-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="b3f19-204">L’application dans un environnement intermédiaire est promue en production de cette manière.</span><span class="sxs-lookup"><span data-stu-id="b3f19-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="b3f19-205">Les étapes suivantes créer un emplacement intermédiaire, déploiement des modifications à ce dernier et échanger l’emplacement intermédiaire avec production après vérification.</span><span class="sxs-lookup"><span data-stu-id="b3f19-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="b3f19-206">Connectez-vous au [Azure Cloud Shell](https://shell.azure.com/bash), s’il n’est pas déjà connecté.</span><span class="sxs-lookup"><span data-stu-id="b3f19-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="b3f19-207">Créez l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-207">Create the staging slot.</span></span>

    <span data-ttu-id="b3f19-208">a.</span><span class="sxs-lookup"><span data-stu-id="b3f19-208">a.</span></span> <span data-ttu-id="b3f19-209">Créez un emplacement de déploiement avec le nom *intermédiaire*.</span><span class="sxs-lookup"><span data-stu-id="b3f19-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="b3f19-210">b.</span><span class="sxs-lookup"><span data-stu-id="b3f19-210">b.</span></span> <span data-ttu-id="b3f19-211">Configurez l’emplacement intermédiaire pour utiliser le déploiement à partir de Git local et récupérez l’URL de déploiement **intermédiaire** .</span><span class="sxs-lookup"><span data-stu-id="b3f19-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="b3f19-212">**Notez cette URL pour référence ultérieure**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="b3f19-213">c.</span><span class="sxs-lookup"><span data-stu-id="b3f19-213">c.</span></span> <span data-ttu-id="b3f19-214">Afficher l’URL de l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-214">Display the staging slot's URL.</span></span> <span data-ttu-id="b3f19-215">Accédez à l’URL pour afficher l’emplacement intermédiaire vide.</span><span class="sxs-lookup"><span data-stu-id="b3f19-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="b3f19-216">**Notez cette URL pour référence ultérieure**.</span><span class="sxs-lookup"><span data-stu-id="b3f19-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="b3f19-217">Dans un éditeur de texte ou Visual Studio, modifiez *pages/index. cshtml* à nouveau afin que l’élément `<h2>` Lise `<h2>Simple Feed Reader - V3</h2>` et enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="b3f19-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="b3f19-218">Validez le fichier dans le référentiel Git local, à l’aide de la page **modifications** dans l’onglet *Team Explorer* de Visual Studio, ou en entrant la commande suivante à l’aide de l’interface de commande de l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="b3f19-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. <span data-ttu-id="b3f19-219">À l’aide du shell de commande de l’ordinateur local, ajoutez l’URL de déploiement intermédiaire en tant que Git distant et push les modifications validées :</span><span class="sxs-lookup"><span data-stu-id="b3f19-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="b3f19-220">a.</span><span class="sxs-lookup"><span data-stu-id="b3f19-220">a.</span></span> <span data-ttu-id="b3f19-221">Ajoutez l’URL distante pour l’environnement intermédiaire dans le référentiel Git local.</span><span class="sxs-lookup"><span data-stu-id="b3f19-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="b3f19-222">b.</span><span class="sxs-lookup"><span data-stu-id="b3f19-222">b.</span></span> <span data-ttu-id="b3f19-223">Transmettent la branche *maître* locale à la branche *principale* *Azure-Staging* distante.</span><span class="sxs-lookup"><span data-stu-id="b3f19-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="b3f19-224">Patientez pendant que Azure génère et déploie l’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="b3f19-225">Pour vérifier que V3 a été déployé sur l’emplacement intermédiaire, ouvrez deux fenêtres de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3f19-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="b3f19-226">Dans une fenêtre, accédez à l’URL d’origine de l’application web.</span><span class="sxs-lookup"><span data-stu-id="b3f19-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="b3f19-227">Dans l’autre fenêtre, accédez à l’URL d’application web intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="b3f19-228">L’URL de production sert V2 de l’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="b3f19-229">L’URL intermédiaire sert V3 de l’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-229">The staging URL serves V3 of the app.</span></span>

    ![Capture d’écran comparant les fenêtres du navigateur](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="b3f19-231">Dans l’interpréteur de commandes, remplacez l’emplacement intermédiaire vérifiée/chaud, les en production.</span><span class="sxs-lookup"><span data-stu-id="b3f19-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="b3f19-232">Vérifiez que l’échange s’est produite en actualisant les fenêtres de deux navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3f19-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Comparaison des fenêtres de navigateur après la permutation](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="b3f19-234">Résumé</span><span class="sxs-lookup"><span data-stu-id="b3f19-234">Summary</span></span>

<span data-ttu-id="b3f19-235">Dans cette section, les tâches suivantes ont été effectuées :</span><span class="sxs-lookup"><span data-stu-id="b3f19-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="b3f19-236">Téléchargé et généré l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="b3f19-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="b3f19-237">Créé une application Web Azure App Service à l’aide d’Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="b3f19-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="b3f19-238">Déployé l’exemple d’application sur Azure à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="b3f19-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="b3f19-239">Déployé une modification à l’application à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3f19-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="b3f19-240">Ajouter un emplacement intermédiaire à l’application web.</span><span class="sxs-lookup"><span data-stu-id="b3f19-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="b3f19-241">Déployé une mise à jour dans l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="b3f19-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="b3f19-242">Échanger les emplacements intermédiaires et de production.</span><span class="sxs-lookup"><span data-stu-id="b3f19-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="b3f19-243">Dans la section suivante, vous allez apprendre à créer un pipeline DevOps avec les Pipelines d’Azure.</span><span class="sxs-lookup"><span data-stu-id="b3f19-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="b3f19-244">Documentation supplémentaire</span><span class="sxs-lookup"><span data-stu-id="b3f19-244">Additional reading</span></span>

* [<span data-ttu-id="b3f19-245">Vue d'ensemble de Web Apps</span><span class="sxs-lookup"><span data-stu-id="b3f19-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="b3f19-246">Créer une application Web .NET Core et SQL Database dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b3f19-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="b3f19-247">Configurer les informations d’identification de déploiement pour Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b3f19-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="b3f19-248">Configurer des environnements intermédiaires dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b3f19-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
