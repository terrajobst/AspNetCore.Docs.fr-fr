---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Gestion du cycle de vie des applications : Du développement à la Production | Microsoft Docs'
author: jrjlee
description: Cette rubrique illustre comment une entreprise fictive gère le déploiement d’une application de web ASP.NET dans les environnements de test, intermédiaire et de production en tant que par...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 47af9504bdef294b987cdd23ab1bcefbeadd4681
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808977"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Gestion du cycle de vie des applications : Du développement à la Production
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique illustre comment une entreprise fictive gère le déploiement d’une application de web ASP.NET dans les environnements de test, intermédiaire et de production dans le cadre d’un processus de développement continu. Tout au long de la rubrique, les liens sont fournis pour plus d’informations et procédures pas à pas sur la façon d’effectuer des tâches spécifiques.
> 
> La rubrique est conçue pour fournir une vue d’ensemble pour un [série de didacticiels](deploying-web-applications-in-enterprise-scenarios.md) sur le déploiement web dans l’entreprise. Ne vous inquiétez pas si vous n’êtes pas familiarisé avec certains des concepts décrits ici&#x2014;les didacticiels qui suivent fournissent des informations détaillées sur l’ensemble de ces tâches et les techniques.
> 
> > [!NOTE]
> > Générales des raisons de simplicité, cette rubrique n’explique pas la mise à jour des bases de données dans le cadre du processus de déploiement. Toutefois, effectuer des mises à jour incrémentielles aux fonctionnalités de bases de données est une exigence de nombreux scénarios de déploiement d’entreprise, et vous trouverez des conseils sur la façon d’y parvenir plus loin dans cette série de didacticiels. Pour plus d’informations, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Vue d'ensemble

Le processus de déploiement illustré ici est basé sur le scénario de déploiement de Fabrikam, Inc. décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](enterprise-web-deployment-scenario-overview.md). Vous devez lire la vue d’ensemble du scénario avant d’étudier de cette rubrique. Essentiellement, le scénario examine la façon dont une organisation gère le déploiement d’une application web relativement complexes, le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), dans différentes phases dans un environnement d’entreprise classique.

À un niveau élevé, la solution de Contact Manager passe par ces étapes dans le cadre du développement et du processus de déploiement :

1. Un développeur archive du code dans Team Foundation Server (TFS) 2010.
2. TFS génère le code et exécute tous les tests unitaires associés au projet d’équipe.
3. TFS déploie la solution dans l’environnement de test.
4. L’équipe de développement vérifie et valide la solution dans l’environnement de test.
5. L’administrateur d’environnement intermédiaire effectue un déploiement « que se passe-t-il si » dans l’environnement intermédiaire, pour établir si le déploiement peut entraîner des problèmes.
6. L’administrateur d’environnement intermédiaire effectue un déploiement en direct dans l’environnement intermédiaire.
7. La solution fait l’objet d’acceptation par l’utilisateur de test dans l’environnement intermédiaire.
8. Les packages de déploiement web sont manuellement importés dans l’environnement de production.

Ces étapes font partie d’un cycle de développement continu.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

Dans la pratique, le processus est légèrement plus complexe que celui-ci, comme vous le verrez lorsque nous examinons chaque étape plus en détail. Fabrikam, Inc. utilise une approche différente pour le déploiement pour chaque environnement cible.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Le reste de cette rubrique examine ces étapes clés de ce cycle de vie de déploiement :

- **Conditions préalables**: comment vous devez configurer votre infrastructure de serveur avant de mettre votre logique de déploiement en place.
- **Initiale de développement et le déploiement**: ce que vous devez faire avant de déployer votre solution pour la première fois.
- **Déploiement à tester**: comment empaqueter et déployer du contenu vers un environnement de test automatiquement lorsqu’un développeur archive dans le nouveau code.
- **Déploiement dans un environnement intermédiaire**: comment déployer des builds à un environnement intermédiaire et de l’exécution de « que se passe-t-il si » déploiements pour vous assurer qu’un déploiement n’entraîne aucun problème.
- **Déploiement en production**: comment importer des packages web dans un environnement de production lors de l’infrastructure de réseau empêche le déploiement à distance.

