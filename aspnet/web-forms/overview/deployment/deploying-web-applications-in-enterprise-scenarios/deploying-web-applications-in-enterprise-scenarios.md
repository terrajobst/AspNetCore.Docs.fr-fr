---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Déploiement d’Applications Web dans les scénarios d’entreprise à l’aide de Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Cette série de didacticiels décrit les outils et techniques que vous pouvez utiliser pour déployer des applications web dans différents scénarios d’entreprise. Elle explique comment tirer meilleur parti...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: fa06538dcd9e087df52a76588e084a527867bf84
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837149"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Déploiement d’Applications Web dans les scénarios d’entreprise à l’aide de Visual Studio 2010
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette série de didacticiels décrit les outils et techniques que vous pouvez utiliser pour déployer des applications web dans différents scénarios d’entreprise. Il explique comment rendre la meilleure façon d’utiliser des technologies telles que Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, l’outil de déploiement Web IIS (Web Deploy), le Web Farm Framework (WFF) et utilitaires comme VSDBCMD.exe à Simplifiez et gérer le processus de déploiement. Il inclut des présentations de concepts et tâches des conseils qui vous aideront à :
> 
> - Passez en revue et établir les exigences de déploiement pour une application web d’entreprise.
> - Configurer des environnements de serveur web test, intermédiaire et de production pour prendre en charge le déploiement web.
> - Configurer le processus d’intégration continue (CI) Team Foundation Server (TFS) pour prendre en charge le déploiement web automatisés.
> - Déployer des applications web métier pour différents environnements serveur avec différentes exigences et restrictions.
> - Déployer les modifications sur les applications web qui sont exécutent dans différents environnements serveur.
> 
> > [!NOTE]
> > Bien que ces didacticiels décrivent l’utilisation de TFS en tant qu’un serveur CI, les instructions sont facilement adaptée à n’importe quel serveur CI. Vous n’avez pas besoin une connaissance détaillée de TFS pour comprendre et exploiter les didacticiels.
> 
> 
> Pour obtenir une traduction italienne de ces didacticiels, visitez [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>À propos des auteurs

Jason Lee est technicien principal avec [contenu Master](http://www.contentmaster.com/) où il a travaillé avec les produits et technologies, notamment SharePoint et ASP.NET, Microsoft depuis plusieurs années. Jason possède un doctorat en informatique et est actuellement MCPD et MCTS certifié. Vous pouvez lire le blog technique de Jason à [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry est technicien principal avec [contenu Master](http://www.contentmaster.com/) qui a écrit des livres blancs, documentation du Kit de développement logiciel, des présentations PowerPoint et des formations en ligne et par un instructeur au cours de sa carrière. Un membre d’origine de l’équipe de documentation d’ASP.NET, il a travaillé avec les technologies web de Microsoft pour plus de dix ans.

## <a name="target-audience"></a>Public cible

Cette série de didacticiels est pour les développeurs d’applications web ASP.NET et aux architectes de solutions qui utilisent Visual Studio 2010 pour créer des applications web d’entreprise. Pour tirer le meilleur parti du contenu, vous devez être familiarisé avec Visual Studio 2010 et êtes déjà familiarisé avec TFS, ainsi que d’une prise de conscience des technologies de plate-forme web Microsoft tels que ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Serveur et les projets de base de données de Visual Studio. Toutefois, vous n’avez pas besoin être familiarisé avec les technologies et outils de déploiement ou avez besoin de savoir comment configurer des systèmes d’intégration continue.

## <a name="requirements"></a>Configuration requise

Pour suivre les procédures pas à pas et effectuer les tâches qui décrivent ces didacticiels, vous devez installer ce logiciel sur votre ordinateur de développement :

- Visual Studio 2010 Premium ou Ultimate Edition avec Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 avec Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Pour effectuer les étapes de déploiement décrites dans ces procédures pas à pas, vous devez avoir accès à des environnements de déploiement application exemple Web. Pour de meilleurs résultats, ces environnements doivent refléter le modèle de déploiement d’entreprise de votre organisation. Vous pouvez ensuite modifier les procédures décrites dans cette documentation pour refléter les environnements de déploiement et les exigences de votre organisation.

## <a name="series-contents"></a>Contenu de la série

Cette section de présentation se compose des deux rubriques supplémentaires. Celles-ci sont conçues pour fournir un contexte plus large pour les didacticiels qui suivent :

- [Déploiement Web d’entreprise : Vue d’ensemble du scénario](enterprise-web-deployment-scenario-overview.md). Cette rubrique décrit le scénario qui sous-tend chacun des didacticiels de cette série. Le scénario se concentre sur les exigences de gestion de cycle de vie des applications (ALM) de la société fictive Fabrikam, Inc. comme il développe une application web d’entreprise.
- [Gestion du cycle de vie des applications : Du développement à la Production](application-lifecycle-management-from-development-to-production.md). Cette rubrique fournit une vue d’ensemble de haut niveau, de bout en bout d’un processus de déploiement. Il illustre comment Fabrikam, Inc. déplace une ASP.NET web application d’entreprise dans les environnements de test, intermédiaire et de production dans le cadre d’un processus de développement continu.

Cette série inclut quatre jeux de didacticiel. Chacun se concentre sur les différents aspects de déploiement web :

- [Déploiement de l’entreprise Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une présentation conceptuelle de fichiers projet MSBuild, le Pipeline de publication Web, Web Deploy et autres technologies apparentées. Il explique comment vous pouvez utiliser ces outils ensemble pour gérer les processus de déploiement complexes.
- [Configuration d’environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel explique comment configurer des serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement de package web à distance en utilisant le Service de l’Agent de déploiement Web (le « agent distant ») ou le Gestionnaire de déploiement Web et le déploiement de la base de données distante. Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement, et décrit comment utiliser le WFF pour répliquer des applications web déployées sur tous les serveurs web dans une batterie de serveurs.
- [Configuration de Team Foundation Server pour le déploiement de Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer TFS pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé en tant que partie d’un processus d’intégration continue et déclenché manuellement des déploiements des builds spécifiques.
- [Avancées de déploiement Web d’entreprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de base de données pour plusieurs environnements, à l’exclusion de fichiers et dossiers de déploiement et en prenant des applications web en mode hors connexion pendant le processus de déploiement .

## <a name="where-to-start"></a>Par où commencer

Cette série de didacticiels utilise un exemple de solution avec un niveau réaliste de complexité, ainsi que d’un scénario de déploiement d’une entreprise fictive, pour fournir une implémentation de référence et pour donner les tâches et les procédures pas à pas un contexte commun. La rubrique suivante, [déploiement Web d’entreprise : vue d’ensemble du scénario](enterprise-web-deployment-scenario-overview.md), présente le scénario et l’exemple de solution. À partir de là, vous pouvez travailler via les didacticiels et les rubriques qui correspondent le mieux à vos besoins.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
