---
title: DevOps avec ASP.NET Core et Azure | Déployer une application dans App Service
author: CamSoper
description: Un guide qui fournit des conseils de bout en bout sur la création d’un pipeline DevOps pour une application ASP.NET Core hébergée dans Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: 33026ed510aae63a9e580aa5d708f94aad778fca
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090935"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="cd425-103">Déployer une application dans App Service</span><span class="sxs-lookup"><span data-stu-id="cd425-103">Deploy an app to App Service</span></span>

<span data-ttu-id="cd425-104">[Azure App Service](/azure/app-service/) est Azure web plateforme d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="cd425-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="cd425-105">Déploiement d’une application web sur Azure App Service peut être effectuée manuellement ou par un processus automatisé.</span><span class="sxs-lookup"><span data-stu-id="cd425-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="cd425-106">Cette section du guide décrit les méthodes de déploiement qui peuvent être déclenchées manuellement ou par script à l’aide de la ligne de commande, ou déclenchement manuellement à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd425-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="cd425-107">Dans cette section, vous allez effectuer les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="cd425-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="cd425-108">Téléchargez et générez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-108">Download and build the sample app.</span></span>
* <span data-ttu-id="cd425-109">Créer une application Web Azure App Service à l’aide d’Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="cd425-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="cd425-110">Déployer l’exemple d’application dans Azure à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="cd425-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="cd425-111">Déployer une modification dans l’application à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd425-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="cd425-112">Ajouter un emplacement intermédiaire à l’application web.</span><span class="sxs-lookup"><span data-stu-id="cd425-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="cd425-113">Déployer une mise à jour dans l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="cd425-114">Échanger les emplacements intermédiaires et de production.</span><span class="sxs-lookup"><span data-stu-id="cd425-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="cd425-115">Télécharger et tester l’application</span><span class="sxs-lookup"><span data-stu-id="cd425-115">Download and test the app</span></span>

