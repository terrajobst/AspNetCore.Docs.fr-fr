---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du Code | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833353"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="5353c-103">Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du Code</span><span class="sxs-lookup"><span data-stu-id="5353c-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="5353c-104">par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5353c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="5353c-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="5353c-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="5353c-106">Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5353c-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="5353c-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5353c-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="5353c-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="5353c-108">Overview</span></span>

<span data-ttu-id="5353c-109">Après le déploiement initial, votre travail de maintenir et de développer votre site web continue, et avant de longue durée vous souhaitez déployer une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="5353c-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="5353c-110">Ce didacticiel vous accompagne tout au long du processus de déploiement d’une mise à jour à votre code d’application.</span><span class="sxs-lookup"><span data-stu-id="5353c-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="5353c-111">La mise à jour que vous implémentez et déployez dans ce didacticiel n’implique pas une modification de la base de données ; Vous verrez en quoi diffère sur le déploiement d’une modification de la base de données dans le didacticiel suivant.</span><span class="sxs-lookup"><span data-stu-id="5353c-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="5353c-112">Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5353c-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="5353c-113">Modifiez le code</span><span class="sxs-lookup"><span data-stu-id="5353c-113">Make a code change</span></span>

<span data-ttu-id="5353c-114">Un exemple simple d’une mise à jour à votre application, vous allez ajouter à la **formateurs** page une liste des cours dispensés par le formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="5353c-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="5353c-115">Si vous exécutez le **formateurs** page, vous remarquerez qu’il n’y **sélectionnez** des liens dans la grille, mais ils ne le faites pas autre chose qu’une marque la couleur grise tour de ligne en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="5353c-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Page des formateurs avec sélection](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="5353c-117">Maintenant vous ajouterez du code qui s’exécute lorsque le **sélectionnez** lien cliqué et affiche une liste des cours dispensés par le formateur sélectionné.</span><span class="sxs-lookup"><span data-stu-id="5353c-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="5353c-118">Dans *Instructors.aspx*, ajoutez le balisage suivant immédiatement après le **ErrorMessageLabel** `Label` contrôle :</span><span class="sxs-lookup"><span data-stu-id="5353c-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="5353c-119">Exécutez la page et sélectionnez un formateur.</span><span class="sxs-lookup"><span data-stu-id="5353c-119">Run the page and select an instructor.</span></span> <span data-ttu-id="5353c-120">Vous voyez une liste de cours dispensés par ce formateur.</span><span class="sxs-lookup"><span data-stu-id="5353c-120">You see a list of courses taught by that instructor.</span></span>

    ![Page des formateurs avec les cours dispensés](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="5353c-122">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="5353c-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="5353c-123">Déployer la mise à jour du code dans l’environnement de test</span><span class="sxs-lookup"><span data-stu-id="5353c-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="5353c-124">Avant de pouvoir utiliser les profils de publication pour déployer sur le test, intermédiaire et de production, vous devez modifier les options de publication de base de données.</span><span class="sxs-lookup"><span data-stu-id="5353c-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="5353c-125">Vous n’avez plus besoin d’exécuter les scripts de déploiement grant et des données pour la base de données d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="5353c-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="5353c-126">Ouvrez le **publier le site Web** Assistant en double-cliquant sur le projet ContosoUniversity en cliquant sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="5353c-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="5353c-127">Cliquez sur le **Test** Profiler dans le **profil** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="5353c-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="5353c-128">Cliquez sur le **paramètres** onglet.</span><span class="sxs-lookup"><span data-stu-id="5353c-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="5353c-129">Sous **DefaultConnection** dans le **bases de données** section, désactivez le **base de données de mise à jour** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="5353c-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="5353c-130">Cliquez sur le **profil** onglet, puis cliquez sur le **intermédiaire** Profiler dans le **profil** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="5353c-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="5353c-131">Lorsque le système vous demande si vous souhaitez enregistrer les modifications apportées à la **Test** de profil, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="5353c-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="5353c-132">Apporter la même modification dans le profil de mise en lots.</span><span class="sxs-lookup"><span data-stu-id="5353c-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="5353c-133">Répétez le processus pour apporter la même modification dans le profil de Production.</span><span class="sxs-lookup"><span data-stu-id="5353c-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="5353c-134">Fermer le **publier le site Web** Assistant.</span><span class="sxs-lookup"><span data-stu-id="5353c-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="5353c-135">Déploiement sur l’environnement de test est maintenant très simple de l’exécution d’un seul clic publier à nouveau.</span><span class="sxs-lookup"><span data-stu-id="5353c-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="5353c-136">Pour accélérer ce processus, vous pouvez utiliser la **publication Web en un clic** barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="5353c-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="5353c-137">Dans le **vue** menu, choisissez **barres d’outils** , puis sélectionnez **publication Web en un clic**.</span><span class="sxs-lookup"><span data-stu-id="5353c-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="5353c-139">Dans **l’Explorateur de solutions**, sélectionnez le projet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="5353c-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="5353c-140">le **publication Web en un clic** barre d’outils, choisissez le **Test** le profil de publication, puis cliquez sur **publier le site Web** (l’icône des flèches pointant vers la gauche et droite).</span><span class="sxs-lookup"><span data-stu-id="5353c-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="5353c-142">Visual Studio déploie l’application mis à jour, et le navigateur s’ouvre automatiquement à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="5353c-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="5353c-143">Exécutez la page des formateurs et sélectionnez un formateur pour vérifier que la mise à jour a été déployé avec succès.</span><span class="sxs-lookup"><span data-stu-id="5353c-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="5353c-144">Vous le feriez normalement également effectuer des tests de régression (autrement dit, le reste du site pour vous assurer que la nouvelle modification n’arrête toutes les fonctionnalités de test).</span><span class="sxs-lookup"><span data-stu-id="5353c-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="5353c-145">Mais pour ce didacticiel vous ignorez cette étape et passez à déployer la mise à jour à intermédiaire et de production.</span><span class="sxs-lookup"><span data-stu-id="5353c-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="5353c-146">Lorsque vous redéployez, Web Deploy détermine automatiquement les fichiers qui ont été modifiés et seules les copies des fichiers modifiés sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="5353c-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="5353c-147">Par défaut, Web Deploy utilise des dates de dernière modification sur les fichiers pour déterminer celles qui ont changé.</span><span class="sxs-lookup"><span data-stu-id="5353c-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="5353c-148">Certains systèmes de contrôle de code source modifier les dates mêmes fichier lorsque vous ne modifiez pas le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="5353c-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="5353c-149">Dans ce cas, vous souhaiterez peut-être configurer Web Deploy pour utiliser les sommes de contrôle pour déterminer quels fichiers ont changé.</span><span class="sxs-lookup"><span data-stu-id="5353c-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="5353c-150">Pour plus d’informations, consultez [pourquoi tous mes fichiers sont redéployés bien que n’avez pas les modifier ?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) dans le Forum aux questions de déploiement ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5353c-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="5353c-151">Mettre l’application hors ligne pendant le déploiement</span><span class="sxs-lookup"><span data-stu-id="5353c-151">Take the application offline during deployment</span></span>

<span data-ttu-id="5353c-152">La modification que vous effectuez un déploiement est maintenant une modification simple à une seule page.</span><span class="sxs-lookup"><span data-stu-id="5353c-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="5353c-153">Mais parfois vous déployez des modifications majeures, ou vous déployez les modifications de code et de base de données, et le site peut-être se comporter correctement si un utilisateur demande une page avant le déploiement est terminé.</span><span class="sxs-lookup"><span data-stu-id="5353c-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="5353c-154">Pour empêcher les utilisateurs d’accéder au site pendant le déploiement est en cours, vous pouvez utiliser un *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="5353c-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="5353c-155">Lorsque vous placez un fichier nommé *application\_offline.htm* dans le dossier racine de votre application, IIS affiche automatiquement ce fichier au lieu d’exécuter votre application.</span><span class="sxs-lookup"><span data-stu-id="5353c-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="5353c-156">Pour empêcher l’accès au cours du déploiement, vous placer *application\_offline.htm* dans le dossier racine, exécutez le processus de déploiement et supprimez *application\_offline.htm* après réussite déploiement.</span><span class="sxs-lookup"><span data-stu-id="5353c-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="5353c-157">Vous pouvez configurer Web Deploy pour mettre automatiquement une valeur par défaut *application\_offline.htm* sur le serveur de fichiers lorsqu’il démarre le déploiement et le supprimer quand elle est terminée.</span><span class="sxs-lookup"><span data-stu-id="5353c-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="5353c-158">Pour faire que tout ce que vous avez à faire est d’ajouter l’élément XML suivant à votre fichier de profil (.pubxml) de publication :</span><span class="sxs-lookup"><span data-stu-id="5353c-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="5353c-159">Pour ce didacticiel, vous verrez comment créer et utiliser un personnalisé *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="5353c-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="5353c-160">À l’aide de *application\_offline.htm* dans le site intermédiaire n’est pas nécessaire, car vous n’avez pas les utilisateurs accédant au site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="5353c-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="5353c-161">Mais il est conseillé d’utiliser la mise en lots de tout tester la façon que vous envisagez de déployer en production.</span><span class="sxs-lookup"><span data-stu-id="5353c-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="5353c-162">Créer application\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="5353c-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="5353c-163">Dans **l’Explorateur de solutions**, avec le bouton droit de la solution, puis cliquez sur **ajouter**, puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="5353c-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="5353c-164">Créer un **HTML Page** nommé *application\_offline.htm* (supprimer le dernier « l » dans le *.html* extension Visual Studio crée par défaut).</span><span class="sxs-lookup"><span data-stu-id="5353c-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="5353c-165">Remplacez le balisage de modèle par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="5353c-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="5353c-166">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="5353c-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="5353c-167">Copier l’application\_offline.htm dans le dossier racine du site web</span><span class="sxs-lookup"><span data-stu-id="5353c-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="5353c-168">Vous pouvez utiliser n’importe quel outil FTP pour copier des fichiers vers le site web.</span><span class="sxs-lookup"><span data-stu-id="5353c-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="5353c-169">[FileZilla](http://filezilla-project.org/) est un outil FTP populaire et est indiqué dans les captures d’écran.</span><span class="sxs-lookup"><span data-stu-id="5353c-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="5353c-170">Pour utiliser un outil FTP, vous avez besoin de trois choses : l’URL de FTP, le nom d’utilisateur et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="5353c-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="5353c-171">L’URL est affichée sur la page de tableau de bord du site web dans le portail de gestion et le nom d’utilisateur et le mot de passe FTP peuvent être trouvées dans le *.publishsettings* fichier que vous avez téléchargé précédemment.</span><span class="sxs-lookup"><span data-stu-id="5353c-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="5353c-172">Les étapes suivantes montrent comment obtenir ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="5353c-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="5353c-173">Dans le portail de gestion Azure, cliquez sur **Sites Web** onglet, puis cliquez sur le site web intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="5353c-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="5353c-174">Sur le **tableau de bord** page, faites défiler jusqu'à trouver le FTP nom d’hôte dans le **aperçu rapide** section.</span><span class="sxs-lookup"><span data-stu-id="5353c-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nom d’hôte FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="5353c-176">Ouvrez la mise en lots *.publishsettings* fichier dans le bloc-notes ou un autre éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="5353c-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="5353c-177">Rechercher le `publishProfile` élément pour le profil FTP.</span><span class="sxs-lookup"><span data-stu-id="5353c-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="5353c-178">Copie le `userName` et `userPWD` valeurs.</span><span class="sxs-lookup"><span data-stu-id="5353c-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Informations d’identification FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="5353c-180">Ouvrez votre outil FTP et l’ouvrez une session sur l’URL FTP.</span><span class="sxs-lookup"><span data-stu-id="5353c-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="5353c-181">Copie *application\_offline.htm* à partir du dossier de solution pour le */site/wwwroot* dossier dans le site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="5353c-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copie app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="5353c-183">Accédez à l’URL de votre site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="5353c-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="5353c-184">Vous pouvez constater que le *application\_offline.htm* page est maintenant affichée au lieu de votre page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="5353c-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm dans la fenêtre de navigateur](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="5353c-186">Vous êtes maintenant prêt à déployer dans un environnement intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="5353c-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="5353c-187">Déployer la mise à jour du code à intermédiaire et de production</span><span class="sxs-lookup"><span data-stu-id="5353c-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="5353c-188">Dans le **publication Web en un clic** barre d’outils, choisissez le **intermédiaire** le profil de publication, puis cliquez sur **publier le site Web**.</span><span class="sxs-lookup"><span data-stu-id="5353c-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="5353c-189">Visual Studio déploie l’application mis à jour et ouvre le navigateur à la page d’accueil du site.</span><span class="sxs-lookup"><span data-stu-id="5353c-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="5353c-190">Le *application\_offline.htm* fichier s’affiche.</span><span class="sxs-lookup"><span data-stu-id="5353c-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="5353c-191">Avant de pouvoir tester pour vérifier la réussite du déploiement, vous devez supprimer le *application\_offline.htm* fichier.</span><span class="sxs-lookup"><span data-stu-id="5353c-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="5353c-192">Revenir à votre outil FTP et supprimer **application\_offline.htm** à partir du site intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="5353c-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="5353c-193">Dans le navigateur, ouvrez la page des formateurs dans le site intermédiaire et sélectionnez un formateur pour vérifier que la mise à jour a été déployé avec succès.</span><span class="sxs-lookup"><span data-stu-id="5353c-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="5353c-194">Suivez la même procédure pour la production comme vous l’avez fait mise en lots.</span><span class="sxs-lookup"><span data-stu-id="5353c-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="5353c-195">Révision des modifications et le déploiement des fichiers spécifiques</span><span class="sxs-lookup"><span data-stu-id="5353c-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="5353c-196">Visual Studio 2012 vous donne également la possibilité de déployer des fichiers individuels.</span><span class="sxs-lookup"><span data-stu-id="5353c-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="5353c-197">Pour un fichier sélectionné, vous pouvez afficher les différences entre la version locale et la version déployée, déployez le fichier dans l’environnement de destination ou copiez le fichier à partir de l’environnement de destination pour le projet local.</span><span class="sxs-lookup"><span data-stu-id="5353c-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="5353c-198">Dans cette section du didacticiel, vous allez apprendre à utiliser ces fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="5353c-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="5353c-199">Apportez une modification à déployer</span><span class="sxs-lookup"><span data-stu-id="5353c-199">Make a change to deploy</span></span>

1. <span data-ttu-id="5353c-200">Ouvrez *Content/Site.css*et rechercher le bloc pour le `body` balise.</span><span class="sxs-lookup"><span data-stu-id="5353c-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="5353c-201">Modifiez la valeur de `background-color` de `#fff` à `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="5353c-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="5353c-202">Visualiser les modifications dans la fenêtre d’aperçu de publication</span><span class="sxs-lookup"><span data-stu-id="5353c-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="5353c-203">Lorsque vous utilisez le **publier le site Web** Assistant pour publier le projet, vous pouvez voir les modifications qui vont être publiés en double-cliquant sur le fichier dans le **aperçu** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="5353c-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="5353c-204">Cliquez sur le projet ContosoUniversity et cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="5353c-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="5353c-205">À partir de la **profil** liste déroulante, sélectionnez le **Test** le profil de publication.</span><span class="sxs-lookup"><span data-stu-id="5353c-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="5353c-206">Cliquez sur **aperçu**, puis cliquez sur **démarrer l’aperçu**.</span><span class="sxs-lookup"><span data-stu-id="5353c-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="5353c-207">Dans le **aperçu** volet, double-cliquez sur **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="5353c-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Double-cliquez sur Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="5353c-209">Le **aperçu des modifications** boîte de dialogue affiche un aperçu des modifications qui seront déployés.</span><span class="sxs-lookup"><span data-stu-id="5353c-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Aperçu des modifications à Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="5353c-211">Si vous double-cliquez sur le *Web.config* fichier, le **aperçu des modifications** boîte de dialogue montre l’effet de votre build transformations de configuration et les transformations de profil de publication.</span><span class="sxs-lookup"><span data-stu-id="5353c-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="5353c-212">À ce stade n’est pas fait tout ce qui entraînerait la *Web.config* fichier sur le serveur à modifier, donc vous devriez ne voir aucune modification.</span><span class="sxs-lookup"><span data-stu-id="5353c-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="5353c-213">Toutefois, le **aperçu des modifications** fenêtre affiche incorrectement deux modifications.</span><span class="sxs-lookup"><span data-stu-id="5353c-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="5353c-214">Il semble que les deux éléments XML seront supprimés.</span><span class="sxs-lookup"><span data-stu-id="5353c-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="5353c-215">Ces éléments sont ajoutés par le processus de publication lorsque vous sélectionnez **Execute Code First Migrations sur le démarrage de l’application** pour une classe de contexte de Code First.</span><span class="sxs-lookup"><span data-stu-id="5353c-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="5353c-216">La comparaison est effectuée avant que le processus de publication ajoute ces éléments, donc il semble qu’ils sont en cours de suppression bien qu’ils ne seront pas supprimés.</span><span class="sxs-lookup"><span data-stu-id="5353c-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="5353c-217">Cette erreur sera corrigée dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="5353c-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="5353c-218">Cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="5353c-218">Click **Close**.</span></span>
6. <span data-ttu-id="5353c-219">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="5353c-219">Click **Publish**.</span></span>
7. <span data-ttu-id="5353c-220">Lorsque le navigateur ouvre la page d’accueil du site de Test, appuyez sur CTRL + F5 pour actualiser dur afin de voir l’effet de la modification CSS.</span><span class="sxs-lookup"><span data-stu-id="5353c-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Effet de changement CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="5353c-222">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="5353c-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="5353c-223">Publier des fichiers spécifiques de l’Explorateur de solutions</span><span class="sxs-lookup"><span data-stu-id="5353c-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="5353c-224">Supposons que vous ne telles que l’arrière-plan est bleu et souhaitez revenir à la couleur d’origine.</span><span class="sxs-lookup"><span data-stu-id="5353c-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="5353c-225">Dans cette section, vous devez restaurer les paramètres d’origine en publiant un fichier spécifique directement à partir de **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="5353c-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="5353c-226">Ouvrez *Content/Site.css* et restaurer le `background-color` à `#fff`.</span><span class="sxs-lookup"><span data-stu-id="5353c-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="5353c-227">Dans **l’Explorateur de solutions**, cliquez sur le *Content/Site.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="5353c-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="5353c-228">Le menu contextuel affiche que trois options de publication.</span><span class="sxs-lookup"><span data-stu-id="5353c-228">The context menu shows three publish options.</span></span>

    ![Publier les options à partir de l’Explorateur de solutions](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="5353c-230">Cliquez sur **aperçu des modifications à Site.css**.</span><span class="sxs-lookup"><span data-stu-id="5353c-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="5353c-231">Une fenêtre s’ouvre pour afficher les différences entre le fichier local et la version de celui-ci dans l’environnement de destination.</span><span class="sxs-lookup"><span data-stu-id="5353c-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-contenu/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="5353c-233">Dans **l’Explorateur de solutions**, avec le bouton droit **Site.css** à nouveau et cliquez sur **Site.css publier**.</span><span class="sxs-lookup"><span data-stu-id="5353c-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="5353c-234">Le **activité de publication Web** fenêtre montre que le fichier a été publié.</span><span class="sxs-lookup"><span data-stu-id="5353c-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Fenêtre d’activité de publication Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="5353c-236">Ouvrez un navigateur à le `http://localhost/contosouniversity` URL et appuyez sur CTRL + F5 pour provoquer un disque dur Actualiser pour voir l’effet de la CSS à modifier.</span><span class="sxs-lookup"><span data-stu-id="5353c-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Page d’accueil avec CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="5353c-238">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="5353c-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="5353c-239">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="5353c-239">Summary</span></span>

<span data-ttu-id="5353c-240">Vous avez maintenant vu plusieurs façons de déployer une mise à jour d’application qui n’implique pas une modification de la base de données, et que vous avez vu comment prévisualiser les modifications pour vérifier que ce qui sera mis à jour est ce que vous attendez.</span><span class="sxs-lookup"><span data-stu-id="5353c-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="5353c-241">La page des formateurs a maintenant un **cours enseignés** section.</span><span class="sxs-lookup"><span data-stu-id="5353c-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Page des formateurs avec les cours dispensés](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="5353c-243">Le didacticiel suivant vous montre comment déployer une modification de la base de données : vous allez ajouter un champ de date de naissance à la base de données et à la page des formateurs.</span><span class="sxs-lookup"><span data-stu-id="5353c-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5353c-244">[Précédent](deploying-to-production.md)
> [Suivant](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="5353c-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
