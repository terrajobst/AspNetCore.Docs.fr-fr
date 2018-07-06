---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Déploiement de l’entreprise Web | Microsoft Docs
author: jrjlee
description: Ce didacticiel décrit la façon de répondre à un grand nombre des défis que vous rencontrerez lorsque vous gérez le déploiement d’applications web de l’échelle de l’entreprise à devel...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 2cb8cb12ec5af376b58b672813af8626cb0f95d8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806550"
---
<a name="web-deployment-in-the-enterprise"></a>Déploiement Web dans l’entreprise
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ce didacticiel décrit la façon de répondre à un grand nombre des défis que vous rencontrerez lorsque vous gérez le déploiement d’applications web de l’échelle de l’entreprise aux environnements de développement, test, intermédiaire et de production. Le didacticiel comprend une solution de référence avec un mélange de contenu conceptuel et de tâches pour vous guider à travers les différentes tâches courantes et les procédures.
> 
> Pour obtenir une traduction italienne de ces didacticiels, visitez [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Problèmes de déploiement d’entreprise

Les organisations rencontrent souvent ces défis lorsqu’ils ressemblent à gérer le déploiement de solutions d’échelle de l’entreprise complexes :

- Vous devez être en mesure de déployer des projets pour plusieurs environnements, comme développeur environnements ou de test, intermédiaires plateformes et serveurs de production. La solution doit être déployé avec différents paramètres de configuration pour chaque environnement.
- Vous devez déployer plusieurs projets dépendants simultanément dans le cadre d’un processus de génération et de déploiement étape unique ou automatisé.
- Vous devez être en mesure de déploiement sur un lecteur à partir d’un processus automatisé. Par exemple, vous souhaitez utiliser un processus d’intégration continue (CI) pour déployer des applications web dans un environnement de test lorsque le nouveau code est archivé.
- Vous devez être en mesure de contrôler le processus de déploiement et de définir des variables de déploiement en dehors de Visual Studio, comme les développeurs ont peu de chances pour que les paramètres de configuration corrects ou les informations d’identification nécessaires pour chaque environnement cible.
- Vous devez déployer des projets de base de données basée sur le schéma et de préserver les données existantes sur les déploiements suivants.
- Vous devez déployer des bases de données d’appartenance sur une base ad hoc sans déployer les données de compte d’utilisateur. Vous devrez peut-être également mettre à jour le schéma des bases de données déployée d’appartenance sans perte de données de compte utilisateur existant.
- Vous devez exclure certains fichiers ou dossiers lorsque vous déployez du contenu pour différents environnements cibles.

## <a name="overview-of-approach"></a>Vue d’ensemble de l’approche

Ce didacticiel, ainsi que les autres didacticiels de cette série, utilise cette approche de haut niveau pour relever les défis que décrits ci-dessus.

- **Utiliser des fichiers de projet personnalisés de Microsoft Build Engine (MSBuild) pour contrôler le processus de génération et de déploiement global.**
- Cela vous permet de générer et déployer chaque projet dans la solution en tant que partie d’une opération unique et scriptable.
- Les paramètres spécifiques à l’environnement sont configurés à l’aide de fichiers de projet spécifiques à l’environnement simple. Contrairement à l’approche Visual Studio centrées sur à l’aide de configurations de solutions et les profils de publication afin de configurer des déploiements pour différents environnements, cette approche vous permet de configurer et gérer le processus de déploiement en dehors de Visual Studio. Cela signifie que les développeurs ne devez passez connaissance des chaînes de connexion, les points de terminaison de service, les informations d’identification du serveur et les autres variables de déploiement pour les environnements de destination.
- Les fichiers de projet personnalisés peuvent être appelées par Team Build dans le cadre d’un flux de travail de Team Foundation Server (TFS). Cela vous permet de configurer le déploiement automatisé pour les scénarios d’intégration continue.

**Pour empaqueter et déployer des projets d’application web, utilisez l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy).**

- Web Deploy fournit une infrastructure qui vous permet d’empaqueter et déployer votre contenu de l’application web sur un serveur web IIS de destination, ainsi que les dépendances, les paramètres de configuration, les paramètres de sécurité et d’autres exigences.
- Vous pouvez contrôler le processus d’empaquetage et déploiement entier à partir de vos fichiers de projet MSBuild personnalisées. Vous pouvez également manipuler les paramètres de configuration qui accompagnent votre package de déploiement web, tels que les chaînes de connexion, les points de terminaison de service et les détails de destination de IIS.
- Web Deploy, ainsi que le Pipeline de publication Web, offre un grand nombre de points d’extensibilité qui vous permettent de personnaliser vos déploiements. Par exemple, il est facile à exclure des fichiers indésirables et des dossiers à partir de vos packages de déploiement web.

