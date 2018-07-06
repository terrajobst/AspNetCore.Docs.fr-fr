---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: (Création d’applications Cloud réalistes avec Azure) de mise en cache distribuée | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: aspnetcontent
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: a165c789ae656025934bc5e3ed8e8caef1c21787
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821617"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Mise en cache (Building Real-World Cloud d’applications distribuées avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Le chapitre précédent examiné la gestion des erreurs temporaires et mentionné la mise en cache comme une stratégie de coupe-circuits. Ce chapitre donne plus d’informations sur la mise en cache, y compris quand l’utiliser, modèles courants pour l’utiliser et comment l’implémenter dans Azure.

## <a name="what-is-distributed-caching"></a>Ce qui est mise en cache distribué

Un cache fournit un débit élevé, accès à faible latence aux données d’application fréquemment sollicitées, en stockant les données en mémoire. Pour une application cloud le type plus utiles de cache est un cache distribué, ce qui signifie que les données ne sont pas stockées sur la mémoire du serveur de site web individuel, mais sur d’autres ressources de cloud et les données mises en cache sont accessible à tous les serveurs d’une application web (ou autre cloud de machines virtuelles qu’ar e utilisé par l’application).

![diagramme montrant plusieurs serveurs web accèdent aux serveurs de cache même](distributed-caching/_static/image1.png)

Lorsque l’application met à l’échelle en ajoutant ou supprimant des serveurs, ou lorsque les serveurs sont remplacés en raison d’erreurs ou des mises à niveau, les données mises en cache restent accessibles à chaque serveur qui exécute l’application.

En évitant l’accès aux données de latence élevée d’un magasin de données persistantes, la mise en cache permet d’améliorer considérablement la réactivité des applications. Par exemple, la récupération des données à partir du cache sont beaucoup plus rapide que son extraction à partir d’une base de données relationnelle.

Un avantage collatéral de mise en cache est réduit le trafic vers le magasin de données persistante, ce qui peut réduire les coûts en cas de sortie de données de facture pour le magasin de données persistantes.

## <a name="when-to-use-distributed-caching"></a>Quand utiliser la mise en cache distribuée

La mise en cache fonctionne mieux pour des charges de travail qui ne lire plus que l’écriture de données, et lorsque le modèle de données prend en charge l’organisation de clé/valeur que vous utilisez pour stocker et récupérer des données dans le cache. Il est également plus utile lorsque des utilisateurs d’applications partagent un grand nombre de données communes ; par exemple, cache fournirait pas autant d’avantages si chaque utilisateur extrait généralement les données uniques pour cet utilisateur. Un exemple où la mise en cache pourrait être très intéressant est un catalogue de produits, étant donné que les données ne changent pas fréquemment, et tous les clients recherchent les mêmes données.

L’avantage de la mise en cache devient plus en plus mesurable plus une application met à l’échelle, comme les limites de débit et les délais de latence de la banque de données persistantes deviennent plus d’une limite sur les performances globales de l’application. Toutefois, vous pouvez implémenter la mise en cache pour des raisons autres qu’aussi les performances. Pour les données qui ne doivent pas être parfaitement à jour lors de son affichage à l’utilisateur, l’accès du cache peut servir un disjoncteur pour lorsque le magasin de données persistants ne répond pas ou indisponible.

## <a name="popular-cache-population-strategies"></a>Stratégies de remplissage de cache bien connu

Pour être en mesure de récupérer des données à partir du cache, vous devez stocker il tout d’abord. Il existe plusieurs stratégies pour obtenir des données dont vous avez besoin dans un cache :

- À la demande / Aside mettre en Cache

    L’application tente de récupérer des données à partir du cache, et lorsque le cache n’a pas les données (un « miss »), l’application stocke les données dans le cache afin qu’il soit disponible la prochaine fois. La prochaine fois que l’application tente d’obtenir les mêmes données, il détecte qu’il recherche dans le cache (un « accès »). Pour éviter l’extraction des données mises en cache qui a changé sur la base de données, vous invalidez le cache lors des modifications au magasin de données.
- Push de données en arrière-plan

    Les services d’arrière-plan transmettre des données dans le cache selon une planification régulière et l’application always extrait à partir du cache. Cette approche fonctionne très bien avec les sources de données de latence élevée qui ne nécessitent pas toujours retourne les données les plus récentes.
- Disjoncteur

    L’application normalement communique directement avec le magasin de données persistantes, mais lorsque le magasin de données persistant a un problème de disponibilité, l’application récupère des données à partir du cache. Données ont été placées dans le cache à l’aide de cache mis de côté ou stratégie de push de données en arrière-plan. Il s’agit d’une erreur de gestion des stratégie plutôt qu’une stratégie d’amélioration des performances.

Afin de conserver les données dans le cache en cours, vous pouvez supprimer les entrées de cache associées lorsque votre application crée, met à jour, ou supprime des données. S’il est parfait pour votre application obtenir parfois des données qui sont légèrement obsolètes, vous pouvez compter sur une heure d’expiration configurables pour définir une limite sur l’ancienneté du cache de données.