## <a name="prerequisites"></a>Prérequis

La première tâche dans n’importe quel scénario de déploiement consiste à vous assurer que votre infrastructure de serveur répond aux exigences de vos outils de déploiement et les techniques. Dans ce cas, Fabrikam, Inc. a configuré son infrastructure de serveur comme suit :

- TFS est configuré pour inclure une collection de projets d’équipe, contrôleurs de build et agents de build. Consultez [configuration Team Foundation Server pour le déploiement de Web automatisée](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) pour plus d’informations.
- L’environnement de test est configuré pour accepter les déploiements à distance à l’aide du Service de l’Agent de déploiement Web (le « agent distant »), comme décrit dans [scénario : configuration d’un environnement de Test pour le déploiement Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) et [ Configurer un serveur Web pour la publication (Agent distant) Web Deploy](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- L’environnement intermédiaire est configuré pour accepter les déploiements à distance à l’aide du point de terminaison du Gestionnaire de déploiement Web, comme décrit dans [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) et [configurer un serveur Web pour le Web de publication Deploy (Gestionnaire Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- L’environnement de production est configuré pour autoriser un administrateur d’importer manuellement les packages de déploiement web dans Internet Information Services (IIS), comme décrit dans [scénario : configuration d’un environnement de Production pour le déploiement Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) et [configurer un serveur Web pour la publication (déploiement hors connexion) de Deploy Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Développement initial et le déploiement

Avant de l’équipe de développement de Fabrikam, Inc. peut déployer la solution Gestionnaire de contacts pour la première fois, il a besoin effectuer ces tâches :

- Créer un nouveau projet d’équipe dans TFS.
- Créez les fichiers de projet Microsoft Build Engine (MSBuild) qui contiennent la logique de déploiement.
- Créez les définitions de build TFS qui déclenchent les processus de déploiement.

### <a name="create-a-new-team-project"></a>Créer un nouveau projet d’équipe

- L’administrateur TFS, Rob Walters, crée un nouveau projet d’équipe pour l’application, comme décrit dans [création d’un projet d’équipe dans TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Ensuite, le développeur senior, Matt Hink, crée une solution squelette. Il vérifie ses fichiers dans le nouveau projet d’équipe dans TFS, comme décrit dans [Ajout de contenu pour le contrôle de code Source](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Créer la logique de déploiement

Matt Hink crée différents fichiers de projet MSBuild personnalisés, à l’aide de l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crée :

- Un fichier de projet nommé *Publish.proj* qui exécute le processus de déploiement. Ce fichier contient des cibles MSBuild qui générer les projets dans la solution, créer des packages web et déploiement les packages vers un environnement de serveur de destination.
- Fichiers de projet spécifique à un environnement nommés *Env-Dev.proj* et *Env-Stage.proj*. Ils contiennent les paramètres qui sont spécifiques à l’environnement de test et l’environnement intermédiaire, respectivement, comme les chaînes de connexion, les points de terminaison de service et les détails du service distant qui recevront le package web. Pour obtenir des conseils sur le choix des paramètres corrects pour les environnements de destination spécifique, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Pour exécuter le déploiement, un utilisateur exécute le *Publish.proj* fichier à l’aide de MSBuild ou Team Build et spécifie l’emplacement du fichier de projet spécifiques à l’environnement approprié (*Env-Dev.proj* ou *Env-Stage.proj*) comme un argument de ligne de commande. Le *Publish.proj* fichier importe ensuite le fichier de projet spécifique à l’environnement pour créer un ensemble complet de la publication d’instructions pour chaque environnement cible.

> [!NOTE]
> Le fonctionnement de ces fichiers de projet personnalisé est indépendant du mécanisme que vous permet d’appeler MSBuild. Par exemple, vous pouvez utiliser la ligne de commande MSBuild directement, comme décrit dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Vous pouvez exécuter les fichiers projet à partir d’un fichier de commandes, comme décrit dans [créer et exécuter un fichier de commandes de déploiement](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Vous pouvez également exécuter les fichiers projet à partir d’une définition de build dans TFS, comme décrit dans [création d’une définition de Build que le déploiement prend en charge](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> Dans chaque cas, le résultat final est le même&#x2014;MSBuild exécute le fichier de projet fusionné et déploie votre solution dans l’environnement cible. Cela vous offre une grande flexibilité dans la façon dont vous déclenchez votre processus de publication.


Une fois qu’il a créé les fichiers de projet personnalisé, Matt les ajoute à un dossier de solution et les vérifie dans le contrôle de code source.

### <a name="create-build-definitions"></a>Créer les définitions de build

Comme une tâche de préparation finale, Matt et Rob collaborent pour créer trois définitions de build pour le nouveau projet d’équipe :

- **DeployToTest**. Cela génère la solution Gestionnaire de contacts et le déploie sur l’environnement de test chaque fois qu’un archivage se produit.
- **DeployToStaging**. Il déploie des ressources à partir d’une build précédente spécifiée dans l’environnement intermédiaire lorsqu’un développeur de files d’attente la build.
- **DeployToStaging-WhatIf**. Cela effectue un déploiement « que se passe-t-il si » dans l’environnement intermédiaire lorsqu’un développeur de files d’attente la build.

Les sections qui suivent fournissent plus de détails sur chacune de ces définitions de build.

## <a name="deployment-to-test"></a>Déploiement de Test

L’équipe de développement chez Fabrikam, Inc. gère les environnements de test pour procéder à une variété d’activités, telles que la vérification et, tests d’utilisabilité, tests de compatibilité, test et la validation ad hoc ou exploratoires de test de logiciels.

L’équipe de développement a créé une définition de build dans TFS nommé **DeployToTest**. Cette définition de build utilise un déclencheur d’intégration continue, ce qui signifie que le processus de génération s’exécute chaque fois qu’un membre de l’équipe de développement de Fabrikam, Inc. effectue un archivage. Lorsqu’une build est déclenchée, la définition de build sera :

- Générez la solution ContactManager.sln. Cette opération génère à son tour chaque projet dans la solution.
- Exécuter des tests unitaires dans la structure de dossiers de solution (si la solution se génère correctement).
- Exécuter les fichiers de projet personnalisés qui contrôlent le processus de déploiement (si la solution générée avec succès et passe tous les tests unitaires).

Le résultat final est que si la solution est générée avec succès et passe des tests unitaires, les packages web et autres ressources de déploiement sont déployés sur l’environnement de test.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Comment fonctionne le processus de déploiement ?

Le **DeployToTest** build fournitures de définition de ces arguments à MSBuild :


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


Le **DeployOnBuild = true** et **DeployTarget = package** propriétés sont utilisées lorsque Team Build génère les projets dans la solution. Lorsque le projet est un projet d’application web, ces propriétés demander à MSBuild pour créer un package de déploiement web pour le projet. Le **TargetEnvPropsFile** propriété indique le *Publish.proj* où trouver le fichier de projet spécifique à un environnement à importer de fichiers.

> [!NOTE]
> Pour obtenir une description détaillée sur la création d’une définition de build comme suit, consultez [création d’une définition de Build que le déploiement prend en charge](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Le *Publish.proj* fichier contient des cibles qui générer chaque projet dans la solution. Toutefois, il inclut également une logique conditionnelle qu’ignore ces cibles de génération si vous exécutez le fichier dans Team Build. Cela vous permet de tirer parti de la fonctionnalité de génération supplémentaires par Team Build, comme la possibilité d’exécuter des tests unitaires. Si la génération de la solution ou l’unité de tests échouent, le *Publish.proj* fichier n’est pas exécuté et l’application ne sera pas déployée.

La logique conditionnelle s’effectue en évaluant le **BuildingInTeamBuild** propriété. Il s’agit d’une propriété MSBuild qui est automatiquement définie sur **true** lorsque vous utilisez Team Build pour générer vos projets.

## <a name="deployment-to-staging"></a>Déploiement dans un environnement intermédiaire

Lorsqu’une build répond à toutes les exigences de l’équipe de développement dans l’environnement de test, l’équipe pouvez choisir de déployer la même build dans un environnement intermédiaire. Environnements intermédiaires sont généralement configurés pour correspondre aux caractéristiques de l’environnement « réel » en tant qu’étroitement que possible, par exemple, en termes de spécifications du serveur, de systèmes d’exploitation et de logiciels et de configuration de réseau ou de production. Environnements intermédiaires sont souvent utilisés pour le test de charge, test d’acceptation utilisateur et plus larges révisions internes nécessaires. Directement depuis le serveur de build, les builds sont déployés dans l’environnement intermédiaire.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Les définitions de build utilisées pour déployer la solution dans l’environnement intermédiaire, **DeployToStaging-WhatIf** et **DeployToStaging**, partagent ces caractéristiques :

- Ils n’en fait créer quoi que ce soit. Lorsque Rob déploie la solution dans l’environnement intermédiaire, il souhaite déployer une build spécifique, existante qui a déjà été vérifiée et validée dans l’environnement de test. Vous devez simplement exécuter les fichiers de projet personnalisés qui contrôlent le processus de déploiement des définitions de build.
- Lorsque Rob déclenche une build, il utilise les paramètres de build pour spécifier quelle build contient les ressources qu’il souhaite déployer à partir du serveur de build.
- Les définitions de build ne sont pas déclenchées automatiquement. Rob manuellement les files d’attente une build lorsqu’il souhaite déployer la solution dans l’environnement intermédiaire.

Voici la procédure à suivre pour un déploiement dans l’environnement intermédiaire :

1. L’administrateur d’environnement intermédiaire, Rob Walters, files d’attente une build à l’aide de la **DeployToStaging-WhatIf** définition de build. Rob utilise les paramètres de définition de build pour spécifier quelle build il souhaite déployer.
2. Le **DeployToStaging-WhatIf** générer les exécutions de définition les fichiers de projet personnalisé en mode « que se passe-t-il si ». Cela génère des fichiers journaux comme si Rob effectuait un déploiement en direct, mais il ne réellement apporte des modifications à l’environnement de destination.
3. Rob passe en revue les fichiers journaux pour déterminer les effets du déploiement sur l’environnement intermédiaire. En particulier, Rob veut vérifier ce qui sera ajouté, ce qui sera mis à jour, et ce qui sera supprimé.
4. Si Rob est satisfaite que le déploiement ne procédez aux modifications indésirables aux ressources existantes ou de données, il les files d’attente une build à l’aide de la **DeployToStaging** définition de build.
5. Le **DeployToStaging** générer les exécutions de définition les fichiers de projet personnalisé. Ces publier les ressources de déploiement sur le serveur web principal dans l’environnement intermédiaire.
6. Le contrôleur de Web Farm Framework (WFF) synchronise les serveurs web dans l’environnement intermédiaire. Cela la rend disponible sur tous les serveurs web dans la batterie de serveurs.

### <a name="how-does-the-deployment-process-work"></a>Comment fonctionne le processus de déploiement ?

Le **DeployToStaging** build fournitures de définition de ces arguments à MSBuild :


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


Le **TargetEnvPropsFile** propriété indique le *Publish.proj* où trouver le fichier de projet spécifique à un environnement à importer de fichiers. Le **OutputRoot** propriété remplace la valeur intégrée et indique l’emplacement du dossier de build qui contient les ressources que vous souhaitez déployer. Lorsque Rob files d’attente la build, il utilise le **paramètres** onglet pour fournir une valeur mise à jour pour le **OutputRoot** propriété.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Pour plus d’informations sur la création d’une définition de build comme suit, consultez [déployer une Build spécifique](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


Le **DeployToStaging-WhatIf** définition de build contient la même logique de déploiement que les **DeployToStaging** définition de build. Toutefois, il inclut l’argument supplémentaire **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


Dans le *Publish.proj* fichier, le **WhatIf** propriété indique que toutes les ressources de déploiement doivent être publiées en mode « que se passe-t-il si ». En d’autres termes, les fichiers journaux sont générés comme si le déploiement est passé à l’avance, mais rien n’est réellement changé dans l’environnement de destination. Cela vous permet d’évaluer l’impact d’un déploiement proposé&#x2014;en particulier, ce qui seront ajouté, ce qui sera mis à jour, et ce qui sera supprimé&#x2014;avant d’apporter des modifications.

> [!NOTE]
> Pour plus d’informations sur la façon de configurer des déploiements « que se passe-t-il si », consultez [exécution d’un déploiement « Que se passe-t-il si »](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Une fois que vous avez déployé votre application sur le serveur web principal dans l’environnement intermédiaire, le WFF se synchronise automatiquement l’application sur tous les serveurs dans la batterie de serveurs.

> [!NOTE]
> Pour plus d’informations sur la configuration de la WFF pour synchroniser les serveurs web, consultez [créer une batterie de serveurs avec l’infrastructure de batterie de serveurs Web](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Déploiement en Production

Quand une build a été approuvée dans l’environnement intermédiaire, l’équipe Fabrikam, Inc. peut publier l’application sur l’environnement de production. L’environnement de production est là l’application « publiée » et qu’il atteint son public cible d’utilisateurs finaux.

L’environnement de production est dans un réseau de périmètre accessibles sur Internet. Cela est isolé du réseau interne qui contient le serveur de builds. L’administrateur d’environnement de production, Lisa Andrews, devez manuellement copier les packages de déploiement web à partir du serveur de build et importez-les dans IIS sur le serveur web de production principal.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Voici la procédure à suivre pour un déploiement dans l’environnement de production :

1. L’équipe de développement conseille Lisa qu’une build est prête pour le déploiement en production. L’équipe conseille Lisa de l’emplacement des packages de déploiement web dans le dossier de dépôt sur le serveur de builds.
2. Lisa collecte les packages web à partir du serveur de build et les copie sur le serveur web principal dans l’environnement de production.
3. Lisa utilise le Gestionnaire IIS pour importer et publier les packages web sur le serveur web principal.
4. Le contrôleur WFF synchronise les serveurs web dans l’environnement de production. Cela la rend disponible sur tous les serveurs web dans la batterie de serveurs.

### <a name="how-does-the-deployment-process-work"></a>Comment fonctionne le processus de déploiement ?

Gestionnaire des services IIS inclut un Assistant Importation de Package des applications qui facilite la publication des packages web sur un site Web IIS. Pour une procédure pas à pas sur la façon d’effectuer cette procédure, consultez [installer manuellement des Packages Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique fourni une illustration du cycle de vie d’une application web de type entreprise à l’échelle du déploiement.

Cette rubrique fait partie d’une série de didacticiels qui fournissent des conseils sur divers aspects du déploiement d’applications web. Dans la pratique, il existe un grand nombre de considérations et tâches supplémentaires à chaque étape du processus de déploiement, et il n’est pas possible pour les couvrir dans une procédure pas à pas unique. Pour plus d’informations, consultez ces didacticiels :

- [Déploiement de l’entreprise Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une introduction complète aux techniques de déploiement web à l’aide de MSBuild et l’outil de déploiement Web IIS (Web Deploy).
- [Configuration d’environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel fournit des conseils sur la façon de configurer des environnements de serveurs Windows pour prendre en charge différents scénarios de déploiement.
- [Configuration de Team Foundation Server pour le déploiement de Web automatisé](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment apprendre à intégrer la logique de déploiement dans le processus de génération TFS.
- [Avancées de déploiement Web d’entreprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel fournit des conseils sur la façon de répondre à certains des défis de déploiement plus complexes auxquels sont confrontées les organisations.

> [!div class="step-by-step"]
> [Précédent](enterprise-web-deployment-scenario-overview.md)
