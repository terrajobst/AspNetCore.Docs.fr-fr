---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Déploiement Web d’entreprise : Vue d’ensemble du scénario | Microsoft Docs'
author: jrjlee
description: Cette série de didacticiels utilise un exemple de solution avec un niveau réaliste de complexité, ainsi que d’un scénario de déploiement d’une entreprise fictive, pour fournir une référence...
ms.author: aspnetcontent
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ce533b246b14c9e041c89a778c022784f09d01a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836744"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Déploiement Web d’entreprise : Vue d’ensemble du scénario
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette série de didacticiels utilise un exemple de solution avec un niveau réaliste de complexité, ainsi que d’un scénario de déploiement d’une entreprise fictive, pour fournir une implémentation de référence et pour donner les tâches et les procédures pas à pas un contexte commun. Cette rubrique décrit le scénario du didacticiel et présente l’exemple de solution.


## <a name="scenario-description"></a>Description du scénario

Fabrikam, Inc., une société fictive, consiste à créer une solution qui permet aux équipes de ventes à distance de stocker et récupérer des informations de contact à partir d’une interface web.

Les processus d’Application Lifecycle Management (ALM) chez Fabrikam, Inc. nécessitent la solution à déployer dans trois environnements de serveur à différents stades du processus de développement de logiciels :

- Un environnement de test ou de « bac à sable » développeur.
- Un environnement de mise en lots basée sur l’intranet.
- Un environnement de production sur Internet.

Chacun de ces environnements a des exigences de sécurité et de configuration différente, et chacun présente des défis liés au déploiement unique.

### <a name="the-fabrikam-inc-server-infrastructure"></a>The Fabrikam, Inc. Infrastructure de serveur

Ceci est l’infrastructure de haut niveau de développement et le déploiement chez Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Les stations de travail de développeur, l’infrastructure de contrôle source, l’environnement de test de développeur et l’environnement intermédiaire que tous résider sur le réseau intranet au sein du domaine Fabrikam.net. L’environnement de production se trouve sur un réseau de périmètre (également appelé DMZ, zone démilitarisée et sous-réseau filtré), qui est isolé du réseau intranet par un pare-feu. Il s’agit d’un scénario de déploiement courant : vous généralement isoler vos serveurs de web accessible sur Internet à partir de votre infrastructure de serveur interne via l’utilisation de pare-feux ou les serveurs de passerelle.

Dans cet exemple :

- Un serveur Team Foundation Server (TFS) 2010 avec un serveur de builds distincts fournit le contrôle de code source et les fonctionnalités d’intégration continue (CI).
- L’environnement de test developer inclut un serveur web d’Internet Information Services (IIS) 7.5 et un serveur de base de données SQL Server 2008 R2.
- L’environnement de production comprend plusieurs serveurs web de IIS 7.5 synchronisées par un serveur de contrôleur de Web Farm Framework (WFF), ainsi que d’un serveur de base de données SQL Server 2008 R2. Dans la pratique, le serveur de base de données peut utiliser le clustering ou de mise en miroir pour améliorer l’évolutivité et la disponibilité.
- L’environnement intermédiaire est conçu pour répliquer la configuration de l’environnement de production aussi fidèlement que possible.
- Les stratégies d’isolation réseau et de pare-feu ne permettent pas de déploiement direct et automatisé à partir de l’intranet pour le réseau de périmètre.

