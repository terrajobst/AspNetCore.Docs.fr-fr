---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Ajout de contenu au contrôle de code Source | Documents Microsoft"
author: jrjlee
description: "Cette rubrique explique comment ajouter du contenu à un contrôle de code source Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à un projet d’équipe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: a6a90a03674cfe7565da0ed56148186ee9525707
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="6c63c-104">Ajout de contenu au contrôle de code Source</span><span class="sxs-lookup"><span data-stu-id="6c63c-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="6c63c-105">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="6c63c-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="6c63c-106">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="6c63c-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="6c63c-107">Cette rubrique explique comment ajouter du contenu à un contrôle de code source Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="6c63c-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="6c63c-108">Il décrit comment ajouter des solutions et des projets à un projet d’équipe dans TFS, et elle explique comment ajouter des dépendances externes, telles que les infrastructures ou les assemblys au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="6c63c-109">Cette rubrique fait partie d’une série de didacticiels basées sur les spécifications de déploiement d’entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution l’a & #x 2014 ; le [solution Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014 ; pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, Windows Service de communication Foundation (WCF) et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="6c63c-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="6c63c-110">Vue d’ensemble de la tâche</span><span class="sxs-lookup"><span data-stu-id="6c63c-110">Task Overview</span></span>

<span data-ttu-id="6c63c-111">Dans la plupart des cas, tous les membres de l’équipe de développement doivent être en mesure d’ajouter du contenu pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="6c63c-112">Pour ajouter une solution au contrôle de code source TFS, vous devez effectuer ces étapes :</span><span class="sxs-lookup"><span data-stu-id="6c63c-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="6c63c-113">Se connecter à un projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="6c63c-113">Connect to a team project.</span></span>
- <span data-ttu-id="6c63c-114">Mapper la structure de dossier du projet équipe sur le serveur à une structure de dossiers sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="6c63c-115">Ajouter la solution et son contenu pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="6c63c-116">Ajoutez toutes les dépendances externes pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="6c63c-117">Cette rubrique vous indique comment effectuer ces procédures.</span><span class="sxs-lookup"><span data-stu-id="6c63c-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="6c63c-118">Les tâches et les procédures pas à pas dans cette rubrique supposent que vous avez déjà créé un nouveau projet d’équipe TFS pour gérer votre contenu.</span><span class="sxs-lookup"><span data-stu-id="6c63c-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="6c63c-119">Pour plus d’informations sur la création d’un nouveau projet d’équipe, consultez [création d’un projet d’équipe dans TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="6c63c-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="6c63c-120">Qui exécute ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="6c63c-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="6c63c-121">Dans la plupart des cas, tous les membres de l’équipe de développement doivent être en mesure d’ajouter et modifier le contenu dans des projets d’équipe spécifique.</span><span class="sxs-lookup"><span data-stu-id="6c63c-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="6c63c-122">Se connecter à un projet d’équipe et créer un mappage de dossier</span><span class="sxs-lookup"><span data-stu-id="6c63c-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="6c63c-123">Avant d’ajouter un contenu au contrôle de code source, vous devez vous connecter à un projet d’équipe et créer un mappage entre la structure de dossiers sur le serveur et le système de fichiers sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="6c63c-124">**Se connecter à un projet d’équipe et de mapper un chemin d’accès local**</span><span class="sxs-lookup"><span data-stu-id="6c63c-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="6c63c-125">Sur votre station de travail de développement, ouvrez Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="6c63c-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="6c63c-126">Dans Visual Studio, sur le **équipe** menu, cliquez sur **se connecter à Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6c63c-127">Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 3 à 6.</span><span class="sxs-lookup"><span data-stu-id="6c63c-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="6c63c-128">Dans le **connexion au projet d’équipe** boîte de dialogue, cliquez sur **serveurs**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="6c63c-129">Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="6c63c-130">Dans le **Ajouter Team Foundation Server** boîte de dialogue, fournissez les détails de votre instance TFS, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="6c63c-131">Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="6c63c-132">Dans le **se connecter au projet d’équipe** boîte de dialogue, sélectionnez l’instance TFS que vous souhaitez vous connecter, sélectionnez l’équipe de projet collection, sélectionnez le projet d’équipe que vous souhaitez ajouter à, puis cliquez sur **connexion**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="6c63c-133">Dans le **Team Explorer** fenêtre, développez votre projet d’équipe, puis double-cliquez sur **contrôle de code Source**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="6c63c-134">Sur le **Explorateur du contrôle de code Source** , cliquez sur **ne pas mappé**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="6c63c-135">Dans le **carte** boîte de dialogue le **dossier Local** zone, accédez au (ou créez) un dossier local à agir en tant que le dossier racine du projet d’équipe, puis cliquez sur **carte**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="6c63c-136">Lorsque vous êtes invité à télécharger les fichiers sources, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="6c63c-137">À ce stade, vous avez mappé le dossier côté serveur pour le projet d’équipe vers un dossier local sur votre station de travail du développeur.</span><span class="sxs-lookup"><span data-stu-id="6c63c-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="6c63c-138">Vous avez également téléchargé tout contenu existant à partir du projet d’équipe à votre structure de dossier local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="6c63c-139">Vous pouvez maintenant commencer à ajouter votre propre contenu de contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="6c63c-140">Ajouter des projets et Solutions au contrôle de code Source</span><span class="sxs-lookup"><span data-stu-id="6c63c-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="6c63c-141">Pour ajouter des projets et solutions au contrôle de code source, vous devez d’abord les déplacer vers le dossier mappé pour le projet d’équipe sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="6c63c-142">Vous pouvez ensuite archiver le contenu à synchroniser vos ajouts sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6c63c-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="6c63c-143">**Pour ajouter des projets au contrôle de code source**</span><span class="sxs-lookup"><span data-stu-id="6c63c-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="6c63c-144">Sur votre station de travail du développeur, déplacez vos projets et solutions à un emplacement approprié dans la structure de dossier mappé pour le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="6c63c-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6c63c-145">De nombreuses organisations ont une meilleure approche à la façon dont les projets et solutions doivent être organisées dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="6c63c-146">Pour obtenir des conseils sur la façon de structurer les dossiers, consultez [How To : Structure de dossiers du contrôle de Source de votre dans Team Foundation Server](https://msdn.microsoft.com/en-us/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c63c-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/en-us/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="6c63c-147">Ouvrez la solution dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="6c63c-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="6c63c-148">Dans le **l’Explorateur de solutions** fenêtre, avec le bouton droit de la solution, puis cliquez sur **ajouter la Solution au contrôle de code Source**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="6c63c-149">Dans certains cas, en fonction de la manière dont votre organisation fonctionne à la structure de contenu dans TFS, vous devrez peut-être ajouter des projets au contrôle de code source individuellement pour fournir un contrôle plus précis sur l’organisation de votre code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="6c63c-150">Vérifiez que le **Explorateur du contrôle de code Source** onglet affiche le contenu que vous avez ajouté au sein de la structure de dossiers de serveur pour le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="6c63c-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="6c63c-151">Le **Explorateur du contrôle de code Source** onglet affiche votre contenu avec aucune autre demande, car vous avez ajouté votre solution à un dossier mappé sur le système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="6c63c-152">Si votre solution se trouvait dans un emplacement non mappé, vous êtes invité à spécifier des emplacements de dossier dans TFS et votre système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="6c63c-153">Sur le **Explorateur du contrôle de code Source** sous l’onglet le **dossiers** volet, cliquez sur le projet d’équipe (par exemple, **ContactManager**), puis cliquez sur **archiver Modifications en attente**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="6c63c-154">Dans le **archiver – fichiers sources** boîte de dialogue, tapez un commentaire, puis cliquez sur **archiver**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="6c63c-155">À ce stade, vous avez ajouté votre solution au contrôle de code source TFS.</span><span class="sxs-lookup"><span data-stu-id="6c63c-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="6c63c-156">Ajouter des dépendances externes pour le contrôle de code Source</span><span class="sxs-lookup"><span data-stu-id="6c63c-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="6c63c-157">Lorsque vous ajoutez un projet ou une solution au contrôle de code source, tous les fichiers et dossiers dans votre projet ou solution seront également être ajoutés.</span><span class="sxs-lookup"><span data-stu-id="6c63c-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="6c63c-158">Toutefois, dans de nombreux cas, projets et solutions également reposent sur des dépendances externes, telles que des assemblys locaux pour fonctionner correctement.</span><span class="sxs-lookup"><span data-stu-id="6c63c-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="6c63c-159">Vous devez ajouter ce genre de ressource pour le contrôle de code source pour vous permettre de Team Build et des autres membres de l’équipe de développement générer avec succès votre code.</span><span class="sxs-lookup"><span data-stu-id="6c63c-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="6c63c-160">Par exemple, la structure de dossiers pour l’exemple de gestionnaire de contacts solution inclut un dossier nommé packages.</span><span class="sxs-lookup"><span data-stu-id="6c63c-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="6c63c-161">Il contient l’assembly et les différentes ressources de prise en charge pour ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="6c63c-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="6c63c-162">Le dossier packages ne fait pas partie de la solution de gestionnaire de contacts, mais la solution ne générera pas correctement sans lui.</span><span class="sxs-lookup"><span data-stu-id="6c63c-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="6c63c-163">Pour activer Team Build générer la solution, vous devez ajouter le dossier packages pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="6c63c-164">L’inclusion d’un dossier de packages est courant de ce qui se passe lorsque vous ajoutez l’Entity Framework, ou des ressources similaires, à votre solution à l’aide de l’extension NuGet pour Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="6c63c-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="6c63c-165">**Pour ajouter du contenu de projet au contrôle de code source**</span><span class="sxs-lookup"><span data-stu-id="6c63c-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="6c63c-166">Assurez-vous que les éléments que vous souhaitez ajouter (par exemple, le dossier packages) se trouvent dans un emplacement approprié au sein d’un dossier mappé sur votre système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="6c63c-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="6c63c-167">Dans Visual Studio 2010, dans le **Team Explorer** fenêtre, développez votre projet d’équipe, puis double-cliquez sur **contrôle de code Source**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="6c63c-168">Sur le **Explorateur du contrôle de code Source** sous l’onglet du **dossiers** volet, sélectionnez le dossier qui contient l’élément ou d’éléments que vous souhaitez ajouter.</span><span class="sxs-lookup"><span data-stu-id="6c63c-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="6c63c-169">Cliquez sur le **ajouter des éléments au dossier** bouton.</span><span class="sxs-lookup"><span data-stu-id="6c63c-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="6c63c-170">Dans le **ajouter au contrôle de code Source** boîte de dialogue, sélectionnez le dossier ou les éléments que vous souhaitez ajouter, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="6c63c-171">Sur le **éléments exclus** , sélectionnez tous les éléments requis qui ont été exclus automatiquement (par exemple, les assemblys), puis cliquez sur **incluent les éléments**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="6c63c-172">Sur le **les éléments à ajouter** , vérifiez que tous les fichiers que vous souhaitez inclure sont répertoriés, puis cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="6c63c-173">Dans le **Explorateur du contrôle de code Source** fenêtre, cliquez sur le **archiver** bouton.</span><span class="sxs-lookup"><span data-stu-id="6c63c-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="6c63c-174">Dans le **archiver – fichiers sources** boîte de dialogue, tapez un commentaire, puis cliquez sur **archiver**.</span><span class="sxs-lookup"><span data-stu-id="6c63c-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="6c63c-175">À ce stade, vous avez ajouté les dépendances externes pour votre solution au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="6c63c-176">Conclusion</span><span class="sxs-lookup"><span data-stu-id="6c63c-176">Conclusion</span></span>

<span data-ttu-id="6c63c-177">Cette rubrique décrit comment se connecter à un projet d’équipe, mapper une structure de dossiers et ajouter du contenu à un contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="6c63c-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="6c63c-178">Pour plus d’informations sur la façon de travailler avec les éléments sous contrôle de code source, consultez [à l’aide de la gestion de Version](https://msdn.microsoft.com/en-us/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c63c-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/en-us/library/ms181368.aspx).</span></span>

<span data-ttu-id="6c63c-179">La rubrique suivante, [configuration d’un serveur de Build TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md), explique comment préparer un serveur TFS Team Build pour générer et déployer votre solution.</span><span class="sxs-lookup"><span data-stu-id="6c63c-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="6c63c-180">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6c63c-180">Further Reading</span></span>

<span data-ttu-id="6c63c-181">Pour plus d’informations sur l’utilisation de contrôle de code source TFS, consultez [à l’aide de la gestion de Version](https://msdn.microsoft.com/en-us/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c63c-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/en-us/library/ms181368.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6c63c-182">[Précédent](creating-a-team-project-in-tfs.md)
[Suivant](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="6c63c-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
