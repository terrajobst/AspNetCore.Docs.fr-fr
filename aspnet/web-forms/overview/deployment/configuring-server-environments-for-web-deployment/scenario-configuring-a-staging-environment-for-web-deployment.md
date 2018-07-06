---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scénario : Configuration d’un environnement intermédiaire pour le déploiement Web | Microsoft Docs'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement web typique pour un environnement intermédiaire et décrit les tâches que vous devez suivre pour configurer un env similaire...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 9c0f12645f10820996a03c232d20ee87031aefaa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820799"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scénario : Configuration d’un environnement intermédiaire pour le déploiement Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit un scénario de déploiement web typique pour un environnement intermédiaire et décrit les tâches que vous devez suivre pour configurer un environnement similaire.


Un grand nombre d’organisations utilisent des environnements intermédiaires pour afficher un aperçu des mises à jour des applications web ou sites Web. Cela donne des personnes au sein de l’organisation une occasion d’Explorer et d’examiner les nouvelles fonctionnalités ou contenu avant que le site « publiée », ou en d’autres termes, est déployé dans un environnement de production. L’environnement intermédiaire est conçu pour répliquer l’environnement de production aussi fidèlement que possible, afin de fournir un aperçu réaliste. Ce type d’environnement intermédiaire a en général, ces caractéristiques :

- L’environnement se compose de plusieurs serveurs web avec équilibrage de charge et un ou plusieurs serveurs de base de données, souvent avec le clustering de basculement et de mise en miroir de base de données.
- Les applications peuvent être déployées manuellement par une équipe de développement ou automatiquement par un serveur Team Build.
- Les utilisateurs ou les comptes de processus qui déploient des applications sont peu susceptibles d’avoir des privilèges d’administrateur sur les serveurs de mise en lots.
- Modifications dans les applications sont déployées fréquemment, par conséquent, l’environnement doit prendre en charge de la seule étape ou le déploiement automatisé.

> [!NOTE]
> Montée en charge un déploiement de base de données sur plusieurs serveurs n’entre pas dans le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, consultez [la documentation en ligne de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).


Par exemple, dans notre [scénario du didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) gère la solution Gestionnaire de contacts. L’administrateur TFS, Rob Walters, a créé une définition de build qui permet aux développeurs de déclencher un déploiement dans l’environnement intermédiaire en fonction des besoins.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Notez que dans la plupart des cas, vous ne sont pas nécessairement déployer la build la plus récente dans l’environnement intermédiaire. Au lieu de cela, vous êtes beaucoup plus susceptible de vouloir déployer une build spécifique qui a déjà fait l’objet de validation et la vérification dans l’environnement de test.

## <a name="solution-overview"></a>Présentation de la solution

Dans ce scénario, vous pouvez déduire ces faits à partir d’une analyse des exigences de déploiement :

- Le compte d’utilisateur ou un processus qui effectue le déploiement n’auront pas des privilèges d’administrateur sur les serveurs de mise en lots, donc les serveurs web intermédiaire doivent prendre en charge le déploiement non administrateur. Par conséquent, vous devez configurer les serveurs web intermédiaire pour utiliser le Gestionnaire de déploiement Web plutôt que l’agent distant.
- L’environnement intermédiaire inclut plusieurs serveurs web, mais il doit prendre en charge de déploiement en un clic ou automatisé, il vous faudra donc à utiliser Web Farm Framework (WFF) pour créer une batterie de serveurs. À l’aide de cette approche, vous pouvez déployer une application sur un serveur web (le serveur principal) et WFF répliquera le déploiement sur tous les autres serveurs de web dans l’environnement intermédiaire.
- Le compte d’utilisateur ou processus qui effectue le déploiement doit avoir les autorisations nécessaires pour créer des bases de données. Par conséquent, vous devez ajouter le compte à la **dbcreator** rôle de serveur sur le serveur de base de données, en plus de configurer le serveur de base de données pour prendre en charge l’accès à distance et le déploiement.

Ces rubriques fournissent toutes les informations que vous avez besoin pour effectuer ces tâches :

- [Créer une batterie de serveurs avec l’infrastructure de batterie de serveurs Web](creating-a-server-farm-with-the-web-farm-framework.md). Cette rubrique décrit comment créer et configurer une batterie de serveurs à l’aide de WFF, afin que les produits de la plate-forme web et les composants, les paramètres de configuration et les sites Web et les applications sont répliquées sur plusieurs serveurs web avec équilibrage de charge.
- [Configurer un serveur Web pour la publication de Web Deploy (Gestionnaire Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Cette rubrique décrit comment créer un serveur web qui prend en charge le déploiement Web de publication, à l’aide de l’approche de l’agent distant, à partir d’une génération propre de Windows Server 2008 R2.
- [Configurer un serveur de base de données pour la publication de Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique décrit comment configurer un serveur de base de données pour prendre en charge l’accès à distance et le déploiement, en commençant à partir d’une installation par défaut de SQL Server 2008 R2.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un environnement de test de développement classique, consultez [scénario : configuration d’un environnement de Test pour le déploiement Web](scenario-configuring-a-test-environment-for-web-deployment.md). Pour obtenir des conseils sur la configuration d’un environnement de production classique, consultez [scénario : configuration d’un environnement de Production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Suivant](scenario-configuring-a-production-environment-for-web-deployment.md)
