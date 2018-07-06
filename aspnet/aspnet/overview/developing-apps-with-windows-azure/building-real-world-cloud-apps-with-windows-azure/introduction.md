---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Création d’applications de Cloud réalistes avec Azure | Microsoft Docs
author: MikeWasson
description: Ce livre électronique vous présente une approche basée sur des modèles pour créer des solutions de cloud réalistes. Ces modèles s’appliquent au processus de développement, ainsi que pour un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 15e5c0a0411e3cd9433544e9a09b6373311daf6b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396212"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Création d’applications Cloud réalistes avec Azure
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Ce livre électronique vous présente une approche basée sur des modèles pour créer des solutions de cloud réalistes. Ces modèles s’appliquent au processus de développement, ainsi qu’à l’architecture et des pratiques de codage.
> 
> Le contenu est basé sur une présentation développé par Scott Guthrie et fournie par lui sur les développeurs conférence Norvégiens en juin 2013 ([partie 1](http://vimeo.com/68215538), [partie 2](http://vimeo.com/68215602)) et à Microsoft Tech Ed Australia dans Septembre 2013 ([partie 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [partie 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Beaucoup d’autres](more-patterns-and-guidance.md#acknowledgments) mis à jour ce contenu et augmenté lors de la transition à partir de la vidéo à la forme écrite.


## <a name="intended-audience"></a>Public visé

Les développeurs qui se trouvent curieux de savoir développer pour le cloud, vous envisagez une migration vers le cloud ou développement cloud trouverez ici une vue d’ensemble concise des concepts et des pratiques que dont ils ont besoin de connaître plus importantes. Les concepts sont illustrés par des exemples concrets et chaque chapitre des liens vers des autres ressources pour plus d’informations. Les exemples et les liens vers des ressources supplémentaires sont pour les services et les infrastructures de Microsoft, mais les principes illustrés s’appliquent aux autres infrastructures de développement web et les environnements en nuage.

Les développeurs qui sont en cours de développement pour le cloud peuvent trouver ici des idées qui vous aide à les rendre plus efficace. Chaque chapitre de la série peut être lu indépendamment, vous pouvez donc choisir et choisissez des rubriques qui vous intéresse.

Toute personne regardé Guthrie *Building Real World Cloud Apps with Azure* présentation et veut plus de détails et informations mises à jour constaterez qu’ici.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Modèles de développement cloud

Ce livre électronique explique treize recommandé de modèles pour le développement en nuage. « Pattern » est utilisé ici au sens large pour désigner une méthode recommandée pour effectuer des opérations : comment mieux d’aller sur le développement, de conception et de codage des applications cloud. Il s’agit de modèles qui vous aideront « appartiennent à la table d’importation principale de réussite » si vous les respectez.

- [Tout automatiser](automate-everything.md).

    - Utiliser des scripts pour optimiser l’efficacité et réduire les erreurs dans les processus répétitifs.
    - Démonstration : Scripts de gestion Azure.
- [Contrôle de code source](source-control.md). 

    - Configurer la structure de branches dans le contrôle de code source afin de faciliter le flux de travail DevOps.
    - Démo : ajouter des scripts pour le contrôle de code source.
    - Démonstration : conserver les données sensibles en dehors du contrôle de code source.
    - Démo : utilisation de Git dans Visual Studio.
- [Intégration et livraison](continuous-integration-and-continuous-delivery.md). 

    - Automatiser la génération et déploiement avec chaque archivage du contrôle source.
- [Meilleures pratiques de développement de Web](web-development-best-practices.md). 

    - Conserver la couche web sans état.
    - Démonstration : mise à l’échelle et l’échelle automatique dans les applications Web dans Azure App Service.
    - Évitez d’état de session.
    - Utiliser un CDN avec une solution de secours lorsque le CDN n’est pas disponible.
    - Utilisez le modèle de programmation asynchrone.
    - Démonstration : async dans ASP.NET MVC et Entity Framework.
- [Authentification unique](single-sign-on.md). 

    - Présentation d’Azure Active Directory.
    - Démo : création d’une application ASP.NET qui utilise Azure Active Directory.
- [Options de stockage de données](data-storage-options.md). 

    - Types de magasins de données.
    - Comment choisir le magasin de données correct.
    - Démonstration : De la base de données SQL Azure.
- [Stratégies de partitionnement des données](data-partitioning-strategies.md). 

    - Partitionner les données verticalement, horizontalement ou les deux pour faciliter la mise à l’échelle d’une base de données relationnelle.
- [Stockage d’objets blob non structurées](unstructured-blob-storage.md). 

    - Store les fichiers dans le cloud à l’aide du service blob.
    - Démo : utilisation du stockage blob dans l’application Fix It.
- [Conception pour surmonter les défaillances](design-to-survive-failures.md). 

    - Types de défaillances.
    - Échec de la portée.
    - Présentation des contrats SLA.
- [Surveillance et télémétrie](monitoring-and-telemetry.md). 

    - Pourquoi vous devez acheter une application de données de télémétrie et écrire votre propre code pour instrumenter votre application.
    - Démonstration : New Relic pour Azure
    - Démonstration : le code de journalisation dans l’application Fix It.
    - Démonstration : injection de dépendances dans l’application Fix It.
    - Démonstration : prise en charge journalisation intégrée dans Azure.
- [Gestion des erreurs temporaires](transient-fault-handling.md). 

    - Logique de nouvelle tentative/temporisation intelligente permet d’atténuer l’effet des défaillances temporaires.
    - Démonstration : nouvelle tentative/temporisation dans Entity Framework 6.
- [Mise en cache distribuée](distributed-caching.md). 

    - Améliorer l’évolutivité et réduire les coûts de transaction de base de données à l’aide de la mise en cache distribuée.
- [Modèle centré sur la file d’attente de travail](queue-centric-work-pattern.md). 

    - Activer la haute disponibilité et améliorer l’évolutivité en coupler faiblement des niveaux web et worker.
    - Démonstration : Files d’attente stockage Azure dans l’application Fix It.
- [Plus de modèles d’application et les conseils cloud](more-patterns-and-guidance.md).
- [Annexe : L’exemple d’application Fix It](the-fix-it-sample-application.md)

    - Problèmes connus
    - Meilleures pratiques
    - Comment télécharger, générer, exécuter et déployer.

Ces modèles s’appliquent à tous les environnements de cloud, mais nous allons les illustrer à l’aide d’exemples basés sur les technologies Microsoft et des services, tels que Visual Studio, Team Foundation Service, ASP.NET et Azure.

Cette suite de ce chapitre présente le Fix It exemple d’application et les applications Web dans un environnement de cloud Azure App Service qui s’exécute de l’application Fix It dans.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>L’exemple d’application de correctif

La plupart des captures d’écran et des exemples de code présentés dans ce livre électronique est basées sur l’application Fix It initialement développée par [Scott Guthrie](https://weblogs.asp.net/scottgu/) pour illustrer les modèles de développement d’application cloud recommandée et pratiques.

![Corriger la page d’accueil application](introduction/_static/image1.png)

L’exemple d’application est un élément de travail simple système de création de tickets. Lorsque vous devez résoudre un problème, vous créez un ticket et l’affecter à une personne et d’autres permettre se connecter et voir les tickets affectés à ces et marquez les tickets comme terminé quand le travail est terminé.

Il est un projet web de Visual Studio standard. Il repose sur ASP.NET MVC et utilise une base de données SQL Server. Il peut s’exécuter localement dans IIS Express et peut être déployé sur un Site Web de Azure à exécuter dans le cloud. Vous pouvez vous connecter à l’aide de l’authentification par formulaire et une base de données locale ou à l’aide d’un fournisseur de réseaux sociaux tels que Google. (Plus tard nous allons montrer également comment vous connecter avec un compte d’organisation Active Directory.)

![Page de connexion](introduction/_static/image2.png)

Une fois que vous êtes connecté dans vous pouvez créer un ticket, affectez-le à une personne et charger une image de ce que vous voulez être résolu.

![Créer une tâche Fix It](introduction/_static/image3.png)

![Corriger la tâche créée](introduction/_static/image4.png)

Vous pouvez suivre la progression d’éléments de travail que vous avez créé, consultez les tickets attribuées, afficher les détails de ticket et de marquer les éléments comme étant terminés.

Il s’agit d’une application très simple à partir d’un point de vue fonctionnalité, mais vous verrez comment le créer afin qu’il peut s’adapter à des millions d’utilisateurs et est résistante aux éléments tels que les échecs de la base de données et les connexions arrêtées. Vous verrez également comment créer un flux de travail de développement agile et automatisés, ce qui vous permet de démarrer simple et rendre l’application plus en plus en effectuant une itération du cycle de développement rapidement et efficacement.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Applications Web dans Azure App Service

L’environnement de cloud utilisé pour l’application Fix It est un service de Azure que nous appelons des Sites Web. Ce service est une façon que vous pouvez héberger votre propre application web dans Azure sans avoir à créer des machines virtuelles et leur mise à jour, installer et configurer IIS, etc. Nous héberger votre site sur nos machines virtuelles et fournissent automatiquement la sauvegarde et de récupération et d’autres services pour vous. Le service Sites Web fonctionne avec ASP.NET, Node.js, PHP et Python. Il vous permet de déployer très rapidement à l’aide de Visual Studio, Web Deploy, FTP, Git ou TFS. Il est généralement que quelques secondes entre l’heure que vous démarrez un déploiement et l’heure de que votre mise à jour est disponible sur Internet. Il est gratuit commencer, et vous pouvez monter que votre trafic augmente.

Dans les coulisses, Web Apps dans Azure App Service fournit un grand nombre de composants architecturaux et les fonctionnalités que vous seriez obligé de créer vous-même si vous envisagez d’héberger un site web à l’aide d’IIS sur vos propres machines virtuelles. Un composant est un point de terminaison de déploiement qui configure IIS et installe votre application sur autant de machines virtuelles que vous souhaitez exécuter votre site sur automatiquement.

![Service de déploiement](introduction/_static/image5.png)

Lorsqu’un utilisateur appuie sur le site web, ils n’atteignent pas les machines virtuelles IIS directement, elles passent par [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) équilibreurs de charge. Vous pouvez les utiliser avec vos propres serveurs, mais l’avantage est qu’ils sont configurés pour vous automatiquement. Ils utilisent une méthode heuristique intelligente qui tient compte des facteurs tels que l’affinité de session, profondeur de file d’attente dans IIS, et l’utilisation du processeur sur chaque ordinateur à diriger le trafic vers les machines virtuelles qui hébergent votre site web.

![Équilibreur de charge ARR](introduction/_static/image6.png)

Si un ordinateur tombe en panne, Azure automatiquement extrait à partir de la rotation, tourne une nouvelle instance de machine virtuelle et commence à diriger le trafic vers la nouvelle instance--tout avec aucun temps d’arrêt pour votre application.

![Récupération automatique à partir de la défaillance d’ordinateur](introduction/_static/image7.png)

Tout cela a lieu automatiquement. Il vous suffit de faire est de créer un site web et déployer votre application, à l’aide de Windows PowerShell, Visual Studio ou le portail de gestion Azure.

Pour rapidement et facilement obtenir un didacticiel qui montre comment créer une application web dans Visual Studio et le déployer sur un Site Web Azure, consultez [prise en main Azure et ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Récapitulatif

Cette introduction a dressé une liste de rubriques, que le livre couvre, des captures d’écran de l’exemple d’application et une brève présentation des applications Web dans un environnement de cloud d’Azure App Service. Un des principaux avantages du développement d’applications dans et pour le cloud est qu’il est facile automatiser les tâches de développement répétitives telles que la création d’un environnement de test et de déploiement de votre code à ce dernier. Comment faire l’objet de la [chapitre suivant](automate-everything.md).

## <a name="resources"></a>Ressources

Pour plus d’informations sur les sujets abordés dans ce chapitre, consultez les ressources suivantes.

Documentation :

- [Applications Web dans Azure App Service](https://azure.microsoft.com/services/app-service/web/). Page du portail de documentation Azure sur les applications Web.
- [Applications Web, Services Cloud et les machines virtuelles : que choisir ?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, comme illustré dans ce chapitre est simplement une des trois méthodes que vous pouvez exécuter des applications web dans Azure. Cet article explique les différences entre les trois méthodes et fournit des conseils sur le choix qui convient le mieux pour votre scénario. Comme les Sites Web, Services Cloud est une fonctionnalité PaaS d’Azure. Machines virtuelles sont une fonctionnalité IaaS. Pour obtenir une explication de PaaS et IaaS, consultez le [Options données](data-storage-options.md#paasiaas) chapitre.

Vidéos :

- [Scott Guthrie commence à l’étape 0 - qu’est le système d’exploitation du Cloud Azure ?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architecture de Sites Web - avec Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Éléments internes de Sites Web Azure avec Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
