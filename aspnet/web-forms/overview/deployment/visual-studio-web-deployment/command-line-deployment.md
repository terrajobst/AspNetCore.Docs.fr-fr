---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement de la ligne de commande | Documents Microsoft'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) ASP.NET web application Azure App Service Web Apps ou un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="1c4a4-103">Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="1c4a4-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="1c4a4-104">par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1c4a4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1c4a4-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="1c4a4-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="1c4a4-106">Cette série de didacticiels vous montre comment déployer (publier) ASP.NET web application Azure App Service Web Apps ou un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="1c4a4-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1c4a4-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="1c4a4-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="1c4a4-108">Overview</span></span>

<span data-ttu-id="1c4a4-109">Ce didacticiel vous montre comment appeler le web de Visual Studio publier le pipeline à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="1c4a4-110">Cela est utile pour les scénarios où vous souhaitez [automatiser le processus de déploiement](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) au lieu de faire manuellement dans Visual Studio, généralement en utilisant un [système de contrôle de version code source](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="1c4a4-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="1c4a4-111">Apportez une modification à déployer</span><span class="sxs-lookup"><span data-stu-id="1c4a4-111">Make a change to deploy</span></span>

<span data-ttu-id="1c4a4-112">Actuellement, la page à propos affiche le code du modèle.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-112">Currently the About page displays the template code.</span></span>

![Sur la page avec le code de modèle](command-line-deployment/_static/image1.png)

<span data-ttu-id="1c4a4-114">Vous allez remplacer qui avec du code qui affiche un résumé de l’inscription d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="1c4a4-115">Ouvrez le *About.aspx* page, de supprimer toutes les marques à l’intérieur de la `MainContent` `Content` élément et insérez le balisage suivant à la place :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="1c4a4-116">Exécutez le projet et sélectionnez le **sur** page.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-116">Run the project and select the **About** page.</span></span>

![Page About](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="1c4a4-118">Déploiement de Test à l’aide de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="1c4a4-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="1c4a4-119">Vous ne déployez une autre modification de base de données, dans ce déploiement de base de données dbDacFx désactiver pour la base de données ContosoUniversity aspnet.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="1c4a4-120">Ouvrez le **publier le site Web** Assistant et dans chacun des trois publier les profils, désactivez le **mise à jour de la base de données** case à cocher sur la **paramètres** onglet.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="1c4a4-121">Dans la page d’accueil de Windows 8, recherchez **invite de commandes développeur pour VS2012**.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="1c4a4-122">Cliquez sur l’icône de **invite de commandes développeur pour VS2012** et cliquez sur **exécuter en tant qu’administrateur**.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="1c4a4-123">Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier de solution avec le chemin d’accès à votre fichier de solution :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="1c4a4-124">MSBuild génère la solution et le déploie sur l’environnement de test.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Sortie de la ligne de commande](command-line-deployment/_static/image3.png)

<span data-ttu-id="1c4a4-126">Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, puis cliquez sur le **sur** page pour vérifier que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="1c4a4-127">Si vous n’avez pas encore créé d’étudiants dans le test, vous verrez une page vide sous la **étudiant corps statistiques** titre.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="1c4a4-128">Accédez à la **étudiants** , cliquez sur **ajouter un étudiant**et ajouter des étudiants, puis revenez à la **sur** page pour voir les statistiques de student.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Sur la page dans un environnement de Test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="1c4a4-130">Options de la clé de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="1c4a4-130">Key command line options</span></span>

<span data-ttu-id="1c4a4-131">La commande que vous avez entré, le chemin d’accès du fichier de solution et deux propriétés sont passés à MSBuild :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="1c4a4-132">Déploiement de la solution de déploiement des projets individuels</span><span class="sxs-lookup"><span data-stu-id="1c4a4-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="1c4a4-133">En spécifiant le fichier de solution entraîne par tous les projets dans la solution à générer.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="1c4a4-134">Si vous avez plusieurs projets web dans la solution, le comportement de MSBuild suivant s’applique :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="1c4a4-135">Les propriétés que vous spécifiez sur la ligne de commande sont passées à chaque projet.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="1c4a4-136">Par conséquent, chaque projet web doit avoir un profil de publication avec le nom que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="1c4a4-137">Si vous spécifiez `/p:PublishProfile=Test`, chaque projet web doit avoir un profil de publication nommé *Test*.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="1c4a4-138">Vous pouvez correctement publier un projet quand même à générer un autre.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="1c4a4-139">Pour plus d’informations, consultez le thread stackoverflow [MSBuild échoue avec deux packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="1c4a4-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="1c4a4-140">Si vous spécifiez un projet individuel au lieu d’une solution, vous devez ajouter un paramètre qui spécifie la version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="1c4a4-141">Si vous utilisez Visual Studio 2012, la ligne de commande serait similaire à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="1c4a4-142">Le numéro de version de Visual Studio 2010 est 10.0.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="1c4a4-143">Pour plus d’informations, consultez [compatibilité de projet Visual Studio et VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) sur le blog de Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="1c4a4-144">Spécifier le profil de publication</span><span class="sxs-lookup"><span data-stu-id="1c4a4-144">Specifying the publish profile</span></span>

<span data-ttu-id="1c4a4-145">Vous pouvez spécifier le profil de publication par nom ou par le chemin complet vers le *.pubxml* de fichiers, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="1c4a4-146">Méthodes prises en charge pour la publication en ligne de commande de publication Web</span><span class="sxs-lookup"><span data-stu-id="1c4a4-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="1c4a4-147">Trois méthodes de publication sont pris en charge pour la publication de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="1c4a4-148">`MSDeploy` -Publier à l’aide de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="1c4a4-149">`Package` -Publier en créant un Package de déploiement Web.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="1c4a4-150">Vous devez l’installer séparément à partir de la commande MSBuild qui le crée.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="1c4a4-151">`FileSystem` -Publier en copiant les fichiers dans un dossier spécifié.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="1c4a4-152">Spécification de la plateforme et la configuration de build</span><span class="sxs-lookup"><span data-stu-id="1c4a4-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="1c4a4-153">La plateforme et la configuration de build doivent être définies dans Visual Studio ou sur la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="1c4a4-154">Les profils de publication incluent des propriétés qui sont nommées `LastUsedBuildConfiguration` et `LastUsedPlatform`, mais vous ne pouvez pas définir ces propriétés afin de déterminer la façon dont le projet est généré.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="1c4a4-155">Pour plus d’informations, consultez [MSBuild : comment définir la propriété de configuration](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) sur le blog de Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="1c4a4-156">Déployer dans l’environnement intermédiaire</span><span class="sxs-lookup"><span data-stu-id="1c4a4-156">Deploy to staging</span></span>

<span data-ttu-id="1c4a4-157">Pour déployer sur Azure, vous devez ajouter le mot de passe à la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="1c4a4-158">Si vous avez enregistré le mot de passe dans le profil de publication dans Visual Studio, il a été stocké dans un format chiffré dans la votre *. pubxml.user* fichier.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="1c4a4-159">Ce fichier n’est pas accessible par MSBuild lorsque vous effectuez un déploiement de la ligne de commande, donc vous devez passer le mot de passe dans un paramètre de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="1c4a4-160">Copier le mot de passe dont vous avez besoin à partir de la *.publishsettings* fichier que vous avez téléchargé précédemment pour le site web intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="1c4a4-161">Le mot de passe est la valeur de la `userPWD` attribut pour le Web Deploy `publishProfile` élément.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Mot de passe de Web Deploy](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="1c4a4-163">Dans la page d’accueil de Windows 8, recherchez **invite de commandes développeur pour VS2012**, puis cliquez sur l’icône pour ouvrir l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="1c4a4-164">(Vous n’avez pas pour l’ouvrir en tant qu’administrateur instant, car vous ne déployez pas sur le serveur IIS sur l’ordinateur local.)</span><span class="sxs-lookup"><span data-stu-id="1c4a4-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="1c4a4-165">Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier de solution avec le chemin d’accès à votre fichier solution et le mot de passe avec votre mot de passe :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="1c4a4-166">Notez que cette ligne de commande inclut un paramètre supplémentaire : `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="1c4a4-167">Comme ce didacticiel est écrit, le `AllowUntrustedCertificate` propriété doit être définie lorsque vous publiez sur Azure à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="1c4a4-168">Quand le correctif pour ce bogue est publié, vous n’aurez pas ce paramètre.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="1c4a4-169">Ouvrez un navigateur et accédez à l’URL de votre site intermédiaire, puis cliquez sur le **sur** page pour vérifier que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="1c4a4-170">Comme vous l’avez vu précédemment pour l’environnement de test, vous devrez créer des étudiants pour voir les statistiques sur les **sur** page.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="1c4a4-171">Déployer en production</span><span class="sxs-lookup"><span data-stu-id="1c4a4-171">Deploy to production</span></span>

<span data-ttu-id="1c4a4-172">Le processus de déploiement en production est similaire au processus de mise en lots.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="1c4a4-173">Copier le mot de passe dont vous avez besoin à partir de la *.publishsettings* fichier que vous avez téléchargé précédemment pour le site web de production.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="1c4a4-174">Ouvrez **invite de commandes développeur pour VS2012**.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="1c4a4-175">Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier de solution avec le chemin d’accès à votre fichier solution et le mot de passe avec votre mot de passe :</span><span class="sxs-lookup"><span data-stu-id="1c4a4-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="1c4a4-176">Pour un site de production réel, s’il existe également une modification de la base de données, vous en général, copiez la *application\_offline.htm* au site avant le déploiement de fichiers et le supprimer après un déploiement réussi.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="1c4a4-177">Ouvrez un navigateur et accédez à l’URL de votre site intermédiaire, puis cliquez sur le **sur** page pour vérifier que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="1c4a4-178">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="1c4a4-178">Summary</span></span>

<span data-ttu-id="1c4a4-179">Vous avez déployé une mise à jour de l’application à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-179">You have now deployed an application update by using the command line.</span></span>

![Sur la page dans un environnement de Test](command-line-deployment/_static/image6.png)

<span data-ttu-id="1c4a4-181">Dans l’étape suivante du didacticiel, vous verrez un exemple montrant comment étendre le site web Pipe-Line de publication.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="1c4a4-182">L’exemple illustre comment déployer des fichiers qui ne sont pas inclus dans le projet.</span><span class="sxs-lookup"><span data-stu-id="1c4a4-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c4a4-183">[Précédent](deploying-a-database-update.md)
> [Suivant](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="1c4a4-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
