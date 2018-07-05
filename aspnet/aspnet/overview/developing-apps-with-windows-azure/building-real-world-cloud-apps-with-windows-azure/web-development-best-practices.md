---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web des meilleures pratiques de développement (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: f31798032c3b690fcb6dfa8326580255f3412826
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371036"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web Development Best Practices (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Les trois premiers modèles ont été sur la configuration d’un processus de développement agile ; les autres sont sur l’architecture et de code. Celle-ci est une collection de meilleures pratiques de développement web :

- [Serveurs web sans état](#stateless) derrière un équilibreur de charge intelligent.
- [Éviter l’état de session](#sessionstate) (ou si vous ne pouvez pas l’éviter, utilisez le cache distribué plutôt que d’une base de données).
- [Utiliser un CDN](#cdn) aux ressources de fichiers statiques de cache de périmètre (images, scripts).
- [Utiliser la prise en charge asynchrone de .NET 4.5](#async) pour éviter de bloquer les appels.

Ces pratiques sont valides pour tout développement web, pas seulement pour les applications cloud, mais elles sont particulièrement importants pour les applications cloud. Ils fonctionnent ensemble pour vous aider à tirer le meilleur parti de la très flexible mise à l’échelle offertes par l’environnement de cloud. Si vous ne suivez pas ces pratiques, vous allez exécuter celui-ci atteint ses limites lorsque vous tentez de mettre à l’échelle de votre application.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Couche web sans état derrière un équilibreur de charge intelligent

*Couche web sans état* signifie que vous ne stockez pas les données d’application dans le système de fichiers ou de mémoire de serveur web. En conservant la couche web sans état vous permet à offrir une meilleure expérience client et à économiser de l’argent :

- Si la couche web est sans état, et il se trouve derrière un équilibreur de charge, vous pouvez répondre rapidement aux modifications de trafic d’application en ajoutant ou supprimant des serveurs dynamiquement. Dans l’environnement cloud où vous payez uniquement pour les ressources de serveur pour aussi les utiliser, cette capacité à répondre aux modifications de la demande peut se traduire économie énorme.
- Un niveau web sans état est une architecture beaucoup plus simple à l’échelle l’application. Qui trop vous permet de répondre aux besoins de mise à l’échelle plus rapidement et moins investir dans du développement et de test dans le processus.
- Les serveurs de cloud, comme des serveurs locaux, doivent être corrigées et redémarré, occasionnellement ; et si la couche web est sans état, un réacheminement de trafic lorsqu’un serveur est temporairement indisponible ne sont pas provoquer des erreurs ou un comportement inattendu.

La plupart des applications de réels ont besoin stocker l’état d’une session web ; l’essentiel ici est ne pas de les stocker sur le serveur web. Vous pouvez stocker l’état d’autres manières, telles que sur le client dans des cookies ou hors processus côté serveur dans l’état de session ASP.NET à l’aide d’un fournisseur de cache. Vous pouvez stocker les fichiers dans [stockage Blob Windows Azure](unstructured-blob-storage.md) au lieu du système de fichiers local.

Ainsi, combien il est facile de mettre à l’échelle une application dans les Sites Web Windows Azure si la couche web est sans état, consultez le **mise à l’échelle** onglet pour un Site de Web Windows Azure dans le portail de gestion :

![Onglet de mise à l’échelle](web-development-best-practices/_static/image1.png)

Si vous souhaitez ajouter des serveurs web, vous pouvez simplement faire glisser le curseur de nombre d’instance à droite. Affectez-lui la valeur 5, puis cliquez sur **enregistrer**, et en quelques secondes, vous avez 5 serveurs web dans Windows Azure, gestion du trafic de votre site web.

![Cinq instances](web-development-best-practices/_static/image2.png)

Vous pouvez tout aussi facilement définir le nombre d’instances jusqu'à 3 ou à 1. Lorsque vous augmentez la taille précédent, vous démarrez économiser de l’argent immédiatement, car Windows Azure est facturé à la minute, pas par heure.

Vous pouvez également indiquer Windows Azure pour augmenter ou diminuer le nombre de serveurs web basés sur l’utilisation du processeur automatiquement. Dans l’exemple suivant, lors de l’utilisation de l’UC tombe en dessous de 60 %, le nombre de serveurs web vont diminuer au minimum de 2, et si l’utilisation du processeur dépasse 80 %, le nombre de serveurs web va être augmenté jusqu'à un maximum de 4.

![Mettre à l’échelle par l’utilisation du processeur](web-development-best-practices/_static/image3.png)

Ou bien, que se passe-t-il si vous savez que votre site sera uniquement occupé pendant les heures ouvrables ? Vous pouvez indiquer Windows Azure pour exécuter plusieurs serveurs pendant la journée et de réduire à un seul serveur soirs, nuits et week-ends. La série de captures d’écran suivante montre comment configurer le site web pour exécuter un serveur en dehors des heures et 4 serveurs pendant les heures de travail à partir de 8 h 00 à 17 h 00.

![Mettre à l’échelle par planification](web-development-best-practices/_static/image4.png)

![Définir les heures de planification](web-development-best-practices/_static/image5.png)

![Planification de la journée](web-development-best-practices/_static/image6.png)

![Planification de weeknight](web-development-best-practices/_static/image7.png)

![Planification de week-end](web-development-best-practices/_static/image8.png)

Et bien sûr tout cela est possible dans les scripts, ainsi que dans le portail.

La capacité de votre application de monter en charge est presque illimitée dans Windows Azure, tant que vous évitez les obstacles en ajoutant ou supprimant les machines virtuelles de serveur, en conservant la couche web sans état de manière dynamique.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Éviter l’état de session

Il n’est souvent pas pratique dans une application de cloud réalistes pour éviter de stocker une forme d’état de session utilisateur, mais certaines approches affectent les performances et évolutivité plus que d’autres. Si vous avez stocker l’état, la meilleure solution consiste à limiter la taille de la quantité d’état et les stocker dans des cookies. Si ce n’est pas possible, la meilleure solution suivante consiste à utiliser l’état de session ASP.NET avec un fournisseur pour [cache distribué en mémoire](distributed-caching.md#sessionstate). La pire solution à partir d’un point de vue de l’évolutivité et de performances est d’utiliser une base de données fournisseur d’état de session.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Utiliser un CDN pour mettre en cache des fichiers statiques

CDN est un acronyme pour Content Delivery Network. Vous fournissez des ressources de fichiers statiques telles que les images et les fichiers de script à un fournisseur CDN, et le fournisseur met en cache ces fichiers dans des centres de données dans le monde entier afin que chaque fois que les utilisateurs accèdent à votre application, ils obtiennent une réponse relativement rapide et une faible latence pour la mise en cache ressources. Cela accélère le temps de chargement globale du site et réduit la charge sur vos serveurs web. CDN est particulièrement important si vous avez atteint un public qui est largement distribué géographiquement.

Windows Azure a un CDN, et vous pouvez utiliser les autres CDN dans une application qui s’exécute dans Windows Azure ou de n’importe quel environnement d’hébergement web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Prise en charge asynchrone de .NET 4.5 permet d’éviter de bloquer les appels

.NET 4.5 amélioré les langages de programmation c# et VB afin de simplifier au maximum gérer des tâches de façon asynchrone. L’avantage de la programmation asynchrone n’est pas seulement pour le traitement parallèle des situations telles que lorsque vous souhaitez lancer plusieurs appels de service web simultanément. Il permet également de votre serveur web effectuer plus efficacement et fiable dans des conditions de charge élevée. Un serveur web uniquement un nombre limité de threads disponibles, et dans des conditions de charge élevée lorsque tous les threads sont en cours d’utilisation, les demandes entrantes doivent attendre jusqu'à ce que les threads ne sont pas libérées. Si votre code d’application ne gère pas les tâches comme la base de données de requêtes et des appels de service web asynchrone, nombre de threads est inutilement immobilisé pendant que le serveur attend une réponse d’e/s. Cela limite la quantité de trafic que le serveur peut gérer dans des conditions de charge élevée. Avec la programmation asynchrone, les threads qui attendent pour un service web ou d’une base de données pour retourner des données sont libérées jusqu'à nouvelles demandes de service jusqu'à ce que les données de la réception. Dans un serveur web occupé, des centaines voire des milliers de demandes peuvent ensuite être traitées rapidement qui sinon d’attente des threads être libéré.

Comme vous l’avez vu précédemment, il est aussi facile à diminuer le nombre de serveurs web gère votre site web car il s’agit en augmenter la taille. Par conséquent, si un serveur peut obtenir un débit supérieur, vous n’avez pas besoin que plusieurs d'entre eux et vous pouvant réduire vos coûts, car vous avez besoin de moins de serveurs pour un volume de trafic donné que vous le feriez dans le cas contraire.

Prise en charge pour le modèle de programmation asynchrone .NET 4.5 est inclus dans ASP.NET 4.5 pour Web Forms, MVC et API Web ; dans Entity Framework 6 et dans le [API de stockage Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Prise en charge asynchrone dans ASP.NET 4.5

Dans ASP.NET 4.5, prise en charge la programmation asynchrone a été ajoutée, non seulement pour la langue, mais aussi pour les infrastructures d’API Web, Web Forms et MVC. Par exemple, une méthode d’action de contrôleur ASP.NET MVC reçoit des données à partir d’une requête web et transmet les données à une vue qui crée ensuite le code HTML à envoyer au navigateur. Souvent, la méthode d’action doit obtenir des données à partir d’un base de données ou un service web pour l’afficher dans une page web ou d’enregistrer les données entrées dans une page web. Dans ces scénarios, il est facile de rendre la méthode d’action asynchrone : au lieu de retourner un *ActionResult* de l’objet, vous retournez *tâche&lt;ActionResult&gt;*  et marquez la méthode avec le *async* mot clé. À l’intérieur de la méthode, lorsqu’une ligne de code lance une opération qui implique des temps d’attente, vous le marquer avec le mot clé await.

Voici une méthode d’action simple qui appelle une méthode de référentiel pour une requête de base de données :

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Et Voici la même méthode qui gère l’appel de la base de données de façon asynchrone :

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

En coulisses, le compilateur génère le code asynchrone approprié. Lorsque l’application effectue l’appel à `FindTaskByIdAsync`, ASP.NET rend le `FindTask` demande se déroule le thread de travail et met à disposition traiter une autre requête. Lorsque le `FindTask` demande est effectuée, un thread est redémarré pour continuer à traiter le code qui vient après cet appel. Pendant l’intervalle entre le moment où le `FindTask` demande est initiée et lorsque les données sont retournées, vous disposez d’un thread disponible pour effectuer des tâches utiles qui seraient être bloqués dans l’attente de la réponse.

Il existe une certaine surcharge pour le code asynchrone, mais dans des conditions de faible charge, cette surcharge est négligeable, tout en conditions de forte charge que vous êtes en mesure de traiter les demandes qui sinon seraient être mises en attente des threads disponibles.

Il a été possible d’effectuer ce type de programmation asynchrone depuis ASP.NET 1.1, mais il était difficile à écrire, sujette aux erreurs et difficile à déboguer. Maintenant que nous avons simplifié le codage pour elle dans ASP.NET 4.5, il n’existe aucune raison pour ne pas faire ces choses-là.

### <a name="async-support-in-entity-framework-6"></a>Prise en charge asynchrone dans Entity Framework 6

Dans le cadre de la prise en charge asynchrone dans 4.5, nous avons livré prise en charge asynchrone pour les appels de service web sockets et le système de fichiers d’e/s, mais le modèle le plus courant pour les applications web consiste à appuyer sur une base de données et nos bibliothèques de données n’a pas prendre en charge async. Entity Framework 6 ajoute à présent prise en charge asynchrone pour l’accès de base de données.

Dans Entity Framework 6 toutes les méthodes qui provoquent une requête ou une commande à envoyer à la base de données ont des versions asynchrones. L’exemple suivant montre la version asynchrone de la *trouver* (méthode).

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Et cette prise en charge asynchrone fonctionne non seulement pour les insertions, les suppressions, les mises à jour et recherche simple, elle fonctionne également avec les requêtes LINQ :

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Il existe un `Async` version de la `ToList` (méthode), car dans ce code, qui est la méthode qui entraîne une requête à envoyer à la base de données. Le `Where` et `OrderByDescending` méthodes uniquement configurer la requête, alors que le `ToListAsync` méthode exécute la requête et enregistre la réponse dans le `result` variable.

## <a name="summary"></a>Récapitulatif

Vous pouvez implémenter les recommandations de développement web présentées ici dans n’importe quel framework et n’importe quel environnement de cloud de programmation du web, mais nous avons les outils dans ASP.NET et Windows Azure pour faciliter leur. Si vous suivez ces modèles, vous pouvez facilement monter en charge la couche web, et vous allez réduire vos dépenses, car chaque serveur sera en mesure de gérer davantage de trafic.

Le [chapitre suivant](single-sign-on.md) examine comment le cloud permet des scénarios d’authentification uniques.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les ressources suivantes.

Serveurs web sans état :

- [Microsoft Patterns et pratiques - Guide de mise à l’échelle](https://msdn.microsoft.com/library/dn589774.aspx).
- [Affinité dans Sites Web Windows Azure de l’Instance de la désactivation ARR](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Billet de blog par Erez Benari, explique l’affinité de session dans les Sites Web Windows Azure.

CDN :

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de vidéos de neuf parties par Ulrich Homann, Marc Mercuri et Mark Simms. Consultez la discussion CDN dans l’épisode 3 en commençant à 1:34:00.
- [Modèle Microsoft Patterns et pratiques hébergement de contenu statique](https://msdn.microsoft.com/library/dn589776.aspx)
- [Révisions CDN](http://www.cdnreviews.com/). Vue d’ensemble des nombreuses CDN.

Programmation asynchrone :

- [À l’aide de méthodes asynchrones dans ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Didacticiel par Rick Anderson.
- [Programmation asynchrone avec Async et Await (c# et Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Livre blanc MSDN expliquant le raisonnement pour la programmation asynchrone, comment il fonctionne dans ASP.NET 4.5 et comment écrire du code pour l’implémenter.
- [Requête de Async Entity Framework et enregistrer](https://msdn.microsoft.com/data/jj819165)
- [Comment créer des Applications Web ASP.NET à l’aide d’Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Vidéo de présentation par Rowan Miller. Inclut une démonstration de graphique de la programmation asynchrone peut faciliter une augmentation considérable du débit de serveur web dans des conditions de charge élevée.
- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de vidéos de neuf parties par Ulrich Homann, Marc Mercuri et Mark Simms. Pour les discussions sur l’impact de la programmation asynchrone sur l’évolutivité, consultez épisode 4 et épisode 8.
- [La magie de l’utilisation de méthodes asynchrones dans ASP.NET 4.5 plus un piège important](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Billet de blog de Scott Hanselman, principalement sur l’utilisation d’async dans les applications ASP.NET Web Forms.

Pour le web supplémentaires meilleures pratiques de développement, consultez les ressources suivantes :

- [Le correctif il exemple d’Application - meilleures pratiques](the-fix-it-sample-application.md#bestpractices). L’annexe de ce livre électronique recense des meilleures pratiques qui ont été implémentées dans l’application Fix It.
- [Liste de vérification de développeur Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Précédent](continuous-integration-and-continuous-delivery.md)
> [Suivant](single-sign-on.md)
