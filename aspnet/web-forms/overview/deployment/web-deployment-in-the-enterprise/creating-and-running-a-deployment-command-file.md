---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Création et exécution d’un déploiement de fichier de commandes | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment créer un fichier de commandes qui vous permettent d’exécuter un déploiement à l’aide de fichiers de projet Microsoft Build Engine (MSBuild) en tant qu’une seule étape, re...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 121df7482f7cbd70b191e518bae791f0642ccc7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828972"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="6f4ba-103">Création et exécution d’un fichier de commande de déploiement</span><span class="sxs-lookup"><span data-stu-id="6f4ba-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="6f4ba-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="6f4ba-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="6f4ba-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="6f4ba-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="6f4ba-106">Cette rubrique décrit comment créer un fichier de commandes qui vous permettent d’exécuter un déploiement à l’aide de fichiers de projet Microsoft Build Engine (MSBuild) comme un processus pas à pas et reproductible.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="6f4ba-107">Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [Contact Manager](the-contact-manager-solution.md) solution&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="6f4ba-108">La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [comprendre le processus de génération](understanding-the-build-process.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="6f4ba-109">Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="6f4ba-110">Vue d’ensemble du processus</span><span class="sxs-lookup"><span data-stu-id="6f4ba-110">Process Overview</span></span>

<span data-ttu-id="6f4ba-111">Dans cette rubrique, vous allez apprendre à créer et exécuter un fichier de commande qui utilise ces fichiers de projet pour effectuer un déploiement reproductible sur votre environnement cible.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="6f4ba-112">Essentiellement, le fichier de commandes doit simplement contenir une commande MSBuild qui :</span><span class="sxs-lookup"><span data-stu-id="6f4ba-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="6f4ba-113">Indique à MSBuild d’exécuter l’indépendant de l’environnement *Publish.proj* fichier.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="6f4ba-114">Indique le *Publish.proj* fichier fichier qui contient les paramètres spécifiques à l’environnement et où les trouver.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="6f4ba-115">Créer une commande MSBuild</span><span class="sxs-lookup"><span data-stu-id="6f4ba-115">Create an MSBuild Command</span></span>

<span data-ttu-id="6f4ba-116">Comme décrit dans [comprendre le processus de génération](understanding-the-build-process.md), le fichier de projet spécifique à un environnement&#x2014;, par exemple, *Env-Dev.proj*&#x2014;est conçu pour être importé dans l’environnement agnostique *Publish.proj* fichier au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="6f4ba-117">Ensemble, ces deux fichiers fournissent un ensemble complet d’instructions qui indiquent comment générer et déployer votre solution à MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="6f4ba-118">Le *Publish.proj* fichier utilise un **importer** élément pour importer le fichier de projet spécifique à l’environnement.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="6f4ba-119">Par conséquent, lorsque vous utilisez MSBuild.exe pour générer et déployer la solution Gestionnaire de contacts, vous devez :</span><span class="sxs-lookup"><span data-stu-id="6f4ba-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="6f4ba-120">Exécutez MSBuild.exe le *Publish.proj* fichier.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="6f4ba-121">Spécifiez l’emplacement du fichier de projet spécifique à un environnement en fournissant un paramètre de ligne de commande nommé **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="6f4ba-122">Pour ce faire, votre commande MSBuild doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="6f4ba-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="6f4ba-123">À ce stade, il est une étape simple pour déplacer vers un déploiement reproductible et à étape unique.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="6f4ba-124">Il vous suffit de faire consiste à ajouter votre commande de MSBuild dans un fichier .cmd.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="6f4ba-125">Dans la solution Gestionnaire de contacts, le dossier de publication inclut un fichier nommé *publier-Dev.cmd* qui effectue exactement cela.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="6f4ba-126">Le **/fl** commutateur indique à MSBuild pour créer un fichier journal nommé *msbuild.log* dans le répertoire de travail dans lequel MSBuild.exe a été appelée.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="6f4ba-127">Pour déployer ou de redéployer la solution Gestionnaire de contacts, il vous suffit est exécuté le *publier-Dev.cmd* fichier.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="6f4ba-128">Lorsque vous exécutez le fichier, MSBuild va :</span><span class="sxs-lookup"><span data-stu-id="6f4ba-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="6f4ba-129">Générer tous les projets de la solution.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="6f4ba-130">Générer des packages web pouvant être déployées pour les projets d’application web.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="6f4ba-131">Générer des fichiers .dbschema et .deploymanifest pour les projets de base de données.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="6f4ba-132">Déployer les packages web sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="6f4ba-133">Déployer la base de données sur le serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="6f4ba-134">Exécuter le déploiement</span><span class="sxs-lookup"><span data-stu-id="6f4ba-134">Run the Deployment</span></span>

<span data-ttu-id="6f4ba-135">Lorsque vous avez créé un fichier de commandes pour votre environnement cible, il se peut que vous devez être en mesure d’effectuer la totalité du déploiement en exécutant simplement le fichier.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="6f4ba-136">**Pour déployer la solution de gestionnaire de contacts dans votre environnement de test**</span><span class="sxs-lookup"><span data-stu-id="6f4ba-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="6f4ba-137">Sur votre station de travail de développeur, ouvrez l’Explorateur Windows, puis accédez à l’emplacement de la *publier-Dev.cmd* fichier.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="6f4ba-138">Double-cliquez sur le fichier pour l’exécuter.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="6f4ba-139">Si un **ouvrir un fichier – Avertissement de sécurité** boîte de dialogue s’affiche, cliquez sur **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="6f4ba-140">Si vos paramètres de configuration et les serveurs de test sont configurés correctement, la fenêtre d’invite de commandes affichera un **Build réussie** message lorsque MSBuild a fini de traiter les fichiers de projet.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="6f4ba-141">S’il s’agit de la première fois que vous avez déployé la solution dans cet environnement, vous devez ajouter le compte d’ordinateur du serveur test web pour le **db\_datawriter** et **db\_datareader**rôles sur le **ContactManager** base de données.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="6f4ba-142">Cette procédure est décrite dans [configurer un serveur de base de données pour déployer de publication Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="6f4ba-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="6f4ba-143">Il vous suffit de vous affecter ces autorisations lorsque vous créez la base de données.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="6f4ba-144">Par défaut, le processus de génération ne recrée pas la base de données sur chaque déploiement&#x2014;au lieu de cela, il compare la base de données existante pour le schéma le plus récent et assurez-vous que les modifications nécessaires.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="6f4ba-145">Par conséquent, vous devez uniquement mapper ces rôles de base de données la première fois que vous déployez la solution.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="6f4ba-146">Ouvrez Internet Explorer et accédez à l’URL de l’application Gestionnaire de contacts (par exemple, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="6f4ba-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="6f4ba-147">Vérifiez que l’application fonctionne comme prévu et que vous êtes en mesure d’ajouter des contacts.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="6f4ba-148">Conclusion</span><span class="sxs-lookup"><span data-stu-id="6f4ba-148">Conclusion</span></span>

<span data-ttu-id="6f4ba-149">Création d’un fichier de commandes qui contient vos instructions MSBuild vous offre un moyen rapide et facile de créer et déployer une solution à projets multiples dans un environnement de destination spécifique.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="6f4ba-150">Si vous avez besoin déployer votre solution de manière répétée dans plusieurs environnements de destination, vous pouvez créer plusieurs fichiers de commandes.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="6f4ba-151">Chaque fichier de commandes, la commande MSBuild génère le même fichier de projet d’application universelle, mais elle spécifie un fichier de projet spécifiques à l’environnement différent.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="6f4ba-152">Par exemple, un fichier de commandes pour publier sur un développeur ou l’environnement de test peut contenir cette commande MSBuild :</span><span class="sxs-lookup"><span data-stu-id="6f4ba-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="6f4ba-153">Un fichier de commandes pour publier dans un environnement intermédiaire peut contenir cette commande MSBuild :</span><span class="sxs-lookup"><span data-stu-id="6f4ba-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="6f4ba-154">Pour obtenir des conseils sur la façon de personnaliser les fichiers de projet spécifique à l’environnement pour vos propres environnements serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="6f4ba-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="6f4ba-155">Vous pouvez également personnaliser le processus de génération pour chaque environnement en substituant les propriétés ou en définissant plusieurs autres commutateurs dans votre commande MSBuild.</span><span class="sxs-lookup"><span data-stu-id="6f4ba-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="6f4ba-156">Pour plus d’informations, consultez [référence de ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f4ba-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f4ba-157">[Précédent](deploying-database-projects.md)
> [Suivant](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="6f4ba-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