<span data-ttu-id="cd425-116">L’application utilisée dans ce guide est une application ASP.NET Core prédéfinie, [lecteur de flux Simple](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="cd425-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="cd425-117">Il s’agit d’une application Pages Razor qui utilise le `Microsoft.SyndicationFeed.ReaderWriter` API pour récupérer un flux RSS/Atom et afficher les éléments d’informations dans une liste.</span><span class="sxs-lookup"><span data-stu-id="cd425-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="cd425-118">N’hésitez pas à examiner le code, mais il est important de comprendre qu’il n’a rien de spécial à cette application.</span><span class="sxs-lookup"><span data-stu-id="cd425-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="cd425-119">Il est simplement une application ASP.NET Core simple à titre d’illustration.</span><span class="sxs-lookup"><span data-stu-id="cd425-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="cd425-120">À partir d’un interpréteur de commandes, vous pouvez télécharger le code, générez le projet et exécutez-le comme suit.</span><span class="sxs-lookup"><span data-stu-id="cd425-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="cd425-121">*Remarque : Linux/Mac OS les utilisateurs doivent apporter les modifications appropriées pour les chemins d’accès, par exemple, à l’aide de la barre oblique (`/`) au lieu de la barre oblique inverse (`\`).*</span><span class="sxs-lookup"><span data-stu-id="cd425-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="cd425-122">Clonez le code dans un dossier sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="cd425-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="cd425-123">Modifier votre dossier de travail pour le *lecture de flux simple* dossier a été créé.</span><span class="sxs-lookup"><span data-stu-id="cd425-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="cd425-124">Restaurer les packages et générez la solution.</span><span class="sxs-lookup"><span data-stu-id="cd425-124">Restore the packages, and build the solution.</span></span>

    ```console
    dotnet build
    ```

4. <span data-ttu-id="cd425-125">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-125">Run the app.</span></span>

    ```console
    dotnet run
    ```

    ![La commande dotnet run a réussi](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="cd425-127">Ouvrez un navigateur et accédez à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cd425-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="cd425-128">L’application vous permet de taper ou coller un URL de flux de syndication et afficher une liste d’éléments d’actualités.</span><span class="sxs-lookup"><span data-stu-id="cd425-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![L’application Affichage du contenu d’un flux RSS](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="cd425-130">Une fois que vous êtes satisfait de l’application fonctionne correctement, l’arrêter en appuyant sur **Ctrl**+**C** dans l’interface de commande.</span><span class="sxs-lookup"><span data-stu-id="cd425-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="cd425-131">Créer l’application Web Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cd425-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="cd425-132">Pour déployer l’application, vous devez créer un Service d’application [application Web](/azure/app-service/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="cd425-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="cd425-133">Après la création de l’application Web, vous allez déployer à partir de votre ordinateur local à l’aide de Git à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="cd425-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="cd425-134">Se connecter à la [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="cd425-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="cd425-135">Remarque : Lorsque vous connectez pour la première fois, Cloud Shell vous invite à créer un compte de stockage pour les fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="cd425-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="cd425-136">Acceptez les valeurs par défaut ou fournissez un nom unique.</span><span class="sxs-lookup"><span data-stu-id="cd425-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="cd425-137">Utiliser Cloud Shell pour les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="cd425-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="cd425-138">a.</span><span class="sxs-lookup"><span data-stu-id="cd425-138">a.</span></span> <span data-ttu-id="cd425-139">Déclarez une variable pour stocker le nom de votre application web.</span><span class="sxs-lookup"><span data-stu-id="cd425-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="cd425-140">Le nom doit être unique à utiliser dans l’URL par défaut.</span><span class="sxs-lookup"><span data-stu-id="cd425-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="cd425-141">À l’aide de la `$RANDOM` fonction Bash pour construire le nom garantit l’unicité et des résultats dans le format `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="cd425-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="cd425-142">b.</span><span class="sxs-lookup"><span data-stu-id="cd425-142">b.</span></span> <span data-ttu-id="cd425-143">Créer un groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="cd425-143">Create a resource group.</span></span> <span data-ttu-id="cd425-144">Groupes de ressources fournissent un moyen pour agréger les ressources Azure pour être gérés en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="cd425-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="cd425-145">Le `az` commande appelle le [Azure CLI](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="cd425-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="cd425-146">L’interface CLI peut être exécuté localement, mais l’utiliser dans l’interpréteur de gagner du temps et configuration.</span><span class="sxs-lookup"><span data-stu-id="cd425-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="cd425-147">c.</span><span class="sxs-lookup"><span data-stu-id="cd425-147">c.</span></span> <span data-ttu-id="cd425-148">Créer un plan App Service dans le niveau S1.</span><span class="sxs-lookup"><span data-stu-id="cd425-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="cd425-149">Un plan App Service est un regroupement d’applications web qui partagent le même niveau tarifaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="cd425-150">Le niveau S1 n’est pas gratuit, mais il est requis pour la fonctionnalité d’emplacements intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="cd425-151">d.</span><span class="sxs-lookup"><span data-stu-id="cd425-151">d.</span></span> <span data-ttu-id="cd425-152">Créer la ressource d’application web à l’aide du plan App Service dans le même groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="cd425-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="cd425-153">e.</span><span class="sxs-lookup"><span data-stu-id="cd425-153">e.</span></span> <span data-ttu-id="cd425-154">Définir les informations d’identification de déploiement.</span><span class="sxs-lookup"><span data-stu-id="cd425-154">Set the deployment credentials.</span></span> <span data-ttu-id="cd425-155">Ces informations d’identification de déploiement s’appliquent à toutes les applications web dans votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="cd425-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="cd425-156">N’utilisez pas les caractères spéciaux dans le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cd425-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="cd425-157">f.</span><span class="sxs-lookup"><span data-stu-id="cd425-157">f.</span></span> <span data-ttu-id="cd425-158">Configurer l’application web pour accepter les déploiements à partir de Git local et l’affichage le *URL de déploiement Git*.</span><span class="sxs-lookup"><span data-stu-id="cd425-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="cd425-159">**Notez cette URL pour faire référence ultérieurement**.</span><span class="sxs-lookup"><span data-stu-id="cd425-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="cd425-160">g.</span><span class="sxs-lookup"><span data-stu-id="cd425-160">g.</span></span> <span data-ttu-id="cd425-161">Afficher le *URL d’application web*.</span><span class="sxs-lookup"><span data-stu-id="cd425-161">Display the *web app URL*.</span></span> <span data-ttu-id="cd425-162">Accédez à cette URL pour voir l’application web vide.</span><span class="sxs-lookup"><span data-stu-id="cd425-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="cd425-163">**Notez cette URL pour faire référence ultérieurement**.</span><span class="sxs-lookup"><span data-stu-id="cd425-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="cd425-164">À l’aide d’une invite de commandes sur votre ordinateur local, accédez au dossier de projet de l’application web (par exemple, `.\simple-feed-reader\SimpleFeedReader`).</span><span class="sxs-lookup"><span data-stu-id="cd425-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="cd425-165">Exécutez les commandes suivantes pour configurer Git pour pousser vers l’URL de déploiement :</span><span class="sxs-lookup"><span data-stu-id="cd425-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="cd425-166">a.</span><span class="sxs-lookup"><span data-stu-id="cd425-166">a.</span></span> <span data-ttu-id="cd425-167">Ajoutez l’URL distante dans le référentiel local.</span><span class="sxs-lookup"><span data-stu-id="cd425-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="cd425-168">b.</span><span class="sxs-lookup"><span data-stu-id="cd425-168">b.</span></span> <span data-ttu-id="cd425-169">Push local *master* créer une branche vers le *azure-prod* à distance *master* branche.</span><span class="sxs-lookup"><span data-stu-id="cd425-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="cd425-170">Vous êtes invité pour les informations d’identification de déploiement que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="cd425-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="cd425-171">Observez la sortie dans l’interface de commande.</span><span class="sxs-lookup"><span data-stu-id="cd425-171">Observe the output in the command shell.</span></span> <span data-ttu-id="cd425-172">Azure génère l’application ASP.NET Core à distance.</span><span class="sxs-lookup"><span data-stu-id="cd425-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="cd425-173">Dans un navigateur, accédez à la *URL d’application Web* et notez l’application a été créée et déployée.</span><span class="sxs-lookup"><span data-stu-id="cd425-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="cd425-174">Des modifications supplémentaires peuvent être validées sur le référentiel Git local avec `git commit`.</span><span class="sxs-lookup"><span data-stu-id="cd425-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="cd425-175">Ces modifications sont envoyées vers Azure avec l’exemple précédent `git push` commande.</span><span class="sxs-lookup"><span data-stu-id="cd425-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="cd425-176">Déploiement avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd425-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="cd425-177">*Remarque : Cette section s’applique à Windows uniquement. Les utilisateurs de Linux et macOS doivent apporter la modification décrite à l’étape 2 ci-dessous. Enregistrez le fichier et valider la modification dans le référentiel local avec `git commit`. Enfin, transmettre la modification avec `git push`, comme illustré dans la première section.*</span><span class="sxs-lookup"><span data-stu-id="cd425-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="cd425-178">L’application a déjà été déployée à partir de l’interface de commande.</span><span class="sxs-lookup"><span data-stu-id="cd425-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="cd425-179">Nous allons utiliser les outils intégrés de Visual Studio pour déployer une mise à jour à l’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="cd425-180">Dans les coulisses, Visual Studio effectue la même opération en tant que l’outils en ligne de commande, mais au sein de l’interface familière de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd425-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="cd425-181">Ouvrez *SimpleFeedReader.sln* dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd425-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="cd425-182">Dans l’Explorateur de solutions, ouvrez *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cd425-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="cd425-183">Modification `<h2>Simple Feed Reader</h2>` à `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="cd425-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="cd425-184">Appuyez sur **Ctrl**+**MAJ**+**B** pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="cd425-185">Dans l’Explorateur de solutions, avec le bouton droit sur le projet, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="cd425-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Avec le bouton droit, publier](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="cd425-187">Visual Studio peut créer une ressource App Service, mais cette mise à jour sera publiée sur le déploiement existant.</span><span class="sxs-lookup"><span data-stu-id="cd425-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="cd425-188">Dans le **choisir une cible de publication** boîte de dialogue, sélectionnez **App Service** dans la liste sur la gauche, puis sélectionnez **sélectionner**.</span><span class="sxs-lookup"><span data-stu-id="cd425-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="cd425-189">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="cd425-189">Click **Publish**.</span></span>
6. <span data-ttu-id="cd425-190">Dans le **App Service** boîte de dialogue, vérifiez que Microsoft ou compte professionnel utilisé pour créer votre abonnement Azure est affiché dans le coin supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="cd425-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="cd425-191">Si elle n’est pas le cas, cliquez sur la liste déroulante et ajoutez-le.</span><span class="sxs-lookup"><span data-stu-id="cd425-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="cd425-192">Vérifiez que le Azure correct **abonnement** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="cd425-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="cd425-193">Pour **vue**, sélectionnez **groupe de ressources**.</span><span class="sxs-lookup"><span data-stu-id="cd425-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="cd425-194">Développez le **AzureTutorial** groupe de ressources et sélectionnez l’application web existante.</span><span class="sxs-lookup"><span data-stu-id="cd425-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="cd425-195">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd425-195">Click **OK**.</span></span>

    ![App Service de boîte de dialogue Publier](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="cd425-197">Visual Studio génère et déploie l’application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="cd425-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="cd425-198">Accédez à l’URL de l’application web.</span><span class="sxs-lookup"><span data-stu-id="cd425-198">Browse to the web app URL.</span></span> <span data-ttu-id="cd425-199">Vérifier que le `<h2>` modification de l’élément est en ligne.</span><span class="sxs-lookup"><span data-stu-id="cd425-199">Validate that the `<h2>` element modification is live.</span></span>

![L’application avec le titre a été modifié](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="cd425-201">Emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="cd425-201">Deployment slots</span></span>

<span data-ttu-id="cd425-202">Emplacements de déploiement prend en charge la mise en lots de modifications sans affecter l’application en cours d’exécution en production.</span><span class="sxs-lookup"><span data-stu-id="cd425-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="cd425-203">Une fois que la version intermédiaire de l’application est validée par une équipe d’assurance qualité, la production et emplacements intermédiaires peuvent être échangés.</span><span class="sxs-lookup"><span data-stu-id="cd425-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="cd425-204">L’application dans un environnement intermédiaire est promue en production de cette manière.</span><span class="sxs-lookup"><span data-stu-id="cd425-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="cd425-205">Les étapes suivantes créer un emplacement intermédiaire, déploiement des modifications à ce dernier et échanger l’emplacement intermédiaire avec production après vérification.</span><span class="sxs-lookup"><span data-stu-id="cd425-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="cd425-206">Se connecter à la [Azure Cloud Shell](https://shell.azure.com/bash), si pas déjà connecté.</span><span class="sxs-lookup"><span data-stu-id="cd425-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="cd425-207">Créez l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-207">Create the staging slot.</span></span>

    <span data-ttu-id="cd425-208">a.</span><span class="sxs-lookup"><span data-stu-id="cd425-208">a.</span></span> <span data-ttu-id="cd425-209">Créer un emplacement de déploiement avec le nom *intermédiaire*.</span><span class="sxs-lookup"><span data-stu-id="cd425-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="cd425-210">b.</span><span class="sxs-lookup"><span data-stu-id="cd425-210">b.</span></span> <span data-ttu-id="cd425-211">Configurer l’emplacement intermédiaire pour utiliser un déploiement à partir de Git local et get le **intermédiaire** URL de déploiement.</span><span class="sxs-lookup"><span data-stu-id="cd425-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="cd425-212">**Notez cette URL pour faire référence ultérieurement**.</span><span class="sxs-lookup"><span data-stu-id="cd425-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="cd425-213">c.</span><span class="sxs-lookup"><span data-stu-id="cd425-213">c.</span></span> <span data-ttu-id="cd425-214">Afficher l’URL de l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-214">Display the staging slot's URL.</span></span> <span data-ttu-id="cd425-215">Accédez à l’URL pour afficher l’emplacement intermédiaire vide.</span><span class="sxs-lookup"><span data-stu-id="cd425-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="cd425-216">**Notez cette URL pour faire référence ultérieurement**.</span><span class="sxs-lookup"><span data-stu-id="cd425-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="cd425-217">Dans un éditeur de texte ou de Visual Studio, modifiez *pages/index.cshtml* à nouveau afin que le `<h2>` élément lit `<h2>Simple Feed Reader - V3</h2>` et enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="cd425-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="cd425-218">Valider le fichier dans le référentiel Git local, à l’aide du **modifications** page dans Visual Studio *Team Explorer* onglet, ou en entrant ce qui suit à l’aide de shell de commande de l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="cd425-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. <span data-ttu-id="cd425-219">À l’aide du shell de commande de l’ordinateur local, ajoutez l’URL de déploiement intermédiaire en tant que Git distant et push les modifications validées :</span><span class="sxs-lookup"><span data-stu-id="cd425-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="cd425-220">a.</span><span class="sxs-lookup"><span data-stu-id="cd425-220">a.</span></span> <span data-ttu-id="cd425-221">Ajoutez l’URL distante pour l’environnement intermédiaire dans le référentiel Git local.</span><span class="sxs-lookup"><span data-stu-id="cd425-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="cd425-222">b.</span><span class="sxs-lookup"><span data-stu-id="cd425-222">b.</span></span> <span data-ttu-id="cd425-223">Push local *master* créer une branche vers le *intermédiaire d’azure* à distance *master* branche.</span><span class="sxs-lookup"><span data-stu-id="cd425-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="cd425-224">Patientez pendant que Azure génère et déploie l’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="cd425-225">Pour vérifier que V3 a été déployé sur l’emplacement intermédiaire, ouvrez deux fenêtres de navigateur.</span><span class="sxs-lookup"><span data-stu-id="cd425-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="cd425-226">Dans une fenêtre, accédez à l’URL d’origine de l’application web.</span><span class="sxs-lookup"><span data-stu-id="cd425-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="cd425-227">Dans l’autre fenêtre, accédez à l’URL d’application web intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="cd425-228">L’URL de production sert V2 de l’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="cd425-229">L’URL intermédiaire sert V3 de l’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-229">The staging URL serves V3 of the app.</span></span>

    ![Comparaison des fenêtres de navigateur](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="cd425-231">Dans l’interpréteur de commandes, remplacez l’emplacement intermédiaire vérifiée/chaud, les en production.</span><span class="sxs-lookup"><span data-stu-id="cd425-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="cd425-232">Vérifiez que l’échange s’est produite en actualisant les fenêtres de deux navigateur.</span><span class="sxs-lookup"><span data-stu-id="cd425-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Comparaison des fenêtres de navigateur après la permutation](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="cd425-234">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="cd425-234">Summary</span></span>

<span data-ttu-id="cd425-235">Dans cette section, les tâches suivantes ont été effectuées :</span><span class="sxs-lookup"><span data-stu-id="cd425-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="cd425-236">Téléchargé et généré l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="cd425-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="cd425-237">Créé une application Web Azure App Service à l’aide d’Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="cd425-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="cd425-238">Déployé l’exemple d’application sur Azure à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="cd425-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="cd425-239">Déployé une modification à l’application à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd425-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="cd425-240">Ajouter un emplacement intermédiaire à l’application web.</span><span class="sxs-lookup"><span data-stu-id="cd425-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="cd425-241">Déployé une mise à jour dans l’emplacement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="cd425-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="cd425-242">Échanger les emplacements intermédiaires et de production.</span><span class="sxs-lookup"><span data-stu-id="cd425-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="cd425-243">Dans la section suivante, vous allez apprendre à créer un pipeline DevOps avec les Pipelines d’Azure.</span><span class="sxs-lookup"><span data-stu-id="cd425-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="cd425-244">Lecture supplémentaire</span><span class="sxs-lookup"><span data-stu-id="cd425-244">Additional reading</span></span>

* [<span data-ttu-id="cd425-245">Vue d’ensemble des applications Web</span><span class="sxs-lookup"><span data-stu-id="cd425-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="cd425-246">Créer une application web .NET Core et SQL Database dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cd425-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="cd425-247">Configurer les informations d’identification de déploiement pour Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cd425-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="cd425-248">Configurer des environnements intermédiaires dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cd425-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
