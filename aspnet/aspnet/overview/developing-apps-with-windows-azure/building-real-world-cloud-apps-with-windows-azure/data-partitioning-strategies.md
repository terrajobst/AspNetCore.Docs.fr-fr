---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Données (création d’applications Cloud réalistes avec Azure) de stratégies de partitionnement | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 7c7530301bf17009c9ffcb0ffa2f375b96f694c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399514"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Données (création d’applications Cloud réalistes avec Azure) de stratégies de partitionnement
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur la série, consultez [le premier chapitre](introduction.md).


Nous avons précédemment vu combien il est facile à l’échelle de la couche web d’une application cloud, en ajoutant et supprimant des serveurs web. Mais si elles vous atteignez même magasin de données, déplace de goulot d’étranglement de votre application à partir de la partie frontale vers le back-end et la couche données est la plus difficile de mettre à l’échelle. Dans ce chapitre, nous examinons comment vous pouvez rendre votre couche de données évolutive en partitionnant les données dans plusieurs bases de données relationnelles, ou en combinant le stockage de base de données relationnelle avec d’autres options de stockage de données.

Il est préférable de configurer un schéma de partitionnement terminé à l’avance pour la même raison mentionnée précédemment : il est très difficile de modifier votre stratégie de stockage de données une fois une application en production. Si vous réfléchissez bien à l’avance sur les différentes approches, vous pouvez éviter d’avoir un « Twitter instant » lorsque votre application se bloque ou tombe en panne pendant un certain temps lorsque vous réorganisez les données et le code d’accès aux données de votre application.

## <a name="the-three-vs-of-data-storage"></a>Les trois Vs du stockage de données

Afin de déterminer si vous avez besoin d’une stratégie de partitionnement et il doit être, tenez compte des trois questions sur vos données :

- Volume – la quantité de données allez-vous stocker en fin de compte ? Quelques gigaoctets ? Quelques centaines de gigaoctets ? Téraoctets ? Pétaoctets ?
- Rapidité – qu’est la vitesse à laquelle vos données va croître ? Il est une application interne qui n’est pas de générer une grande quantité de données ? Une application externe qui les clients téléchargent les images et vidéos dans ?
- Diverses – type de données va stocker ? Relationnelles, images, les paires clé-valeur, les graphiques sociaux ?

Si vous pensez que vous allez avoir beaucoup de volume, vélocité ou divers, vous devez étudier soigneusement le type de schéma de partitionnement sera mieux permettre à votre application à l’échelle efficacement, car elle augmente, et pour vous assurer, vous ne rencontrez les goulots d’étranglement.

Il existe essentiellement trois approches de partitionnement :

- Partitionnement vertical
- Partitionnement horizontal
- Partitionnement hybride

## <a name="vertical-partitioning"></a>Partitionnement vertical

Portioning verticale est comme le fractionnement d’une table par colonnes : un ensemble de colonnes est mis dans une banque de données et un autre ensemble de colonnes passe à un magasin de données différent.

Par exemple, supposons que mon application stocke des données sur les personnes, y compris les images :

![Table de données](data-partitioning-strategies/_static/image1.png)

Lorsque vous représenter ces données sous forme de table et examinez les diverses variétés de données, vous pouvez voir que les trois colonnes de gauche ont des données de chaîne qui peuvent être stockées efficacement à une base de données relationnelle, tandis que les deux colonnes à droite sont essentiellement des tableaux d’octets c ome à partir des fichiers d’image. Il est possible de stockage des données de fichier image dans une base de données relationnelle, et beaucoup de gens faire, car ils ne souhaitent pas enregistrer les données dans le système de fichiers. Ils ne disposent pas d’un système de fichiers capable de stocker les volumes de données requis, ou ils ne souhaiterez gérer une sauvegarde distincte et de restauration du système. Cette approche fonctionne bien pour les bases de données sur site et de petites quantités de données dans les bases de données cloud. Dans l’environnement local, il peut être plus facile de simplement laisser le DBA s’occuper de tous les éléments.

Mais dans une base de données cloud, le stockage est relativement coûteux, et un volume élevé d’images peut développer la taille de la base de données au-delà des limites à partir duquel elle peut fonctionner efficacement. Vous pouvez résoudre ces problèmes en partitionnant les données verticalement, ce qui signifie que vous choisissez le magasin de données le plus approprié pour chaque colonne dans votre table de données. Ce qui peut fonctionner mieux pour cet exemple est de placer les données de chaîne dans une base de données relationnel et les images dans le stockage Blob.

![Table de données partitionnée verticalement](data-partitioning-strategies/_static/image2.png)

Stockage des images dans le stockage Blob au lieu d’une base de données est plus pratique dans le cloud que dans un environnement sur site, car vous n’avez pas à vous soucier de la configuration des serveurs de fichiers ou la gestion de sauvegarde et restauration des données stockées en dehors de la base de données relationnelle : tous les qui est gérée automatiquement pour vous par le service de stockage Blob.

C’est l’approche de partitionnement nous avons implémenté dans l’application Fix It, et nous allons examiner le code correspondant dans le [chapitre de stockage d’objets Blob](unstructured-blob-storage.md). Sans ce schéma de partitionnement et en supposant une taille d’image moyenne de 3 mégaoctets, l’application Fix It serait uniquement en mesure de stocker environ 40 000 tâches avant d’atteindre la taille maximale de la base de données de 150 Go. Après avoir supprimé les images, la base de données peut stocker 10 fois plus de tâches ; Vous pouvez aller beaucoup plus de temps avant d’avoir à réfléchir à l’implémentation d’un schéma de partitionnement horizontal. Et comme l’application met à l’échelle, vos dépenses croître plus lentement, car la majeure partie de vos besoins en stockage vont dans le stockage Blob très peu onéreux.

