---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configuration de la Solution de gestionnaire de contacts | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment télécharger et configurer la solution de Contact Manager pour exécuter localement sur une station de travail de développeur.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 479dbb8d2edbe9fb953ea9e1312ffb8fdbd3e2fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802129"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="aeeda-103">Configuration de la Solution de gestionnaire de contacts</span><span class="sxs-lookup"><span data-stu-id="aeeda-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="aeeda-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="aeeda-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="aeeda-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="aeeda-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="aeeda-106">Cette rubrique décrit comment télécharger et configurer la solution de Contact Manager pour exécuter localement sur une station de travail de développeur.</span><span class="sxs-lookup"><span data-stu-id="aeeda-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="aeeda-107">Configuration système requise</span><span class="sxs-lookup"><span data-stu-id="aeeda-107">System Requirements</span></span>

<span data-ttu-id="aeeda-108">Pour exécuter la solution de Contact Manager localement et effectuer les autres tâches décrites dans ce didacticiel, vous devez installer ce logiciel sur votre station de travail de développeur :</span><span class="sxs-lookup"><span data-stu-id="aeeda-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="aeeda-109">Visual Studio 2010 Service Pack 1, Premium ou Édition intégrale</span><span class="sxs-lookup"><span data-stu-id="aeeda-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="aeeda-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="aeeda-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="aeeda-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="aeeda-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="aeeda-112">IIS outil de déploiement Web (Web Deploy) 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="aeeda-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="aeeda-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="aeeda-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="aeeda-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="aeeda-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="aeeda-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="aeeda-115">.NET Framework 4</span></span>
- <span data-ttu-id="aeeda-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="aeeda-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="aeeda-117">À l’exception de Visual Studio 2010, vous pouvez télécharger et installer les dernières versions de tous ces produits et les composants par le biais du [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="aeeda-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="aeeda-118">Téléchargez et extrayez la Solution</span><span class="sxs-lookup"><span data-stu-id="aeeda-118">Download and Extract the Solution</span></span>

<span data-ttu-id="aeeda-119">Vous pouvez télécharger l’exemple d’application Gestionnaire de contacts à partir de MSDN Code Gallery [ici](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="aeeda-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="aeeda-120">Configurer et exécuter la Solution</span><span class="sxs-lookup"><span data-stu-id="aeeda-120">Configure and Run the Solution</span></span>

<span data-ttu-id="aeeda-121">Pour configurer et exécuter la solution de gestionnaire de contacts sur votre ordinateur local, vous devez effectuer ces étapes :</span><span class="sxs-lookup"><span data-stu-id="aeeda-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="aeeda-122">Si vous n’avez pas déjà, créez une base de services d’application ASP.NET locale avec les fonctionnalités de gestion des appartenances et de rôles est activées.</span><span class="sxs-lookup"><span data-stu-id="aeeda-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="aeeda-123">Modifier les chaînes de connexion dans le *web.config* fichiers pour pointer vers votre instance locale de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="aeeda-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="aeeda-124">Exécutez la solution à partir de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aeeda-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="aeeda-125">Le reste de cette section fournit plus d’informations sur comment effectuer chacune de ces tâches.</span><span class="sxs-lookup"><span data-stu-id="aeeda-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="aeeda-126">**Pour créer la base de données de services d’application**</span><span class="sxs-lookup"><span data-stu-id="aeeda-126">**To create the application services database**</span></span>

1. <span data-ttu-id="aeeda-127">Ouvrez une invite de commandes de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aeeda-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="aeeda-128">Pour ce faire, sur le **Démarrer** menu, pointez sur **tous les programmes**, cliquez sur **Microsoft Visual Studio 2010**, cliquez sur **Visual Studio Tools**, puis Cliquez sur **invite de commandes Visual Studio (2010)**.</span><span class="sxs-lookup"><span data-stu-id="aeeda-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="aeeda-129">À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE :</span><span class="sxs-lookup"><span data-stu-id="aeeda-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="aeeda-130">Utilisez le **– C** commutateur pour spécifier la chaîne de connexion pour votre serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="aeeda-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="aeeda-131">Utilisez le **– A** commutateur pour spécifier les fonctionnalités que vous souhaitez ajouter à la base de données de services de l’application.</span><span class="sxs-lookup"><span data-stu-id="aeeda-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="aeeda-132">Dans ce cas, **m** indique que vous souhaitez ajouter la prise en charge pour le fournisseur d’appartenances et **r** indique que vous souhaitez ajouter la prise en charge pour le Gestionnaire de rôles.</span><span class="sxs-lookup"><span data-stu-id="aeeda-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="aeeda-133">Utilisez le **– d** commutateur pour spécifier un nom pour votre base de données de services d’application.</span><span class="sxs-lookup"><span data-stu-id="aeeda-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="aeeda-134">Si vous omettez ce commutateur, l’utilitaire crée une base de données portant le nom par défaut **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="aeeda-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="aeeda-135">Lorsque la base de données a été créé avec succès, l’invite de commandes affiche une confirmation.</span><span class="sxs-lookup"><span data-stu-id="aeeda-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="aeeda-136">Pour plus d’informations sur le compte aspnet\_regsql utilitaire, consultez [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="aeeda-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="aeeda-137">L’étape suivante consiste à vous assurer que les chaînes de connexion dans la solution Gestionnaire de contacts pointent vers votre instance locale de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="aeeda-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="aeeda-138">**Pour mettre à jour les chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="aeeda-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="aeeda-139">Ouvrez la solution de gestionnaire de contacts dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aeeda-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="aeeda-140">Dans le **l’Explorateur de solutions** fenêtre, développez le **ContactManager.Mvc** de projet, puis double-cliquez sur le **Web.config** nœud.</span><span class="sxs-lookup"><span data-stu-id="aeeda-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aeeda-141">Le projet ContactManager.Mvc inclut deux *web.config* fichiers.</span><span class="sxs-lookup"><span data-stu-id="aeeda-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="aeeda-142">Vous devez modifier le fichier au niveau du projet.</span><span class="sxs-lookup"><span data-stu-id="aeeda-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="aeeda-143">Dans le **connectionStrings** élément, vérifiez que la chaîne de connexion nommée **ApplicationServices** pointe vers votre base de services d’application ASP.NET locale.</span><span class="sxs-lookup"><span data-stu-id="aeeda-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="aeeda-144">Dans le **l’Explorateur de solutions** fenêtre, développez le **ContactManager.Service** de projet, puis double-cliquez sur le **Web.config** nœud.</span><span class="sxs-lookup"><span data-stu-id="aeeda-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="aeeda-145">Dans le **connectionStrings** élément, dans la chaîne de connexion nommée **ContactManagerContext**, vérifiez que le **Source de données** propriété est définie sur votre instance locale de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="aeeda-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="aeeda-146">Vous n’avez pas besoin de modifier quoi que ce soit d’autre dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="aeeda-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="aeeda-147">Enregistrez tous les fichiers ouverts.</span><span class="sxs-lookup"><span data-stu-id="aeeda-147">Save all open files.</span></span>

<span data-ttu-id="aeeda-148">Vous devez maintenant être prêt à exécuter la solution de gestionnaire de contacts sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aeeda-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="aeeda-149">Si vous suivez ces étapes sans d’abord créer une base de données de services d’application, ASP.NET crée la base de données la première fois que vous essayez de créer un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="aeeda-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="aeeda-150">Toutefois, création manuelle de la base de données vous donne un contrôle plus important sur l’ensemble de fonctionnalités de services application que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="aeeda-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="aeeda-151">**Pour exécuter la solution de gestionnaire de contacts**</span><span class="sxs-lookup"><span data-stu-id="aeeda-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="aeeda-152">Dans Visual Studio 2010, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="aeeda-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="aeeda-153">Internet Explorer démarre et demande l’URL de l’application ASP.NET MVC 3 Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="aeeda-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="aeeda-154">Par défaut, l’application affiche le **tous les Contacts** page.</span><span class="sxs-lookup"><span data-stu-id="aeeda-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="aeeda-155">Ajouter plusieurs contacts et vérifiez que l’application fonctionne comme prévu.</span><span class="sxs-lookup"><span data-stu-id="aeeda-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="aeeda-156">Accédez à `http://localhost:50114/Account/Register` (ajuster l’URL si vous hébergez l’application sur un port différent).</span><span class="sxs-lookup"><span data-stu-id="aeeda-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="aeeda-157">Ajouter un nom d’utilisateur, une adresse e-mail et un mot de passe et vérifiez que vous êtes en mesure d’inscrire un compte avec succès.</span><span class="sxs-lookup"><span data-stu-id="aeeda-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="aeeda-158">Accédez à `http://localhost:50114/Account/LogOn` (ajuster l’URL si vous hébergez l’application sur un port différent).</span><span class="sxs-lookup"><span data-stu-id="aeeda-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="aeeda-159">Vérifiez que vous êtes en mesure d’ouvrir une session en utilisant le compte que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="aeeda-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="aeeda-160">Fermez Internet Explorer pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="aeeda-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="aeeda-161">Conclusion</span><span class="sxs-lookup"><span data-stu-id="aeeda-161">Conclusion</span></span>

<span data-ttu-id="aeeda-162">À ce stade, la solution de Contact Manager doit être entièrement configurée pour s’exécuter sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aeeda-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="aeeda-163">Lorsque vous travaillez via les autres rubriques dans ce didacticiel, vous pouvez utiliser la solution en tant que référence.</span><span class="sxs-lookup"><span data-stu-id="aeeda-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="aeeda-164">La rubrique suivante, [présentation du fichier de projet](understanding-the-project-file.md), explique comment vous pouvez utiliser les fichiers de projet Microsoft Build Engine (MSBuild) personnalisés au sein de la solution de Contact Manager pour contrôler le processus de déploiement.</span><span class="sxs-lookup"><span data-stu-id="aeeda-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aeeda-165">[Précédent](the-contact-manager-solution.md)
> [Suivant](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="aeeda-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
