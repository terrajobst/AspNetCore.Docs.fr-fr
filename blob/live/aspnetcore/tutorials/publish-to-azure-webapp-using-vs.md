---
title: "Publier une application ASP.NET Core sur Azure à l’aide de Visual Studio"
author: rick-anderson
description: "Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="631a2-104">/en-us</span><span class="sxs-lookup"><span data-stu-id="631a2-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="631a2-105">Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="631a2-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="631a2-106">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) et [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="631a2-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="631a2-107">Consultez la page [Publier sur Azure à partir de Visual Studio pour Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) si vous travaillez sur un Mac.</span><span class="sxs-lookup"><span data-stu-id="631a2-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="631a2-108">Installer</span><span class="sxs-lookup"><span data-stu-id="631a2-108">Set up</span></span>

* <span data-ttu-id="631a2-109">Ouvrez un [compte Azure gratuit](https://aka.ms/K5y5yh) si vous n’en avez pas un.</span><span class="sxs-lookup"><span data-stu-id="631a2-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="631a2-110">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="631a2-110">Create a web app</span></span>

<span data-ttu-id="631a2-111">Dans la page de démarrage de Visual Studio, sélectionnez **Fichier > Nouveau > Projet...**</span><span class="sxs-lookup"><span data-stu-id="631a2-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menu Fichier](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="631a2-113">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="631a2-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="631a2-114">Dans le volet gauche, sélectionnez **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="631a2-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="631a2-115">Dans le volet central, sélectionnez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="631a2-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="631a2-116">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="631a2-116">Select **OK**.</span></span>

![Boîte de dialogue Nouveau projet](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="631a2-118">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="631a2-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="631a2-119">Sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="631a2-119">Select **Web Application**.</span></span>
* <span data-ttu-id="631a2-120">Sélectionnez **Modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="631a2-120">Select **Change Authentication**.</span></span>

![Boîte de dialogue Nouveau projet](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="631a2-122">La boîte de dialogue **Modifier l’authentification** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="631a2-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="631a2-123">Sélectionnez **Comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="631a2-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="631a2-124">Sélectionnez **OK** pour revenir à la **nouvelle application web ASP.NET Core**, puis sélectionnez à nouveau **OK**.</span><span class="sxs-lookup"><span data-stu-id="631a2-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Boîte de dialogue Nouvelle application web ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="631a2-126">Visual Studio crée la solution.</span><span class="sxs-lookup"><span data-stu-id="631a2-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="631a2-127">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="631a2-127">Run the app</span></span>

* <span data-ttu-id="631a2-128">Appuyez sur CTRL+F5 pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="631a2-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="631a2-129">Testez les liens **À propos de** et **Contact**.</span><span class="sxs-lookup"><span data-stu-id="631a2-129">Test the **About** and **Contact** links.</span></span>

![Application web ouverte dans Microsoft Edge sur localhost](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="631a2-131">Inscrire un utilisateur</span><span class="sxs-lookup"><span data-stu-id="631a2-131">Register a user</span></span>

* <span data-ttu-id="631a2-132">Sélectionnez **S’inscrire**, puis inscrivez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="631a2-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="631a2-133">Vous pouvez utiliser une adresse e-mail fictive.</span><span class="sxs-lookup"><span data-stu-id="631a2-133">You can use a fictitious email address.</span></span> <span data-ttu-id="631a2-134">Quand vous effectuez l’envoi, la page affiche l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="631a2-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="631a2-135">*« Erreur de serveur interne : Une opération de base de données a échoué lors du traitement de la requête. Exception SQL : Impossible d’ouvrir la base de données. Appliquer des migrations existantes pour le contexte de base de données d’application peut résoudre ce problème. »*</span><span class="sxs-lookup"><span data-stu-id="631a2-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="631a2-136">Sélectionnez **Appliquer les migrations**, puis, une fois la page mise à jour, actualisez-la.</span><span class="sxs-lookup"><span data-stu-id="631a2-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erreur de serveur interne : Une opération de base de données a échoué lors du traitement de la requête.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="631a2-140">L’application affiche l’adresse e-mail utilisée pour inscrire le nouvel utilisateur et un lien **Se déconnecter**.</span><span class="sxs-lookup"><span data-stu-id="631a2-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Application web ouverte dans Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="631a2-143">Déployer l’application sur Azure</span><span class="sxs-lookup"><span data-stu-id="631a2-143">Deploy the app to Azure</span></span>

<span data-ttu-id="631a2-144">Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez **Publier...**</span><span class="sxs-lookup"><span data-stu-id="631a2-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextuel ouvert avec le lien Publier mis en surbrillance](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="631a2-146">Dans la boîte de dialogue **Publier** :</span><span class="sxs-lookup"><span data-stu-id="631a2-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="631a2-147">Sélectionnez **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="631a2-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="631a2-148">Sélectionnez l’icône d’engrenage, puis **Créer un profil**.</span><span class="sxs-lookup"><span data-stu-id="631a2-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="631a2-149">Sélectionnez **Créer un profil**.</span><span class="sxs-lookup"><span data-stu-id="631a2-149">Select **Create Profile**.</span></span>

![Boîte de dialogue Publier](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="631a2-151">Créer des ressources Azure</span><span class="sxs-lookup"><span data-stu-id="631a2-151">Create Azure resources</span></span>

<span data-ttu-id="631a2-152">La boîte de dialogue **Créer App Service** s’affiche :</span><span class="sxs-lookup"><span data-stu-id="631a2-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="631a2-153">Entrez votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="631a2-153">Enter your subscription.</span></span>
* <span data-ttu-id="631a2-154">Les champs d’entrée **Nom de l’application**, **Groupe de ressources** et **Plan App Service** sont renseignés.</span><span class="sxs-lookup"><span data-stu-id="631a2-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="631a2-155">Vous pouvez conserver ces noms ou les changer.</span><span class="sxs-lookup"><span data-stu-id="631a2-155">You can keep these names or change them.</span></span>

![Boîte de dialogue App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="631a2-157">Sélectionnez l’onglet **Services** pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="631a2-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="631a2-158">Sélectionnez l’icône **+** verte pour créer une base de données SQL.</span><span class="sxs-lookup"><span data-stu-id="631a2-158">Select the green **+** icon to create a new SQL Database</span></span>

![Nouvelle base de données SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="631a2-160">Sélectionnez **Nouveau...** dans la boîte de dialogue **Configurer la base de données SQL** pour créer une base de données.</span><span class="sxs-lookup"><span data-stu-id="631a2-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nouvelle base de données et nouveau serveur SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="631a2-162">La boîte de dialogue **Configurer le serveur SQL Server** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="631a2-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="631a2-163">Entrez un nom d’utilisateur et un mot de passe administrateur, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="631a2-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="631a2-164">Vous pouvez conserver le **Nom du serveur** par défaut.</span><span class="sxs-lookup"><span data-stu-id="631a2-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="631a2-165">« admin » n’est pas autorisé comme nom d’utilisateur administrateur.</span><span class="sxs-lookup"><span data-stu-id="631a2-165">"admin" is not allowed as the administrator user name.</span></span>

![Boîte de dialogue Configurer le serveur SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="631a2-167">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="631a2-167">Select **OK**.</span></span>

<span data-ttu-id="631a2-168">Visual Studio retourne la boîte de dialogue **Créer App Service**.</span><span class="sxs-lookup"><span data-stu-id="631a2-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="631a2-169">Sélectionnez **Créer** dans la boîte de dialogue **Créer App Service**.</span><span class="sxs-lookup"><span data-stu-id="631a2-169">Select **Create** on the **Create App Service** dialog.</span></span>

![Boîte de dialogue Configurer la base de données SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="631a2-171">Visual Studio crée l’application web et SQL Server sur Azure.</span><span class="sxs-lookup"><span data-stu-id="631a2-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="631a2-172">Cette étape peut prendre quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="631a2-172">This step can take a few minutes.</span></span> <span data-ttu-id="631a2-173">Pour plus d’informations sur les ressources créées, consultez [Ressources supplémentaires](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="631a2-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="631a2-174">Quand le déploiement est terminé, sélectionnez **Paramètres** :</span><span class="sxs-lookup"><span data-stu-id="631a2-174">When deployment completes, select **Settings**:</span></span>

![Boîte de dialogue Configurer le serveur SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="631a2-176">Dans la page **Paramètres** de la boîte de dialogue **Publier** :</span><span class="sxs-lookup"><span data-stu-id="631a2-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="631a2-177">Développez **Bases de données**, puis cochez **Utilisez cette chaîne de connexion au moment de l’exécution**.</span><span class="sxs-lookup"><span data-stu-id="631a2-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="631a2-178">Développez **Migrations Entity Framework**, puis cochez **Appliquer cette migration lors de la publication**.</span><span class="sxs-lookup"><span data-stu-id="631a2-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="631a2-179">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="631a2-179">Select **Save**.</span></span> <span data-ttu-id="631a2-180">Visual Studio retourne à la boîte de dialogue **Publier**.</span><span class="sxs-lookup"><span data-stu-id="631a2-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Boîte de dialogue Publier : panneau Paramètres](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="631a2-182">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="631a2-182">Click **Publish**.</span></span> <span data-ttu-id="631a2-183">Visual Studio publie votre application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="631a2-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="631a2-184">Quand le déploiement est terminé, l’application est ouverte dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="631a2-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="631a2-185">Tester votre application dans Azure</span><span class="sxs-lookup"><span data-stu-id="631a2-185">Test your app in Azure</span></span>

* <span data-ttu-id="631a2-186">Testez les liens **À propos de** et **Contact**</span><span class="sxs-lookup"><span data-stu-id="631a2-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="631a2-187">Inscrivez un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="631a2-187">Register a new user</span></span>

![Application web ouverte dans Microsoft Edge sur Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="631a2-189">Mettre à jour l’application</span><span class="sxs-lookup"><span data-stu-id="631a2-189">Update the app</span></span>

* <span data-ttu-id="631a2-190">Modifiez la page Razor *Pages/About.cshtml*, puis changez son contenu.</span><span class="sxs-lookup"><span data-stu-id="631a2-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="631a2-191">Par exemple, vous pouvez modifier le paragraphe pour indiquer « Hello ASP.NET Core! » : [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="631a2-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="631a2-192">Cliquez avec le bouton droit sur le projet, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="631a2-192">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextuel ouvert avec le lien Publier mis en surbrillance](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="631a2-194">Une fois l’application publiée, vérifiez que les changements apportés sont disponibles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="631a2-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Vérifiez que la tâche est terminée](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="631a2-196">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="631a2-196">Clean up</span></span>

<span data-ttu-id="631a2-197">Après avoir testé l’application, accédez au [portail Azure](https://portal.azure.com/), puis supprimez l’application.</span><span class="sxs-lookup"><span data-stu-id="631a2-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="631a2-198">Sélectionnez **Groupes de ressources**, puis sélectionnez le groupe de ressources que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="631a2-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Portail Azure : Groupes de ressources dans le menu de la barre latérale](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="631a2-200">Dans la page **Groupes de ressources**, sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="631a2-200">In the **Resource groups** page, select **Delete**.</span></span>

![Portail Azure : page Groupes de ressources](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="631a2-202">Entrez le nom du groupe de ressources, puis sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="631a2-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="631a2-203">Votre application et toutes les autres ressources créées dans ce didacticiel sont désormais supprimées d’Azure.</span><span class="sxs-lookup"><span data-stu-id="631a2-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="631a2-204">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="631a2-204">Next steps</span></span>

* [<span data-ttu-id="631a2-205">Déploiement continu sur Azure avec Visual Studio et Git</span><span class="sxs-lookup"><span data-stu-id="631a2-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="631a2-206">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="631a2-206">Additonal resources</span></span>

* [<span data-ttu-id="631a2-207">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="631a2-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="631a2-208">Groupes de ressources Azure</span><span class="sxs-lookup"><span data-stu-id="631a2-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="631a2-209">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="631a2-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)