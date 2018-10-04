---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Des erreurs temporaires gestion (création d’applications de Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 5c9a408acb074a31aeb347ad8e33ba5319530563
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577273"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Erreurs temporaires gestion (création d’applications de Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Lorsque vous créez une application de cloud du monde réel, l’une des choses que vous devez penser consiste à gérer les interruptions de service temporaire. Ce problème est important identifie de façon unique dans les applications cloud, car vous êtes dépendent des connexions réseau et les services externes. Vous trouverez souvent des problèmes peu généralement auto-adaptation, et si vous n’êtes pas prêt à les gérer intelligemment, ils amèneront entraîne une mauvaise expérience pour vos clients.

## <a name="causes-of-transient-failures"></a>Causes des défaillances temporaires

Vous trouverez dans l’environnement de cloud qui a échoué et les connexions se produisent régulièrement de base de données supprimée. C’est en partie parce que vous allez via les équilibreurs de charge plus par rapport à l’environnement local dans lequel votre serveur web et le serveur de base de données ont une connexion physique directe. En outre, parfois, lorsque vous êtes dépendant d’un service mutualisé, vous verrez les appels au service get plus lent ou délai d’attente, car une autre personne qui utilise le service il sollicite fortement. Dans d’autres cas, vous pouvez être l’utilisateur qui a atteint le service trop fréquemment, et le service limite délibérément vous – refuse les connexions – pour vous éviter de nuire aux autres clients du service.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Utiliser la logique de nouvelle tentative/temporisation intelligente pour atténuer l’effet des défaillances temporaires

Au lieu de lever une exception et affichage d’une page non disponible ou une erreur à votre client, vous pouvez reconnaître les erreurs sont généralement transitoires, et automatiquement réessayer l’opération qui a provoqué l’erreur, qui espère avant de la durée pendant laquelle vous allez être réussie. La plupart du temps que l’opération réussira à la deuxième tentative, et vous allez récupérer à partir de l’erreur sans le client ayant été prenant en charge qu’un problème est survenu.

Il existe plusieurs façons vous pouvez implémenter la logique de nouvelle tentative actives.

- Le Microsoft Patterns &amp; pratiques groupe a un [bloc applicatif de pannes gestion](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) qui fait tout pour vous si vous utilisez ADO.NET pour l’accès de base de données SQL (et non via Entity Framework). Vous venez de définir une stratégie pour les nouvelles tentatives : combien de fois pour retenter une requête ou commande le délai d’attente entre les tentatives – et de type wrap votre SQL du code dans un *à l’aide de* bloc.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH prend également en charge [Azure In-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) et [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Lorsque vous utilisez Entity Framework vous généralement ne sont pas travailler directement avec les connexions SQL, afin que vous ne pouvez pas utiliser ce package de modèles et pratiques, mais Entity Framework 6 génère ce type de logique de nouvelle tentative dans le framework. De la même façon, vous spécifiez la stratégie de nouvelle tentative, et puis EF utilise cette stratégie lorsqu’il accède à la base de données.

    Pour utiliser cette fonctionnalité dans l’application Fix It, tout ce que nous devons faire est d’ajouter une classe qui dérive de *DbConfiguration* et activer la logique de nouvelle tentative.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Pour les exceptions de base de données SQL qui le framework identifie comme des erreurs temporaires en général, le code affiché indique à EF de réessayer l’opération jusqu'à 3 fois, avec un délai de temporisation exponentielle entre les nouvelles tentatives et un délai maximal de 5 secondes. Temporisation exponentielle signifie que, après chaque tentative ayant échoué, il attendra pendant plus de temps avant de réessayer. Si trois tentatives dans une ligne échouent, il lève une exception. La section suivante sur les disjoncteurs explique la raison pour laquelle vous souhaitez temporisation exponentielle et un nombre limité de nouvelles tentatives.

    Vous pouvez avoir des problèmes similaires lorsque vous utilisez le service de stockage Azure, que l’application Fix It pour les objets BLOB, et l’API de client de stockage .NET implémente déjà le même genre de logique. Vous spécifiez simplement la stratégie de nouvelle tentative, ou vous n’êtes pas obligé même pas si vous êtes satisfait des paramètres par défaut.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Coupe-circuits

Il existe plusieurs raisons pourquoi vous ne souhaitez pas réessayer trop de fois sur une période trop longue :

- Nouvelle tentative de manière permanente des demandes ayant échoués trop d’utilisateurs peut dégrader l’expérience des autres utilisateurs. Si des millions de personnes sont tous rendre répétées demandes de nouvelle tentative vous pourriez monopolisent les files d’attente de distribution IIS et empêcher votre application traite les demandes qu’il sinon pu gérer avec succès.
- Si tout le monde est une nouvelle tentative en raison d’un échec du service, il peuvent être tellement demandes en file d’attente que le service obtient alors submergé quand elle commence à récupérer.
- Si l’erreur est due à la limitation et qu’une fenêtre de temps que le service utilise pour la limitation, tentatives continues peuvent déplacer cette fenêtre et entraîner la limitation continuer.
- Vous pouvez avoir un utilisateur en attente pour une page web à restituer. Fabrication personnes attente trop long peut être ennuyeux de plus que relativement rapidement leur conseillant d’essayer ultérieurement.

Temporisation exponentielle résout certains de ces problèmes en limitant la fréquence des tentatives de qu'un service peut obtenir à partir de votre application. Mais vous devez également disposer *disjoncteurs*: cela signifie qu’à un certain de nouvelle tentative seuil de votre application s’arrête une nouvelle tentative et accepte d’autres actions, telles qu’une des opérations suivantes :

- Personnalisé de secours. Si vous ne pouvez pas obtenir une cotation à partir de Reuters, peut-être que vous pouvez l’obtenir à partir de Bloomberg ; ou, si vous ne pouvez pas obtenir des données à partir de la base de données, peut-être que vous pouvez l’obtenir à partir du cache.
- Échec en mode silencieux. Si ce dont vous avez besoin à partir d’un service n’est pas tout ou rien pour votre application, simplement retourner null lorsque vous ne pouvez pas obtenir les données. Si vous affichez une tâche Fix It et que le service Blob ne répond pas, vous pouvez afficher les détails de la tâche sans l’image.
- Échouer rapidement. Erreur de l’utilisateur pour éviter d’inonder le service avec une nouvelle demandes susceptible de provoquer une interruption de service pour d’autres utilisateurs ou d’étendre une fenêtre de limitation. Vous pouvez afficher un message convivial « réessayez plus tard ».

Il n’existe aucune stratégie universelle de nouvelle tentative. Vous pouvez réessayer plusieurs fois et attendre plus longtemps dans un processus de travail asynchrone en arrière-plan que vous le feriez dans une application web synchrone où un utilisateur est en attente d’une réponse. Vous pouvez attendre de plus entre les nouvelles tentatives pour un service de base de données relationnelle que vous le feriez pour un service de cache. Voici des exemples recommandé de réessayer des stratégies à vous donner une idée de la façon dont les nombres peuvent varier. (Aucun délai avant la première nouvelle tentative signifie « Première rapide ».

![Exemples de stratégies de nouvelle tentative](transient-fault-handling/_static/image1.png)

Pour des conseils de stratégie de nouvelle tentative de la base de données SQL, consultez [résoudre les erreurs transitoires et connexion à SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Récapitulatif

Une nouvelle tentative/stratégie de temporisation peut aider à rendre des erreurs temporaires invisible au client la plupart du temps, et Microsoft fournit des infrastructures que vous pouvez utiliser pour réduire votre travail implémentation d’une stratégie si vous utilisez ADO.NET, Entity Framework ou Azure Service de stockage.

Dans le [chapitre suivant](distributed-caching.md), nous allons étudier comment améliorer les performances et fiabilité à l’aide de la mise en cache distribuée.

## <a name="resources"></a>Ressources

Pour plus d'informations, reportez-vous aux ressources suivantes :

Documentation

- [Meilleures pratiques pour la création de Services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc par Mark Simms et Michael Thomassy. Semblable à la série de prévention de défaillance, mais entrera en procédures plus de détails. Consultez la section données de télémétrie et Diagnostics.
- [FailSafe : Guidance for Architectures résilientes du Cloud recommandations](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc par Marc Mercuri, Ulrich Homann et Andrew Townhill. Version de la page Web de la série de vidéos de prévention de défaillance.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez la nouvelle tentative modèle, un modèle de superviseur de l’Agent du planificateur.
- [Tolérance de panne dans la base de données SQL Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Billet de blog de Tony Petrossian.
- [Entity Framework - résilience des connexions / logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835). Comment utiliser et personnaliser la gestion des fonctionnalités d’Entity Framework 6 d’erreurs temporaires.
- [Résilience des connexions et Interception des commandes avec Entity Framework dans une Application ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quatrième dans une série de didacticiels neuf parties, montre comment configurer la fonctionnalité de résilience de connexion EF 6 pour SQL Database.

Vidéos

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de neuf en parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels. Consultez la section traitant des disjoncteurs en épisode 3 en commençant à 40:55.
- [Construction Big : Leçons apprises de clients Azure - partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms parle de conception pour les défaillances, temporaire de l’erreur gestion et l’instrumentation de tous les éléments.

Exemple de code

- [Cloud Service Fundamentals dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application créée par le Microsoft Azure Customer Advisory Team qui montre comment utiliser le [Enterprise Library temporaires bloc gestion des erreurs](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Pour plus d’informations, consultez [couche Cloud Service Fundamentals Data Access – gestion des erreurs temporaires](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH est recommandé pour l’accès de base de données à l’aide d’ADO.NET directement (sans utiliser Entity Framework).

> [!div class="step-by-step"]
> [Précédent](monitoring-and-telemetry.md)
> [Suivant](distributed-caching.md)