**Utiliser l’utilitaire VSDBCMD.exe pour déployer et mettre à jour des schémas de base de données.**

- VSDBCMD vous permet de déployer des bases de données à partir d’un fichier de schéma de base de données (.dbschema), qui est générée lorsque vous générez un projet de base de données de Visual Studio. En revanche, la fonctionnalité de déploiement de base de données incluse dans le déploiement Web est plus adaptée au déploiement de bases de données existantes à partir d’une instance de SQL Server locale.
- Contrairement aux fonctionnalités de Visual Studio pour le déploiement de projets de base de données, VSDBCMD vous permet de déployer des mises à jour différentielles à une base de données cible. Cela vous permet de conserver toutes les données existantes pendant que vous mettez à niveau le schéma de base de données.
- Vous pouvez exécuter des commandes VSDBCMD à partir de vos fichiers de projet MSBuild personnalisées.

## <a name="content-map"></a>Plan de contenu

Ce didacticiel inclut des rubriques qui se répartissent en quatre domaines principaux.

Ces rubriques présentent la solution de référence&#x2014;la solution Gestionnaire de contacts&#x2014;et décrivent comment télécharger et configurez-le sur votre ordinateur local :

- [La solution Gestionnaire de contacts](the-contact-manager-solution.md)
- [Configuration de la solution Gestionnaire de contacts](setting-up-the-contact-manager-solution.md)

Ces rubriques présentent des fichiers projet MSBuild, décrivent comment vous pouvez créer et utiliser des fichiers de projet personnalisé et passez en revue le processus de déploiement pour la solution Gestionnaire de contacts :

- [Présentation du fichier projet](understanding-the-project-file.md)
- [Présentation du processus de génération](understanding-the-build-process.md)

Ces rubriques décrivent le déploiement d’applications web, y compris comment la génération et l’empaquetage fonctionnement du processus, comment le processus de génération s’intègre avec le Pipeline de publication Web, comment modifier les paramètres de déploiement et comment déployer des packages web vers la destination environnements :

- [Génération et empaquetage des projets d’application web](building-and-packaging-web-application-projects.md)
- [Configuration des paramètres pour le déploiement de package web](configuring-parameters-for-web-package-deployment.md)
- [Déploiement de packages web](deploying-web-packages.md)

- [Déploiement de projets de base de données](deploying-database-projects.md) décrit les différentes techniques que vous pouvez utiliser pour déployer des projets de base de données de Visual Studio, ainsi que les avantages et inconvénients de chaque approche. [Création et exécution d’un fichier de commandes de déploiement](creating-and-running-a-deployment-command-file.md) décrit comment créer un fichier de commande simple qui encapsule votre logique de déploiement et vous permet de déployer des solutions complexes comme un processus pas à pas.
- Enfin, [installer manuellement des Packages Web](manually-installing-web-packages.md) s’achève le didacticiel en vous montrant pour importer des packages web dans IIS.

## <a name="key-technologies"></a>Technologies clés

Les rubriques de ce didacticiel utilisent principalement ces technologies pour gérer la génération et déploiement :

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- L’utilitaire de déploiement de base de données VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Autres didacticiels de cette série

Cela fait partie d’une série de cinq didacticiels sur le déploiement de web-échelle de l’entreprise. Voici les autres didacticiels de la série :

- [Déploiement d’Applications Web dans les scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ce contenu introduction fournit l’arrière-plan contextuel de la série de didacticiels. Elle décrit le scénario du didacticiel, et illustre comment les tâches et les procédures pas à pas décrit tout au long de la série s’intègrent à un processus d’Application Lifecycle Management (ALM) plus large.
- [Configuration d’environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel explique comment configurer des serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement de package web à distance en utilisant le Service de l’Agent de déploiement Web (l’agent distant) ou le Gestionnaire de déploiement Web et le déploiement de la base de données distante. Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement, et décrit l’utilisation de Web Farm Framework (WFF) pour répliquer des applications web déployées sur tous les serveurs web dans une batterie de serveurs.
- [Configuration de Team Foundation Server pour le déploiement de Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer TFS pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé en tant que partie d’un processus d’intégration continue et déclenché manuellement des déploiements des builds spécifiques.
- [Avancées de déploiement Web d’entreprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de base de données pour plusieurs environnements, à l’exclusion de fichiers et dossiers de déploiement et en prenant des applications web en mode hors connexion pendant le processus de déploiement .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
