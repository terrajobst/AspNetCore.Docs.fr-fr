---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de Code | Documents Microsoft"
author: tdykstra
description: "Cette série de didacticiels vous montre comment déployer (publier) ASP.NET web application Azure App Service Web Apps ou un fournisseur d’hébergement tiers, en utilisant des éléments..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="ae1ad-103">Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du Code</span><span class="sxs-lookup"><span data-stu-id="ae1ad-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="ae1ad-104">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ae1ad-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="ae1ad-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="ae1ad-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="ae1ad-106">Cette série de didacticiels vous montre comment déployer (publier) ASP.NET web application Azure App Service Web Apps ou un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="ae1ad-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ae1ad-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="ae1ad-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="ae1ad-108">Overview</span></span>

<span data-ttu-id="ae1ad-109">Après le déploiement initial, votre travail de maintenance et de développement de votre site web se poursuit et avant de longue durée, vous souhaiterez déployer une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="ae1ad-110">Ce didacticiel vous guide dans le processus de déploiement d’une mise à jour à votre code d’application.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="ae1ad-111">La mise à jour que vous implémentez et déployez dans ce didacticiel n’implique pas une modification de la base de données ; Vous verrez que ce qui est différent sur le déploiement d’une modification de la base de données dans le didacticiel suivant.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="ae1ad-112">Rappel : Si vous obtenez un message d’erreur, ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="ae1ad-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="ae1ad-113">Modifier un code</span><span class="sxs-lookup"><span data-stu-id="ae1ad-113">Make a code change</span></span>

