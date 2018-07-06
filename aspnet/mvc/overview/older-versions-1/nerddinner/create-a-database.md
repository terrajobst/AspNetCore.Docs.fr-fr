---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Créer une base de données | Microsoft Docs
author: microsoft
description: Étape 2 montre les étapes pour créer la base de données contenant tous le dîner et RSVP des données pour notre application NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 2a2ed8c91aa3ec0da63cd8e8686ff601f6e66374
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818964"
---
<a name="create-a-database"></a>Créer une base de données
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 2 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 2 montre les étapes pour créer la base de données contenant tous le dîner et RSVP des données pour notre application NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner étape 2 : Création de la base de données

Nous allons utiliser une base de données pour stocker toutes les données dîner et RSVP pour notre application NerdDinner.

Les étapes ci-dessous montrent la création de la base de données à l’aide de l’édition gratuite de SQL Server Express (que vous pouvez facilement installer à l’aide de la version 2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Tout le code que nous allons écrire fonctionne avec SQL Server Express et SQL Server complète.

### <a name="creating-a-new-sql-server-express-database"></a>Création d’une nouvelle base de données SQL Server Express

Nous allons commencer en cliquant sur notre projet web, puis sélectionnez le **Add -&gt;un nouvel élément** commande de menu :

![](create-a-database/_static/image1.png)

Cela fera apparaître la boîte de dialogue « Ajouter un nouvel élément » de Visual Studio. Nous allons filtrer par la catégorie « Données » et sélectionnez le modèle d’élément « Base de données SQL Server » :

![](create-a-database/_static/image2.png)

Nous allons nommer la base de données SQL Server Express, que nous souhaitons créer « NerdDinner.mdf » et appuyez sur OK. Visual Studio vous demande si nous souhaitons ajouter ce fichier à notre \App\_répertoire de données (qui est un répertoire déjà le programme d’installation avec accès en lecture et écriture des ACL de sécurité) :

![](create-a-database/_static/image3.png)

Nous cliquons sur « Oui » et notre nouvelle base de données est créé et ajouté à l’Explorateur de solutions :

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Création de Tables au sein de notre base de données

Nous disposons désormais d’une nouvelle base de données vide. Nous allons y ajouter des tables.

Pour ce faire, nous allons vous accédez à la fenêtre d’onglet « Explorateur de serveurs » dans Visual Studio, qui permet de gérer les serveurs et bases de données. Bases de données SQL Server Express, stockées dans le \App\_dossier de données de notre application apparaissent automatiquement dans l’Explorateur de serveurs. Nous pouvons éventuellement utiliser l’icône « Se connecter à la base de données » en haut de la fenêtre « Explorateur de serveurs » pour ajouter d’autres bases de données SQL Server (locales et distants) à la liste ainsi :

![](create-a-database/_static/image5.png)

Nous allons ajouter deux tables à notre base de données NerdDinner : un pour stocker nos dîners et l’autre pour effectuer le suivi de protocole RSVP acceptations leur. Nous pouvons créer des tables en cliquant sur le dossier « Tables » au sein de notre base de données et en choisissant la commande de menu « Ajouter une nouvelle Table » :

![](create-a-database/_static/image6.png)

Un concepteur de tables qui nous permet de configurer le schéma de la table s’ouvre. Pour notre table « Dîners », nous allons ajouter 10 colonnes de données :

![](create-a-database/_static/image7.png)

Nous voulons la colonne « DinnerID » soit une clé primaire unique pour la table. Nous pouvons le configurer en cliquant sur la colonne « DinnerID » et en choisissant l’élément de menu « Définir la clé primaire » :

![](create-a-database/_static/image8.png)

En plus de rendre DinnerID une clé primaire, nous voulons également configurez-le en tant que « identity » colonne dont la valeur est incrémentée automatiquement comme de nouvelles lignes de données sont ajoutées à la table (ce qui signifie que la première ligne dîner insérée aura un DinnerID 1, le deuxième inséré ligne aura un DinnerID de 2, etc.).

Nous pouvons ce faire, sélectionnez la colonne « DinnerID », puis utilisez l’éditeur « Propriétés de colonne » pour définir la propriété « (identité est) » sur la colonne « Oui ». Nous allons utiliser les valeurs par défaut de l’identité standard (commencent à 1 et incrémente de 1 sur chaque nouvelle ligne dîner) :

![](create-a-database/_static/image9.png)

Nous allons ensuite enregistrer notre table en tapant Ctrl + S ou à l’aide de la **fichier -&gt;enregistrer** commande de menu. Cela vous invite à nous pour nommer la table. Nous nommerons « Dîners » :

![](create-a-database/_static/image10.png)

Notre nouvelle table dîners puis apparaîtront dans notre base de données dans l’Explorateur de serveurs.

Nous allons puis répétez les étapes ci-dessus et créer une table « RSVP ». Cette table avec comporte 3 colonnes. Nous le programme d’installation de la colonne RsvpID comme clé primaire et également rendre une colonne d’identité :

![](create-a-database/_static/image11.png)

Nous allons enregistrer et attribuez-lui le nom « RSVP ».

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configuration d’une relation de clé étrangère entre les Tables

Maintenant, nous avons deux tables au sein de notre base de données. Notre dernière étape de conception de schéma sera pour configurer une relation « un-à-plusieurs » entre ces deux tables – afin que nous pouvons associer chaque ligne Dinner à zéro ou plusieurs lignes RSVP qui s’y appliquent. Nous le ferons en configurant la RSVP colonne du tableau « DinnerID » pour avoir une relation de clé étrangère à la colonne « DinnerID » dans la table « Dîners ».

Pour ce faire, nous allons ouvrir le tableau RSVP dans le Concepteur de tables en double-cliquant dessus dans l’Explorateur de serveurs. Nous allons ensuite sélectionner la colonne « DinnerID » qu’il contient, avec le bouton droit, puis choisissez la commande de menu contextuel « Relationshps... » :

![](create-a-database/_static/image12.png)

Cela fera apparaître une boîte de dialogue que nous pouvons utiliser pour les relations entre les tables d’installation :

![](create-a-database/_static/image13.png)

Nous allons cliquez sur le bouton « Ajouter » pour ajouter une nouvelle relation à la boîte de dialogue. Une fois qu’une relation a été ajoutée, nous allons développez le nœud d’arborescence « Tables et spécification de colonne » dans la grille des propriétés à droite de la boîte de dialogue, puis cliquez sur le bouton «... » à droite de celui-ci :

![](create-a-database/_static/image14.png)

En cliquant sur le bouton «... » s’affiche une autre boîte de dialogue qui permet de spécifier les tables et les colonnes sont impliqués dans la relation, ainsi que nous permettent de nommer la relation.

Nous modifie la Table de clé primaire pour être « Dîners » et sélectionnez la colonne « DinnerID » dans la table dîners comme clé primaire. Notre table RSVP sera la table de clé étrangère et le protocole RSVP. Colonne de DinnerID sera associé en tant que la clé étrangère :

![](create-a-database/_static/image15.png)

Maintenant chaque ligne dans la table RSVP à associer à une ligne dans la table dîner. SQL Server sera maintenir l’intégrité référentielle pour nous et nous empêche d’ajouter une nouvelle ligne RSVP si elle ne pointe pas vers une ligne dîner valide. Il nous empêchera également de supprimer une ligne dîner s’il existe toujours RSVP lignes faisant référence à celui-ci.

### <a name="adding-data-to-our-tables"></a>Ajout de données à nos Tables

Nous allons terminer en ajoutant des exemples de données à la table dîners. Nous pouvons ajouter des données à une table en cliquant dessus dans l’Explorateur de serveurs et en choisissant la commande « Afficher les données de Table » :

![](create-a-database/_static/image16.png)

Nous allons ajouter quelques lignes de données dîner que nous pouvons utiliser plus tard que nous commencions à implémenter l’application :

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Étape suivante

Nous avons terminé la création de notre base de données. Nous allons maintenant créer des classes de modèle que nous pouvons utiliser pour interroger et mettre à jour.

> [!div class="step-by-step"]
> [Précédent](create-a-new-aspnet-mvc-project.md)
> [Suivant](build-a-model-with-business-rule-validations.md)
