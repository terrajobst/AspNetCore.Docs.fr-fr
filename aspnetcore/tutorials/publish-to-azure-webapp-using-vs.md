---
title: Publier une application ASP.NET Core sur Azure avec Visual Studio
author: rick-anderson
description: Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 98dea9cfbafb53de8ba82ef06f4885d9fbf50123
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705615"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a><span data-ttu-id="8eb35-103">Publier une application ASP.NET Core sur Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8eb35-103">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>

<span data-ttu-id="8eb35-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="8eb35-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="8eb35-105">Consultez [Publier sur Azure à partir de Visual Studio pour Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) si vous utilisez macOS.</span><span class="sxs-lookup"><span data-stu-id="8eb35-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on macOS.</span></span>

<span data-ttu-id="8eb35-106">Pour résoudre un problème de déploiement App Service, consultez <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="8eb35-106">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="set-up"></a><span data-ttu-id="8eb35-107">Installer</span><span class="sxs-lookup"><span data-stu-id="8eb35-107">Set up</span></span>

* <span data-ttu-id="8eb35-108">Ouvrez un [compte Azure gratuit](https://azure.microsoft.com/free/dotnet/) si vous n’en avez pas.</span><span class="sxs-lookup"><span data-stu-id="8eb35-108">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="8eb35-109">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="8eb35-109">Create a web app</span></span>

<span data-ttu-id="8eb35-110">Dans la page de démarrage de Visual Studio, sélectionnez **Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="8eb35-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menu Fichier](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="8eb35-112">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="8eb35-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8eb35-113">Dans le volet gauche, sélectionnez **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="8eb35-114">Dans le volet central, sélectionnez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8eb35-115">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-115">Select **OK**.</span></span>

![Boîte de dialogue Nouveau projet](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="8eb35-117">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="8eb35-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="8eb35-118">Sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-118">Select **Web Application**.</span></span>
* <span data-ttu-id="8eb35-119">Sélectionnez **Modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-119">Select **Change Authentication**.</span></span>

![Boîte de dialogue Nouveau projet](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="8eb35-121">La boîte de dialogue **Modifier l’authentification** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="8eb35-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="8eb35-122">Sélectionnez **Comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="8eb35-123">Sélectionnez **OK** pour revenir à la **nouvelle application web ASP.NET Core**, puis sélectionnez à nouveau **OK**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Boîte de dialogue Nouvelle application web ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="8eb35-125">Visual Studio crée la solution.</span><span class="sxs-lookup"><span data-stu-id="8eb35-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8eb35-126">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="8eb35-126">Run the app</span></span>

* <span data-ttu-id="8eb35-127">Appuyez sur CTRL+F5 pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="8eb35-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="8eb35-128">Testez les liens **À propos de** et **Contact**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-128">Test the **About** and **Contact** links.</span></span>

![Application web ouverte dans Microsoft Edge sur localhost](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="8eb35-130">Inscrire un utilisateur</span><span class="sxs-lookup"><span data-stu-id="8eb35-130">Register a user</span></span>

* <span data-ttu-id="8eb35-131">Sélectionnez **S’inscrire**, puis inscrivez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8eb35-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="8eb35-132">Vous pouvez utiliser une adresse e-mail fictive.</span><span class="sxs-lookup"><span data-stu-id="8eb35-132">You can use a fictitious email address.</span></span> <span data-ttu-id="8eb35-133">Quand vous effectuez l’envoi, la page affiche l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="8eb35-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="8eb35-134">*« Erreur de serveur interne : Une opération de base de données a échoué lors du traitement de la requête. Exception SQL : Impossible d'ouvrir la base de données. Appliquer des migrations existantes pour le contexte de base de données d’application peut résoudre ce problème. »*</span><span class="sxs-lookup"><span data-stu-id="8eb35-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="8eb35-135">Sélectionnez **Appliquer les migrations**, puis, une fois la page mise à jour, actualisez-la.</span><span class="sxs-lookup"><span data-stu-id="8eb35-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erreur de serveur interne : Une opération de base de données a échoué lors du traitement de la requête.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="8eb35-139">L’application affiche l’adresse e-mail utilisée pour inscrire le nouvel utilisateur et un lien **Se déconnecter**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Application web ouverte dans Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="8eb35-142">Déployer l’application sur Azure</span><span class="sxs-lookup"><span data-stu-id="8eb35-142">Deploy the app to Azure</span></span>

<span data-ttu-id="8eb35-143">Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez **Publier...**</span><span class="sxs-lookup"><span data-stu-id="8eb35-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextuel ouvert avec le lien Publier mis en surbrillance](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="8eb35-145">Dans la boîte de dialogue **Publier** :</span><span class="sxs-lookup"><span data-stu-id="8eb35-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="8eb35-146">Sélectionnez **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="8eb35-147">Sélectionnez l’icône d’engrenage, puis **Créer un profil**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="8eb35-148">Sélectionnez **Créer un profil**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-148">Select **Create Profile**.</span></span>

![Boîte de dialogue Publier](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="8eb35-150">Créer des ressources Azure</span><span class="sxs-lookup"><span data-stu-id="8eb35-150">Create Azure resources</span></span>

<span data-ttu-id="8eb35-151">La boîte de dialogue **Créer App Service** s’affiche :</span><span class="sxs-lookup"><span data-stu-id="8eb35-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="8eb35-152">Entrez votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="8eb35-152">Enter your subscription.</span></span>
* <span data-ttu-id="8eb35-153">Les champs d’entrée **Nom de l’application**, **Groupe de ressources** et **Plan App Service** sont renseignés.</span><span class="sxs-lookup"><span data-stu-id="8eb35-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="8eb35-154">Vous pouvez conserver ces noms ou les changer.</span><span class="sxs-lookup"><span data-stu-id="8eb35-154">You can keep these names or change them.</span></span>

![Boîte de dialogue App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="8eb35-156">Sélectionnez l’onglet **Services** pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="8eb35-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="8eb35-157">Sélectionnez l’icône **+** verte pour créer une base de données SQL.</span><span class="sxs-lookup"><span data-stu-id="8eb35-157">Select the green **+** icon to create a new SQL Database</span></span>

![Nouvelle base de données SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="8eb35-159">Sélectionnez **Nouveau...** dans la boîte de dialogue **Configurer la base de données SQL** pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="8eb35-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nouvelle base de données et nouveau serveur SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="8eb35-161">La boîte de dialogue **Configurer le serveur SQL Server** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="8eb35-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="8eb35-162">Entrez un nom d’utilisateur et un mot de passe administrateur, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="8eb35-163">Vous pouvez conserver le **Nom du serveur** par défaut.</span><span class="sxs-lookup"><span data-stu-id="8eb35-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="8eb35-164">« admin » n’est pas autorisé comme nom d’utilisateur administrateur.</span><span class="sxs-lookup"><span data-stu-id="8eb35-164">"admin" isn't allowed as the administrator user name.</span></span>

![Boîte de dialogue Configurer le serveur SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="8eb35-166">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-166">Select **OK**.</span></span>

<span data-ttu-id="8eb35-167">Visual Studio retourne la boîte de dialogue **Créer App Service**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="8eb35-168">Sélectionnez **Créer** dans la boîte de dialogue **Créer App Service**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Boîte de dialogue Configurer la base de données SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="8eb35-170">Visual Studio crée l’application web et SQL Server sur Azure.</span><span class="sxs-lookup"><span data-stu-id="8eb35-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="8eb35-171">Cette étape peut prendre quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="8eb35-171">This step can take a few minutes.</span></span> <span data-ttu-id="8eb35-172">Pour plus d’informations sur les ressources créées, consultez [Ressources supplémentaires](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="8eb35-172">For information on the resources created, see [Additional resources](#additional-resources).</span></span>

<span data-ttu-id="8eb35-173">Quand le déploiement est terminé, sélectionnez **Paramètres** :</span><span class="sxs-lookup"><span data-stu-id="8eb35-173">When deployment completes, select **Settings**:</span></span>

![Boîte de dialogue Configurer le serveur SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="8eb35-175">Dans la page **Paramètres** de la boîte de dialogue **Publier** :</span><span class="sxs-lookup"><span data-stu-id="8eb35-175">On the **Settings** page of the **Publish** dialog:</span></span>

* <span data-ttu-id="8eb35-176">Développez **Bases de données**, puis cochez **Utilisez cette chaîne de connexion au moment de l’exécution**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
* <span data-ttu-id="8eb35-177">Développez **Migrations Entity Framework**, puis cochez **Appliquer cette migration lors de la publication**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="8eb35-178">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-178">Select **Save**.</span></span> <span data-ttu-id="8eb35-179">Visual Studio retourne à la boîte de dialogue **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Boîte de dialogue Publier : Panneau des paramètres](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="8eb35-181">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-181">Click **Publish**.</span></span> <span data-ttu-id="8eb35-182">Visual Studio publie votre application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="8eb35-182">Visual Studio publishes your app to Azure.</span></span> <span data-ttu-id="8eb35-183">Quand le déploiement est terminé, l’application est ouverte dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="8eb35-183">When the deployment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="8eb35-184">Tester votre application dans Azure</span><span class="sxs-lookup"><span data-stu-id="8eb35-184">Test your app in Azure</span></span>

* <span data-ttu-id="8eb35-185">Testez les liens **À propos de** et **Contact**</span><span class="sxs-lookup"><span data-stu-id="8eb35-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="8eb35-186">Inscrivez un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="8eb35-186">Register a new user</span></span>

![Application web ouverte dans Microsoft Edge sur Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="8eb35-188">Mettre à jour l’application</span><span class="sxs-lookup"><span data-stu-id="8eb35-188">Update the app</span></span>

* <span data-ttu-id="8eb35-189">Modifiez la page Razor *Pages/About.cshtml*, puis changez son contenu.</span><span class="sxs-lookup"><span data-stu-id="8eb35-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="8eb35-190">Par exemple, vous pouvez modifier le paragraphe pour indiquer « Hello ASP.NET Core! » :</span><span class="sxs-lookup"><span data-stu-id="8eb35-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="8eb35-191">Cliquez avec le bouton droit sur le projet, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-191">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextuel ouvert avec le lien Publier mis en surbrillance](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="8eb35-193">Une fois l’application publiée, vérifiez que les changements apportés sont disponibles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="8eb35-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Vérifiez que la tâche est terminée](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="8eb35-195">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="8eb35-195">Clean up</span></span>

<span data-ttu-id="8eb35-196">Après avoir testé l’application, accédez au [portail Azure](https://portal.azure.com/), puis supprimez l’application.</span><span class="sxs-lookup"><span data-stu-id="8eb35-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="8eb35-197">Sélectionnez **Groupes de ressources**, puis sélectionnez le groupe de ressources que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="8eb35-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Portail Azure : Groupes de ressources dans le menu de la barre latérale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="8eb35-199">Dans la page **Groupes de ressources**, sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-199">In the **Resource groups** page, select **Delete**.</span></span>

![Portail Azure : Page Groupes de ressources](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="8eb35-201">Entrez le nom du groupe de ressources, puis sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="8eb35-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="8eb35-202">Votre application et toutes les autres ressources créées dans ce didacticiel sont désormais supprimées d’Azure.</span><span class="sxs-lookup"><span data-stu-id="8eb35-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="8eb35-203">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="8eb35-203">Next steps</span></span>

* <xref:host-and-deploy/azure-apps/azure-continuous-deployment>

## <a name="additional-resources"></a><span data-ttu-id="8eb35-204">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8eb35-204">Additional resources</span></span>

* <span data-ttu-id="8eb35-205">Pour Visual Studio Code, consultez [Profils de publication](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="8eb35-205">For Visual Studio Code, see [Publish profiles](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span></span>
* [<span data-ttu-id="8eb35-206">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8eb35-206">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="8eb35-207">Groupes de ressources Azure</span><span class="sxs-lookup"><span data-stu-id="8eb35-207">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="8eb35-208">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="8eb35-208">Azure SQL Database</span></span>](/azure/sql-database/)
* <xref:host-and-deploy/visual-studio-publish-profiles>
* <xref:host-and-deploy/azure-apps/troubleshoot>
