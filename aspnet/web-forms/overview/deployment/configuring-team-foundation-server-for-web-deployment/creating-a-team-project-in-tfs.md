---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Création d’un projet d’équipe dans TFS | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment créer un nouveau projet d’équipe dans Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 98725b9d2f669e6520f24c3a8122be086002e96a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810222"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="30530-103">Création d’un projet d’équipe dans TFS</span><span class="sxs-lookup"><span data-stu-id="30530-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="30530-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="30530-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="30530-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="30530-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="30530-106">Cette rubrique décrit comment créer un nouveau projet d’équipe dans Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="30530-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="30530-107">Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="30530-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="30530-108">Présentation de la tâche</span><span class="sxs-lookup"><span data-stu-id="30530-108">Task Overview</span></span>

<span data-ttu-id="30530-109">Pour configurer et utiliser un nouveau projet d’équipe dans TFS, vous devez suivre ces étapes générales :</span><span class="sxs-lookup"><span data-stu-id="30530-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="30530-110">Accorder des autorisations à l’utilisateur qui crée le nouveau projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="30530-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="30530-111">Créer le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="30530-111">Create the team project.</span></span>
- <span data-ttu-id="30530-112">Accorder des autorisations aux membres de l’équipe qui travaillent sur le projet.</span><span class="sxs-lookup"><span data-stu-id="30530-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="30530-113">Vérifiez dans la partie du contenu.</span><span class="sxs-lookup"><span data-stu-id="30530-113">Check in some content.</span></span>

