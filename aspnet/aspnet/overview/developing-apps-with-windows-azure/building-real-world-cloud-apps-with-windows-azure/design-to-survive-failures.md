---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Conception pour surmonter les défaillances (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 72b26fd58866df1296d06bd9e3bc695900cfbf65
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578469"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Concevoir pour surmonter les défaillances (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


L’une des choses que vous aurez plus à vous lorsque vous générez tout type d’application, mais en particulier une qui s’exécute dans le cloud où un grand nombre de personnes utiliseront, est la conception de l’application afin qu’il peut normalement gérer les défaillances et continuer à assurer la valeur de mesure possibles. Avec le temps, choses se tromper dans n’importe quel environnement ou n’importe quel système logiciel. Comment votre application gère ces situations détermine comment contrarie vos clients obtiendront et combien de temps passé à analyser et résoudre les problèmes.

## <a name="types-of-failures"></a>Types de défaillances

Il existe deux catégories d’échecs que vous souhaitez gérer différemment :

- Temporaire, la réparation automatique des défaillances telles que des problèmes de connectivité réseau intermittente.
- Échecs durable qui nécessitent une intervention.

Pour les erreurs temporaires, vous pouvez implémenter une stratégie de nouvelle tentative pour vous assurer que la plupart du temps, l’application récupère rapidement et automatiquement. Vos clients peuvent remarquer des temps de réponse légèrement plus longue, mais sinon ils ne seront pas affectés. Nous allons montrer quelques méthodes permettant de gérer ces erreurs dans le [chapitre de gestion des erreurs temporaires](transient-fault-handling.md).

Pour avoir surmonté échecs, vous pouvez implémenter surveillance et la journalisation des fonctionnalités qui vous informe rapidement lorsque des problèmes surviennent et qui facilite l’analyse de la cause racine. Nous allons montrer quelques méthodes pour vous aider à rester informé de ces types d’erreurs dans le [chapitre de surveillance et télémétrie](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Étendue de l’échec

Vous devez également réfléchir à la portée de l’échec : si un seul ordinateur est affecté, un service entier comme base de données SQL, de stockage ou d’une région entière.

![Étendue de l’échec](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Échecs d’ordinateur

Dans Azure, un serveur défaillant est automatiquement remplacé par un autre, et une application cloud bien conçue récupère à partir de ce type d’échec rapidement et automatiquement. Précédemment, nous souligner les avantages de l’évolutivité d’un niveau web sans état, et la facilité de récupération à partir d’un serveur défaillant est un autre avantage de l’abandon de l’état. Facilité de récupération est également un des avantages des fonctionnalités du platform-as-a-service (PaaS) telles que la base de données SQL et Azure App Service Web Apps. Défaillances de matériel sont rares, mais quand elles se produisent que ces services de les gérer automatiquement ; vous n’êtes pas obligé même écrire du code pour gérer les défaillances de l’ordinateur lorsque vous utilisez l’un de ces services.

### <a name="service-failures"></a>Échecs du service

Applications cloud utilisent généralement plusieurs services. Par exemple, l’application Fix It utilise le service de base de données SQL, le service de stockage, et l’application web est déployée sur Azure App Service. Ce que fera votre application si un des services que vous dépendez échoue ? Pour certains service échecs convivial « Désolé, réessayez plus tard » message risque d’être la meilleure solution pour vous. Mais dans de nombreux scénarios vous pouvez le faire mieux. Par exemple, quand votre magasin de données back-end est arrêté, vous pouvez accepter l’entrée d’utilisateur, afficher « a été reçu votre demande » et stocker l’entrée quelque part autre temporairement ; Ensuite, lorsque le service que vous avez besoin est opérationnel, là encore, vous pouvez récupérer l’entrée et le traiter.

Le [modèle de travail centré sur la file d’attente](queue-centric-work-pattern.md) chapitre montre une façon de gérer ce scénario. L’application Fix It stocke les tâches dans la base de données SQL, mais il n’a pas à quitter son lors de la base de données SQL est arrêté. Dans ce chapitre, nous allons voir comment stocker des entrées d’utilisateur pour une tâche dans une file d’attente et utiliser un processus de travail pour lire la file d’attente et de mettre à jour de la tâche. Si SQL est arrêté, la possibilité de créer des tâches Fix It est pas affectée ; le processus de travail bien attendre et traiter les nouvelles tâches lors de la base de données SQL est disponible.

### <a name="region-failures"></a>Défaillances de la région

Des régions entières peuvent échouer. Une catastrophe naturelle peut détruire un centre de données, il peut obtenir aplati par un meteor, la ligne trunk dans le centre de données pourrait être tronquée par un exploitant par une vache avec rétro, etc. Que faire si votre application est hébergée dans le centre de données appliquer ? Il est possible de configurer votre application dans Azure pour exécuter simultanément dans plusieurs régions afin que s’il existe un incident dans un, vous continuer à exécuter dans une autre région. Ces défaillances sont extrêmement rares occurrences, et ne quittez pas la plupart des applications via les gués et nécessaires pour garantir un service ininterrompu des pannes de ce type. Consultez la section de ressources à la fin de ce chapitre pour plus d’informations sur la façon de conserver votre application disponible même par le biais d’une défaillance de région.

Un Azure vise à faciliter le traitement de tous ces types de défaillances beaucoup plus faciles, et vous verrez des exemples de la façon dont nous faisons que dans les chapitres suivants.

## <a name="slas"></a>Contrats SLA

Personnes entendez souvent sur les contrats de niveau de service (SLA) dans l’environnement cloud. En fait, il s’agit promesses qui rendent des entreprises sur la fiabilité de leur service. Un contrat SLA signifie que vous de 99,9 % doit attendre que le service fonctionne correctement 99,9 % du temps. Qui est une valeur typique pour un contrat SLA et il ressemble à un nombre très élevé, mais vous ne réalisez pas combien de temps bas. correspond réellement 1 %. Voici un tableau qui indique le temps d’inactivité différents pourcentages de contrat SLA concerner sur une année, un mois et une semaine.

![Table de contrat SLA](design-to-survive-failures/_static/image2.png)

Par conséquent, un contrat SLA signifie que votre service de 99,9 % est en panne 8,76 heures par an ou 43,2 minutes par mois. C’est plus temps d’arrêt que la plupart des gens le pensent. Par conséquent, en tant que développeur que vous souhaitez n’oubliez pas qu’un certain laps de temps d’arrêt est possible et de gérer de façon normale. À un moment donné quelqu'un va être à l’aide de votre application et un service va être arrêté et que vous souhaitez réduire l’impact négatif de que sur le client.

Une chose que vous devez connaître un contrat SLA est quel laps de temps qu’il fait référence à : l’horloge réinitialisée chaque semaine, chaque mois ou chaque année ? Dans Azure nous réinitialiser l’horloge chaque mois, ce qui vous convient le mieux à un contrat SLA annuels, dans la mesure où un contrat SLA annuels pourrait masquer incorrectes mois en les décalant avec une série de bonne mois.

Nous aspirons bien sûr toujours faire mieux que le contrat SLA ; vous serez généralement moins importante que celle vers le bas. La promesse est que si nous sommes jamais plus longtemps que le maximum de temps d’arrêt, vous pouvez demander le remboursement de. La somme d’argent que vous recevez probablement n’aurait pas entièrement compense l’impact commercial de l’excès de temps d’arrêt, mais cet aspect du contrat SLA agit comme une stratégie de mise en œuvre et vous permet de savoir que nous le prennent très au sérieux.

### <a name="composite-slas"></a>Contrats SLA composites

Une chose importante à réfléchir à lorsque vous examinez des contrats SLA est l’impact de l’utilisation de plusieurs services dans une application, avec chaque service disposant d’un contrat SLA distinct. Par exemple, l’application Fix It utilise Azure App Service Web Apps, stockage Azure et SQL Database. Voici leurs numéros de contrat SLA à la date de que ce livre électronique est en cours d’écriture dans décembre 2013 :

![Contrat SLA Site Web, stockage, SQL Database](design-to-survive-failures/_static/image3.png)

Quelle est la valeur maximale temps prévu pour l’application en fonction de ces contrats de service SLA d’arrêt ? Vous pourriez penser que votre temps d’arrêt serait égal à la pire pourcentage SLA ou 99,9 % dans ce cas. Qui est true si les trois services toujours échec en même temps, mais qui n’est pas nécessairement ce qui se passe réellement. Chaque service peut échouer indépendamment à des moments différents, donc vous devez calculer le contrat SLA composite en multipliant les numéros de contrat SLA individuels.

![Contrat SLA composite](design-to-survive-failures/_static/image4.png)

Par conséquent, votre application peut être bas pas seulement 43,2 minutes un mois mais 3 fois cette quantité, 108 minutes par mois – et toujours dans les limites de contrat SLA Azure.

Ce problème n’est pas unique dans Azure. Nous fournissons en fait le meilleur cloud contrats SLA de n’importe quel service de cloud disponibles, et vous avez des questions similaires à traiter si vous utilisez des services de cloud de n’importe quel fournisseur. Ce que cela met en surbrillance est l’importance de penser comment vous pouvez concevoir votre application pour gérer les échecs de service inévitable de façon appropriée, car ils peuvent se produire assez souvent pour avoir un impact sur vos clients ou utilisateurs.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Contrats SLA cloud par rapport à l’expérience de temps d’arrêt d’entreprise

Personnes parfois, par exemple, « Dans mon application d’entreprise j’ai jamais avoir ces problèmes. » Si vous demandez combien de temps bas mois réellement sont-ils, ils disent généralement, « Eh bien, il se produit occasionnellement. » Et si vous vous demandez la fréquence à laquelle ils admettent que « Parfois nous avez besoin de sauvegarder ou installer un nouveau logiciel de serveur ou de mise à jour. » Bien sûr, qui compte comme temps d’arrêt. La plupart des applications d’entreprise, sauf si elles sont surtout critiques sont en fait hors service depuis plus de la durée d’exécution autorisée par notre contrat SLA de service. Mais lorsqu’il est votre serveur et votre infrastructure et vous êtes responsable pour lui et dans le contrôle de celui-ci, vous avez tendance à se sentent moins secret sur les temps d’inactivité. Dans un environnement de cloud, vous êtes dépendant de quelqu'un d’autre et que vous ne connaissez pas ce qui se passait, donc vous pouvez ont tendance à devenir plus soucieux quant à elle.

Lorsqu’une entreprise réalise un pourcentage de temps supérieure que vous obtenez à partir d’un contrat SLA de cloud, ils faites-le en passant beaucoup plus d’argent sur du matériel. Un service cloud peut faire mais aurait à facturer bien plus encore pour ses services. Au lieu de cela, vous tirez parti d’un service économique et concevez vos logiciels afin que les échecs inévitables entraînent une interruption minimale pour vos clients. Votre travail en tant qu’un concepteur d’application cloud n’est pas tellement à éviter l’échec afin d’éviter une éventuelle catastrophe et procéder en se concentrant sur les logiciels, pas sur le matériel. Tandis que les applications d’entreprise s’efforcent d’optimiser le temps moyen entre défaillances, les applications cloud s’efforcent afin de réduire le temps moyen de récupération.

### <a name="not-all-cloud-services-have-slas"></a>Pas tous les services cloud ont des contrats SLA

N’oubliez pas également pas chaque service cloud même qu’un contrat SLA. Si votre application dépend d’un service sans garantie de temps d’exécution, vous pourriez être bas beaucoup plus de temps que vous pouvez l’imaginer. Par exemple, si vous permettent de connecter votre site à l’aide d’un fournisseur de réseaux sociaux tels que Facebook ou Twitter, contactez le fournisseur de services pour savoir s’il existe un contrat SLA, et vous constaterez peut-être quasi-certitude n’existe pas. Mais si le service d’authentification tombe en panne ou ne parvient pas à prendre en charge le volume de requêtes que vous lui envoyez, vos clients sont verrouillés hors de votre application. Vous pourriez être vers le bas pour les jours ou plus. Les créateurs d’une nouvelle application attendu des centaines de millions de téléchargements et a pris une dépendance sur l’authentification Facebook – mais n’a pas communiquer avec Facebook avant de passer en direct et découvertes trop tard qu’aucun contrat SLA pour ce service s’est produite.

### <a name="not-all-downtime-counts-toward-slas"></a>Pas tous les nombres de temps d’arrêt vers les contrats SLA

Certains services de cloud peuvent délibérément de refuser votre application les utilise trop. Il s’agit *limitation*. Si un service a un contrat SLA, il doit indiquer les conditions dans lesquelles vous pouvez être limité, et la conception de votre application doit éviter ces conditions et réagir de façon appropriée pour la limitation si cela se produit. Par exemple, si les demandes à un service commencent à échouer lorsque vous dépassez un certain nombre par seconde, vous souhaitez vous assurer que les nouvelles tentatives automatiques ne se produisent pas si vite qu’ils provoquent la limitation continuer. Nous aurons plus d’informations sur la limitation dans le [chapitre de gestion des erreurs temporaires](transient-fault-handling.md).

## <a name="summary"></a>Récapitulatif

Ce chapitre a essayé de vous aider à comprendre pourquoi une application de cloud du monde réel doit être conçu pour faire face aux défaillances de manière appropriée. En commençant par le [chapitre suivant](monitoring-and-telemetry.md), les autres modèles de cette série abordent plus en détail certaines stratégies que vous pouvez utiliser pour ce faire :

- Ont de bonnes [surveillance et télémétrie](monitoring-and-telemetry.md), de sorte que vous découvrez rapidement échecs qui nécessitent une intervention, et vous disposez de suffisamment d’informations pour les résoudre.
- [Gérer les erreurs temporaires](transient-fault-handling.md) en implémentant la logique de nouvelle tentative intelligent afin que votre application récupère automatiquement lorsqu’il peut et revient à [disjoncteur](transient-fault-handling.md#circuitbreakers) logique lorsqu’il ne peut pas.
- Utilisez [mise en cache distribuée](distributed-caching.md) afin de réduire les problèmes de connexion, la latence et débit avec un accès de base de données.
- Implémentez faible couplage le [modèle centré sur la file d’attente de travail](queue-centric-work-pattern.md), de sorte que votre frontal d’application peut continuer à fonctionner lorsque le serveur principal est arrêté.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les chapitres suivants de ce livre électronique et les ressources suivantes.

Documentation :

- [FailSafe : Guidance for Architectures résilientes du Cloud recommandations](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc par Marc Mercuri, Ulrich Homann et Andrew Townhill. Version de la page Web de la série de vidéos de prévention de défaillance.
- [Meilleures pratiques pour la création de Services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc par Mark Simms et Michael Thomassy.
- [Azure technique continuité](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Livre blanc en Patrick Wickline, Jason Roth.
- [Récupération d’urgence et haute disponibilité pour les Applications Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Livre blanc par Michael McKeown, Hanu Kommalapati et Jason Roth.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez le Guide de déploiement de centre de données multiples, modèle disjoncteur.
- [Prise en charge Azure - contrats de niveau de Service](https://azure.microsoft.com/support/legal/sla/).
- [Continuité d’activité dans la base de données SQL Azure](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentation sur la base de données SQL haute disponibilité et reprise après sinistre fonctionnalités de récupération.
- [Haute disponibilité et récupération d’urgence pour SQL Server dans Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vidéos :

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de neuf en parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels. Épisodes 1 et 8 allez plus en profondeur les raisons de conception d’applications de cloud pour surmonter les défaillances. Voir aussi la discussion de suivi de la limitation dans l’épisode 2 en commençant à la discussion des disjoncteurs dans épisode 3 en commençant à 40:55 49:57 et la discussion sur les points de défaillance et modes d’échec dans l’épisode 2 en commençant à 56:05.
- [Construction Big : Leçons apprises de clients Azure - partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parle de la conception de l’échec et l’instrumentation de tous les éléments. Semblable à la série de prévention de défaillance, mais entrera en procédures plus de détails.

> [!div class="step-by-step"]
> [Précédent](unstructured-blob-storage.md)
> [Suivant](monitoring-and-telemetry.md)