<span data-ttu-id="ae1ad-114">Un exemple simple d’une mise à jour à votre application, vous allez ajouter à la **instructeurs** page d’une liste de dispensés par le formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="ae1ad-115">Si vous exécutez le **instructeurs** page, vous remarquerez qu’il n’y **sélectionnez** des liens dans la grille, mais qu’ils n’autre chose qu’une marque le gris de tour de ligne en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Page de formateurs de sélection](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="ae1ad-117">Maintenant vous allez ajouter du code qui s’exécute lorsque le **sélectionnez** lien est sélectionnée et affiche une liste de dispensés par le formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="ae1ad-118">Dans *Instructors.aspx*, ajoutez le balisage suivant immédiatement après le **ErrorMessageLabel** `Label` contrôle :</span><span class="sxs-lookup"><span data-stu-id="ae1ad-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="ae1ad-119">Exécutez la page et sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-119">Run the page and select an instructor.</span></span> <span data-ttu-id="ae1ad-120">Vous consultez une liste de dispensés par ce formateur.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-120">You see a list of courses taught by that instructor.</span></span>

    ![Page instructeurs avec dispensés](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="ae1ad-122">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="ae1ad-123">Déployer la mise à jour du code à l’environnement de test</span><span class="sxs-lookup"><span data-stu-id="ae1ad-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="ae1ad-124">Avant de pouvoir utiliser les profils de publication pour déployer sur le test, intermédiaire et de production, vous devez modifier les options de publication de base de données.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="ae1ad-125">Vous n’avez plus besoin d’exécuter les scripts de déploiement grant et des données pour la base de données d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="ae1ad-126">Ouvrez le **publier le site Web** Assistant en cliquant sur le projet ContosoUniversity en cliquant sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="ae1ad-127">Cliquez sur le **Test** profil dans le **profil** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="ae1ad-128">Cliquez sur le **paramètres** onglet.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="ae1ad-129">Sous **DefaultConnection** dans les **bases de données** section, désactivez le **base de données de mise à jour** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="ae1ad-130">Cliquez sur le **profil** onglet, puis cliquez sur le **intermédiaire** profil dans le **profil** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="ae1ad-131">Lorsque le système vous demande si vous souhaitez enregistrer les modifications apportées à la **Test** , cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="ae1ad-132">Apporter la même modification dans le profil de mise en lots.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="ae1ad-133">Répétez le processus pour apporter la même modification dans le profil de Production.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="ae1ad-134">Fermer le **publier le site Web** Assistant.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="ae1ad-135">Déploiement sur l’environnement de test est maintenant très simple de l’exécution d’un seul clic publier à nouveau.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="ae1ad-136">Pour accélérer ce processus, vous pouvez utiliser la **publication Web en un clic** barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="ae1ad-137">Dans le **vue** menu, choisissez **barres d’outils** , puis sélectionnez **publication Web en un clic**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="ae1ad-139">Dans **l’Explorateur de solutions**, sélectionnez le projet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="ae1ad-140">le **publication Web en un clic** barre d’outils, choisissez le **Test** le profil de publication, puis cliquez sur **publier le site Web** (l’icône avec flèches pointant vers la gauche et droite).</span><span class="sxs-lookup"><span data-stu-id="ae1ad-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="ae1ad-142">Visual Studio déploie l’application mis à jour, et le navigateur s’ouvre automatiquement à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="ae1ad-143">Exécutez la page de formateurs et sélectionnez un formateur pour vérifier que la mise à jour a été déployé.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="ae1ad-144">Vous le feriez normalement également effectuer un test de régression (autrement dit, le reste du site pour vous assurer que la nouvelle modification n’a pas été arrête toutes les fonctionnalités de test).</span><span class="sxs-lookup"><span data-stu-id="ae1ad-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="ae1ad-145">Mais pour ce didacticiel, vous allez ignorer cette étape et passez pour déployer la mise à jour à intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="ae1ad-146">Lorsque vous redéployez, Web Deploy détermine automatiquement les fichiers qui ont été modifiés et seules les copies des fichiers modifiés sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="ae1ad-147">Par défaut, Web Deploy utilise dernière modification des dates sur les fichiers pour déterminer celles qui ont été modifiés.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="ae1ad-148">Certains systèmes de contrôle de code source modifier les dates même de fichier lorsque vous ne modifiez pas le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="ae1ad-149">Dans ce cas, vous pouvez souhaiter configurer Web Deploy pour utiliser les sommes de contrôle pour déterminer quels fichiers ont été modifiés.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="ae1ad-150">Pour plus d’informations, consultez [pourquoi tous mes fichiers obtenir redéployés bien que n’avez pas les modifier ?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) dans le Forum aux questions de déploiement de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="ae1ad-151">Déconnecter l’application en mode hors connexion pendant le déploiement</span><span class="sxs-lookup"><span data-stu-id="ae1ad-151">Take the application offline during deployment</span></span>

<span data-ttu-id="ae1ad-152">La modification que vous déployez est maintenant une modification simple à une seule page.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="ae1ad-153">Mais parfois vous déployez les modifications plus importantes, ou déployer des modifications de code et de base de données et le site peut-être se comporter correctement si un utilisateur demande une page avant le déploiement est terminé.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="ae1ad-154">Pour empêcher les utilisateurs d’accéder au site pendant le déploiement est en cours, vous pouvez utiliser un *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="ae1ad-155">Lorsque vous placez un fichier nommé *application\_offline.htm* dans le dossier racine de votre application, IIS affiche automatiquement ce fichier au lieu d’exécuter votre application.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="ae1ad-156">Donc pour empêcher l’accès au cours du déploiement, de placer des *application\_offline.htm* dans le dossier racine, exécutez le processus de déploiement, puis supprimez *application\_offline.htm* après réussite déploiement.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="ae1ad-157">Vous pouvez configurer Web Deploy pour mettre automatiquement une valeur par défaut *application\_offline.htm* sur le serveur de fichiers lorsqu’il démarre le déploiement et le supprimer quand elle s’achève.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="ae1ad-158">Pour que tous les que vous avez à faire est ajouter l’élément XML suivant à votre fichier de profil (.pubxml) de publication :</span><span class="sxs-lookup"><span data-stu-id="ae1ad-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="ae1ad-159">Pour ce didacticiel, vous allez apprendre à créer et utiliser un personnalisé *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="ae1ad-160">À l’aide de *application\_offline.htm* dans le site intermédiaire n’est pas nécessaire, car vous n’avez pas les utilisateurs accèdent au site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="ae1ad-161">Mais il est conseillé d’utiliser la mise en lots tout tester la façon que vous envisagez de déployer en production.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="ae1ad-162">Créer l’application\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="ae1ad-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="ae1ad-163">Dans **l’Explorateur de solutions**, avec le bouton droit de la solution, puis cliquez sur **ajouter**, puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="ae1ad-164">Créer un **HTML Page** nommé *application\_offline.htm* (supprimer le dernier « l » dans le *.html* extension Visual Studio crée par défaut).</span><span class="sxs-lookup"><span data-stu-id="ae1ad-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="ae1ad-165">Remplacez le balisage de modèle par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="ae1ad-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="ae1ad-166">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="ae1ad-167">Application de la copie\_offline.htm dans le dossier racine du site web</span><span class="sxs-lookup"><span data-stu-id="ae1ad-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="ae1ad-168">Vous pouvez utiliser n’importe quel outil FTP pour copier les fichiers sur le site web.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="ae1ad-169">[FileZilla](http://filezilla-project.org/) est un outil FTP populaire et est indiqué dans les captures d’écran.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="ae1ad-170">Pour utiliser un outil FTP, vous avez besoin de trois opérations : l’URL du serveur FTP, le nom d’utilisateur et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="ae1ad-171">L’URL est indiqué sur la page de tableau de bord du site web dans le portail de gestion Azure, et le nom d’utilisateur et un mot de passe FTP sont accessibles dans le *.publishsettings* fichier que vous avez téléchargé précédemment.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="ae1ad-172">Les étapes suivantes montrent comment obtenir ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="ae1ad-173">Dans le portail de gestion Azure, cliquez sur **Sites Web** onglet, puis cliquez sur le site web intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="ae1ad-174">Sur le **tableau de bord** page, accédez à la recherche de nom de l’hôte FTP dans la **aperçu rapide** section.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nom d’hôte FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="ae1ad-176">Ouvrez le transit *.publishsettings* fichier dans le bloc-notes ou un autre éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="ae1ad-177">Rechercher les `publishProfile` élément pour le profil FTP.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="ae1ad-178">Copie le `userName` et `userPWD` valeurs.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Informations d’identification FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="ae1ad-180">Ouvrir votre outil FTP et vous ouvrez une session sur l’URL du serveur FTP.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="ae1ad-181">Copie *application\_offline.htm* à partir du dossier de solution pour le */site/wwwroot* dossier dans le site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copie app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="ae1ad-183">Accédez à l’URL de votre site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="ae1ad-184">Vous constatez que la *application\_offline.htm* page est maintenant affichée au lieu de votre page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm dans la fenêtre du navigateur](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="ae1ad-186">Vous êtes maintenant prêt à déployer dans l’environnement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="ae1ad-187">Déployer la mise à jour du code de test et de production</span><span class="sxs-lookup"><span data-stu-id="ae1ad-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="ae1ad-188">Dans le **publication Web en un clic** barre d’outils, choisissez le **intermédiaire** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="ae1ad-189">Visual Studio déploie l’application de mises à jour et ouvre le navigateur à la page d’accueil du site.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="ae1ad-190">Le *application\_offline.htm* fichier s’affiche.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="ae1ad-191">Avant de pouvoir tester pour vérifier la réussite du déploiement, vous devez supprimer la *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="ae1ad-192">Revenir à votre outil FTP et supprimer **application\_offline.htm** à partir du site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="ae1ad-193">Dans le navigateur, ouvrez la page de formateurs dans le site intermédiaire et sélectionnez un formateur pour vérifier que la mise à jour a été déployé.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="ae1ad-194">Suivez la même procédure pour la production, comme vous l’avez fait mise en lots.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="ae1ad-195">Vérification des modifications et le déploiement de fichiers spécifiques</span><span class="sxs-lookup"><span data-stu-id="ae1ad-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="ae1ad-196">Visual Studio 2012 vous donne également la possibilité de déployer des fichiers individuels.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="ae1ad-197">Pour un fichier sélectionné, vous pouvez afficher les différences entre la version locale et la version déployée, déployez le fichier dans l’environnement de destination ou copiez le fichier à partir de l’environnement de destination pour le projet local.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="ae1ad-198">Dans cette section du didacticiel vous allez apprendre à utiliser ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="ae1ad-199">Apportez une modification à déployer</span><span class="sxs-lookup"><span data-stu-id="ae1ad-199">Make a change to deploy</span></span>

1. <span data-ttu-id="ae1ad-200">Ouvrez *Content/Site.css*et recherchez le bloc pour le `body` balise.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="ae1ad-201">Modifiez la valeur de `background-color` de `#fff` à `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="ae1ad-202">Visualiser les modifications dans la fenêtre d’aperçu avant publication</span><span class="sxs-lookup"><span data-stu-id="ae1ad-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="ae1ad-203">Lorsque vous utilisez la **publier le site Web** Assistant pour publier le projet, vous pouvez voir les modifications vont être publiés en double-cliquant sur le fichier dans le **aperçu** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="ae1ad-204">Cliquez sur le projet ContosoUniversity, sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="ae1ad-205">À partir de la **profil** la liste déroulante, sélectionnez le **Test** le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="ae1ad-206">Cliquez sur **aperçu**, puis cliquez sur **démarrer la version préliminaire**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="ae1ad-207">Dans le **aperçu** volet, double-cliquez sur **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Double-cliquez sur Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="ae1ad-209">Le **aperçu des modifications** boîte de dialogue affiche un aperçu des modifications qui seront déployées.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Aperçu des modifications à Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="ae1ad-211">Si vous double-cliquez sur le *Web.config* fichier, le **aperçu des modifications** boîte de dialogue montre l’effet de la génération des transformations de configuration et les transformations de profil de publication.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="ae1ad-212">À ce stade n’est pas fait tout ce qui entraînerait la *Web.config* fichier sur le serveur à modifier, vous donc vous attendre à ne voir aucune modification.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="ae1ad-213">Toutefois, le **aperçu des modifications** fenêtre affiche incorrectement deux modifications.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="ae1ad-214">Il semble que les deux éléments XML seront supprimés.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="ae1ad-215">Ces éléments sont ajoutés par le processus de publication lorsque vous sélectionnez **exécuter Migrations Code First sur le démarrage de l’application** pour une classe de contexte de Code First.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="ae1ad-216">La comparaison est effectuée avant que le processus de publication ajoute ces éléments, donc il semble qu’ils sont en cours de suppression même si elles ne seront pas supprimés.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="ae1ad-217">Cette erreur sera corrigée dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="ae1ad-218">Cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-218">Click **Close**.</span></span>
6. <span data-ttu-id="ae1ad-219">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-219">Click **Publish**.</span></span>
7. <span data-ttu-id="ae1ad-220">Lorsque le navigateur s’ouvre à la page d’accueil du site de Test, appuyez sur CTRL + F5 pour actualiser dur afin de voir l’effet de la modification CSS.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Effet de modification CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="ae1ad-222">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="ae1ad-223">Publier des fichiers spécifiques à partir de l’Explorateur de solutions</span><span class="sxs-lookup"><span data-stu-id="ae1ad-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="ae1ad-224">Supposons que vous ne l’arrière-plan est bleu et souhaitez revenir à la couleur d’origine.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="ae1ad-225">Dans cette section, vous devez restaurer les paramètres d’origine par la publication d’un fichier spécifique, directement à partir de **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="ae1ad-226">Ouvrez *Content/Site.css* et restaurer le `background-color` à `#fff`.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="ae1ad-227">Dans **l’Explorateur de solutions**, cliquez sur le *Content/Site.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="ae1ad-228">Le menu contextuel affiche les que trois options de publication.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-228">The context menu shows three publish options.</span></span>

    ![Publier les options à partir de l’Explorateur de solutions](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="ae1ad-230">Cliquez sur **aperçu des modifications à Site.css**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="ae1ad-231">Une fenêtre s’ouvre pour afficher les différences entre le fichier local et la version de celui-ci dans l’environnement de destination.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-contenu/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="ae1ad-233">Dans **l’Explorateur de solutions**, avec le bouton droit **Site.css** à nouveau et cliquez sur **publier le Site.css**.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="ae1ad-234">Le **de l’activité de publication Web** fenêtre indique que le fichier a été publié.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Fenêtre de l’activité de publication Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="ae1ad-236">Ouvrez un navigateur à le `http://localhost/contosouniversity` URL et appuyez sur CTRL + F5 pour provoquer un disque dur Actualiser pour afficher l’effet de la feuille de style à modifier.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Page d’accueil avec CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="ae1ad-238">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="ae1ad-239">Résumé</span><span class="sxs-lookup"><span data-stu-id="ae1ad-239">Summary</span></span>

<span data-ttu-id="ae1ad-240">Vous avez vu désormais plusieurs façons de déployer une mise à jour d’application qui n’implique pas une modification de la base de données, et que vous avez vu comment prévisualiser les modifications pour vérifier que ce qui sera mis à jour est ce que vous attendez.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="ae1ad-241">La page de formateurs a maintenant un **cours traités** section.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Page instructeurs avec dispensés](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="ae1ad-243">Le didacticiel suivant vous montre comment déployer une modification de la base de données : vous allez ajouter un champ de date de naissance à la base de données et à la page de formateurs.</span><span class="sxs-lookup"><span data-stu-id="ae1ad-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ae1ad-244">[Précédent](deploying-to-production.md)
[Suivant](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="ae1ad-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