<span data-ttu-id="30530-114">Cette rubrique vous explique comment effectuer ces procédures, et il va identifier les utilisateurs et rôles de travail qui sont susceptibles d’être responsable de chaque procédure.</span><span class="sxs-lookup"><span data-stu-id="30530-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="30530-115">N’oubliez pas que, selon la structure de votre organisation, chacune de ces tâches peut être la responsabilité d’une autre personne.</span><span class="sxs-lookup"><span data-stu-id="30530-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="30530-116">Les tâches et les procédures pas à pas dans cette rubrique supposent que vous avez installé et configuré TFS, et que vous avez créé une collection de projets d’équipe dans le cadre du processus de configuration.</span><span class="sxs-lookup"><span data-stu-id="30530-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="30530-117">Pour plus d’informations sur ces hypothèses et pour des informations plus générales sur le scénario, consultez [configurer un serveur de builds TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="30530-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="30530-118">Accorder des autorisations au créateur du projet d’équipe</span><span class="sxs-lookup"><span data-stu-id="30530-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="30530-119">Pour créer un nouveau projet d’équipe, vous avez besoin de ces autorisations :</span><span class="sxs-lookup"><span data-stu-id="30530-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="30530-120">Vous devez avoir le **créer des projets** autorisation sur la couche application TFS.</span><span class="sxs-lookup"><span data-stu-id="30530-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="30530-121">En règle générale, vous accordez cette autorisation en ajoutant des utilisateurs à la **Project Collection Administrators** groupe TFS.</span><span class="sxs-lookup"><span data-stu-id="30530-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="30530-122">Le **Team Foundation Administrators** groupe global inclut également cette autorisation.</span><span class="sxs-lookup"><span data-stu-id="30530-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="30530-123">Vous devez être autorisé à créer de nouveaux sites d’équipe au sein de la collection de sites SharePoint qui correspond à la collection de projets d’équipe TFS.</span><span class="sxs-lookup"><span data-stu-id="30530-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="30530-124">En règle générale, vous accordez cette autorisation en ajoutant l’utilisateur à un groupe SharePoint avec **contrôle total** collection de sites de droits sur SharePoint.</span><span class="sxs-lookup"><span data-stu-id="30530-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="30530-125">Si vous utilisez des fonctionnalités de SQL Server Reporting Services, vous devez être un membre de la **Team Foundation Content Manager** rôle dans Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="30530-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="30530-126">Qui exécute ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="30530-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="30530-127">En règle générale, la personne ou le groupe qui administre le déploiement de TFS effectue également ces procédures.</span><span class="sxs-lookup"><span data-stu-id="30530-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="30530-128">Comme il s’agit d’un jeu de privilèges élevés d’autorisations, les nouveaux projets d’équipe sont généralement créés par un petit sous-ensemble d’utilisateurs avec la responsabilité d’administrer un déploiement de TFS.</span><span class="sxs-lookup"><span data-stu-id="30530-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="30530-129">Les développeurs ne pas généralement accordées les autorisations requises pour créer des projets d’équipe.</span><span class="sxs-lookup"><span data-stu-id="30530-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="30530-130">Accorder des autorisations dans TFS</span><span class="sxs-lookup"><span data-stu-id="30530-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="30530-131">Si vous souhaitez permettre aux utilisateurs de créer des projets d’équipe, la première tâche de haut niveau consiste à ajouter l’utilisateur à la **Project Collection Administrators** groupe pour la collection de projets d’équipe.</span><span class="sxs-lookup"><span data-stu-id="30530-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="30530-132">**Pour ajouter un utilisateur au groupe Project Collection Administrators**</span><span class="sxs-lookup"><span data-stu-id="30530-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="30530-133">Sur le serveur TFS, sur le **Démarrer** menu, pointez sur **tous les programmes**, cliquez sur **Microsoft Team Foundation Server 2010**, puis cliquez sur **Team Foundation Console d’administration**.</span><span class="sxs-lookup"><span data-stu-id="30530-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="30530-134">Dans l’arborescence de navigation, développez **couche Application**, puis cliquez sur **Collections de projets d’équipe**.</span><span class="sxs-lookup"><span data-stu-id="30530-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="30530-135">Dans le **Collections de projets d’équipe** volet, sélectionnez le projet d’équipe collection que vous souhaitez gérer.</span><span class="sxs-lookup"><span data-stu-id="30530-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="30530-136">Sur le **général** , cliquez sur **l’appartenance au groupe**.</span><span class="sxs-lookup"><span data-stu-id="30530-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="30530-137">Dans le **groupes globaux** boîte de dialogue, sélectionnez le **Project Collection Administrators** de groupe, puis cliquez sur **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="30530-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="30530-138">Dans le **propriétés du groupe Team Foundation Server** boîte de dialogue, sélectionnez **utilisateur de Windows ou un groupe**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="30530-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="30530-139">Dans le **sélectionnez utilisateurs, ordinateurs ou groupes** boîte de dialogue, tapez le nom d’utilisateur de l’utilisateur que vous souhaitez être en mesure de créer de nouveaux projets d’équipe, cliquez sur **vérifier les noms**, puis cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="30530-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="30530-140">Dans le **propriétés du groupe Team Foundation Server** boîte de dialogue, cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="30530-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="30530-141">Dans le **groupes globaux** boîte de dialogue, cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="30530-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="30530-142">Accorder des autorisations dans SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="30530-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="30530-143">Ensuite, vous devez donner à l’utilisateur l’autorisation de créer de nouveaux sites d’équipe dans la collection de sites SharePoint qui correspond à votre collection de projets d’équipe TFS.</span><span class="sxs-lookup"><span data-stu-id="30530-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="30530-144">**Pour accorder des autorisations Contrôle total sur la collection de sites SharePoint**</span><span class="sxs-lookup"><span data-stu-id="30530-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="30530-145">Dans la Console Administration Team Foundation Server, sur le **Collections de projets d’équipe** , sélectionnez la collection de projets d’équipe à gérer.</span><span class="sxs-lookup"><span data-stu-id="30530-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="30530-146">Sur le **SharePoint Site** onglet, notez la valeur de la **emplacement du Site par défaut actuel** URL.</span><span class="sxs-lookup"><span data-stu-id="30530-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="30530-147">Ouvrez Internet Explorer, puis accédez à l’URL que vous avez noté à l’étape 2.</span><span class="sxs-lookup"><span data-stu-id="30530-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30530-148">Si vous n’êtes pas connecté à Windows en tant que l’utilisateur qui a créé la collection de projets d’équipe, vous devez vous connecter à SharePoint en tant que cet utilisateur pour pouvoir continuer.</span><span class="sxs-lookup"><span data-stu-id="30530-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="30530-149">Sur le **Actions du Site** menu, cliquez sur **paramètres du Site**.</span><span class="sxs-lookup"><span data-stu-id="30530-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="30530-150">Sur le **paramètres du Site** page sous **utilisateurs et des autorisations**, cliquez sur **personnes et groupes**.</span><span class="sxs-lookup"><span data-stu-id="30530-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="30530-151">Dans le volet de navigation de gauche, cliquez sur **groupes**.</span><span class="sxs-lookup"><span data-stu-id="30530-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="30530-152">Sur le **personnes et groupes : tous les groupes** , cliquez sur **configurer les groupes pour ce Site**.</span><span class="sxs-lookup"><span data-stu-id="30530-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="30530-153">Vous pouvez recevoir un <strong>HTTP 404 Not Found</strong> erreur en raison d’un bogue de codage HTTP double.</span><span class="sxs-lookup"><span data-stu-id="30530-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="30530-154">Si cela se produit, remplacez l’URL par ceci :</span><span class="sxs-lookup"><span data-stu-id="30530-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="30530-155">`[site_collection_URL]/_layouts/permsetup.aspx` Par exemple :</span><span class="sxs-lookup"><span data-stu-id="30530-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="30530-156">Sur le **configurer les groupes pour ce Site** page, ajoutez l’utilisateur qui crée des projets d’équipe pour le **propriétaires** de groupe, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="30530-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="30530-157">Pour plus d’informations sur l’activation aux utilisateurs de créer de nouveaux projets d’équipe au sein d’une collection de projets d’équipe, consultez [définir les autorisations d’administrateur pour les Collections de projets d’équipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="30530-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="30530-158">Créer un nouveau projet d’équipe et ajouter des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="30530-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="30530-159">Une fois que vous avez les autorisations nécessaires, vous pouvez utiliser la **Team Explorer** fenêtre dans Visual Studio 2010 pour créer un nouveau projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="30530-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="30530-160">Cette approche fournit un Assistant qui collecte toutes les informations requises et effectue les tâches nécessaires dans TFS, SharePoint et SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="30530-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="30530-161">Vous devez également accorder des autorisations sur le nouveau projet d’équipe aux membres de l’équipe de développement, pour leur permettre d’ajouter et modifier le contenu.</span><span class="sxs-lookup"><span data-stu-id="30530-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="30530-162">Qui exécute ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="30530-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="30530-163">En général un administrateur TFS ou un chef d’équipe développeur effectue ces procédures.</span><span class="sxs-lookup"><span data-stu-id="30530-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="30530-164">Créer un nouveau projet d’équipe</span><span class="sxs-lookup"><span data-stu-id="30530-164">Create a New Team Project</span></span>

<span data-ttu-id="30530-165">La procédure suivante décrit comment créer un nouveau projet d’équipe dans TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="30530-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="30530-166">**Pour créer un nouveau projet d’équipe**</span><span class="sxs-lookup"><span data-stu-id="30530-166">**To create a new team project**</span></span>

1. <span data-ttu-id="30530-167">Sur le **Démarrer** menu, pointez sur **tous les programmes**, cliquez sur **Microsoft Visual Studio 2010**, avec le bouton droit **Microsoft Visual Studio 2010**, puis cliquez sur **exécuter en tant qu’administrateur**.</span><span class="sxs-lookup"><span data-stu-id="30530-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30530-168">Si vous n’exécutez pas Visual Studio 2010 en tant qu’administrateur, l’Assistant Nouveau projet d’équipe échoue sur la dernière étape.</span><span class="sxs-lookup"><span data-stu-id="30530-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="30530-169">Si le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="30530-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="30530-170">Dans Visual Studio, sur le **équipe** menu, cliquez sur **se connecter à Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="30530-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30530-171">Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 4 à 7.</span><span class="sxs-lookup"><span data-stu-id="30530-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="30530-172">Dans le **connexion au projet d’équipe** boîte de dialogue, cliquez sur **serveurs**.</span><span class="sxs-lookup"><span data-stu-id="30530-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="30530-173">Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="30530-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="30530-174">Dans le **Ajouter Team Foundation Server** boîte de dialogue, fournissez les détails de votre instance TFS, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="30530-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="30530-175">Dans le **ajouter/supprimer Team Foundation Server** boîte de dialogue, cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="30530-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="30530-176">Dans le **se connecter au projet d’équipe** boîte de dialogue, sélectionnez l’instance TFS que vous souhaitez vous connecter à, sélectionnez l’équipe de projet collection que vous souhaitez ajouter à, puis cliquez sur **Connect**.</span><span class="sxs-lookup"><span data-stu-id="30530-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="30530-177">Dans le **Team Explorer** fenêtre, clic droit, l’équipe de collection de projets, puis cliquez sur **nouveau projet d’équipe**.</span><span class="sxs-lookup"><span data-stu-id="30530-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="30530-178">Dans le **nouveau projet d’équipe** boîte de dialogue, indiquez un nom et une description pour le projet d’équipe, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="30530-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30530-179">Si votre projet d’équipe comprend des espaces, vous risquez de rencontrer des problèmes lorsque vous accédez à l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) permet de déployer des packages à partir du chemin de sortie.</span><span class="sxs-lookup"><span data-stu-id="30530-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="30530-180">Espaces dans le chemin d’accès peuvent rendent beaucoup plus difficile exécuter des commandes Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="30530-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="30530-181">Sur le **sélectionner un modèle de processus** , sélectionnez le modèle de processus que vous souhaitez utiliser pour gérer le processus de développement, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="30530-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30530-182">Pour plus d’informations sur les modèles de processus pour TFS, consultez [modèles de processus et outils](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="30530-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="30530-183">Sur le **paramètres du Site d’équipe** page, laissez les paramètres par défaut inchangés, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="30530-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="30530-184">Ce paramètre crée ou identifie, un site d’équipe SharePoint qui est associé au projet d’équipe TFS.</span><span class="sxs-lookup"><span data-stu-id="30530-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="30530-185">Votre équipe de développement peut utiliser ce site pour gérer la documentation, participer fils de discussion, créez des pages wiki et effectuer diverses autres tâches qui ne sont pas liés au code.</span><span class="sxs-lookup"><span data-stu-id="30530-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="30530-186">Pour plus d’informations, consultez [Interactions entre les produits SharePoint et Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="30530-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="30530-187">Sur le **spécifier les paramètres de contrôle Source** page, laissez les paramètres par défaut inchangés, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="30530-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="30530-188">Ce paramètre identifie ou crée l’emplacement dans l’arborescence des dossiers TFS qui agira comme un dossier racine de votre contenu.</span><span class="sxs-lookup"><span data-stu-id="30530-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="30530-189">Sur le **confirmer les paramètres de projet d’équipe** , cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="30530-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="30530-190">Lorsque le nouveau projet d’équipe a été créé, dans le **projet d’équipe créé** , cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="30530-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="30530-191">Ajouter des utilisateurs à un projet d’équipe</span><span class="sxs-lookup"><span data-stu-id="30530-191">Add Users to a Team Project</span></span>

<span data-ttu-id="30530-192">Maintenant que vous avez créé le projet d’équipe, vous pouvez accorder des autorisations aux utilisateurs pour leur permettre de démarrer l’ajout et la collaboration sur le contenu.</span><span class="sxs-lookup"><span data-stu-id="30530-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="30530-193">**Pour ajouter des utilisateurs à un projet d’équipe**</span><span class="sxs-lookup"><span data-stu-id="30530-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="30530-194">Dans Visual Studio 2010, dans le **Team Explorer** fenêtre, cliquez sur le projet d’équipe, pointez sur **paramètres du projet d’équipe**, puis cliquez sur **l’appartenance au groupe**.</span><span class="sxs-lookup"><span data-stu-id="30530-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="30530-195">Pour autoriser un utilisateur à ajouter, modifier et supprimer du code sous contrôle de code source, vous devez l’ajouter à la **contributeurs** groupe.</span><span class="sxs-lookup"><span data-stu-id="30530-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="30530-196">Dans le **groupes de projets** boîte de dialogue, sélectionnez le **contributeurs** de groupe, puis cliquez sur **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="30530-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="30530-197">Dans le **propriétés du groupe Team Foundation Server** boîte de dialogue, sélectionnez **utilisateur de Windows ou un groupe**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="30530-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="30530-198">Dans le **sélectionnez utilisateurs, ordinateurs ou groupes** boîte de dialogue, tapez le nom d’utilisateur de l’utilisateur que vous souhaitez ajouter au projet d’équipe, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="30530-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="30530-199">Dans le **propriétés du groupe Team Foundation Server** boîte de dialogue, cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="30530-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="30530-200">Dans le **groupes de projets** boîte de dialogue, cliquez sur **fermer**.</span><span class="sxs-lookup"><span data-stu-id="30530-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="30530-201">Conclusion</span><span class="sxs-lookup"><span data-stu-id="30530-201">Conclusion</span></span>

<span data-ttu-id="30530-202">À ce stade, votre nouveau projet d’équipe est prête à être utilisée, et votre équipe de développeurs peut démarrer Ajout de contenu et la collaboration sur le processus de développement.</span><span class="sxs-lookup"><span data-stu-id="30530-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="30530-203">La rubrique suivante, [Ajout de contenu à un contrôle de code Source](adding-content-to-source-control.md), explique comment ajouter du contenu à un contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="30530-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="30530-204">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="30530-204">Further Reading</span></span>

<span data-ttu-id="30530-205">Pour obtenir des conseils plus large sur la création de projets d’équipe dans TFS, consultez [créer un projet d’équipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="30530-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="30530-206">Pour plus d’informations sur l’activation aux utilisateurs de créer de nouveaux projets d’équipe au sein d’une collection de projets d’équipe, consultez [définir les autorisations d’administrateur pour les Collections de projets d’équipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="30530-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="30530-207">Pour plus d’informations sur l’ajout d’utilisateurs aux projets d’équipe, consultez [ajouter des utilisateurs aux projets d’équipe](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="30530-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="30530-208">[Précédent](configuring-team-foundation-server-for-web-deployment.md)
> [Suivant](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="30530-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
