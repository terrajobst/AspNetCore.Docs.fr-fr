---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scénario : Configuration d’un environnement de Production pour le déploiement Web | Microsoft Docs'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement web typique pour un environnement de production et décrit les tâches que vous devez suivre pour configurer un texte similaire...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3627d18e1b102c574726282780239464be04c04b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830726"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scénario : Configuration d’un environnement de Production pour le déploiement Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit un scénario de déploiement web typique pour un environnement de production et décrit les tâches que vous devez suivre pour configurer un environnement similaire.


L’environnement de production constitue la destination finale pour une application web ou un site Web. À ce stade, votre application a été testé, a été déployée dans un environnement intermédiaire et est prête à « go live ». Les caractéristiques d’un environnement de production peuvent varier considérablement en fonction de la nature et l’objectif de votre contenu web, la taille de votre organisation, votre public cible et bien d’autres facteurs. Dans un scénario de l’échelle de l’entreprise, l’environnement de production peut avoir ces caractéristiques :

- L’environnement se compose de plusieurs serveurs web avec équilibrage de charge et un ou plusieurs serveurs de base de données, souvent avec le clustering de basculement et de mise en miroir de base de données.
- Si l’environnement est accessible sur Internet, il est susceptible d’être séparées à partir de votre réseau interne. Il peut être sur un autre sous-réseau dans un réseau de périmètre, il peut être sur un autre domaine, et il peut être sur une infrastructure réseau complètement différent.
- Les développeurs et les comptes de processus de serveur de build sont peu susceptibles de disposer des privilèges d’administrateur sur les serveurs de production.
- Modifications dans les applications sont déployées de manière moins fréquente que test ou les déploiements de mise en lots.

> [!NOTE]
> Montée en charge un déploiement de base de données sur plusieurs serveurs n’entre pas dans le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, consultez [la documentation en ligne de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Par exemple, dans notre [scénario du didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un serveur Team Build inclut les définitions de build qui permettent aux utilisateurs de générer la solution Gestionnaire de contacts et le déployer dans un environnement intermédiaire en une seule étape. Lorsque l’application est prête à être déployée en production, en raison de contraintes imposées par les exigences de sécurité et l’infrastructure réseau, l’administrateur d’environnement de production doit copier le package web sur un serveur web de production et importez manuellement Il via le Gestionnaire des Services Internet (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Présentation de la solution

Dans ce scénario, vous pouvez déduire ces faits à partir d’une analyse des exigences de déploiement :

- En raison des restrictions de sécurité et la configuration du réseau, vous ne pouvez pas configurer l’environnement de production pour prendre en charge le déploiement d’un clic ou automatisé. Déploiement hors connexion est la seule approche viable dans ce scénario.
- L’environnement de production comprend plusieurs serveurs web, afin de pouvoir utiliser Web Farm Framework (WFF) pour créer une batterie de serveurs. À l’aide de cette approche, l’administrateur doit uniquement importer l’application sur un serveur web (le serveur principal) et WFF répliquera le déploiement sur tous les autres serveurs de web dans l’environnement de production.

Ces rubriques fournissent toutes les informations que vous avez besoin pour effectuer ces tâches :

- [Créer une batterie de serveurs avec l’infrastructure de batterie de serveurs Web](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique décrit comment créer et configurer une batterie de serveurs à l’aide de WFF, afin que les produits de la plate-forme web et les composants, les paramètres de configuration et les sites Web et les applications sont répliquées sur plusieurs serveurs web avec équilibrage de charge.
- [Configurer un serveur Web pour la publication (déploiement hors connexion) de Deploy Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Cette rubrique décrit comment créer un serveur web qui permet aux administrateurs importent et déploiement des packages web manuellement, en commençant à partir d’une génération propre de Windows Server 2008 R2.
- [Configurer un serveur de base de données pour la publication de Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique décrit comment configurer un serveur de base de données pour prendre en charge l’accès à distance et le déploiement, en commençant à partir d’une installation par défaut de SQL Server 2008 R2.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un environnement de test de développement classique, consultez [scénario : configuration d’un environnement de Test pour le déploiement Web](scenario-configuring-a-test-environment-for-web-deployment.md). Pour obtenir des conseils sur la configuration d’un environnement intermédiaire classique, consultez [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Suivant](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
