---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scénario : Configuration d’un environnement de Test pour le déploiement Web | Microsoft Docs'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement web typique pour un développeur ou environnements de test et décrit les tâches que vous devez suivre pour configurer un incident de service...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382492"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scénario : Configuration d’un environnement de Test pour le déploiement Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit un scénario de déploiement web typique pour un développeur ou environnements de test et décrit les tâches que vous devez suivre pour configurer un environnement similaire.


Lorsque les développeurs travaillent sur les applications web, ils ont souvent accès à un environnement de serveur qu’ils peuvent utiliser pour tester les modifications apportées à leurs applications dans un paramètre réaliste. Ce type d’environnement de développement ou de test possède généralement ces caractéristiques :

- L’environnement se compose d’un serveur web unique et un serveur de base de données unique.
- Les développeurs ont généralement des privilèges d’administrateur sur les serveurs, pour leur permettre de configurer l’environnement pour les besoins de leurs applications.
- Modifications dans les applications sont déployées fréquemment, par conséquent, l’environnement doit prendre en charge de la seule étape ou le déploiement automatisé.

Par exemple, dans notre [scénario du didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink est développeur chez Fabrikam, Inc. Matt travaille sur la solution Gestionnaire de contacts et doit régulièrement déployer les modifications dans un environnement de test. Matt est un administrateur sur le serveur web de test et le serveur de base de données de test. Initialement, Matt doit être en mesure de déployer la solution à l’environnement de test directement.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

En tant que le travail progresse et aux développeurs plus Rejoignez l’équipe, de la solution est configurée pour l’intégration continue (CI) dans Team Foundation Server (TFS) du Gestionnaire de contacts. Chaque fois qu’un développeur archive dans le contenu, Team Build doit générer la solution, exécutez des tests unitaires et automatiquement déployer la solution dans l’environnement de test.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Présentation de la solution

L’environnement de test doit prendre en charge de la seule étape ou automatisé de déploiement à partir d’un ordinateur distant, vous avez le choix entre deux approches principales. Vous pouvez :

- Configurer le serveur web de test pour prendre en charge le déploiement à l’aide du Service de l’Agent de déploiement Web (le « agent distant »).
- Configurer le serveur web de test pour prendre en charge le déploiement à l’aide du Gestionnaire Web Deploy.

> [!NOTE]
> Vous pouvez également utiliser [Web déployer à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (le terme « temp agent »). Cela est similaire à l’approche de l’agent distant en termes d’exigences et contraintes.


Dans ce cas, les développeurs ont des privilèges d’administrateur sur les serveurs de destination, et l’environnement de test n’est pas soumis aux contraintes de sécurité strict, le choix logique consiste donc à configurer le serveur web de test pour prendre en charge le déploiement à l’aide de l’agent distant. Cela est moins complexe et requiert une configuration initiale moins que l’approche de gestionnaire de déploiement Web. Vous devrez également configurer votre serveur de base de données pour prendre en charge le déploiement et l’accès à distance.

Ces rubriques fournissent toutes les informations que vous avez besoin pour effectuer ces tâches :

- [Configurer un serveur Web pour la publication (Agent distant) Web Deploy](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Cette rubrique décrit comment créer un serveur web qui prend en charge le déploiement Web de publication, à l’aide de l’approche de l’agent distant, à partir d’une génération propre de Windows Server 2008 R2.
- [Configurer un serveur de base de données pour la publication de Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique décrit comment configurer un serveur de base de données pour prendre en charge l’accès à distance et le déploiement, en commençant à partir d’une installation par défaut de SQL Server 2008 R2.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un environnement intermédiaire classique, consultez [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Pour obtenir des conseils sur la configuration d’un environnement de production classique, consultez [scénario : configuration d’un environnement de Production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](choosing-the-right-approach-to-web-deployment.md)
> [Suivant](scenario-configuring-a-staging-environment-for-web-deployment.md)