La configuration de chacun de ces environnements est décrite plus en détail dans le deuxième didacticiel [configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Rôles d’équipe pour ALM

Ces utilisateurs sont impliqués dans la création, la gestion, la génération et la publication de la solution de gestionnaire de contacts :

- Matt Hink est un développeur d’applications web chez Fabrikam, Inc. Il fait partie de l’équipe qui a développé la solution Gestionnaire de contacts à l’aide de Visual Studio 2010. Matt dispose des droits d’administrateur complets sur les serveurs dans l’environnement de test de développeur, ce qui lui permet de configurer l’environnement pour répondre à ses besoins. Il a également l’accès utilisateur à l’instance de Visual Studio 2010 TFS où il stocke le code source pour la solution Gestionnaire de contacts.
- Rob Walters est un administrateur de serveur pour l’équipe de développement de Fabrikam, Inc. Rob a un accès administratif sur le serveur TFS afin qu’il peut configurer tous les aspects de TFS et Team Build. Rob également avec un accès administratif aux tests et aux serveurs web intermédiaire et agit comme l’administrateur de base de données (DBA) pour les serveurs de base de données dans des environnements intermédiaires et le test. Rob a configuré Team Build sur le serveur TFS pour effectuer ces tâches :

    - Générer et exécuter des tests unitaires sur l’application chaque fois qu’un utilisateur archive un fichier à TFS. Il s’agit d’élément de configuration.
    - Déployer l’application Gestionnaire de contacts à l’environnement de test automatiquement une fois que l’application réussit les tests unitaires. Cela inclut la base de données de publication pour les serveurs de test sur le déploiement initial et les mises à jour la base de données après le déploiement initial.
    - Déployez l’application Gestionnaire de contacts dans l’environnement intermédiaire dans un processus pas à pas.
    - Créer un package Web un administrateur de serveur Web et un DBA peuvent utiliser pour publier l’application à l’environnement de production.
- Lisa Andrews est responsable du déploiement d’applications sur les serveurs de production de Fabrikam, Inc. un administrateur du serveur. Elle a un accès en lecture au partage où TFS Team Build stocke le package de déploiement web une fois qu’il génère l’application Gestionnaire de contacts. Elle a également un accès administratif pour les serveurs web de production afin qu’elle peut déployer l’application en production. En outre, elle agit comme l’administrateur qui déploie des bases de données et les mises à jour de la base de données sur le serveur de base de données dans l’environnement de production.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>La Solution de gestionnaire de contacts

La solution Gestionnaire de Contact est conçue pour permettre aux utilisateurs inscrits, connecté en ajouter et modifier les informations de contact via une interface web. La solution de Contact Manager se compose de quatre projets individuels :

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Il s’agit d’un projet d’application web ASP.NET MVC3 qui représente le point d’entrée pour la solution. Il offre des fonctionnalités d’application web de base, comme offrant aux utilisateurs la possibilité de créer et d’afficher les détails du contact. L’application s’appuie sur un service Windows Communication Foundation (WCF) pour gérer les contacts et une base de données ASP.NET application services pour gérer l’authentification et l’autorisation.
- **ContactManager.Database**. Il s’agit d’un projet de base de données de Visual Studio 2010. Le projet définit le schéma d’une base de données que les magasins de coordonnées.
- **ContactManager.Service**. Il s’agit d’un projet de service web WCF. L’expose WCF un point de terminaison qui permet aux appelants d’effectuer créer, récupérer, mettre à jour et supprimer des opérations sur la base de données du Gestionnaire de contacts. Le service repose sur la base de données du Gestionnaire de contacts et de l’assembly ContactManager.Common.dll.
- **ContactManager.Common**. Il s’agit d’un projet de bibliothèque de classes. Le service WCF s’appuie sur les types définis dans cet assembly.

Une évaluation complète de la solution et ses exigences de déploiement est fournie dans le premier didacticiel de cette série, [déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tâches de déploiement

Plusieurs tâches distinctes sont impliqués dans le déploiement d’applications dans différents environnements dans une grande organisation. Voici les principales tâches couvrent les tutoriels :

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Voici une liste de chaque étape du processus de déploiement du point de vue des utilisateurs décrits plus haut dans ce document :

1. Tous les membres de l’équipe de révision de la solution de gestionnaire de contacts dans Visual Studio 2010 pour déterminer les problèmes et exigences de déploiement clés.
2. Matt Hink peut déployer la solution de gestionnaire de contacts directement à partir de la station de travail de développeur à l’environnement de test de développement, d’effectuer un test initial de la logique de déploiement.
3. Matt Hink ajoute l’application au contrôle de code source dans TFS.
4. Rob Walters crée différentes définitions de build pour la solution de Contact Manager dans Team Build. Une définition de build utilise CI pour déployer la solution dans l’environnement de développement de test chaque fois qu’un utilisateur archive dans le nouveau code. Une autre définition de build permet aux utilisateurs des déploiements de déclencheur dans l’environnement intermédiaire en fonction des besoins.
5. Chaque fois qu’un utilisateur archive dans le nouveau code, Team Build automatiquement génère les composants de solution, exécute les tests unitaires et déploie la solution dans l’environnement de test développeur si la génération a réussi et la passe des tests unitaires.
6. Quand un utilisateur déclenche un déploiement dans l’environnement intermédiaire, la solution est empaquetée et déployée dans un processus pas à pas. Ce processus génère également un package pour le déploiement manuel à l’environnement de production.
7. Lisa Andrews déploie l’application sur l’environnement de production en important manuellement le package web créé à l’étape 6.

### <a name="key-deployment-issues"></a>Principaux problèmes de déploiement

La solution de gestionnaire de contacts et le scénario de Fabrikam, Inc. Mettez en surbrillance les différents problèmes courants et défis que vous pouvez rencontrer lorsque vous déployez complexe, les solutions d’échelle de l’entreprise. Exemple :

- Vous devez être en mesure de déployer des projets pour plusieurs environnements, comme développeur environnements ou de test, intermédiaires plateformes et serveurs de production. La solution doit être déployé avec différents paramètres de configuration pour chaque environnement.
- Vous devez déployer plusieurs projets dépendants simultanément dans le cadre d’un processus de génération et de déploiement étape unique ou automatisé.
- Vous devez être en mesure de déploiement sur un lecteur à partir d’un processus automatisé. Par exemple, vous souhaitez utiliser un processus d’intégration continue pour déployer des applications web dans un environnement intermédiaire lorsque le nouveau code est archivé.
- Vous devez être en mesure de contrôler le processus de déploiement et de définir des variables de déploiement en dehors de Visual Studio, comme les développeurs ont peu de chances pour que les paramètres de configuration corrects ou les informations d’identification nécessaires pour chaque environnement cible.
- Vous devez déployer des projets de base de données basée sur le schéma et de préserver les données existantes sur les déploiements suivants.
- Vous devez déployer des bases de données d’appartenance sur une base ad hoc sans déployer les données de compte d’utilisateur. Vous devrez peut-être également mettre à jour le schéma des bases de données déployée d’appartenance sans perte de données de compte utilisateur existant.
- Vous devez exclure certains fichiers ou dossiers lorsque vous déployez du contenu pour différents environnements cibles.

En outre, la gestion de déploiement lors de mises à jour sont fréquentes et incrémentielle lève une exception de certaines problématiques supplémentaires. Exemple :

- Pour exécuter des tests unitaires chaque fois qu’un développeur archive dans le nouveau code. Vous souhaitez déployer la solution si le code passe les tests unitaires uniquement.
- Lorsque vous déployez une application web dans un environnement intermédiaire ou de production, vous souhaitez rediriger les utilisateurs vers un *application\_offline.htm* fichier pendant la durée du processus de déploiement.
- Vous souhaitez connecter les activités de déploiement. Le processus de déploiement doit envoyer des notifications par courrier électronique des déploiements réussies ou échoués aux destinataires désignés.
- En cas d’un déploiement automatisé, le processus de déploiement doit réessayer le déploiement actuel ou déployer le package web précédente à la place.

> [!div class="step-by-step"]
> [Précédent](deploying-web-applications-in-enterprise-scenarios.md)
> [Suivant](application-lifecycle-management-from-development-to-production.md)