## <a name="horizontal-partitioning-sharding"></a>Partitionnement horizontal (sharding)

Portioning horizontale est comme le fractionnement d’une table par lignes : un ensemble de lignes est mis dans une banque de données et passe d’un autre ensemble de lignes dans un magasin de données différent.

Étant donné le même jeu de données, une autre option consisterait à stocker les différentes plages de noms de clients dans différentes bases de données.

![Table de données partitionnée horizontalement](data-partitioning-strategies/_static/image3.png)

Voulez-vous être très prudents à votre schéma de partitionnement pour vous assurer que les données sont réparties uniformément afin d’éviter les zones réactives. Cet exemple simple à l’aide de la première lettre du nom ne répond pas à cette exigence, étant donné que beaucoup de gens ont des noms qui commencent par certaines lettres courants. Vous atteignez les limites de taille de table plus tôt que prévu, car certaines bases de données seraient devenir très volumineuses, tandis que la plupart resterait petite.

Un inconvénient du partitionnement horizontal est qu’il peut être difficile d’effectuer des requêtes sur l’ensemble des données. Dans cet exemple, une requête aurait à tirer sur différentes bases de données jusqu'à 26 pour obtenir toutes les données stockées par l’application.

## <a name="hybrid-partitioning"></a>Partitionnement hybride

Vous pouvez combiner le partitionnement vertical et horizontal. Par exemple, dans l’exemple de données, vous pourrez stocker les images dans le stockage d’objets Blob et partitionner horizontalement les données de chaîne.

![Hybride de table de données partitionnée](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partitionnement d’une application de production

Sur le plan conceptuel, il est facile de voir le fonctionne d’un schéma de partitionnement, mais n’importe quel schéma de partitionnement augmente la complexité du code et ajoute des complications nouveau nombre que vous devez traiter. Si vous déplacez des images dans le stockage blob, que faire lorsque le service de stockage est hors service ? Façon dont vous gérez la sécurité de l’objet blob ? Que se passe-t-il si la base de données et le stockage blob désynchronisé ? Si vous êtes le partitionnement, comment allez-vous gérer interrogation de toutes les bases de données ?

Les complications sont faciles à gérer en tant que vous êtes sur leur planification avant de passer à la production. De nombreuses personnes qui ne souhaitent qu’elles avaient plus tard. En moyenne notre équipe Customer Advisory Team (CAT) Obtient des appels téléphoniques paniqués sur une fois par mois à partir de clients dont les applications sont marché décolle de manière très volumineuse, et ils n’avez pas cette planification. Et ils disent que quelque chose comme : « aide ! J’ai tout placer dans un magasin de données unique, et dans les 45 jours je vais à manquer d’espace sur ce dernier ! » Et si vous avez beaucoup de logique métier intégrée à la façon dont vous accéder à votre magasin de données et que les clients qui utilisent votre application, il n’existe aucun bon moment pour aller vers le bas pour un jour pendant la migration. Nous nous retrouvons parcourant efforts herculean pour aider à la partition de client de leurs données à la volée avec aucun temps d’arrêt. Il est très intéressant et très effrayant, et non quelque chose que vous souhaitez être impliqué dans si vous pouvez l’éviter ! Penser en amont à ce sujet et les intégrer à votre application rend votre vie beaucoup plus facile si l’application devient plus tard.

## <a name="summary"></a>Récapitulatif

Un schéma de partitionnement efficace peut permettre à votre application cloud à l’échelle à plusieurs pétaoctets de données dans le cloud sans les goulots d’étranglement. Et vous n’êtes pas obligé de payer à l’avance les énormes machines ou infrastructure étendue comme vous le feriez si vous exécutiez l’application dans un centre de données sur site. Dans le cloud, vous pouvez vous pouvez ajouter de façon incrémentielle capacité que vous en avez besoin, vous payez uniquement pour autant que vous utilisez lorsque vous l’utilisez.

Dans le [chapitre suivant](unstructured-blob-storage.md) nous allons voir comment l’application Fix It implémente le partitionnement vertical en stockant les images dans le stockage Blob.

## <a name="resources"></a>Ressources

Pour plus d’informations sur les stratégies de partitionnement, consultez les ressources suivantes.

Documentation :

- [Meilleures pratiques pour la création de Services à grande échelle sur les Services Cloud Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc par Mark Simms et Michael Thomassy.
- [Microsoft Patterns and Practices - modèles de conception Cloud](https://msdn.microsoft.com/library/dn568099.aspx). Consultez les conseils de partitionnement des données, modèle de partitionnement.

Vidéos :

- [Prévention de défaillance : Création de Services de Cloud évolutives, durables](https://channel9.msdn.com/Series/FailSafe). Série de neuf en parties par Ulrich Homann, Marc Mercuri et Mark Simms. Présente les principaux concepts et principes d’architecture de manière très accessible et intéressante, avec récits dessinés à partir de l’expérience de Microsoft Customer Advisory Team (CAT) de clients réels. Consultez la discussion de partitionnement dans épisode 7.
- [Construction Big : Leçons apprises à partir de clients Windows Azure - partie I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms traite des schémas de partitionnement, les stratégies de partitionnement, comment implémenter le partitionnement et les fédérations de base de données SQL, en commençant à 19:49. Semblable à la série de prévention de défaillance, mais entrera en procédures plus de détails.

Exemple de code :

- [Cloud Service Fundamentals dans Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application qui inclut une base de données partitionnée. Pour obtenir une description du schéma de partitionnement implémentée, consultez [DAL : partitionnement de SGBDR](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) sur le blog de Windows Azure.

> [!div class="step-by-step"]
> [Précédent](data-storage-options.md)
> [Suivant](unstructured-blob-storage.md)
