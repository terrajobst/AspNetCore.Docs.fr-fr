---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configuration d’environnements de serveur pour le déploiement Web | Microsoft Docs
author: jrjlee
description: Ce didacticiel vous explique comment configurer des environnements de serveur pour la prise en charge un seul clic, ou automatisé, de déploiement de site Web et de publication dans différents du scénario de différentes...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: df40cc9d16b9e6b3fe0b659b405106e68cee62b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835561"
---
<a name="configuring-server-environments-for-web-deployment"></a>Configuration d’environnements de serveur pour le déploiement Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ce didacticiel vous explique comment configurer des environnements de serveur pour la prise en charge un seul clic, ou automatisé, de déploiement de site Web et de publication dans divers scénarios différents. Le didacticiel inclut des rubriques pour vous guider dans diverses tâches, telles que la configuration d’un serveur web pour prendre en charge les approches spécifiques pour le déploiement et la configuration d’une batterie de serveurs Web Farm Framework (WFF), ainsi que des présentations basées sur des scénarios qui fournissent à la fin conseils de bout en bout plus haut niveau.
> 
> Ce didacticiel utilise le scénario de déploiement de Fabrikam, Inc. décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) comme point de référence pour l’infrastructure réseau et des exemples.
> 
> Pour obtenir une traduction italienne de ces didacticiels, visitez [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Ce didacticiel inclut les rubriques suivantes :

- [Choix de la bonne approche pour le déploiement web](choosing-the-right-approach-to-web-deployment.md)
- [Scénario : configuration d’un environnement de test pour le déploiement web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scénario : configuration d’un environnement de préproduction pour le déploiement web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scénario : configuration d’un environnement de production pour le déploiement web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configuration d’un serveur web pour la publication Web Deploy (Agent distant)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configuration d’un serveur web pour la publication Web Deploy (Gestionnaire Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configuration d’un serveur web pour la publication Web Deploy (Déploiement hors connexion)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configuration d’un serveur de base de données pour la publication Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md)
- [Création d’une batterie de serveurs avec le framework de batterie de serveurs web](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configuration des propriétés de déploiement pour un environnement cible](configuring-deployment-properties-for-a-target-environment.md)

La première rubrique [choix de l’approche de droite pour le déploiement Web](choosing-the-right-approach-to-web-deployment.md), décrit les approches principales que vous pouvez utiliser pour publier des applications web à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) 2.0. Il identifie également les scénarios correspondant à chaque approche. À ce stade, chaque rubrique scénario fournit une vue d’ensemble des tâches que vous devez suivre et identifie les rubriques que vous aurez besoin de travailler pour vous aider à effectuer ces tâches.

Si vous utilisez l’approche de fichier de projet de fractionnement décrite dans [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md) pour générer et déployer votre solution, la dernière rubrique, [configuration des propriétés de déploiement pour un environnement cible](configuring-deployment-properties-for-a-target-environment.md), explique comment configurer les fichiers de projet spécifique à l’environnement pour le déploiement pour différents environnements de destination.

## <a name="key-technologies"></a>Technologies clés

Ce didacticiel se concentre sur l’utilisation de ces produits et technologies pour prendre en charge le déploiement web :

- IIS 7.5
- Web Deploy 2.x
- WFF 2.x
- Service de gestion IIS Web (WMSvc)

Il aborde également le didacticiel sur l’utilisation de Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 et ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Autres didacticiels de cette série

Cela fait partie d’une série de cinq didacticiels sur le déploiement de web-échelle de l’entreprise. Voici les autres didacticiels de la série :

- [Déploiement d’Applications Web dans les scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ce contenu introduction fournit l’arrière-plan contextuel de la série de didacticiels. Elle décrit le scénario du didacticiel, et illustre comment les tâches et les procédures pas à pas décrit tout au long de la série s’intègrent à un processus d’Application Lifecycle Management (ALM) plus large.
- [Déploiement de l’entreprise Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une présentation conceptuelle de fichiers de projet Microsoft Build Engine (MSBuild), le Pipeline de publication Web, Web Deploy et autres technologies apparentées. Il explique comment vous pouvez utiliser ces outils ensemble pour gérer les processus de déploiement complexes.
- [Configuration de Team Foundation Server pour le déploiement de Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer Team Foundation Server (TFS) pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé en tant que partie d’un processus d’intégration continue (CI) et déclenché manuellement des déploiements des builds spécifiques.
- [Avancées de déploiement Web d’entreprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de base de données pour plusieurs environnements, à l’exclusion de fichiers et dossiers de déploiement et en prenant des applications web en mode hors connexion pendant le processus de déploiement .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
