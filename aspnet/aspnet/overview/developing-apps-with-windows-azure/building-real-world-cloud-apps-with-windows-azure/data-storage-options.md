---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Options de stockage de données (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 43e7a03f3bebcca820452ea10e13459d8d275b5a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363505"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Options de stockage de données (création d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


La plupart des gens sont utilisés pour les bases de données relationnelles, et ils ont tendance à négliger les autres options de stockage de données lorsqu’elles vous concevez une application cloud. Le résultat peut être des problèmes de performances, coûts élevés, voire pire, étant donné que [NoSQL](http://en.wikipedia.org/wiki/NoSQL) bases de données (non relationnelles) peuvent gérer certaines tâches plus efficacement que les bases de données relationnelles. Lorsque les clients nous demandent de l’aide pour résoudre un problème de stockage de données critiques, il est souvent parce qu’ils ont une base de données relationnelle dans laquelle une des options NoSQL aurait fonctionné mieux. Dans ce cas le client aurait été préférable si elles avaient implémenté la solution NoSQL avant de déployer l’application en production.

En revanche, il serait également une erreur de supposer qu’une base de données NoSQL faire tout bien ou suffisamment bien. Unique meilleures données Gestion obligatoirement pour toutes les tâches de stockage de données ; solutions de gestion des données différentes sont optimisées pour différentes tâches. La plupart des applications de cloud réalistes ont une variété de besoins de stockage de données et sont souvent pris en charge meilleures par une combinaison de plusieurs solutions de stockage de données.

L’objectif de ce chapitre consiste à vous donner un sens plus large d’options de stockage de données disponibles pour une application cloud et des conseils de base sur la façon de choisir celles qui correspondent à votre scénario. Il est préférable de connaître les options disponibles pour vous et réfléchir à leurs forces et les faiblesses avant de développer une application. Modification des options de stockage de données dans une application de production peut être extrêmement difficile, tels que de devoir modifier un moteur jet tandis que le plan est en cours.

## <a name="data-storage-options-on-azure"></a>Options de stockage de données sur Azure

Le cloud rend relativement facile d’utiliser une variété de relationnelles et les magasins de données NoSQL. Voici quelques-unes des plateformes de stockage de données que vous pouvez utiliser dans Azure.

![](data-storage-options/_static/image1.png)

Le tableau présente les quatre types de bases de données NoSQL :

- [Bases de données clé/valeur](https://msdn.microsoft.com/library/dn313285.aspx#sec7) stocker un objet sérialisé unique pour chaque valeur de clé. Elles sont parfaites pour le stockage de grands volumes de données pour lesquelles vous souhaitez obtenir un élément pour une valeur de clé donnée et que vous n’avez pas à une requête basée sur d’autres propriétés de l’élément.

    [Stockage d’objets Blob Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) est une base de données clé/valeur qui fonctionne comme le stockage de fichiers dans le cloud, avec les valeurs de clés qui correspondent aux noms de dossier et fichier. Vous récupérer un fichier par son dossier et le nom de fichier, ne pas en recherchant des valeurs dans le contenu du fichier.

    [Stockage Table Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) est également une base de données clé/valeur. Chaque valeur est appelée un *entité* (semblable à une ligne identifiée par une clé de partition et la clé de ligne) et contient plusieurs *propriétés* (semblable aux colonnes, mais pas toutes les entités dans une table à partager le même colonnes). Interrogation sur les colonnes autres que la clé est très inefficace et doit être évitée. Par exemple, vous pouvez stocker des données de profil utilisateur, avec une seule partition stockant des informations sur un seul utilisateur. Vous pouvez stocker des données telles que le nom d’utilisateur, un hachage de mot de passe, la date de naissance et ainsi de suite, dans les propriétés distinctes d’une entité ou dans des entités distinctes dans la même partition. Mais vous ne voudriez pas à interroger pour tous les utilisateurs avec une plage de dates de naissance, et vous ne pouvez pas exécuter une requête de jointure entre votre table des profils et une autre table. Stockage de table est plus évolutif et moins coûteux qu’une base de données relationnelle, mais il ne permet pas les requêtes complexes ou des jointures.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sont des bases de données clé/valeur dans laquelle les valeurs sont *documents*. « Document » n’est pas utilisé dans le sens d’un document Word ou Excel, mais signifie une collection de champs nommés et des valeurs, qui peut être un document enfant. Par exemple, dans une table d’historique de commande un document de commande peut-être numéro de commande, date de commande et les champs de client ; et le champ client peut avoir des champs nom et adresse. La base de données encode des données de champ dans un format tel que XML, YAML, JSON ou BSON ; ou il peut utiliser le texte brut. Une fonctionnalité qui définit les bases de données de document en dehors des bases de données clé/valeur est la possibilité d’interroger sur des champs et définir des index secondaires pour rendre une interrogation plus efficace. Ainsi, une base de données de document plus adapté pour les applications qui doivent récupérer des données en fonction de critères plus complexes que la valeur de la clé de document. Par exemple, dans une base de données document de l’historique des commandes client, vous pouvez interroger sur divers champs tels que l’ID de produit, ID de client, nom du client et ainsi de suite. [MongoDB](http://www.mongodb.org/) est une base de données de documents les plus courants.
- [Bases de données de familles de colonnes](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sont des magasins de données qui vous permettent de structurer le stockage de données dans des regroupements de colonnes liées appelées familles de colonnes de clé/valeur. Par exemple, une base de données de recensement peut avoir un groupe de colonnes pour le nom d’une personne (tout d’abord, deuxième prénom), un groupe pour l’adresse de personne et un groupe pour les informations de profil de la personne (DOB, sexe, etc.). La base de données peut ensuite stocker chaque famille de colonnes dans une partition distincte tout en conservant toutes les données d’une personne associée à la même clé. Vous pouvez ensuite lire toutes les informations de profil sans avoir à lire toutes les informations nom et l’adresse. [Cassandra](http://cassandra.apache.org/) est une base de données de familles de colonnes les plus courants.
- [Bases de données du graphique](https://msdn.microsoft.com/library/dn313285.aspx#sec10) stockent des informations comme une collection d’objets et relations. L’objectif d’une base de données de graphique consiste à permettre à une application à effectuer efficacement les requêtes qui parcourent le réseau des objets et des relations entre eux. Par exemple, les objets peuvent être employés dans une base de données ressources humaines, et vous souhaiterez peut-être pour faciliter les requêtes telles que « recherchent tous les employés travaillant directement ou indirectement pour Scott. » [Neo4j](http://www.neo4j.org/) est une base de données de graphique populaires.

Par rapport aux bases de données relationnelles, les options de NoSQL offrent bien plus l’évolutivité et la rentabilité de stockage et d’analyse des données non structurées. L’inconvénient est qu’ils ne fournissent pas les queryability riche et les fonctionnalités d’intégrité des données robuste de bases de données relationnelles. NoSQL fonctionne bien pour les données de journal IIS, ce qui implique un volume élevé sans avoir besoin pour les requêtes de jointure. NoSQL fonctionnerait pas très bien pour les transactions, bancaires qui nécessite l’intégrité des données absolu et implique plusieurs relations à d’autres données relatives aux comptes.

Il existe également une catégorie la plus récente de plate-forme de base de données appelée [NewSQL](http://en.wikipedia.org/wiki/NewSQL) qui combine l’évolutivité d’une base de données NoSQL avec le queryability et l’intégrité transactionnelle de la base de données relationnelle. Bases de données NewSQL sont conçues pour stockage distribué et le traitement des requêtes, ce qui est souvent difficile à implémenter dans les bases de données « OldSQL ». [NuoDB](http://www.nuodb.com/) est un exemple d’une base de données NewSQL qui peut être utilisé sur Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop et MapReduce

Les grands volumes de données que vous pouvez stocker dans les bases de données NoSQL peuvent être difficiles à analyser efficacement en temps voulu. À faire que vous pouvez utiliser une infrastructure telle que [Hadoop](http://hadoop.apache.org/) qui implémente [MapReduce](http://en.wikipedia.org/wiki/MapReduce) fonctionnalité. Essentiellement un processus MapReduce fait ce qui suit :

- Limite la taille des données qui doivent être traitées en sélectionnant les données de stocker uniquement les données que vous avez besoin en fait d’analyser. Par exemple, vous souhaitez connaître la composition de votre utilisateur de base par année de naissance, afin seulement des années de naissance de votre magasin de données de profil utilisateur.
- Décomposer les données en plusieurs parties et les envoyer à d’autres ordinateurs pour le traitement. Ordinateur A calcule le nombre de personnes avec des dates de 1950-1959, l’ordinateur B effectue 1960-1969, etc. Ce groupe d’ordinateurs est appelé un *Hadoop cluster*.
- Placer les résultats de chaque partie retour ensemble une fois le traitement sur les parties est effectué. Vous disposez maintenant d’une liste relativement courte de combien de personnes pour chaque année de naissance et la tâche de calcul de pourcentages dans cette liste globale est facile à gérer.

Sur Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) afin de pouvoir traiter, analyser et obtenir de nouvelles informations provenant de big data à l’aide de la puissance de Hadoop. Par exemple, vous pouvez l’utiliser pour analyser les journaux de serveur web :

- Activer la journalisation du serveur web à votre compte de stockage. Cela configure Azure pour écrire des journaux dans le Service Blob pour chaque requête HTTP à votre application. Le Service Blob est en gros fichier de stockage cloud, et il s’intègre parfaitement avec HDInsight. 

    ![Journaux de stockage d’objets Blob](data-storage-options/_static/image2.png)
- À mesure que l’application augmente le trafic, les journaux IIS du serveur web sont écrits dans stockage Blob. 

    ![Journaux de serveur Web](data-storage-options/_static/image3.png)
- Dans le portail, cliquez sur **New** - **Data Services** - **HDInsight** - **création rapide**, et spécifiez un nom de cluster HDInsight, la taille du cluster (nombre de nœuds de données de cluster HDInsight) et un nom d’utilisateur et le mot de passe pour le cluster HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Vous pouvez maintenant configurer des tâches MapReduce pour analyser vos journaux et obtenez des réponses aux questions telles que :

- Les heures du jour mon application obtient-il le trafic plus ou moins ?
- Dans quels pays est mon trafic en provenance de ?
- Quel est le revenu moyen voisinage des zones que mon trafic provient. (Il existe un jeu de données public qui vous donne un revenu de voisinage par adresse IP, et qui peut correspondre à par rapport à l’adresse IP dans les journaux du serveur web).
- Comment ne revenu de voisinage est corrélé à des pages spécifiques ou des produits sur le site ?

Vous pouvez ensuite utiliser les réponses aux questions comme celles-ci pour cibler des publicités en fonction de la probabilité qu’un client serait intéressé ou susceptibles d’acheter un produit particulier.

Comme expliqué dans la [automatiser tout chapitre](automate-everything.md), la plupart des fonctions que vous pouvez effectuer dans le portail peut être automatisé et qui inclut la configuration et l’exécution des travaux d’analyse HDInsight. Un script de HDInsight typique peut comporter les étapes suivantes :

- Approvisionner un cluster HDInsight et liez-la à votre compte de stockage pour l’entrée de stockage d’objets Blob.
- Téléchargez les exécutables de tâche MapReduce (fichiers .jar ou .exe) dans le cluster HDInsight.
- Soumettre un MapReduce qui stocke les données de sortie vers le stockage Blob.
- Attendez que la tâche se termine.
- Supprimer le cluster HDInsight.
- Accéder à la sortie à partir du stockage d’objets Blob.

En exécutant un script qui fait tout cela, vous réduisez la durée pendant laquelle le cluster HDInsight est approvisionné, ce qui réduit les coûts.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plateforme en tant que Service (PaaS) par rapport à l’Infrastructure en tant que Service (IaaS)

Les options de stockage de données répertoriées précédemment incluent Platform-as-a-Service (PaaS) et les solutions d’Infrastructure-as-a-Service (IaaS). PaaS signifie que nous gérons l’infrastructure matérielle et logicielle et que vous utilisez simplement le service. Base de données SQL est une fonctionnalité PaaS d’Azure. Vous demander de bases de données, et dans les coulisses Azure configure configure les machines virtuelles et configure les bases de données sur ces derniers. Vous n’avez pas un accès direct aux machines virtuelles et que vous n’êtes pas obligé de les gérer. IaaS signifie que vous configurez, configurez et gérez des machines virtuelles qui s’exécutent dans notre infrastructure de centre de données, et vous placer tout ce que vous voulez sur ces derniers. Nous offrons une galerie d’images de machines virtuelles préconfigurées pour les configurations courantes de machine virtuelle. Par exemple, vous pouvez installer les images de machines virtuelles préconfigurées pour Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, base de données Oracle, etc.

Les solutions de données PaaS proposés par Azure sont les suivantes :

- Azure SQL Database (anciennement SQL Azure). Un cloud base de données relationnelle basé sur SQL Server.
- Stockage Table Azure. Clé/valeur de la base de données NoSQL.
- Stockage d’objets Blob Azure. Stockage de fichiers dans le cloud.

Pour IaaS, vous pouvez exécuter tout ce que vous pouvez charger sur une machine virtuelle, par exemple :

- Bases de données relationnelles comme SQL Server, Oracle, MySQL, SQL Compact, SQLite ou Postgres.
- Magasins de données de clé/valeur comme Riak, Cassandra, Memcached et Redis.
- Magasins de données de colonne, telle que HBase.
- Bases de données de document comme MongoDB, RavenDB et CouchDB.
- Bases de données de graphique comme Neo4j.

![Options de stockage de données sur Azure](data-storage-options/_static/image5.png)

L’option IaaS vous donne les options de stockage de données presque illimitées, et bon nombre d'entre eux sont particulièrement faciles à utiliser, car vous pouvez créer des machines virtuelles à l’aide des images préconfigurées. Par exemple, dans le portail de gestion atteindre **Machines virtuelles**, cliquez sur le **Images** onglet, puis cliquez sur **parcourir le dépôt de machines virtuelles**.

![Parcourir le dépôt de machines virtuelles](data-storage-options/_static/image6.png)

Vous voyez ensuite une liste de [des centaines d’images de machine virtuelle préconfigurées](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), et vous pouvez créer une machine virtuelle à partir d’une image qui a un système de gestion de base de données est préinstallé, comme MongoDB, Neo4J, Redis, Cassandra ou CouchDB :

![MongoDB dans le dépôt de machines virtuelles](data-storage-options/_static/image7.png)

Azure permet les options de stockage de données IaaS aussi simples à utiliser que possible, mais les offres PaaS présentent de nombreux avantages qui les rendent plus rentable et pratique dans de nombreux scénarios :

- Vous n’êtes pas obligé de créer des machines virtuelles, vous utilisez simplement le portail ou un script pour configurer un magasin de données. Si vous souhaitez un magasin de données de 200 téraoctets, vous pouvez simplement cliquer sur un bouton ou exécuter une commande, et en quelques secondes, il est prêt à utiliser.
- Vous n’êtes pas obligé de gérer ou de corriger les machines virtuelles utilisées par le service. Microsoft fait pour vous automatiquement.-vous n’êtes pas obligé de vous inquiétez pas sur la configuration d’infrastructure pour la mise à l’échelle ou haute disponibilité ; Microsoft gère tout cela pour vous.
- Vous n’êtes pas obligé d’acheter des licences ; les frais de licence sont inclus dans les frais de service.
- Vous payez uniquement ce que vous utilisez.

Options de stockage de données PaaS dans Azure incluent les offres par des fournisseurs tiers. Par exemple, vous pouvez choisir le [module complémentaire MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) à partir du Store de Azure pour approvisionner une base de données en tant que service.

## <a name="choosing-a-data-storage-option"></a>En choisissant une option de stockage de données

Sans une approche consiste pour tous les scénarios. Si tout le monde indique que cette technologie est la réponse, la première chose à poser est « Qu’est la question ? », étant donné que les différentes solutions sont optimisées pour des choses différentes. Il offre des avantages du modèle relationnel ; C’est pourquoi il a été autour de la condition. Mais il existe également des côtés de bas to SQL qui peuvent être traités par une solution NoSQL.

Ce que nous le constatons souvent recommandé d’utiliser une approche de composition, où vous utilisez SQL et NoSQL dans une solution unique. Même lorsque l'on dit ils intégrez NoSQL, si vous explorez qu’ils le font vous souvent trouver qu’ils utilisent plusieurs infrastructures NoSQL différents : ils utilisent [CouchDB](http://wiki.apache.org/couchdb/Introduction), et [Redis](http://redis.io/)et [ Riak](http://basho.com/riak/) pour des choses différentes. Même Facebook, qui utilise largement NoSQL, utilise différentes infrastructures de NoSQL pour les différentes parties du service. La possibilité de combiner des approches de stockage de données est une des choses intéressante sur le cloud, car il est facile à utiliser plusieurs solutions de données et de les intégrer dans une même application.

Voici quelques questions à se soucier lorsque vous choisissez une approche :

| Sémantique de données | -Nouveautés du stockage et données accès aux données essentielles sémantique (vous stockez des données relationnelles ou non structurées) ? Données non structurées telles que des fichiers multimédias correspond le mieux dans le stockage d’objets blob. une collection de données associées telles que les produits, les inventaires, fournisseurs, les commandes des clients, etc., correspond le mieux à une base de données relationnelle. |
| --- | --- |
| Prise en charge de la requête | -Est-il facile d’interroger les données ? -Les types de questions pouvant être efficacement invité ? Magasins de données clé/valeur sont efficaces pour l’obtention d’une seule ligne, étant donnée une valeur de clé mais pas pour les requêtes complexes. Pour un magasin de données de profil utilisateur où vous obtenez toujours les données pour un utilisateur particulier, un magasin de données clé/valeur fonctionnait bien ; pour un catalogue de produits dans lequel vous souhaitez obtenir différents regroupements basés sur les différents attributs de produit une base de données relationnel peut-être mieux fonctionner. Bases de données NoSQL peuvent stocker de grands volumes de données efficacement, mais vous devez structure autour de la façon dont l’application interroge les données de la base de données, et cela, les requêtes ad hoc est plus difficile à faire. Avec une base de données relationnelle, vous pouvez créer presque n’importe quel type de requête. |
| Projection fonctionnelle | -Possibilité de questions, agrégations, etc., être exécutée côté serveur ? Si j’exécute SELECT COUNT (\*) à partir d’une table dans SQL, il très efficacement effectuer tout le travail sur le serveur et renvoie le nombre que je cherche. Si je souhaite le même calcul à partir d’un magasin de données NoSQL qui ne prennent pas en charge d’agrégation, ceci est une « requête unbounded » inefficace et va probablement expirer. Même si la requête aboutit que je dois récupérer toutes les données à partir du serveur au client et compter les lignes sur le client. -Quels langages ou les types d’expressions peuvent être utilisés ? Avec une base de données relationnelle, je peux utiliser SQL. Avec certaines bases de données NoSQL telles que le stockage Table Azure, je vais utiliser [OData](http://www.odata.org/), et tout ce que je peux faire est de filtrer sur la clé primaire et obtenir des projections (Sélectionner un sous-ensemble des champs disponibles). |
| Grande évolutivité | -La fréquence et quantité nécessaire les données à l’échelle ? -La plateforme n’en mode natif implémente la montée en puissance ? -Est-il facile à Ajout/Suppression de capacité (taille et le débit) ? Tables et des bases de données relationnelles ne sont pas partitionnées automatiquement pour les rendre évolutives, afin qu’ils soient difficiles à mettre à l’échelle au-delà de certaines limitations. Magasins de données NoSQL comme le stockage Azure Table de partition, par nature, tout, et il n’existe pratiquement aucune limite à l’ajout de partitions. Vous pouvez facilement faire évoluer le stockage Table jusqu'à 200 téraoctets, mais la taille maximale de la base de données pour la base de données SQL Azure est de 500 gigaoctets. Vous pouvez monter des données relationnelles en partitionnant en plusieurs bases de données, mais la configuration d’une application pour prendre en charge ce modèle implique beaucoup de travail de programmation. |
| Instrumentation et la facilité de gestion | -Simple est la plateforme pour instrumenter, surveiller et gérer ? Vous devez rester informé sur l’intégrité et les performances de votre magasin de données, vous devez savoir à l’avance quelles métriques une plateforme vous offre gratuitement, et ce que vous devez développer vous-même. |
| Opérations | -Simple est la plateforme pour déployer et exécuter sur Azure ? PaaS ? IaaS ? Linux ? Stockage de table et de la base de données SQL sont faciles à configurer sur Azure. Plateformes qui ne sont pas des solutions PaaS Azure intégrées nécessitent davantage d’efforts. |
| Prise en charge de l’API | -Est une API disponible qui le rend facile à utiliser avec la plateforme ? Pour le Service de Table Azure, il existe un SDK avec une API .NET qui prend en charge le modèle de programmation asynchrone .NET 4.5. Si vous écrivez une application .NET, il sera beaucoup plus facile à écrire et tester le code pour le Service de Table Azure par rapport à une autre clé/valeur colonne store plateforme de données qui n’a aucune API ou une moins complète. |
| Cohérence transactionnelle de l’intégrité et de données | -Est-elle critique que la plateforme prend en charge les transactions afin de garantir la cohérence des données ? Pour suivre les e-mails en bloc envoyées, les performances et à faible coût de stockage peuvent être plus importantes que la prise en charge automatique pour les transactions ou de l’intégrité référentielle dans la plateforme de données, rendre le Service de Table Azure un bon choix. Pour le suivi de compte bancaire équilibre ou fournisseur une plateforme de base de données relationnelle qui fournit des garanties transactionnelles forts serait un meilleur choix. |
| Continuité des activités | -Quel sont faciles sauvegarde, restauration et récupération d’urgence ? Tôt ou tard les données de production seront être corrompues et vous aurez besoin d’une fonction d’annulation. Bases de données relationnelles présentent d’autres fonctionnalités de restauration précis, telles que la capacité à restaurer à un point dans le temps. Comprendre les fonctionnalités de restauration sont disponibles dans chaque plateforme que vous envisagez d’est un facteur important à prendre en compte. |
| Coût | -Si plusieurs plateformes peut prendre en charge votre charge de travail de données, ils se différencient-elles coût ? Par exemple, si vous utilisez ASP.NET Identity, vous pouvez stocker des données de profil utilisateur dans le Service de Table Azure ou Azure SQL Database. Si vous ne devez pas les riches interrogation des installations de base de données SQL, vous pouvez choisir les Tables Azure dans la partie, car il coûte beaucoup moins pour un volume de stockage donné. |

Ce que nous recommandons généralement est de connaître la réponse aux questions dans chacune de ces catégories avant de choisir vos solutions de stockage de données.

En outre, votre charge de travail peut-être avoir des exigences spécifiques qui certaines plateformes peuvent mieux que d’autres prennent en charge. Exemple :

- Requiert de votre application auditer les fonctionnalités ?
- Quelles sont vos exigences de longévité de données--avez-vous besoin de fonctions d’archivage ou de purge automatisée ?
- Vous avez des besoins de sécurité spécialisée ? Par exemple, les données incluent des informations d’identification personnelle (informations d’identification personnelle), mais vous devez être en mesure de s’assurer que les informations d’identification personnelle sont exclue de résultats de la requête.
- Si vous avez des données qui ne peuvent pas être stockées dans le cloud pour des raisons réglementaires ou technologiques, vous devrez peut-être une plateforme de stockage de données cloud qui facilite l’intégration avec votre stockage en local.

## <a name="demo--using-sql-database-in-azure"></a>Démonstration – à l’aide de la base de données SQL dans Azure

L’application Fix It utilise une base de données relationnelle pour stocker les tâches. Le script Windows PowerShell de création environnement indiqué dans le [automatiser tout chapitre](automate-everything.md) crée deux instances de base de données SQL. Vous pouvez voir ces dans le portail en cliquant sur le **bases de données SQL** onglet.

![Bases de données SQL dans le portail](data-storage-options/_static/image8.png)

Il est également facile de créer des bases de données à l’aide du portail.

Cliquez sur **nouveau : Data Services** -- **base de données SQL** -- **création rapide**, entrez un nom de base de données, choisissez un serveur que vous avez déjà dans votre compte ou créez-en un, puis cliquez sur **créer la base de données SQL**.

![Nouvelle base de données SQL](data-storage-options/_static/image9.png)

Attendez quelques secondes, et que vous avez une base de données dans Azure, vous pouvez utiliser.

![Base de données SQL créée](data-storage-options/_static/image10.png)

Pour Azure en quelques secondes, qu’il vous faudra un jour ou une semaine ou plus pour accomplir dans l’environnement local. Et étant donné que vous pouvez tout aussi facilement créer bases de données automatiquement dans un script ou à l’aide d’une API de gestion, vous pouvez dynamiquement augmenter en répartissant vos données sur plusieurs bases de données < o : p >, tant que votre application qui a été programmée. </o : p >

Il s’agit d’un exemple de notre modèle Platform-as-a-Service. Vous n’êtes pas obligé de gérer les serveurs, nous le faisons. Vous n’êtes pas obligé de vous inquiétez pas sur les sauvegardes, nous le faisons. Il est en cours d’exécution dans une haute disponibilité, les données dans la base de données sont automatiquement répliquées sur trois serveurs. Si un ordinateur meurt, nous basculent automatiquement et vous ne perdez aucune donnée. Le serveur est mis à jour régulièrement, vous n’avez pas besoin de vous inquiétez pas à ce sujet.

Cliquez sur un bouton et vous obtenez la chaîne de connexion exact, vous avez besoin et que vous pouvez commencer immédiatement à l’aide de la nouvelle base de données.

![Chaînes de connexion](data-storage-options/_static/image11.png)

Le tableau de bord vous montre l’historique de connexion et la quantité de stockage utilisée.

![Tableau de bord de base de données SQL](data-storage-options/_static/image12.png)

Vous pouvez gérer des bases de données dans le portail ou à l’aide des outils SQL Server, vous êtes déjà familiarisé avec, notamment SQL Server Management Studio (SSMS) et les outils de Visual Studio, l’Explorateur d’objet de SQL Server (SSOX) et l’Explorateur de serveurs.

![SSOX](data-storage-options/_static/image13.png)

Un autre avantage est le modèle de tarification. Vous pouvez démarrer le développement avec une base de données gratuite de 20 Mo et une base de données de production commence à environ 5 $ par mois. Vous payez uniquement pour la quantité de données que vous stockez réellement dans la base de données, pas la capacité maximale. Vous n’êtes pas obligé d’acheter une licence.

Base de données SQL est facile de mise à l’échelle. Pour l’application Fix It, la base de données que nous créons dans notre script d’automatisation est limitée à 1 Go. Si vous souhaitez mettre à l’échelle jusqu'à 150 Go, vous pouvez accéder au portail et modifier ce paramètre ou exécuter une commande de l’API REST, et en quelques secondes, vous avez une base de données de 150 Go que vous pouvez déployer des données dans.

![Tailles et les éditions de base de données SQL](data-storage-options/_static/image14.png)

C’est la puissance du cloud pour mettre en place infrastructure rapidement et facilement et l’utiliser immédiatement.

L’application Fix It utilise deux bases de données SQL, un pour l’appartenance (authentification et autorisation) et l’autre pour les données, et c’est tout ce que vous avez à faire pour configurer et mettre à l’échelle. Vous avez vu précédemment comment approvisionner les bases de données via des scripts Windows PowerShell, et maintenant nous avons également vu combien il est facile à faire dans le portail.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework au lieu de l’accès direct de la base de données à l’aide d’ADO.NET

L’application Fix It accède à ces bases de données à l’aide d’Entity Framework, ORM (mappeur objet-relationnel) de recommandée par Microsoft pour les applications .NET. Un ORM est un excellent outil qui facilite la productivité des développeurs, mais la productivité se fait au détriment de dégradation des performances dans certains scénarios. Dans une application cloud du monde réel ne sont pas de faire un choix entre l’utilisation d’EF ou à l’aide d’ADO.NET directement, vous allez utiliser les deux. La plupart du temps lorsque vous écrivez du code qui fonctionne avec la base de données, obtenir des performances maximale n’est pas critique et vous pouvez tirer parti de la codification simplifiée et les tests que vous obtenez avec Entity Framework. Dans les situations où la surcharge de EF entraînerait des performances inacceptables, vous pouvez écrire et exécuter vos propres requêtes à l’aide d’ADO.NET, dans l’idéal, en appelant des procédures stockées.

Quelle que soit la méthode utilisée pour accéder à la base de données que vous souhaitez réduire de « verbosité » autant que possible. En d’autres termes, si vous pouvez obtenir toutes les données que vous avez besoin dans une plus grande requête jeu de résultats plutôt que des dizaines ou des centaines de petits, qui est généralement préférable d’utiliser. Par exemple, si vous avez besoin pour les étudiants de liste et les cours qu’ils sont inscrits dans, il est généralement préférable obtenir toutes les données dans une jointure de requêtes plutôt que l’obtention des étudiants dans une seule requête et l’exécution des requêtes distinctes pour les cours de chaque étudiant.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bases de données SQL et Entity Framework dans l’application Fix It

Dans l’application Fix It le `FixItContext` classe qui dérive d’Entity Framework `DbContext` classe, identifie la base de données et spécifie les tables dans la base de données. Le contexte spécifie un jeu d’entités (table) pour les tâches, et le code passe dans le contexte le nom de chaîne de connexion. Ce nom fait référence à une chaîne de connexion qui est définie dans le fichier Web.config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La chaîne de connexion dans le *Web.config* fichier est nommé appdb (ici pointant vers la base de données de développement local) :

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework crée un *FixItTasks* table basée sur les propriétés incluses dans le `FixItTask` classe d’entité. Il s’agit d’une simple classe POCO (Plain Old CLR Object), ce qui signifie qu’il n’héritent ou avez des dépendances sur Entity Framework. Mais Entity Framework sait comment créer une table basée sur celui-ci et exécutez les opérations CRUD (créer-read-update-delete) avec lui.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Table de FixItTasks](data-storage-options/_static/image15.png)

L’application Fix It inclut une interface de référentiel qu’il utilise pour les opérations CRUD utilisation de la banque de données.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Notez que les méthodes de référentiel sont toutes asynchrones, afin de tout accès aux données peut être effectuée d’une manière totalement asynchrone.

L’implémentation de référentiel appelle les méthodes async Entity Framework pour travailler avec les données, y compris LINQ interroge ainsi que pour insérer, mettre à jour et les opérations de suppression. Voici un exemple de code pour la recherche d’une tâche Fix It.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Vous remarquerez il existe également certaines minutage et code de journalisation erreur ici, nous allons étudier que plus tard dans le [chapitre de surveillance et télémétrie](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Choix de la base de données SQL (PaaS) par rapport à SQL Server dans une machine virtuelle (IaaS)

Un avantage sur SQL Server et de la base de données SQL Azure est que le modèle de programmation de base pour chacun d’eux est identique. Vous pouvez utiliser la plupart des mêmes compétences dans les deux environnements. Vous pouvez même utiliser une base de données SQL Server dans le développement et une instance de base de données SQL dans le cloud, qui est le paramétrage de l’application Fix It.

Comme alternative, vous pouvez exécuter le même serveur SQL dans le cloud que vous exécutez en local en l’installant sur des machines virtuelles IaaS. Pour certaines applications héritées, le SQL Server en cours d’exécution dans une machine virtuelle peut être une meilleure solution. Une base de données SQL Server s’exécutant sur une machine virtuelle dédiée, il a plusieurs ressources sont disponibles pour qu’une base de données de base de données SQL qui s’exécute sur un serveur partagé. Cela signifie que d’une base de données SQL Server peut être plus grande et toujours performantes. En règle générale, plus la taille de la base de données et de la taille de la table, mieux le cas d’usage fonctionne pour la base de données SQL (PaaS).

Voici quelques conseils sur le choix entre les deux modèles.

| Base de données SQL Azure (PaaS) | SQL Server sur une Machine virtuelle (IaaS) |
| --- | --- |
| **Les professionnels de le** -vous n’êtes pas obligé de créer ou gérer des machines virtuelles, mettre à jour ou correctifs du système d’exploitation ou de SQL ; Azure fait pour vous. -Haute disponibilité intégrée, avec un contrat SLA de niveau de base de données. -Faible coût total de possession (TCO), car vous payez uniquement ce que vous utilisez (aucune licence n’est requise). -La bonne pour gérer un grand nombre de bases de données plus petits (&lt;= 500 Go chacun). -Permet de créer dynamiquement des nouvelles bases de données pour activer la montéent en puissance. | ***Les professionnels de le*** - fonctionnalité compatibles avec on-premises SQL Server. -Vous pouvez implémenter SQL Server [haute disponibilité via AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) dans 2 + des machines virtuelles, avec un contrat SLA de niveau de la machine virtuelle. -Vous avez le contrôle total sur la gestion de SQL. -Vous pouvez réutiliser déjà propriétaire ou de payer à l’heure pour l’une des licences SQL. -Appropriés pour gérer des moins mais plus volumineux (1 To +) bases de données. |
| **Inconvénients** -certaines fonctionnalités des écarts par rapport à la version locale SQL Server (un manque de [intégration du CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [prise en charge la compression](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.)-limite de taille de base de données de 500 Go. | ***Inconvénients*** - mises à jour/correctifs (système d’exploitation et SQL) sont de votre responsabilité - création et gestion de bases de données relèvent de votre responsabilité - disque IOPS (opérations d’entrée/sortie par seconde), limité à environ 8 000 (via les lecteurs de 16 données). |

Si vous souhaitez utiliser SQL Server sur une machine virtuelle, vous pouvez utiliser votre propre licence SQL Server, ou vous pouvez payer pour l’une par heure. Par exemple, dans le portail ou via l’API REST, vous pouvez créer une nouvelle machine virtuelle à l’aide d’une image de SQL Server.

![Créer une machine virtuelle avec SQL Server](data-storage-options/_static/image16.png)

![Liste des images de machine virtuelle SQL Server](data-storage-options/_static/image17.png)

Lorsque vous créez une machine virtuelle avec une image de SQL Server, nous calculons le coût de licence de SQL Server à l’heure en fonction de votre utilisation de la machine virtuelle. Si vous avez un projet qui ne va s’exécuter pendant quelques mois, il est plus économique payer par heure. Si vous pensez que votre projet va dernière depuis des années, il est plus économique d’acheter la licence comme vous le faites normalement.

## <a name="summary"></a>Récapitulatif

Le cloud computing, ce qui la rend pratique pour combiner et approches de stockage de données de correspondance pour mieux répondre aux besoins de votre application. Si vous créez une nouvelle application, qu’après mûre réflexion sur les questions répertoriées ici afin de prendre des approches qui continueront à fonctionner correctement lorsque votre application se développe. Le [chapitre suivant](data-partitioning-strategies.md) explique certaines stratégies de partitionnement que vous pouvez utiliser pour combiner plusieurs approches de stockage de données.

## <a name="resources"></a>Ressources

Pour plus d'informations, voir les ressources ci-dessous.

Choix d’une plate-forme de base de données :

- [Accès aux données pour les Solutions hautement évolutives : utilisation de SQL, NoSQL et la persistance POLYGLOTTE](http://aka.ms/dag-doc). Livre électronique par Microsoft Patterns and Practices qui explore en profondeur les différents types de données stocke disponibles pour les applications cloud.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/ff898430.aspx). Consultez Data Consistency Primer, réplication des données et obtenir des conseils de synchronisation, le modèle de Table d’Index, le modèle de vue matérialisée.
- [BASE : Acid Alternative](http://queue.acm.org/detail.cfm?id=1394128). L’article sur les compromis entre la cohérence des données et l’évolutivité.
- [Bases de sept données dans sept semaines : un Guide pour les bases de données modernes et du mouvement NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Livre par Eric Redmond et Jim R. Wilson. Fortement recommandé pour l’introduction de vous-même à l’ensemble des plateformes de stockage de données disponibles aujourd'hui.

Choix entre SQL Server et de la base de données SQL :

- [Version d’évaluation Premium pour obtenir des conseils de base de données SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introduction à SQL Database Premium et conseils pour le choisir sur les éditions de base de données Web et Business SQL.
- [Instructions et Limitations (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Page du portail qui établit un lien vers la documentation sur les limitations de base de données SQL, y compris un qui se concentre sur les fonctionnalités de SQL Server de cette base de données SQL ne prend en charge.
- [SQL Server dans Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Page du portail qui établit un lien vers la documentation sur l’exécution de SQL Server dans Azure.
- [Scott Guthrie explique les bases de données SQL dans Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). présentation vidéo de 6 minutes à base de données SQL par Scott Guthrie.
- [Modèles d’application et les stratégies de développement pour SQL Server dans Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

À l’aide d’Entity Framework et la base de données SQL dans une application Web ASP.NET

- [Bien démarrer avec EF 6 avec MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Neuf-série de didacticiels qui vous guide à travers la création d’une application MVC qui utilise EF et déploie la base de données vers Azure et SQL Database.
- [Déploiement de Web ASP.NET à l’aide de Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Douze-série de didacticiels qui prendra plus en détail comment déployer une base de données en utilisant EF Code First.
- [Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et base de données SQL vers un Site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Didacticiel pas à pas qui vous guide à travers la création d’une application web qui utilise l’authentification, stocke les tables de l’application dans la base de données d’appartenance, modifie le schéma de base de données et déploie l’application sur Azure.
- [ASP.NET Data Access Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282414). Liens vers des ressources pour travailler avec EF et de la base de données SQL.

À l’aide de MongoDB sur Azure :

- [MongoLab - MongoDB sur Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Page du portail pour obtenir une documentation sur l’exécution de MongoDB sur Azure.
- [Créer un site web Azure qui se connecte à MongoDB exécuté sur une machine virtuelle dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Didacticiel pas à pas qui montre comment utiliser une base de données dans une application web ASP.NET.

HDInsight (Hadoop sur Azure) :

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portail vers la documentation de HDInsight sur le [Azure](https://azure.microsoft.com/) site Web.
- [Hadoop et HDInsight : Big Data dans Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Article de MSDN Magazine de Bruno Terkaly et Ricardo Villalobos, présentation de Hadoop sur Azure.
- [Microsoft Patterns and Practices - conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Modèle MapReduce de voir.

> [!div class="step-by-step"]
> [Précédent](single-sign-on.md)
> [Suivant](data-partitioning-strategies.md)
