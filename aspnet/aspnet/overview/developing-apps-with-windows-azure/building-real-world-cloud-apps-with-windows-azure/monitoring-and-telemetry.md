---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Surveillance et télémétrie (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: cec29e27e834317661f80eed93cb8b81a0786644
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836454"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Surveillance et télémétrie (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Beaucoup de gens s’appuient sur les clients pour leur permettre de savoir quand leur application est arrêtée. Qui n’est pas vraiment une bonne pratique en tout lieu et surtout pas dans le cloud. Il n’existe aucune garantie de notification rapide, et lorsque vous soyez averti, vous obtenez souvent des données minimales ou trompeuses sur ce qui est arrivé. Avec le bon de télémétrie et systèmes de journalisation que vous pouvez être conscient de ce qui se passait avec votre application et lorsque quelque chose déraille vous immédiatement et vous disposez des informations de dépannage utiles pour travailler avec.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acheter ou louer une solution de télémétrie

> [!NOTE]
> Cet article a été écrit avant [Application Insights](https://azure.microsoft.com/services/application-insights/) a été publiée. Application Insights est la meilleure approche pour les solutions de données de télémétrie sur Azure. Consultez [configurer Application Insights pour votre site Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) pour plus d’informations.


L’une des choses qui est génial sur l’environnement de cloud est qu’il est très facile d’acheter ou de louer votre façon victoire. Données de télémétrie sont un exemple. Sans beaucoup d’efforts, vous pouvez obtenir un système de télémétrie très bon jusqu'à et en cours d’exécution, très à moindre coût. Il existe un ensemble de partenaires excellents qui s’intègrent à Azure, et certains d'entre eux ont des niveaux gratuits – afin d’obtenir des données de télémétrie de base pour rien. Voici quelques-unes de ceux actuellement disponibles sur Azure :

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [DynaTrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

Depuis mars 2015, [Microsoft Application Insights pour Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) n’est pas encore disponible, mais est disponible en version préliminaire pour essayer. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) inclut également des fonctionnalités d’analyse.

Nous allons étudier rapidement configurer New Relic pour montrer comment il est facile à utiliser un système de télémétrie.

Dans le portail de gestion Azure, inscrivez-vous pour le service. Cliquez sur **New**, puis cliquez sur **Store**. Le **sélectionner un module complémentaire** boîte de dialogue s’affiche. Faites défiler et cliquez sur **New Relic**.

![Sélectionner un module complémentaire](monitoring-and-telemetry/_static/image1.png)

Cliquez sur la flèche droite, puis choisissez le niveau de service que vous souhaitez. Pour cette démonstration, nous allons utiliser le niveau gratuit.

![Personnaliser le module complémentaire](monitoring-and-telemetry/_static/image2.png)

Cliquez sur la flèche droite, confirmez le « acheter » et New Relic s’affiche maintenant comme module complémentaire dans le portail.

![Revoir l’achat](monitoring-and-telemetry/_static/image3.png)

![Nouveau module complémentaire de Relic dans le portail de gestion](monitoring-and-telemetry/_static/image4.png)

Cliquez sur **informations de connexion**, puis copiez la clé de licence.

![Informations de connexion](monitoring-and-telemetry/_static/image5.png)

Accédez à la **configurer** onglet pour votre application web dans le portail, définissez **analyse des performances** à **module complémentaire**et définissez le **choisir un module complémentaire** liste déroulante à **New Relic**. Puis cliquez sur **enregistrer**.

![New Relic dans l’onglet Configurer](monitoring-and-telemetry/_static/image6.png)

Dans Visual Studio, installez le package NuGet New Relic dans votre application.

![Analytique de développeur dans l’onglet Configurer](monitoring-and-telemetry/_static/image7.png)

Déployer l’application sur Azure et l’utiliser. Créez quelques tâches Fix It pour fournir des activités pour New Relic surveiller.

Puis revenez à la **New Relic** page dans le **modules complémentaires** onglet du portail et cliquez sur **gérer**. Le portail vous envoie au portail de gestion de New Relic, à l’aide de l’authentification unique pour l’authentification afin que vous n’êtes pas obligé de retaper vos informations d’identification. La page Vue d’ensemble présente une série de statistiques de performances. (Cliquez sur l’image pour afficher la taille complète de la page Vue d’ensemble.)

[![Nouvel onglet de surveillance Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Voici quelques-unes des statistiques, que vous pouvez voir :

- Temps de réponse moyen à différents moments de la journée.

    ![Temps de réponse](monitoring-and-telemetry/_static/image10.png)
- Taux de débit (nombre de requêtes par minute) à différents moments de la journée.

    ![Débit](monitoring-and-telemetry/_static/image11.png)
- Temps processeur de serveur consacré à la gestion des différentes requêtes HTTP.

    ![Temps de Transaction Web](monitoring-and-telemetry/_static/image12.png)
- Temps processeur passé dans les différentes parties du code d’application :

    ![Détails de la trace](monitoring-and-telemetry/_static/image13.png)
- Statistiques de performances historiques.

    ![Historique des performances](monitoring-and-telemetry/_static/image14.png)
- Appels aux services externes tels que le service Blob et les statistiques sur la manière fiable et plus réactive le service a.

    ![Services externes](monitoring-and-telemetry/_static/image15.png)

    ![Services externes](monitoring-and-telemetry/_static/image16.png)

    ![Service externe](monitoring-and-telemetry/_static/image17.png)
- Informations sur l’emplacement dans le monde ou dans l’application web des États-Unis trafic d'où proviennent.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Vous pouvez également configurer les rapports et événements. Par exemple, vous pouvez par exemple à tout moment vous commencer à voir des erreurs, envoyez un e-mail à l’équipe du support technique alerte le problème.

![Rapports](monitoring-and-telemetry/_static/image19.png)

New Relic est qu’un exemple d’un système de télémétrie ; Vous pouvez obtenir tout cela à partir d’autres services. La beauté du cloud qui est sans avoir à écrire du code et, pour un minimum ou sans frais, vous soudainement pouvez obtenir beaucoup plus d’informations sur l’utilisation de votre application et ce que vos clients rencontrez effectivement.

<a id="log"></a>
## <a name="log-for-insight"></a>Journal pour obtenir des informations

Un package de télémétrie est une première étape, mais vous devez toujours instrumenter votre propre code. Le service de télémétrie vous indique quand il existe un problème et vous indique ce que les utilisateurs sont confrontés, mais il peut ne pas vous donne un grand nombre de compréhension sur ce qui se passe dans votre code.

Vous ne souhaitez pas avoir à distance à un serveur de production pour voir ce que fait votre application. Cela peut être pratique lorsque vous avez un seul serveur, mais qu’en est-il lorsque vous avez mis à l’échelle à des centaines de serveurs et que vous ne connaissez pas celles qui vous avez besoin à distance à ? La journalisation doit fournir suffisamment d’informations que vous ne devez jamais à distance à des serveurs de production pour analyser et déboguer des problèmes. Que vous devez consigner suffisamment d’informations afin que vous pouvez isoler les problèmes uniquement via les journaux.

### <a name="log-in-production"></a>Ouvrez une session en production

De nombreuses personnes activer le suivi en production uniquement lorsqu’il existe un problème et à déboguer. Cela peut introduire un délai important entre l’heure que vous êtes conscient d’un problème et le temps de qu'obtenir des informations de dépannage utiles à ce sujet. Et les informations que vous obtenez ne peuvent pas être utiles pour les erreurs intermittentes.

Ce que nous vous recommandons de l’environnement cloud où le stockage est bon marché est toujours laisser une session en production. De cette façon, lorsque des erreurs se produisent, vous en avez pas connecté et que vous avez des données historiques qui peuvent aider à vous analysez les problèmes de développement dans le temps ou se produisent régulièrement à des moments différents. Vous pouvez automatiser un processus de purge pour supprimer les anciens journaux, mais vous constaterez peut-être qu’il est plus onéreux configurer un tel processus qu’il doit conserver les journaux.

La dépense supplémentaire de journalisation est triviale par rapport à la quantité de résolution des problèmes de temps et argent que vous pouvez enregistrer en présentant toutes les informations que vous devez déjà disponible lorsqu’une erreur survient. Puis quand quelqu'un vous dit qu’ils comportait une erreur aléatoire plus tard environ 8:00 Hier soir, mais ils ne vous souvenez pas de l’erreur, vous trouverez facilement quel était le problème.

Pour moins de 4 $ un mois, vous pouvez conserver quant à 50 Go de journaux et l’impact sur les performances de journalisation est trivial tant que vous gardez l’esprit--afin d’éviter les goulots d’étranglement de performances, assurez-vous que votre bibliothèque de journalisation est asynchrone.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Différencier des journaux qui informent des journaux qui nécessitent une action

Journaux sont destinés à notifier (je veux que vous sachiez quelque chose) ou ACT (vous allez devoir faire quelque chose). Veillez à écrire uniquement des journaux d’ACT pour les problèmes qui nécessitent réellement une personne ou un processus automatisé pour prendre des mesures. Trop de journaux ACT créera le bruit, nécessiter trop de travail à passer en revue tout cela pour rechercher les problèmes authentiques. Et si vos journaux ACT déclenche automatiquement des actions telles que l’envoi de courrier électronique au personnel de support, évitez de laisser des milliers de telles actions déclenchées par un seul problème.

Dans le suivi .NET System.Diagnostics, journaux peuvent être affectés au niveau erreur, avertissement, Info et Debug/Verbose. Vous pouvez différencier ACT à partir des journaux de notifier en réservant le niveau d’erreur pour les journaux de ACT et pour les journaux de notifier à l’aide de niveaux inférieurs.

![Niveaux de journalisation](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurer les niveaux de journalisation au moment de l’exécution

S’il est donc utile de disposer d’une session toujours en production, une autre pratique recommandée consiste à implémenter une infrastructure de journalisation qui vous permet d’ajuster au moment de l’exécution le niveau de détail que vous êtes connecté, sans avoir à redéployer ou le redémarrage de votre application. Par exemple, lorsque vous utilisez la fonctionnalité de traçage dans `System.Diagnostics` vous pouvez créer d’erreur, avertissement, informations et journaux de débogage/Verbose. Nous vous recommandons de vous vous connectez toujours Error, Warning, Info se connecte en production, et vous souhaitez être en mesure d’ajouter dynamiquement la journalisation de débogage/Verbose pour la résolution des problèmes sur un cas par cas.

Applications Web dans Azure App Service ont une prise en charge intégrée pour l’écriture `System.Diagnostics` journaux vers le système de fichiers, le stockage de Table ou le stockage d’objets Blob. Vous pouvez sélectionner différents niveaux de journalisation pour chaque destination de stockage, et vous pouvez modifier le niveau de journalisation à la volée sans avoir à redémarrer votre application. La prise en charge du stockage Blob rend plus facile d’exécuter [HDInsight](https://docs.microsoft.com/azure/hdinsight/) travaux d’analyse sur les journaux des applications, car HDInsight sait comment travailler directement avec le stockage d’objets Blob.

### <a name="log-exceptions"></a>Enregistrer des Exceptions

Ne placez pas simplement *exception. ToString()* dans votre code de journalisation. Cela laisse des informations contextuelles. Dans le cas d’erreurs SQL, il omet le numéro d’erreur SQL. Pour toutes les exceptions, inclure des informations de contexte, l’exception lui-même et les exceptions internes pour vous assurer que vous fournissez tous les éléments qui seront nécessaires pour le dépannage. Par exemple, les informations de contexte peuvent inclure le nom du serveur, un identificateur de transaction et un nom d’utilisateur (mais pas le mot de passe ou les clés secrètes !).

Si vous comptez sur chaque développeur à adoptent l’attitude avec l’exception de journalisation, certains d'entre eux ne. Pour veiller à ce qu’il obtient le droit de façon à chaque fois, générer directement dans votre interface de l’enregistreur d’événements de gestion des exceptions : passez l’objet d’exception à la classe d’enregistreur d’événements et connecter les données d’exception correctement dans la classe de journalisation.

### <a name="log-calls-to-services"></a>Journal des appels aux services

Nous recommandons vivement d’écrire un journal chaque fois que votre application appelle un service, qu’il s’agisse d’une base de données ou une API REST ou un service externe. Inclure dans vos journaux non seulement une indication de réussite ou échec, mais la durée de chaque demande. Dans l’environnement de cloud vous verrez souvent des problèmes liés à des ralentissements plutôt que des pannes complètes. Quelque chose qui prend généralement 10 millisecondes peut soudainement commencez à prendre une seconde. Lorsque quelqu'un vous indique que votre application est lente, vous souhaitez être en mesure de rechercher sur New Relic ou n’importe quel service de télémétrie, vous avez et validez leur expérience et que vous souhaitez être en mesure de rechercher sont vos propres journaux de plonger dans les détails de la raison pour laquelle elle est lente.

### <a name="use-an-ilogger-interface"></a>Utiliser une interface ILogger

Ce que ceci est recommandé lorsque vous créez une application de production consiste à créer une simple *ILogger* interface et de respecter certaines méthodes qu’il contient. Cela rend plus facile de changer l’implémentation de journalisation par la suite et pas parcourir tout votre code pour cela. Nous aurions pu utiliser le `System.Diagnostics.Trace` classe tout au long de l’application Fix It, mais au lieu de cela nous allons l’utiliser en arrière-plan dans une classe de journalisation qui implémente *ILogger*, et nous nous assurons *ILogger* méthode appelle tout au long l’application.

De cette façon, si vous voulez enrichir la journalisation, vous pouvez remplacer [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) avec tout mécanisme de journalisation souhaité. Par exemple, que votre application grandit vous pouvez décider que vous souhaitez utiliser un package de journalisation plus complète comme [NLog](http://nlog-project.org/) ou [bloc applicatif de journalisation de bibliothèque Enterprise](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) est un autre framework de journalisation populaires mais il ne fait pas la journalisation asynchrone.)

Une des raisons possibles à l’aide d’une infrastructure telle que NLog sont de faciliter la division de la sortie de journalisation dans les magasins de données distincts de gros volumes de grande valeur. Qui vous aide à stocker efficacement d’importants volumes de données de notifier que vous n’avez pas besoin de l’exécuter rapidement des requêtes, tout en conservant un accès rapide aux données de ACT.

### <a name="semantic-logging"></a>Journalisation sémantique

Pour un moyen relativement nouveau effectuer une journalisation qui peut produire des informations de diagnostic plus utiles, consultez [Enterprise Library journalisation sémantique Application bloc (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). PRÉPARE utilise [suivi d’événements pour Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) et [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) prennent en charge dans .NET 4.5 pour vous permettre de créer des journaux plus structurées et peut être interrogés. Vous définissez une méthode différente pour chaque type d’événement que vous vous connectez, ce qui vous permet de personnaliser les informations que vous écrivez. Par exemple, pour se connecter à une erreur de base de données SQL que vous pouvez appeler un `LogSQLDatabaseError` (méthode). Pour ce type d’exception, vous connaissez qu'une information clé étant le numéro d’erreur, vous pouvez inclure un paramètre de numéro d’erreur dans la signature de méthode et enregistrer le numéro d’erreur comme un champ distinct dans l’enregistrement du journal que vous écrivez. Étant donné que le nombre est dans un champ distinct vous pouvez plus facilement et de façon fiable obtenir des rapports basés sur les numéros d’erreur SQL que vous pourriez le faire si vous ont été simplement concaténant le numéro d’erreur dans une chaîne de message.

## <a name="logging-in-the-fix-it-app"></a>Journalisation dans le correctif il application

### <a name="the-ilogger-interface"></a>L’interface ILogger

Voici le *ILogger* interface dans l’application Fix It.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Ces méthodes permettent d’écrire des journaux de quatre niveaux pris en charge par *System.Diagnostics*. Les méthodes TraceApi sont pour la journalisation des appels de service externe avec des informations sur la latence. Vous pouvez également ajouter un ensemble de méthodes pour le niveau de débogage/Verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>L’implémentation de l’enregistreur d’événements de l’interface ILogger

L’implémentation de l’interface est très simple. Il en fait simplement appelle à la norme *System.Diagnostics* méthodes. L’extrait suivant montre les trois méthodes d’informations et un pour chacun des autres.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Appel des méthodes ILogger

Chaque fois que le code dans l’application Fix It intercepte une exception, il appelle un *ILogger* méthode pour journaliser les détails de l’exception. Et chaque fois qu’elle effectue un appel à la base de données, service Blob ou une API REST, il démarre un chronomètre avant l’appel, arrête le chronomètre lorsque le service est retournée et enregistre le temps écoulé, ainsi que des informations sur la réussite ou l’échec.

Notez que le message du journal inclut le nom de classe et le nom de la méthode. Il est conseillé de vous assurer que les messages de journal identifient quelle partie du code d’application de rédaction.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

À présent pour chaque fois que l’application Fix It a émis un appel à la base de données SQL, vous pouvez voir l’appel, la méthode qui l’a appelée, et exactement combien de temps il a fallu.

![Requête de base de données SQL dans les journaux](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si vous allez parcourir les journaux, vous pouvez voir que la durée des appels de base de données est variable. Que les informations peut être utiles : étant donné que l’application enregistre tout cela, vous pouvez analyser les tendances historiques dans comment le service de base de données s’effectue au fil du temps. Par exemple, un service peut être rapide la plupart du temps mais requêtes risquent d’échouer réponses peut être ralentie ou à certains moments de la journée.

Vous pouvez faire la même chose pour le service Blob – pour chaque fois que l’application télécharge un nouveau fichier, il existe un journal, et vous pouvez voir exactement comment le temps nécessaire pour charger chaque fichier.

![Journal de chargement d’objet BLOB](monitoring-and-telemetry/_static/image23.png)

Il s’agit de quelques lignes de code à écrire chaque fois que vous appelez un service, et maintenant chaque fois qu’une personne dit qu'a rencontré un problème, vous savez exactement quel était le problème, une erreur, ou même si elle a été simplement lente. Vous pouvez identifier la source du problème sans avoir à distance à un serveur ou activer la journalisation après l’erreur se produit et espère pouvoir recréer.

## <a name="dependency-injection-in-the-fix-it-app"></a>L’Injection de dépendances dans le correctif il application

Vous vous demandez peut-être comment le constructeur de référentiel dans l’exemple ci-dessus Obtient l’enregistreur d’événements implémentation d’interface :

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Pour associer l’interface à l’implémentation de l’application utilise [l’injection de dépendances](http://en.wikipedia.org/wiki/Dependency_injection)(DI) avec [AutoFac](http://autofac.org/). L’injection de dépendance vous permet à utiliser un objet basé sur une interface à différents endroits de votre code. il suffit de spécifier au même endroit de l’implémentation qui est utilisée lorsque l’interface est instancié. Cela rend plus facile de modifier l’implémentation : par exemple, vous souhaiterez peut-être remplacer l’enregistreur d’événements System.Diagnostics avec un enregistreur d’événements NLog. Ou, pour les tests automatisés, vous souhaiterez remplacer par une version fictive de l’enregistreur d’événements.

L’application Fix It utilise l’injection de dépendances dans tous les référentiels et tous les contrôleurs. Les constructeurs des classes de contrôleur obtenir un *ITaskRepository* la même façon que le référentiel Obtient une interface de l’enregistreur d’événements de l’interface :

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L’application utilise la bibliothèque de l’injection de dépendances AutoFac pour fournir automatiquement *TaskRepository* et *enregistreur d’événements* instances pour ces constructeurs.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Ce code indique globalement que n’importe où un constructeur a besoin un *ILogger* l’interface, de passer une instance de la *enregistreur d’événements* (classe), et chaque fois qu’il a besoin un *IFixItTaskRepository*l’interface, de passer une instance de la *FixItTaskRepository* classe.

[AutoFac](http://autofac.org/) est une des nombreuses infrastructures d’injection de dépendance que vous pouvez utiliser. Un autre populaires est [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), qui est recommandé et pris en charge par Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Prise en charge de la journalisation intégrée dans Azure

Azure prend en charge les types suivants de [journalisation pour les applications Web dans Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Suivi System.Diagnostics (vous pouvez activer et désactiver et définir des niveaux à la volée sans redémarrage du site).
- Événements de Windows.
- Journaux IIS (HTTP/FREB).

Azure prend en charge les types suivants de [journalisation dans les Services de Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Suivi de System.Diagnostics.
- Compteurs de performance.
- Événements de Windows.
- Journaux IIS (HTTP/FREB).
- Analyse du répertoire personnalisé.

L’application Fix It utilise le suivi de System.Diagnostics. Il vous suffit de faire pour activer la journalisation dans une application web de System.Diagnostics est de retourner un commutateur dans le portail ou d’appeler l’API REST. Dans le portail, cliquez sur le **Configuration** onglet pour votre site et faites défiler pour voir le **Application Diagnostics** section. Vous pouvez activer ou désactiver la journalisation et sélectionnez le niveau de journalisation souhaité. Vous pouvez avoir à Azure d’écrire les journaux dans le système de fichiers ou à un compte de stockage.

![Diagnostics d’application et les diagnostics de site dans l’onglet Configurer](monitoring-and-telemetry/_static/image24.png)

Après avoir activé la journalisation dans Azure, vous pouvez voir les journaux dans la fenêtre Sortie de Visual Studio lors de leur création.

![Menu de journaux de diffusion en continu](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu de journaux de diffusion en continu](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Vous pouvez également avoir des journaux écrits dans votre compte de stockage et affichez les avec n’importe quel outil qui peut accéder au service de la Table de stockage Azure, tel que **Explorateur de serveurs** dans Visual Studio ou [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Journaux dans l’Explorateur de serveurs](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Récapitulatif

C’est très simple à implémenter un système de télémétrie d’out-of-the-box, instrumenter journalisation dans votre propre code et configurer la journalisation dans Azure. Et si vous rencontrez des problèmes de production, la combinaison d’un système de télémétrie et de journaux personnalisés vous aideront à résoudre rapidement les problèmes avant qu’ils deviennent des problèmes majeurs pour vos clients.

Dans le [chapitre suivant](transient-fault-handling.md) nous allons étudier comment gérer les erreurs temporaires de sorte qu’ils ne deviennent des problèmes de production que vous devez examiner.

## <a name="resources"></a>Ressources

Pour plus d'informations, voir les ressources ci-dessous.

Documentation principalement sur les données de télémétrie :

- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez les conseils d’Instrumentation et les données de télémétrie, des conseils de Service de contrôle, surveillance des points de terminaison de contrôle d’intégrité et Reconfiguration du Runtime modèle.
- [Minime pincement dans le Cloud : l’activation de surveillance sur les sites Web Azure à des performances de Relic nouvelle](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Meilleures pratiques pour la création de Services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc par Mark Simms et Michael Thomassy. Consultez la section données de télémétrie et Diagnostics.
- [Développement de nouvelle génération avec Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Article de MSDN Magazine.

Documentation principalement sur la journalisation :

- [Bloc d’Application de journalisation sémantique (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie présente le cas pour la journalisation sémantique avec prépare.
- [Création de journaux structurées et explicites avec journalisation sémantique](https://channel9.msdn.com/Events/Build/2013/3-336). (Vidéo) Julian Dominguez présente le cas pour la journalisation sémantique avec prépare.
- [EF6 Enregistrement SQL – partie 1 : journalisation Simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers montre comment enregistrer des requêtes exécutées par Entity Framework dans EF 6.
- [Résilience des connexions et Interception des commandes avec Entity Framework dans une Application ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quatrième dans une série de didacticiels neuf parties, montre comment utiliser la fonctionnalité de l’interception de commande EF 6 pour enregistrer les commandes SQL envoyées à la base de données par Entity Framework.
- [Améliorer la journalisation à l’aide des attributs d’informations appelant 5.0 c#](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Comment se connecter facilement le nom de la méthode d’appel sans coder en dur dans les littéraux ou à l’aide de la réflexion pour obtenir de manuellement.

Documentation principalement sur la résolution des problèmes :

- [Dépannage d’Azure &amp; débogage blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools : l’utilitaire de Diagnostic utilisé par l’équipe de Support du développeur Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Présente et fournit un lien de téléchargement pour un outil qui peut être utilisé sur une machine virtuelle Azure pour télécharger et exécuter un large éventail d’outils de diagnostics et d’analyse. Utile lorsque vous avez besoin pour diagnostiquer un problème sur une machine virtuelle particulière.
- [Dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un didacticiel pas à pas pour bien démarrer avec le suivi System.Diagnostics et débogage à distance.

Vidéos :

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de neuf en parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels. Épisodes 4 et 9 concernent la surveillance et télémétrie. Épisode 9 inclut une vue d’ensemble de la surveillance des services MetricsHub, AppDynamics, New Relic et PagerDuty.
- [Construction Big : Leçons apprises de clients Azure - partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parle de la conception de l’échec et l’instrumentation de tous les éléments. Semblable à la série de prévention de défaillance, mais entrera en procédures plus de détails.

Exemple de code :

- [Cloud Service Fundamentals dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application créé par l’équipe de conseil clientèle Microsoft Azure. Montre les données de télémétrie et les méthodes de journalisation, comme expliqué dans les articles suivants. L’exemple implémente la journalisation de l’application à l’aide de [NLog](http://nlog-project.org/). Pour obtenir une documentation connexe, consultez le [série de quatre articles du wiki TechNet sur les données de télémétrie et de journalisation](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Précédent](design-to-survive-failures.md)
> [Suivant](transient-fault-handling.md)
