---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scénario : Configuration d’un environnement de Test pour le déploiement Web | Documents Microsoft'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement web par défaut pour le développeur ou environnements de test et décrit les tâches que vous devez effectuer pour configurer un si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879853"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="05edc-103">Scénario : Configuration d’un environnement de Test pour le déploiement Web</span><span class="sxs-lookup"><span data-stu-id="05edc-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="05edc-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="05edc-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="05edc-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="05edc-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="05edc-106">Cette rubrique décrit un scénario de déploiement web par défaut pour le développeur ou environnements de test et décrit les tâches que vous devez effectuer pour configurer un environnement similaire.</span><span class="sxs-lookup"><span data-stu-id="05edc-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="05edc-107">Lorsque les développeurs travaillent sur des applications web, souvent donnés accès à un environnement de serveur qu’ils peuvent utiliser pour tester les modifications apportées à leurs applications dans un paramètre réaliste.</span><span class="sxs-lookup"><span data-stu-id="05edc-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="05edc-108">En règle générale, ce type d’environnement de développement ou de test possède les caractéristiques :</span><span class="sxs-lookup"><span data-stu-id="05edc-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="05edc-109">L’environnement se compose d’un serveur web unique et un serveur de base de données unique.</span><span class="sxs-lookup"><span data-stu-id="05edc-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="05edc-110">Les développeurs ont généralement des privilèges d’administrateur sur les serveurs, pour leur permettre de configurer l’environnement sur les exigences de leurs applications.</span><span class="sxs-lookup"><span data-stu-id="05edc-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="05edc-111">Modifications dans les applications sont déployées des intervalles fréquents, par conséquent, l’environnement doit prendre en charge de la seule étape ou déploiement automatisé.</span><span class="sxs-lookup"><span data-stu-id="05edc-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="05edc-112">Par exemple, dans notre [scénario du didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink est un développeur chez Fabrikam, Inc. Matt travaille sur la solution de gestionnaire de contacts et régulièrement doit déployer les modifications dans un environnement de test.</span><span class="sxs-lookup"><span data-stu-id="05edc-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="05edc-113">Matt est un administrateur sur le serveur web de test et le serveur de base de données de test.</span><span class="sxs-lookup"><span data-stu-id="05edc-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="05edc-114">Au départ, Matt doit être en mesure de déployer la solution dans l’environnement de test directement.</span><span class="sxs-lookup"><span data-stu-id="05edc-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="05edc-115">En tant que le travail progresse aux développeurs plus de jointure et l’équipe, la solution est configurée pour l’intégration continue (CI) dans Team Foundation Server (TFS) du Gestionnaire de contacts.</span><span class="sxs-lookup"><span data-stu-id="05edc-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="05edc-116">Chaque fois qu’un développeur vérifie dans le contenu, Team Build doit générer la solution, exécutez des tests unitaires et automatiquement déployer la solution dans l’environnement de test.</span><span class="sxs-lookup"><span data-stu-id="05edc-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="05edc-117">Vue d’ensemble de la solution</span><span class="sxs-lookup"><span data-stu-id="05edc-117">Solution Overview</span></span>

<span data-ttu-id="05edc-118">L’environnement de test doit prendre en charge de la seule étape ou automatisé déploiement à partir d’un ordinateur distant, vous avez le choix entre deux approches principales.</span><span class="sxs-lookup"><span data-stu-id="05edc-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="05edc-119">Vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="05edc-119">You can:</span></span>

- <span data-ttu-id="05edc-120">Configurer le serveur web de test pour prendre en charge le déploiement à l’aide du Service de l’Agent de déploiement Web (le « agent à distance »).</span><span class="sxs-lookup"><span data-stu-id="05edc-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="05edc-121">Configurer le serveur web de test pour prendre en charge le déploiement à l’aide du Gestionnaire de déploiement Web.</span><span class="sxs-lookup"><span data-stu-id="05edc-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="05edc-122">Vous pouvez également utiliser [Web déployer à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (le « agent temporaire »).</span><span class="sxs-lookup"><span data-stu-id="05edc-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="05edc-123">Cela est similaire à l’approche de l’agent distant en termes d’exigences et des contraintes.</span><span class="sxs-lookup"><span data-stu-id="05edc-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="05edc-124">Dans ce cas, les développeurs ont des privilèges d’administrateur sur les serveurs de destination, et l’environnement de test n’est pas soumis aux contraintes de sécurité strictes, le choix logique pour configurer le serveur web de test pour prendre en charge le déploiement à l’aide de l’agent distant.</span><span class="sxs-lookup"><span data-stu-id="05edc-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="05edc-125">Cela est moins complexe et requiert une configuration initiale moins que l’approche du Gestionnaire de déploiement Web.</span><span class="sxs-lookup"><span data-stu-id="05edc-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="05edc-126">Vous devez également configurer votre serveur de base de données pour prendre en charge le déploiement et accès à distance.</span><span class="sxs-lookup"><span data-stu-id="05edc-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="05edc-127">Ces rubriques fournissent toutes les informations dont vous avez besoin pour effectuer ces tâches :</span><span class="sxs-lookup"><span data-stu-id="05edc-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="05edc-128">[Configurer un serveur Web pour la publication (Agent distant) de déploiement Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="05edc-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="05edc-129">Cette rubrique décrit comment créer un serveur web qui prend en charge le déploiement Web de publication, à l’aide de l’approche de l’agent distant, à partir d’une génération complète de Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="05edc-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="05edc-130">[Configurer un serveur de base de données de publication de déploiement Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="05edc-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="05edc-131">Cette rubrique décrit comment configurer un serveur de base de données pour prendre en charge l’accès à distance et le déploiement, à partir d’une installation par défaut de SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="05edc-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="05edc-132">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="05edc-132">Further Reading</span></span>

<span data-ttu-id="05edc-133">Pour obtenir des conseils sur la configuration d’un environnement intermédiaire classique, consultez [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="05edc-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="05edc-134">Pour obtenir des conseils sur la configuration d’un environnement de production typique, consultez [scénario : configuration d’un environnement de Production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="05edc-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05edc-135">[Précédent](choosing-the-right-approach-to-web-deployment.md)
> [Suivant](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="05edc-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
