---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Ajout de contenu au contrôle de code Source | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment ajouter du contenu à un contrôle de code source Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à un projet d’équipe...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838369"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="9643f-104">Ajout de contenu à un contrôle de code Source</span><span class="sxs-lookup"><span data-stu-id="9643f-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="9643f-105">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9643f-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9643f-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="9643f-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9643f-107">Cette rubrique explique comment ajouter du contenu à un contrôle de code source Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="9643f-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="9643f-108">Il décrit comment ajouter des solutions et des projets à un projet d’équipe dans TFS, et elle explique comment ajouter des dépendances externes telles que des infrastructures ou des assemblys au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="9643f-109">Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="9643f-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9643f-110">Présentation de la tâche</span><span class="sxs-lookup"><span data-stu-id="9643f-110">Task Overview</span></span>

<span data-ttu-id="9643f-111">Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter du contenu à un contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="9643f-112">Pour ajouter une solution au contrôle de code source dans TFS, vous devez suivre ces étapes générales :</span><span class="sxs-lookup"><span data-stu-id="9643f-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="9643f-113">Se connecter à un projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="9643f-113">Connect to a team project.</span></span>
- <span data-ttu-id="9643f-114">Mapper la structure de dossiers du projet équipe sur le serveur à une structure de dossiers sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9643f-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="9643f-115">Contrôle de code source, ajoutez la solution et son contenu.</span><span class="sxs-lookup"><span data-stu-id="9643f-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="9643f-116">Ajoutez toutes les dépendances externes pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="9643f-117">Cette rubrique sera vous montrent comment effectuer ces procédures.</span><span class="sxs-lookup"><span data-stu-id="9643f-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="9643f-118">Les tâches et les procédures pas à pas dans cette rubrique supposent que vous avez déjà créé un nouveau projet d’équipe TFS pour gérer votre contenu.</span><span class="sxs-lookup"><span data-stu-id="9643f-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="9643f-119">Pour plus d’informations sur la création d’un nouveau projet d’équipe, consultez [création d’un projet d’équipe dans TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="9643f-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="9643f-120">Qui exécute ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="9643f-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="9643f-121">Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter et modifier le contenu dans des projets d’équipe spécifique.</span><span class="sxs-lookup"><span data-stu-id="9643f-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="9643f-122">Connectez-vous à un projet d’équipe et créez un mappage de dossier</span><span class="sxs-lookup"><span data-stu-id="9643f-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="9643f-123">Avant d’ajouter n’importe quel contenu pour le contrôle de code source, vous devez vous connecter à un projet d’équipe et créer un mappage entre la structure de dossiers sur le serveur et le système de fichiers sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9643f-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="9643f-124">**Se connecter à un projet d’équipe et mapper un chemin d’accès local**</span><span class="sxs-lookup"><span data-stu-id="9643f-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="9643f-125">Sur votre station de travail de développement, ouvrez Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9643f-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="9643f-126">Dans Visual Studio, sur le **équipe** menu, cliquez sur **se connecter à Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="9643f-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9643f-127">Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 3 à 6.</span><span class="sxs-lookup"><span data-stu-id="9643f-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="9643f-128">Dans le **connexion au projet d’équipe** boîte de dialogue, cliquez sur **serveurs**.</span><span class="sxs-lookup"><span data-stu-id="9643f-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="9643f-129">Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9643f-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="9643f-130">Dans le **Ajouter Team Foundation Server** boîte de dialogue, fournissez les détails de votre instance TFS, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9643f-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="9643f-131">Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="9643f-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="9643f-132">Dans le **se connecter au projet d’équipe** boîte de dialogue, sélectionnez l’instance TFS que vous souhaitez vous connecter à, sélectionnez l’équipe de collection de projets, sélectionnez le projet d’équipe que vous souhaitez ajouter à et puis cliquez sur **Connect**.</span><span class="sxs-lookup"><span data-stu-id="9643f-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="9643f-133">Dans le **Team Explorer** fenêtre, développez votre projet d’équipe, puis double-cliquez sur **contrôle de code Source**.</span><span class="sxs-lookup"><span data-stu-id="9643f-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="9643f-134">Sur le **Explorateur du contrôle de Source** , cliquez sur **ne pas mappé**.</span><span class="sxs-lookup"><span data-stu-id="9643f-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="9643f-135">Dans le **carte** boîte de dialogue le **dossier Local** boîte, accédez à (ou créez) un dossier local à agir en tant que le dossier racine du projet d’équipe, puis cliquez sur **carte**.</span><span class="sxs-lookup"><span data-stu-id="9643f-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="9643f-136">Lorsque vous êtes invité à télécharger les fichiers sources, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="9643f-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="9643f-137">À ce stade, vous avez mappé le dossier côté serveur pour le projet d’équipe à un dossier local sur votre station de travail de développeur.</span><span class="sxs-lookup"><span data-stu-id="9643f-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="9643f-138">Vous avez également téléchargé le contenu existant du projet d’équipe à votre structure de dossier local.</span><span class="sxs-lookup"><span data-stu-id="9643f-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="9643f-139">Vous pouvez maintenant commencer à ajouter votre propre contenu pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="9643f-140">Ajouter des projets et Solutions au contrôle de code Source</span><span class="sxs-lookup"><span data-stu-id="9643f-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="9643f-141">Pour ajouter des projets et solutions au contrôle de code source, vous devez tout d’abord les déplacer vers le dossier du projet d’équipe mappé sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9643f-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="9643f-142">Vous pouvez ensuite vérifier dans le contenu à synchroniser vos ajouts avec le serveur.</span><span class="sxs-lookup"><span data-stu-id="9643f-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="9643f-143">**Pour ajouter des projets au contrôle de code source**</span><span class="sxs-lookup"><span data-stu-id="9643f-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="9643f-144">Sur votre station de travail de développeur, déplacez vos projets et solutions vers un emplacement approprié dans la structure de dossier mappé pour le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="9643f-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9643f-145">De nombreuses organisations ont une meilleure approche à comment les projets et solutions doivent être répartis dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="9643f-146">Pour obtenir des conseils sur la façon de structurer les dossiers, consultez [How To : dossiers de contrôle de Source de votre Structure dans Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="9643f-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="9643f-147">Ouvrez la solution dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9643f-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="9643f-148">Dans le **l’Explorateur de solutions** fenêtre, avec le bouton droit de la solution, puis cliquez sur **ajouter la Solution au contrôle de code Source**.</span><span class="sxs-lookup"><span data-stu-id="9643f-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="9643f-149">Dans certains cas, selon la façon dont votre organisation souhaite de structure de contenu dans TFS, vous devrez peut-être ajouter des projets au contrôle de code source individuellement pour fournir un contrôle plus précis sur la façon dont votre code source est organisé.</span><span class="sxs-lookup"><span data-stu-id="9643f-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="9643f-150">Vérifiez que le **Explorateur du contrôle de Source** onglet affiche le contenu que vous avez ajouté au sein de la structure de dossiers de serveur pour le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="9643f-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="9643f-151">Le **Explorateur du contrôle de Source** onglet affiche votre contenu avec aucune autre invite puisque vous avez ajouté votre solution à un dossier mappé sur le système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="9643f-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="9643f-152">Si votre solution se trouvait dans un emplacement non mappé, vous êtes invité à spécifier des emplacements de dossier dans TFS et votre système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="9643f-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="9643f-153">Sur le **Explorateur du contrôle de Source** sous l’onglet le **dossiers** volet, cliquez sur le projet d’équipe (par exemple, **ContactManager**), puis cliquez sur **archiver Modifications en attente**.</span><span class="sxs-lookup"><span data-stu-id="9643f-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="9643f-154">Dans le **archiver – fichiers sources** boîte de dialogue, tapez un commentaire, puis cliquez sur **archiver**.</span><span class="sxs-lookup"><span data-stu-id="9643f-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="9643f-155">À ce stade, vous avez ajouté votre solution au contrôle de code source dans TFS.</span><span class="sxs-lookup"><span data-stu-id="9643f-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="9643f-156">Ajouter des dépendances externes pour le contrôle de code Source</span><span class="sxs-lookup"><span data-stu-id="9643f-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="9643f-157">Lorsque vous ajoutez un projet ou une solution au contrôle de code source, tous les fichiers et dossiers au sein de votre projet ou solution seront également l’ajouter.</span><span class="sxs-lookup"><span data-stu-id="9643f-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="9643f-158">Toutefois, dans de nombreux cas, projets et solutions également reposent sur des dépendances externes, tels que des assemblys locaux pour fonctionner correctement.</span><span class="sxs-lookup"><span data-stu-id="9643f-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="9643f-159">Vous devez ajouter toutes ces ressources pour le contrôle de code source pour permettre à Team Build et des autres membres de l’équipe de développement générer votre code avec succès.</span><span class="sxs-lookup"><span data-stu-id="9643f-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="9643f-160">Par exemple, la structure de dossiers pour l’exemple de solution Contact Manager inclut un dossier nommé packages.</span><span class="sxs-lookup"><span data-stu-id="9643f-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="9643f-161">Il contient l’assembly et les différentes ressources de prise en charge pour ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="9643f-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="9643f-162">Le dossier packages ne fait pas partie de la solution de Contact Manager, mais la solution ne sera pas généré avec succès sans lui.</span><span class="sxs-lookup"><span data-stu-id="9643f-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="9643f-163">Pour activer Team Build générer la solution, vous devez ajouter le dossier packages de contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="9643f-164">L’inclusion d’un dossier de packages est typique de ce qui se passe lorsque vous ajoutez l’Entity Framework, ou des ressources similaires, à votre solution à l’aide de l’extension NuGet pour Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9643f-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="9643f-165">**Pour ajouter du contenu de projet au contrôle de code source**</span><span class="sxs-lookup"><span data-stu-id="9643f-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="9643f-166">Assurez-vous que les éléments que vous souhaitez ajouter (par exemple, le dossier packages) sont dans un emplacement approprié au sein d’un dossier mappé sur votre système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="9643f-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="9643f-167">Dans Visual Studio 2010, dans le **Team Explorer** fenêtre, développez votre projet d’équipe, puis double-cliquez sur **contrôle de code Source**.</span><span class="sxs-lookup"><span data-stu-id="9643f-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="9643f-168">Sur le **Explorateur du contrôle de Source** sous l’onglet le **dossiers** volet, sélectionnez le dossier qui contient l’élément ou d’éléments que vous souhaitez ajouter.</span><span class="sxs-lookup"><span data-stu-id="9643f-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="9643f-169">Cliquez sur le **ajouter des éléments au dossier** bouton.</span><span class="sxs-lookup"><span data-stu-id="9643f-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="9643f-170">Dans le **ajouter au contrôle de code Source** boîte de dialogue, sélectionnez le dossier ou les éléments que vous souhaitez ajouter, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="9643f-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="9643f-171">Sur le **éléments exclus** , sélectionnez tous les éléments requis qui ont été exclus automatiquement (par exemple, il s’agit d’assemblys), puis cliquez sur **inclure les éléments**.</span><span class="sxs-lookup"><span data-stu-id="9643f-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="9643f-172">Sur le **éléments à ajouter** , vérifiez que tous les fichiers que vous souhaitez inclure sont répertoriées, puis cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="9643f-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="9643f-173">Dans le **Explorateur du contrôle de Source** fenêtre, cliquez sur le **archiver** bouton.</span><span class="sxs-lookup"><span data-stu-id="9643f-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="9643f-174">Dans le **archiver – fichiers sources** boîte de dialogue, tapez un commentaire, puis cliquez sur **archiver**.</span><span class="sxs-lookup"><span data-stu-id="9643f-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="9643f-175">À ce stade, vous avez ajouté les dépendances externes pour votre solution au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9643f-176">Conclusion</span><span class="sxs-lookup"><span data-stu-id="9643f-176">Conclusion</span></span>

<span data-ttu-id="9643f-177">Cette rubrique décrit comment se connecter à un projet d’équipe, mappez une structure de dossiers et ajouter du contenu à un contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="9643f-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="9643f-178">Pour plus d’informations sur la façon de travailler avec des éléments sous contrôle de code source, consultez [à l’aide de la gestion de Version](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="9643f-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="9643f-179">La rubrique suivante, [configuration d’un serveur de Build TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md), explique comment préparer un serveur TFS Team Build pour générer et déployer votre solution.</span><span class="sxs-lookup"><span data-stu-id="9643f-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9643f-180">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9643f-180">Further Reading</span></span>

<span data-ttu-id="9643f-181">Pour obtenir des informations plus complètes sur l’utilisation de contrôle de code source dans TFS, consultez [à l’aide de la gestion de Version](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="9643f-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9643f-182">[Précédent](creating-a-team-project-in-tfs.md)
> [Suivant](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="9643f-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