Vous pouvez configurer l’expiration absolue (temps écoulé depuis que l’élément de cache a été créé) ou expiration (temps écoulé depuis l’heure du dernier qu'accès d’un élément de cache). Expiration absolue est utilisée lorsque vous êtes dépendant le mécanisme d’expiration du cache pour empêcher que les données ne devienne trop caduque. Dans l’application Fix It, nous allons supprimer manuellement les éléments du cache obsolète, et nous allons utiliser l’expiration décalée à conserver les données les plus récentes dans le cache. Quelle que soit la stratégie d’expiration que vous choisissez, le cache sera automatiquement supprimer les anciens éléments (moins récemment utilisées ou LRU) lors de la limite de mémoire du cache est atteinte.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Exemple de code de type cache-aside pour application Fix It

Dans l’exemple de code suivant, nous commencez par vérifier le cache lors de la récupération d’une tâche Fix It. Si la tâche se trouve dans le cache, nous renvoyons Si ce n’est pas trouvé, nous l’obtenir à partir de la base de données et les stocker dans le cache. Les modifications que vous effectueriez ajouter la mise en cache pour le `FindTaskByIdAsync` méthode sont mises en surbrillance.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Lorsque vous mettez à jour ou supprimez une tâche Fix It, vous devez invalider la tâche (supprimer) la mise en cache. Sinon, les futures tentatives de lire que cette tâche doit continuer à obtenir les anciennes données du cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Il s’agit des exemples pour illustrer le code de mise en cache simple ; la mise en cache n’a pas été implémentée dans le projet téléchargeable de Fix It.

## <a name="azure-caching-services"></a>Services de mise en cache Azure

Azure offre les services de mise en cache suivantes : [Cache Redis Azure](https://msdn.microsoft.com/library/dn690523.aspx) et [Cache géré Azure](https://msdn.microsoft.com/library/dn386094.aspx). Cache Redis Azure est basé sur ce fameux [Cache Redis open source](http://redis.io/) et est le premier choix pour la plupart mise en cache de scénarios.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>État de session ASP.NET à l’aide d’un fournisseur de cache

Comme mentionné dans le [chapitre de web développement meilleures pratiques](web-development-best-practices.md), une bonne pratique consiste à éviter en utilisant l’état de session. Si votre application requiert l’état de session, la prochaine meilleure solution consiste à éviter le fournisseur en mémoire par défaut, car la montée en puissance (plusieurs instances du serveur web) qui ne permet pas. Le fournisseur d’état de session ASP.NET SQL Server permet à un site qui s’exécute sur plusieurs serveurs web d’utiliser l’état de session, mais elle entraîne un coût de latence élevée par rapport à un fournisseur en mémoire. La meilleure solution si vous devez utiliser l’état de session consiste à utiliser un fournisseur de cache, tel que le [fournisseur d’état de Session pour Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Récapitulatif

Vous avez vu comment l’application Fix It peut implémenter la mise en cache afin d’améliorer l’évolutivité et des temps de réponse et pour activer l’application de continuer à être réactif pour les opérations de lecture lors de la base de données n’est pas disponible. Dans le [chapitre suivant](queue-centric-work-pattern.md) nous allons montrer comment améliorer l’évolutivité et de rendre l’application continuent d’être réactifs pour les opérations d’écriture davantage.

## <a name="resources"></a>Ressources

Pour plus d’informations sur la mise en cache, consultez les ressources suivantes.

Documentation

- [Cache Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentation officielle de MSDN sur la mise en cache dans Azure.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez les conseils de mise en cache et le modèle Cache-Aside.
- [FailSafe : Guidance for Architectures résilientes du Cloud recommandations](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc par Marc Mercuri, Ulrich Homann et Andrew Townhill. Consultez la section sur la mise en cache.
- [Meilleures pratiques pour la création de Services à grande échelle sur Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Livre blanc par Mark Simms et Michael Thomassy. Consultez la section sur la mise en cache distribuée.
- [Mise en cache sur le chemin d’accès à l’évolutivité distribuée](https://msdn.microsoft.com/magazine/dd942840.aspx). Un article de MSDN Magazine (2009) plus anciens, mais une introduction clairement écrite à la mise en cache distribuée en général ; passe en détail les sections de mise en cache des livres blancs de prévention de défaillance et les meilleures pratiques.

Vidéos

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de neuf en parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente une vue de 400-niveau de la procédure permettant de concevoir des applications cloud. Cette série se concentre sur la théorie et les raisons pour lesquelles ; Pour plus d’informations pratiques, consultez la série Big construction par Mark Simms. Consultez la discussion sur la mise en cache dans l’épisode 3 en commençant à 1:24:14.
- [Construction Big : Leçons apprises de clients Azure - partie I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies explique en commençant de mise en cache distribuée à 46:00. Semblable à la série de prévention de défaillance, mais entrera en procédures plus de détails. La présentation a été donnée au 31 octobre 2012, donc elle ne couvre pas le service de mise en cache des applications Web dans Azure App Service qui a été introduite dans 2013.

Exemple de code

- [Cloud Service Fundamentals dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application qui implémente la mise en cache distribuée. Consultez le blog d’accompagnement [Cloud Service Fundamentals – principes fondamentaux de la mise en cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Précédent](transient-fault-handling.md)
> [Suivant](queue-centric-work-pattern.md)
