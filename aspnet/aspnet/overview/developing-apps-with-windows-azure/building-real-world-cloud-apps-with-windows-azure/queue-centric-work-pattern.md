---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Modèle de travail centré à la file d’attente (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: f9916e4ecbe6234ee12bcb56519e7e2c0e490972
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840186"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Modèle de travail centré à la file d’attente (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Précédemment, nous avons vu qu’à l’aide de plusieurs services peut entraîner un contrat SLA de « composite », où le contrat SLA effective de l’application est la *produit* des contrats SLA individuels. Par exemple, l’application Fix It utilise des Sites Web, de stockage et de base de données SQL. Si l’un de ces services échoue, l’application renvoie une erreur à l’utilisateur.

La mise en cache est un bon moyen de gérer les défaillances temporaires pour le contenu en lecture seule. Mais que se passe-t-il si votre application a besoin effectuer le travail ? Par exemple, lorsque l’utilisateur soumet une nouvelle tâche Fix It, l’application ne peut pas mettre simplement la tâche dans le cache. L’application doit écrire la tâche Fix It dans un magasin de données persistantes, elles peuvent être traitées.

C’est là qu’intervient le modèle centré sur la file d’attente de travail. Ce modèle permet à couplage lâche entre un niveau web et un service principal.

Voici comment le modèle fonctionne. Lorsque l’application reçoit une demande, il place un élément de travail dans une file d’attente et retourne immédiatement la réponse. Ensuite, un processus principal distinct extrait des éléments de travail à partir de la file d’attente et effectue le travail.

Le modèle de travail centré à la file d’attente est utile pour :

- Travail existe beaucoup de temps (une latence élevée).
- Travail qui nécessite un service externe qui n’est pas toujours disponible.
- Le travail gourmandes en ressources (processeur élevé).
- Travail qui pourraient tirer parti de taux d’audit (en fonction des pics soudains charge).

## <a name="reduced-latency"></a>Latence réduite

Files d’attente sont utiles à tout moment que travailler du temps. Si une tâche prend quelques secondes ou plus, au lieu de bloquer l’utilisateur final, placez l’élément de travail dans une file d’attente. Indiquer à l’utilisateur « Nous y travaillons » et ensuite utiliser un écouteur de file d’attente pour traiter la tâche en arrière-plan.

Par exemple, lorsque vous achetez un objet à un détaillant en ligne, le site web confirme votre commande immédiatement. Mais cela ne signifie pas que parlez est déjà dans un camion remis. Ils placer une tâche dans une file d’attente et en arrière-plan qu’ils sont la vérification de crédit, préparation de vos éléments pour l’expédition et ainsi de suite.

Pour les scénarios avec une latence courte, la durée totale de bout en bout peut être plus longue à l’aide d’une file d’attente, par rapport à effectuer la tâche de façon synchrone. Mais même dans ce cas, les autres avantages peuvent annuler cet inconvénient.

## <a name="increased-reliability"></a>Fiabilité accrue

Dans la version de Fix It que nous avons étudié jusqu’ici, le serveur web frontal est étroitement lié à la base de données SQL principale. Si le service de base de données SQL n’est pas disponible, l’utilisateur reçoit une erreur. Si de nouvelles tentatives ne fonctionnent pas (autrement dit, l’échec est plus de temporaire), la seule chose que vous pouvez faire est affiche une erreur et de demander à l’utilisateur de réessayer ultérieurement.

![Diagramme montrant les serveurs web frontaux échec lorsque la base de données SQL principale a échoué](queue-centric-work-pattern/_static/image1.png)

À l’aide de files d’attente, lorsqu’un utilisateur soumet une tâche Fix It, l’application écrit un message à la file d’attente. La charge utile de message est un [JSON](http://json.org/) la représentation sous forme de la tâche. Dès que le message est écrit dans la file d’attente, l’application retourne et immédiatement un message de réussite à l’utilisateur.

Si un des services principaux, tels que la base de données SQL ou de l’écouteur de file d’attente--mis hors connexion, les utilisateurs peuvent néanmoins soumettre de nouvelles tâches Fix It. Les messages simplement file d’attente jusqu'à ce que les services principaux sont à nouveau disponibles. À ce stade, les services principaux pourra rattraper le dans le backlog.

![Diagramme montrant le web frontal continuer à fonctionner lorsqu’il existe une erreur de base de données SQL](queue-centric-work-pattern/_static/image2.png)

En outre, maintenant vous pouvez ajouter plus de logique back-end sans vous soucier de la résilience du serveur frontal. Par exemple, vous souhaiterez peut-être envoyer un e-mail ou un message SMS pour le propriétaire de chaque fois qu’un nouveau Fix It est affecté. Si l’e-mail ou le service SMS devient indisponible, vous puissiez traiter tout le reste et puis de placer un message dans une file d’attente distincte pour l’envoi de messages e-mail/SMS.

Auparavant, notre contrat SLA efficace était Web Apps &times; stockage &times; SQL Database = 99,7 %. (Consultez [conception pour surmonter les défaillances](design-to-survive-failures.md).)

Lorsque nous modifions l’application d’utiliser une file d’attente, le serveur web frontal dépend uniquement des applications Web et le stockage, pour un contrat SLA de 99,8 % composite. (Notez que les files d’attente font partie du service stockage Azure, ils sont inclus dans le même contrat SLA en tant que stockage d’objets blob).

Si vous devez encore mieux à 99,8 %, vous pouvez créer deux files d’attente dans deux régions différentes. Désigner l’une comme le serveur principal et l’autre en tant que la base de données secondaire. Dans votre application, basculer vers la file d’attente secondaire si la file d’attente principale n’est pas disponible. Le risque des deux ne soit pas disponible en même temps est très faible.

## <a name="rate-leveling-and-independent-scaling"></a>Taux d’audit et de mise à l’échelle indépendante

Files d’attente servent également à ce que l'on appelle *taux nivellement* ou *nivellement de charge*.

Applications Web sont souvent sujettes à des pics soudains de trafic. Vous pouvez utiliser la mise à l’échelle pour ajouter automatiquement des serveurs web pour gérer le trafic web accrue, à l’échelle automatique n’est peut-être pas capable de réagir assez rapidement pour gérer les pics soudain de charge. Si les serveurs web peuvent décharger certaines tâches de travail, qu'il faut faire en écrivant un message dans une file d’attente, ils peuvent gérer davantage de trafic. Un service principal peut ensuite lire les messages de la file d’attente et les traiter. La profondeur de la file d’attente s’agrandir ou réduire lors des variations de la charge entrante.

Une grande partie de son travail fastidieux déchargé vers un service principal, la couche web permettre plus facilement répondre aux pics soudains de trafic. Et vous réalisez des économies, car toute quantité donnée de trafic peut être gérée par moins de serveurs web.

Vous pouvez faire évoluer le niveau web et le service principal indépendamment. Par exemple, vous devrez peut-être les trois serveurs web, mais qu’un seul serveur traitement file d’attente des messages. Ou, si vous exécutez une tâche de calcul intensif en arrière-plan, vous devrez peut-être plusieurs serveurs principaux.

![](queue-centric-work-pattern/_static/image3.png)

Mise à l’échelle fonctionne avec les services principaux, ainsi qu’avec la couche web. Vous pouvez augmenter ou réduire le nombre de machines virtuelles qui traitent les tâches dans la file d’attente, en fonction de l’utilisation du processeur des machines virtuelles principales. Ou bien, vous pouvez la mise à l’échelle selon le nombre d’éléments dans une file d’attente. Par exemple, vous pouvez indiquer l’échelle automatique pour tenter de maintenir pas plus de 10 éléments dans la file d’attente. Si la file d’attente a plus de 10 éléments, à l’échelle automatique ajoutera des machines virtuelles. Lorsque qu’ils auront rattrapé, mise à l’échelle sera détruire les machines virtuelles supplémentaires.

## <a name="adding-queues-to-the-fix-it-application"></a>Ajout de files d’attente pour le correctif il Application

Pour implémenter le modèle de file d’attente, nous devons apporter deux modifications à l’application Fix It.

- Lorsqu’un utilisateur soumet une nouvelle tâche Fix It, placez la tâche dans la file d’attente, au lieu de leur écriture dans la base de données.
- Créer un service back-end qui traite les messages dans la file d’attente.

Dans la file d’attente, nous allons utiliser le [Service de stockage de file d’attente Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Une autre option consiste à utiliser [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Pour déterminer quel service de file d’attente à utiliser, tenez compte des comment votre application a besoin envoyer et recevoir les messages dans la file d’attente :

- Si vous avez coopératifs producteurs et consommateurs concurrents, envisagez d’utiliser le Service de stockage de file d’attente Azure. « Producteurs coopérant » signifie que plusieurs processus sont Ajout de messages à une file d’attente. « Consommateurs concurrents » signifie que plusieurs processus extraient des messages de la file d’attente pour les traiter, mais un message donné peut uniquement être traité par un « consommateur ». Si vous avez besoin de davantage de débit que vous pouvez obtenir avec une seule file d’attente, utilisez des files d’attente supplémentaires et/ou des comptes de stockage supplémentaires.
- Si vous avez besoin d’un [modèle publication/abonnement](http://en.wikipedia.org/wiki/Publish/subscribe), envisagez d’utiliser des files d’attente de Bus de Service Azure.

L’application Fix It correspond à la coopération de producteurs et un modèle de consommateurs concurrents.

Une autre considération est la disponibilité des applications. Le Service de stockage de file d’attente fait partie du même service que nous utilisons pour le stockage blob, donc l’utiliser n’a aucun effet sur notre contrat SLA. Azure Service Bus est un service distinct avec son propre contrat SLA. Si nous utilisons des files d’attente de Bus de Service, nous devrions factoriser dans un pourcentage de SLA supplémentaire, et notre contrat SLA composite serait inférieur. Lorsque vous choisissez un service de file d’attente, assurez-vous que vous comprenez l’impact de votre choix sur la disponibilité de l’application. Pour plus d’informations, consultez le [ressources](#resources) section.

## <a name="creating-queue-messages"></a>Création de Messages de file d’attente

Pour placer une tâche Fix It sur la file d’attente, le serveur web frontal effectue les étapes suivantes :

1. Créer un [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instance. Le `CloudQueueClient` instance est utilisée pour exécuter des demandes sur le Service de file d’attente.
2. Créer la file d’attente, s’il n’existe pas encore.
3. Sérialiser la tâche Fix It.
4. Appelez [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) pour placer le message dans la file d’attente.

Nous allons faire ce travail dans le constructeur et `SendMessageAsync` méthode d’un nouveau `FixItQueueManager` classe.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Ici, nous utilisons le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) bibliothèque pour sérialiser le correctif au format JSON. Vous pouvez utiliser n’importe quelle approche de sérialisation que vous préférez. JSON a l’avantage d’être contrôlable de visu, tout en étant moins détaillés que XML.

Code de qualité production serait ajouter la logique de gestion des erreurs, mettre en pause si la base de données devenue indisponible, gérer plus nette de récupération, créer la file d’attente sur le démarrage de l’application et gérer «[incohérents « messages](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un message incohérent est un message qui ne peut pas être traité pour une raison quelconque. Vous ne voulez des messages incohérents à se trouvent dans la file d’attente, où le rôle de travail en permanence tente de les traiter, échec, essayez à nouveau, échec et ainsi de suite.)

Dans l’application frontale de MVC, nous devons mettre à jour le code qui crée une nouvelle tâche. Au lieu de placer la tâche dans le référentiel, appelez le `SendMessageAsync` méthode illustrée ci-dessus.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Traitement des Messages de file d’attente

Pour traiter les messages dans la file d’attente, nous allons créer un service principal. Le service de serveur principal s’exécutera une boucle infinie qui effectue les étapes suivantes :

1. Obtenez le message suivant à partir de la file d’attente.
2. Désérialiser le message à une tâche Fix It.
3. Écrire la tâche Fix It dans la base de données.

Pour héberger le service principal, nous allons créer un Service Cloud Azure qui contient un *rôle de travail*. Un rôle de travail se compose d’un ou plusieurs machines virtuelles qui peuvent effectuer un traitement de serveur principal. Le code qui s’exécute dans ces machines virtuelles demandent les messages à partir de la file d’attente dès qu’elles deviennent disponibles. Pour chaque message, nous allons désérialiser la charge utile JSON et écrire une instance de l’entité de résoudre il tâche dans la base de données, à l’aide du même référentiel que nous avons utilisé précédemment dans la couche web.

Les étapes suivantes montrent comment ajouter un worker projet de rôle pour une solution qui contient un projet web standard. Ces étapes ont déjà été effectuées dans le projet Fix It que vous pouvez télécharger.

Tout d’abord ajouter un projet de Service Cloud à la solution Visual Studio. Avec le bouton droit de la solution et sélectionnez **ajouter**, puis **nouveau projet**. Dans le volet gauche, développez **Visual C#** et sélectionnez **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Dans le **nouveau Service Cloud Azure** boîte de dialogue, développez le **Visual C#** nœud dans le volet gauche. Sélectionnez **rôle de travail** et cliquez sur l’icône de flèche droite.

![](queue-centric-work-pattern/_static/image6.png)

(Notez que vous pouvez également ajouter un *rôle web*. Nous pourrions exécuter le Fix It frontal dans le même Service Cloud au lieu de son exécution dans un Site Web Azure. Qui présente certains avantages de simplification du coordonner les connexions entre les serveurs frontaux et principaux. Toutefois, pour simplifier cette démonstration, nous allons en conservant le serveur frontal dans une application Web Azure App Service et ne s’exécute le serveur principal dans un Service Cloud.)

Un nom par défaut est assigné au rôle de travail. Pour modifier le nom, pointez la souris sur le rôle de travail dans le volet droit, puis cliquez sur l’icône de crayon.

![](queue-centric-work-pattern/_static/image7.png)

Cliquez sur **OK** pour terminer la boîte de dialogue. Cette opération ajoute deux projets à la solution Visual Studio.

- un projet Azure qui définit le service cloud, y compris les informations de configuration.
- Un projet de rôle de travail qui définit le rôle de travail.

![](queue-centric-work-pattern/_static/image8.png)

Pour plus d’informations, consultez [création d’un projet Azure avec Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dans le rôle de travail, nous interroger la présence de messages en appelant le `ProcessMessageAsync` méthode de la `FixItQueueManager` classe que nous avons vu précédemment.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Le `ProcessMessagesAsync` méthode vérifie s’il existe un message en attente. S’il en existe un, il désérialise le message dans un `FixItTask` entité et enregistre l’entité dans la base de données. Il effectue une itération jusqu'à ce que la file d’attente est vide.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Facturer l’interrogation pour les messages de file d’attente entraîne une petite transaction, ainsi, quand aucun message n’est en attente de traitement, le rôle de travail `RunAsync` méthode attend une seconde avant d’interroger à nouveau en appelant `Task.Delay(1000)`.

Dans un projet web, ajout de code asynchrone peut automatiquement améliorer les performances, car IIS gère un pool de threads limité. Qui n’est pas le cas dans un projet de rôle de travail. Pour améliorer l’évolutivité du rôle worker, vous pouvez écrire du code multithread ou utiliser du code asynchrone pour implémenter [programmation parallèle](https://msdn.microsoft.com/library/ff963553.aspx). L’exemple n’implémente pas la programmation parallèle, mais montre comment rendre le code asynchrone afin de pouvoir implémenter la programmation parallèle.

## <a name="summary"></a>Récapitulatif

Dans ce chapitre, vous avez vu comment améliorer l’évolutivité, la fiabilité et la réactivité de l’application en implémentant le modèle de travail centré à la file d’attente.

C’est la dernière des 13 modèles abordés dans ce livre électronique, mais il existe bien sûr bien d’autres modèles et pratiques qui peuvent vous aider à créer des applications de cloud réussie. Le [dernier chapitre](more-patterns-and-guidance.md) fournit des liens vers des ressources pour les rubriques qui n’ont pas été traités dans ces 13 modèles.

<a id="resources"></a>
## <a name="resources"></a>Ressources

Pour plus d’informations sur les files d’attente, consultez les ressources suivantes.

Documentation :

- [Partie de files d’attente de stockage Microsoft Azure 1 : Mise en route](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Article de Roman Schacherl.
- [L’exécution de tâches en arrière-plan](https://msdn.microsoft.com/library/ff803365.aspx), chapitre 5 de [migration des Applications vers le Cloud, 3ème édition](https://msdn.microsoft.com/library/ff728592.aspx) à partir de Microsoft Patterns and Practices. (En particulier, la section [« À l’aide de Azure files d’attente stockage »](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Meilleures pratiques pour optimiser la rentabilité des Solutions de messagerie basée sur la file d’attente sur Azure et l’évolutivité](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Livre blanc par Valery Mizonov.
- [Comparaison des files d’attente Azure et files d’attente de Bus de Service](https://msdn.microsoft.com/magazine/jj159884.aspx). Article de MSDN Magazine, fournit des informations supplémentaires qui peuvent vous aider à choisir le service de file d’attente à utiliser. L’article mentionne que Service Bus dépend ACS pour l’authentification, ce qui signifie que vos files d’attente service bus n’étaient pas disponibles lors de l’ACS n’est pas disponible. Toutefois, étant donné que l’article a été écrit, SB a été modifiée pour pouvoir utiliser [jetons SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) comme alternative à ACS.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez primer de messagerie asynchrone, canaux et filtres de modèle, modèle de Transaction de compensation, modèle de consommateurs concurrents, le modèle CQRS.
- [CQRS Journey](https://msdn.microsoft.com/library/jj554200). E-book sur CQRS par Microsoft Patterns and Practices.

Vidéo :

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de vidéos de neuf parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels. Pour une introduction au service de stockage Azure et files d’attente, consultez l’épisode 5 en commençant à 35:13.

> [!div class="step-by-step"]
> [Précédent](distributed-caching.md)
> [Suivant](more-patterns-and-guidance.md)
