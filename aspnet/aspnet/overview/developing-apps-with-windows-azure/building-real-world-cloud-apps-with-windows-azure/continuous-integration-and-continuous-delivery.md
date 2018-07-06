---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Intégration continue et livraison continues (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 40687c490fdfb7764ac9ca8af6fffd054362d552
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819413"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Intégration continue et livraison continues (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Les deux premiers recommandé de modèles de processus de développement ont été [automatiser tout](automate-everything.md) et [contrôle de code Source](source-control.md), et le troisième modèle de processus de les combine. Intégration continue (CI) signifie que chaque fois qu’un développeur archive le code au référentiel source, une build est déclenchée automatiquement. Livraison continue (CD) prend une étape supplémentaire : après une build et tests d’unités automatisés sont réussies, vous déployez automatiquement l’application dans un environnement où vous pouvez effectuer un test plus approfondie.

Le cloud vous permet de réduire le coût de maintenance d’un environnement de test, car vous payez uniquement pour les ressources d’environnement, que vous utilisez. Votre processus CD peut configurer l’environnement de test lorsque vous en avez besoin, et peut interrompre l’environnement lorsque vous avez terminé test.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Workflow d’intégration continue et livraison continues

En général nous vous recommandons d’effectuer la livraison continue pour le développement et de vos environnements intermédiaires. La plupart des équipes, même chez Microsoft, nécessitent un processus de révision et approbation manuels pour le déploiement de production. Pour une production déploiement, que vous souhaiterez peut-être vous assurer qu’elle se produit lorsque des personnes clés de l’équipe de développement sont disponibles pour la prise en charge, ou pendant les périodes de faible trafic. Mais rien ne vous empêche d’automatiser complètement vos environnements de développement et de test afin que tout développeur doit faire est d’archiver une modification et un environnement est configuré pour les tests d’acceptation.

Le diagramme suivant tiré [un Microsoft Patterns and Practices e-book sur la livraison continue](http://aka.ms/ReleasePipeline) illustre un flux de travail classique. Cliquez sur l’image pour l’afficher à taille réelle dans son contexte d’origine.

[![Flux de travail de diffusion en continu](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Comment le cloud permet économique CI et CD

Il est facile d’automatisation de ces processus dans Azure. Étant donné que vous exécutez tous les éléments dans le cloud, il est inutile d’acheter ou de gérer des serveurs pour vos builds ou de vos environnements de test. Et vous n’êtes pas obligé d’attendre pour être disponible pour effectuer vos tests sur un serveur. Avec chaque build que vous effectuez, vous pouvez faire tourner un environnement de test dans Azure à l’aide de votre script d’automatisation, les tests d’acceptation d’exécution ou plus approfondie de tests par rapport à elle et puis lorsque vous avez terminé de simplement détruire. Et si vous exécutez uniquement ce serveur pour 2 heures, 8 heures ou un jour, la somme d’argent que vous devez payer est minime, car vous payez uniquement pour la durée pendant laquelle un ordinateur est en cours d’exécution. Par exemple, l’environnement requis pour le correctif que si l’application coûte environ 1 cent par heure si vous monter d’un niveau à partir du niveau gratuit. Au cours d’un mois, si vous n’avez exécuté l’environnement une heure à la fois, votre environnement de test est probablement prix est inférieur à un latte vous achetez au client de Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS fournit un certain nombre de fonctionnalités pour vous aider au développement d’applications à partir de la planification de déploiement.

- Il prend en charge Git (distribué) et le contrôle de code source TFVC (centralisé).
- Il offre un service de build élastique, ce qui signifie qu’il crée des serveurs de builds quand elles sont nécessaires et les retire lorsqu’ils ont terminé de dynamiquement. Vous pouvez lancer automatiquement une build lorsque qu’un utilisateur archive des modifications du code source, et que vous n’êtes pas obligé d’avoir allouer et de payer pour vos propres serveurs de build qui se trouvent la plupart du temps inactifs. Le service de build est gratuit, que vous ne dépassez pas un certain nombre de builds. Si vous prévoyez d’effectuer un grand nombre de builds, vous pouvez payer un peu supplémentaire pour les serveurs de build réservé.
- Il prend en charge la livraison continue vers Azure.
- Il prend en charge le test de charge automatisée. Test de charge est essentiel pour une application cloud, mais est souvent négligé jusqu'à ce qu’il soit trop tard. Test de charge simule une utilisation intensive d’une application par des milliers d’utilisateurs, ce qui vous permet de trouver les goulots d’étranglement et améliorer le débit, avant de publier l’application en production.
- Il prend en charge les collaborations de salle d’équipe, qui facilite la communication en temps réel et la collaboration pour les petites équipes agiles.
- Il prend en charge la gestion de projet agile.


Pour plus d’informations sur l’intégration continue et livraison de VSTS, consultez [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Si vous avez besoin d’une gestion de projet de la clé en main, collaboration d’équipe et la solution de contrôle de code source, consultez VSTS. Le service est gratuit jusqu'à 5 utilisateurs, et vous pouvez souscrire à l’adresse [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Récapitulatif

Les modèles de développement trois cloud première ont été sur la façon d’implémenter un processus de développement prévisible, fiable et reproductible avec le temps de cycle faible. Dans le [chapitre suivant](web-development-best-practices.md) , nous commençons à examiner les modèles d’architecture et de codage.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez [déployer une application web dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Consultez également les ressources suivantes :

- [Création d’un Pipeline de mise en production avec Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Ateliers pratiques, E-book et exemple de code par Microsoft Patterns and Practices, fournit une introduction complète à la livraison continue. Couvre l’utilisation de Visual Studio Lab Management et Visual Studio Release Management.
- [ALM Rangers DevOps outils et conseils](https://aka.ms/vsarsolutions/). Le groupe ALM Rangers a introduit le DevOps Workbench exemple le Guide de solution et des conseils pratiques en collaboration avec les modèles &amp; livre de pratiques *création d’un Pipeline de mise en production avec TFS 2012*, comme un excellent moyen de démarrer apprendre les concepts de DevOps &amp; Release Management pour TFS 2012 et pour vos premiers pas. Le guide explique comment créer une seule fois et déployer dans plusieurs environnements.
- [Test de livraison continue avec Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Livre électronique par Microsoft Patterns and Practices, explique comment intégrer les tests automatisés avec la livraison continue.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Code source pour un outil conçu pour capturer une build à partir de TFS (basé sur une étiquette), générez-le, empaqueter, permet à un utilisateur dans le rôle DevOps pour configurer des aspects spécifiques de celui-ci et les intégrer à Azure. L’outil effectue le suivi du processus de déploiement afin de permettre les opérations « Annuler » pour une version précédemment déployée. L’outil n’a aucune dépendance externe et peut fonctionner autonome à l’aide d’API de TFS et le Kit de développement.
- [Livraison continue : Logiciel fiable libère via l’automatisation du déploiement, de Test et de Build](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Livre de Jez Humble.
- [Libérez-le ! Concevoir et déployer des logiciels de prêt pour la Production](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Livre de Michael T. Nygard.

> [!div class="step-by-step"]
> [Précédent](source-control.md)
> [Suivant](web-development-best-practices.md)
