---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: Déploiement d’une base de données (VB) | Microsoft Docs
author: rick-anderson
description: Déploiement d’une application web ASP.NET implique l’obtention de fichiers nécessaires et aux ressources à partir de l’environnement de développement à l’environnement de production. Pour da...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: c49b963cb5cfc40d8a0b03eb3ca722e3b789eab2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836273"
---
<a name="deploying-a-database-vb"></a>Déploiement d’une base de données (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> Déploiement d’une application web ASP.NET implique l’obtention de fichiers nécessaires et aux ressources à partir de l’environnement de développement à l’environnement de production. Pour les applications web orientées données, cela inclut le schéma de base de données et les données. Ce didacticiel est le premier d’une série qui explore les étapes nécessaires pour déployer la base de données à partir de l’environnement de développement en production.


### <a name="introduction"></a>Introduction

Déploiement d’une application web ASP.NET implique l’obtention de fichiers nécessaires et aux ressources à partir de l’environnement de développement à l’environnement de production. Au cours des six derniers didacticiels, nous avons vu déployant une simple application web de critiques de livres. Ce site de démonstration était composé d’un nombre de ressources côté serveur - pages ASP.NET, les fichiers de configuration, un `Web.sitemap` fichier et ainsi de suite - ainsi que des ressources côté client, comme les images et des fichiers CSS. Mais qu’en est-il des piloté par les données des applications web ? Quelles étapes supplémentaires sont nécessaires pour déployer une application web qui utilise une base de données ?

Sur les didacticiels plusieurs suivants, nous étudierons les étapes nécessaires pour déployer une application web pilotée par les données. Ce didacticiel commence par examiner comment obtenir un schéma s de base de données et du contenu à partir de l’environnement de développement à l’environnement de production, tandis que le didacticiel suivant examine les modifications de configuration nécessaires. Après que nous allons explorer les défis du déploiement d’une base de données qui utilise les Services d’Application (l’appartenance, rôles, profil et ainsi de suite).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examen de l’Application Web de révisions de mis à jour

Afin d’illustrer le déploiement d’une application web pilotée par les données, je ve mis à jour l’application web de critiques de livres à partir d’un site Web simple, statique vers un piloté par les données. Comme auparavant, il existe deux versions de l’application dans ce téléchargement didacticiel s : une qui utilise le modèle de projet d’Application Web et l’autre qui utilise le modèle de projet de Site Web.

La mise à jour critiques de livres web application utilise un [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) base de données, qui est stocké dans le site s `App_Data` dossier (`~/App_Data/Reviews.mdf`). Si vous avez installé sur votre ordinateur SQL Server 2008 la démonstration doit s’exécuter sans erreur. Si vous avez une version antérieure de SQL Server, vous pouvez soit installer le gratuit SQL Server 2008 Express Edition, ou vous pouvez utiliser la base de données de téléchargement des scripts disponibles dans ce didacticiel s pour créer la base de données vous-même.

Le `Reviews.mdf` base de données contient quatre tables :

- `Genres` -inclut un enregistrement pour chaque genre, telles que la technologie, Fiction et entreprise.
- `Books` -inclut un enregistrement pour chaque révision, avec des colonnes comme `Title`, `GenreId`, `ReviewDate`, et `Review`, entre autres.
- `Authors` -inclut des informations sur chaque auteur qui a contribué à un livre de révision.
- `BooksAuthors` -une table de jointure plusieurs-à-plusieurs qui spécifie les auteurs ont écrit quels livres.
  

La figure 1 illustre un diagramme de ces quatre tables.


[![Les opérations de mappage d’Application Web livre évaluations de base de données sont composée de quatre Tables.](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**Figure 1**: l’Application Web de révisions livre s de base de données est composée de quatre Tables ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image3.jpg))


La version précédente du site Web critiques de livres avait une page ASP.NET distincte pour chaque livre. Par exemple, il a été une page nommée `~/Tech/TYASP35.aspx` qui contenait la revue pour *enseigner vous-même ASP.NET 3.5 des dernières 24 heures*. Cette nouvelle version piloté par les données du site Web a les révisions stockées dans la base de données et une page ASP.NET unique, Review.aspx?ID=*bookId*, qui affiche la révision pour le catalogue spécifié. De même, il existe un Genre.aspx?ID=*genreId* page qui répertorie les livres de révision du genre spécifié.

Les figures 2 et 3 affichent le `Genre.aspx` et `Review.aspx` pages en action. Notez l’URL dans la barre d’adresse pour chaque page. Dans la Figure 2 il s Genre.aspx ? ID = 85d164ba-1123-4 c 47-82a0-c8ec75de7e0e. Étant donné que 85d164ba-1123-4c47-82a0-c8ec75de7e0e est le `GenreId` valeur pour le genre de technologie, les lectures de titre de page s « Technologie révisions » et la liste à puces énumère ces avis sur le site qui relèvent de ce genre.


[![La Page de Genre de technologie](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**Figure 2**: la Page de Genre de technologie ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image6.jpg))


[![La révision de Teach Yourself ASP.NET 3.5 dans 24 heures](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**Figure 3**: la révision pour *enseigner vous-même ASP.NET 3.5 des dernières 24 heures* ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image9.jpg))


L’application web passe en revue du livre inclut également une section d’administration où les administrateurs peuvent ajouter, modifier et supprimer des genres, révisions et créer des informations. Actuellement, n’importe quel visiteur peut accéder à la section administration. Dans un futur didacticiel nous ajouter la prise en charge pour les comptes d’utilisateur et autoriser uniquement les utilisateurs autorisés dans les pages d’administration.

Si vous téléchargez l’application de critiques de livres, n’oubliez pas que son objectif est de montrer le déploiement d’une application orientée données. Il ne présente pas les meilleures pratiques en ce qui concerne la conception de l’application. Par exemple, il n’existe aucun distinct couche DAL (Data Access) ; les pages ASP.NET communiquent directement avec la base de données via le contrôle SqlDataSource ou le code ADO.NET dans leurs classes de code-behind. Pour une plus approfondi de la création d’applications pilotées par les données à l’aide d’une architecture à plusieurs niveaux, reportez-vous à mon [ *utilisation des données* didacticiels](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bases de données sur le développement et la Production

Lorsque vous démarrez le développement sur une application web pilotée par les données, vous devez spécifier une chaîne de connexion de base de données, qui fournit des détails de l’application sur la façon de se connecter à la base de données. Cette chaîne de connexion spécifie, entre autres choses, le serveur de base de données, le nom de la base de données et les informations de sécurité. En règle générale, la base de données utilisé par l’application pendant le développement est différent de celui de la base de données utilisée quand il s en production. Il existe de nombreux avantages liés à l’aide de différentes bases de données pour le développement et production. Ne pas avoir une autre base de données dans des moyens de développement vous ont à vous soucier d’accidentellement la modification ou la suppression des données actives. Il vous permet également de placer dans les données de test factice ou apporter des modifications avec rupture au modèle de données sans avoir à vous soucier de l’impact sur l’application en production. L’inconvénient d’avoir une autre base de données dans les environnements de développement et de production est que lorsque l’application est déployée la base de données et toutes les modifications pertinentes sur le schéma de base de données s ou de données doivent également être déployées.

Avant le premier déploiement, il existe une seule instance de la base de données, et cette instance est dans l’environnement de développement. Lors du déploiement de l’application en production pour la première fois, nous devons non seulement copier les fichiers nécessaires côté serveur et côté client, mais également copier la base de données à partir de l’environnement de développement vers l’environnement de production. Il s’agit où nous en sommes droit maintenant avec l’application web critiques de livres - la base de données réside dans le `App_Data` dossier dans notre environnement de développement n’ayant pas encore été envoyées à l’environnement de production.

Une fois que l’application a été déployée, il existe deux copies de la base de données. Que l’application arrive à maturité, nouvelles fonctionnalités peuvent être ajoutées, ce qui nécessite une modification au modèle de données (telles que l’ajout de nouvelles colonnes aux tables existantes, apporter des modifications aux colonnes existantes, ajout de nouvelles tables et ainsi de suite). Lorsque l’application web est ensuite déployée, les modifications appliquées à la base de données dans l’environnement de développement dans la mesure où le dernier déploiement doit être appliqué à la base de données de production. Certaines stratégies pour la gestion de ce processus sont traitées dans un futur didacticiel. Ce didacticiel se concentre sur le déploiement de la base de données à partir de l’environnement de développement en production.

## <a name="deploying-the-database-to-the-production-environment"></a>Déploiement de la base de données dans l’environnement de Production

Le reste de ce didacticiel examine comment déployer la base de données à partir de l’environnement de développement à l’environnement de production. Si vous suivez vous avez besoin pour vous assurer que votre compte avec votre fournisseur d’hébergement web inclut Microsoft SQL Server database prend en charge. Vous devez également avoir des informations à portée de main, à savoir le nom du serveur de base de données, le nom de la base de données et le nom d’utilisateur et mot de passe utilisé pour se connecter à la base de données.

Comme indiqué précédemment dans ce didacticiel, la base de données du site Web s critiques de livres est une base de données SQL Server 2008 Express Edition stockée dans le `App_Data` dossier. Il serait, mettez au point de penser que déploiement d’une base de données serait simplement à copier le `App_Data` dossier à partir de l’environnement de développement à l’environnement de production. Toutefois, la plupart des fournisseurs d’hébergement web ne prennent pas en charge hébergement bases de données dans le `App_Data` dossier en raison des raisons de sécurité. Au lieu de cela, les hôtes web fournissent un compte sur un serveur de base de données SQL Server dans leur environnement. Déploiement de la base de données à partir de votre environnement de développement à l’environnement de production nécessite l’obtention de votre base de données enregistré sur votre serveur de base de données s web hôte.

Par conséquent, comment obtenir votre base de données à partir de l’environnement de développement à l’environnement de production ? Il existe plusieurs façons de procéder selon les services offerts par votre hôte web. Avec certains hôtes, tels que DiscountASP.NET, vous pouvez FTP une sauvegarde de la base de données ou le texte réel `.mdf` fichier à votre site Web et ensuite, restaurez le fichier de sauvegarde à partir du Panneau de configuration ou attacher la `.mdf` fichier sur le serveur de base de données SQL Server. Avec ces outils de déploiement de la base de données est aussi simple que la copie la `App_Data` dossier dans lequel l’environnement de production, puis l’attacher via le panneau de configuration. Il s’agit peut-être le moyen le plus simple et plus rapide pour publier votre base de données pour la première fois.

Une autre approche consiste à utiliser l’Assistant Publication de base de données. L’Assistant Publication de base de données est une application de bureau Windows qui génère les commandes SQL pour créer votre schéma s de base de données - les tables, procédures stockées, vues, fonctions définies par l’utilisateur et ainsi de suite - et, éventuellement, les données dans ses tables. Vous pouvez ensuite vous connecter à votre serveur web hôte fournisseur s de base de données via SQL Server Management Studio et puis exécutez ce script pour dupliquer la base de données de production. Mieux encore, si votre fournisseur d’hébergement web prend en charge Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) vous pouvez avoir le script généré par l’Assistant Publication de base de données exécutée automatiquement sur le serveur de base de données à votre place. Étant donné que l’Assistant Publication de base de données génère un script qui crée le schéma de base de données s et les données, il fonctionne indépendamment du fait que votre fournisseur d’hébergement web offre des fonctionnalités comme l’attachement à un chargé `.mdf` fichier.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Générer les commandes SQL pour créer le schéma de base de données et les données à l’aide de l’Assistant Publication de base de données

Laissez s guident dans l’utilisation de l’Assistant Publication de base de données à déployer la base de données critiques de livres en production. Si vous utilisez Visual Studio 2008 ou au-delà, l’Assistant Publication de base de données est déjà installé. Si vous utilisez Visual Studio 2005, vous devez d’abord [Téléchargez et installez](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) l’Assistant.

Ouvrez Visual Studio et accédez à la `Reviews.mdf` base de données. Si vous utilisez Visual Web Developer, accédez à l’Explorateur de base de données ; Si vous utilisez Visual Studio, utilisez l’Explorateur de serveurs. La figure 4 illustre le `Reviews.mdf` base de données dans l’Explorateur de base de données dans Visual Web Developer. Comme le montre la Figure 4, le `Reviews.mdf` base de données se compose de quatre tables, trois procédures stockées et une fonction définie par l’utilisateur.


[![Localisez la base de données dans l’Explorateur de base de données ou l’Explorateur de serveurs](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**Figure 4**: localiser la base de données dans l’Explorateur de base de données ou l’Explorateur de serveurs ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image12.jpg))


Avec le bouton droit sur le nom de la base de données et choisissez l’option « Publier au fournisseur » dans le menu contextuel. Cette action lance l’Assistant Publication de base de données (voir Figure 5). Cliquez sur Suivant pour passer au-delà de l’écran de démarrage.


[![L’écran d’accueil Assistant de publication de base de données](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**Figure 5**: l’écran démarrage Assistant Publication de base de données ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image15.jpg))


Le deuxième écran de l’Assistant répertorie les bases de données accessibles à l’Assistant Publication de base de données et vous permet de choisir s’il faut écrire tous les objets dans la base de données ou choisir les objets au script. Sélectionnez la base de données appropriée et laissez l’option « Script tous les objets dans la base de données sélectionnée » cochée.

> [!NOTE]
> Si vous obtenez l’erreur « Il y a aucun objet dans la base de données *databaseName* des types scriptables par cet Assistant » lorsque vous cliquez sur suivant dans l’écran illustré à la Figure 6, assurez-vous que le chemin d’accès à votre fichier de base de données n’est pas trop long. Comme indiqué dans [cet élément de discussion](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014) sur la page de projet Assistant Publication de base de données, cette erreur peut se produire si le chemin d’accès au fichier de base de données est trop long.


[![L’écran d’accueil Assistant de publication de base de données](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**Figure 6**: l’écran démarrage Assistant Publication de base de données ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image18.jpg))


À partir de l’écran suivant vous pouvez générer un fichier de script ou, si votre hôte web prend en charge, publier la base de données directement sur votre serveur web hôte fournisseur s de base de données. Comme le montre la Figure 7, j’ai le script écrit dans le fichier `C:\REVIEWS.MDF.sql`.


[![La base de données dans un fichier de script ou la publier directement sur votre fournisseur d’hébergement Web](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**Figure 7**: la base de données dans un fichier de Script ou la publier directement sur votre fournisseur d’hébergement Web ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image21.jpg))


L’écran suivant vous invite à entrer une variété d’options de script. Vous pouvez spécifier si le script doit inclure des instructions drop pour supprimer ces objets existants. Par défaut est True, ce qui est correct lors du déploiement d’une base de données pour la première fois. Vous pouvez également spécifier si la base de données cible est SQL Server 2000, SQL Server 2005 ou SQL Server 2008. Enfin, vous pouvez indiquer s’il faut écrire le schéma et les données, uniquement les données, ou uniquement le schéma. Le schéma est la collection d’objets de base de données, les tables, procédures stockées, vues et ainsi de suite. Les données sont les informations qui résident dans les tables.

Comme l’illustre la Figure 8, j’ai avons l’Assistant configuré pour supprimer des objets de base de données existante, pour générer un script pour une base de données SQL Server 2008 et pour publier le schéma et les données.


[![Spécifiez la publication Options](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**Figure 8**: spécifiez les Options de publication ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image24.jpg))


Les deux écrans finales résument les actions qui sont sur le point d’entreprendre, puis afficher l’état de l’écriture de scripts. Le résultat net de l’exécution de l’Assistant est que nous avons un fichier de script qui contient les commandes SQL nécessaires pour créer la base de données sur la production et le remplir avec les mêmes données sur le développement.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>L’exécution des commandes SQL sur la base de données des environnement de Production

Maintenant que nous avons le script qui contient les commandes SQL pour créer la base de données et toutes ses données qui reste est d’exécuter le script sur la base de données de production. Certains fournisseurs d’hébergement web offrent une zone de texte dans leur panneau où vous pouvez entrer des commandes SQL à exécuter sur votre base de données. Si vous avez un fichier de script très volumineux, cette option peut ne pas fonctionner (le `REVIEWS.MDF.sql` fichier de script est plus 425 Ko, par exemple).

Une meilleure approche consiste à se connecter directement au serveur de base de données de production à l’aide de SQL Server Management Studio (SSMS). Si vous avez un non - Express Edition de SQL Server installée sur votre ordinateur puis que vous avez déjà installé de SSMS. Sinon, vous pouvez [Téléchargez et installez](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) une copie gratuite de SQL Server Management Studio Express Edition.

Lancez SSMS et connectez-vous à votre serveur de base de données web hôte s en utilisant les informations fournies par votre fournisseur d’hébergement web.


[![Se connecter à votre serveur Web hôte fournisseur s de base de données](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**Figure 9**: se connecter à votre fournisseur d’hébergement Web s serveur de base de données ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image27.jpg))


Développez l’onglet bases de données et recherchez votre base de données. Cliquez sur le bouton Nouvelle requête dans le coin supérieur gauche de la barre d’outils, collez les commandes SQL à partir du fichier de script créé par l’Assistant Publication de base de données, puis cliquez sur le bouton d’exécution pour exécuter ces commandes sur le serveur de base de données de production. Si votre fichier de script est particulièrement important, il peut prendre plusieurs minutes pour exécuter les commandes.


[![Se connecter à votre serveur Web hôte fournisseur s de base de données](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**Figure 10**: se connecter à votre fournisseur d’hébergement Web s serveur de base de données ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image30.jpg))


S résume-t-elle est ! À ce stade, la base de données de développement a été dupliqué en production. Si vous actualisez la base de données dans SSMS, vous devez voir les nouveaux objets de base de données. Figure 11 illustre les tables de base de données de production, les procédures stockées et les fonctions définies par l’utilisateur, qui reflètent ceux sur la base de données de développement. Et étant donné que nous lui avons demandé l’Assistant Publication de base de données pour publier les données, les tables de s de base de données de production ont les mêmes données que les tables de s de base de données de développement au moment de que l’Assistant a été exécuté. La figure 12 illustre les données dans le `Books` table sur la base de données de production.


[![Les objets de base de données ont été dupliqués sur la base de données de Production](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**Figure 11**: la base de données objets ont été dupliqués sur la base de données de Production ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image33.jpg))


[![La base de données de Production contient les mêmes données que la base de données de développement](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**Figure 12**: la base de données de Production contient les mêmes données que sur la base de données de développement ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image36.jpg))


À ce stade, nous avons déployé uniquement la base de données de développement en production. Nous n’avons pas encore examiné de déploiement de l’application web elle-même ou examiner les modifications de configuration nécessaires pour que l’application de production à utiliser la base de données de production. Nous aborderons ces problèmes dans le didacticiel suivant !

## <a name="summary"></a>Récapitulatif

Déploiement d’une application web pilotée par les données nécessite la copie de la base de données utilisée pendant le développement à l’environnement de production. Plusieurs fournisseurs d’hébergement web offrent des outils permettant de simplifier le processus de déploiement d’une base de données. Par exemple, avec DiscountASP.NET, vous pouvez FTP votre base de données `.mdf` fichier (ou une sauvegarde), puis attachez la base de données sur le serveur de base de données à partir du panneau. Une autre option qui fonctionne indépendamment de quelles fonctionnalités offerts par votre fournisseur hôte web est outil Assistant Publication de base de données Microsoft s, ce qui génère un script de commandes SQL pour créer le schéma de s de base de données de développement et les données. Une fois que ce script a été généré, vous pouvez l’exécuter sur la base de données de production.

Maintenant que la base de données de critiques de livres web application s est sur la production, nous pouvons déployer l’application. Toutefois, les informations de configuration web application s spécifient la chaîne de connexion à la base de données, et cette chaîne de connexion fait référence à la base de données de développement. Nous devons mettre à jour ces informations de chaîne de connexion lors du déploiement du site de production. Le didacticiel suivant examine ces différences de configuration et décrit les étapes nécessaires pour publier le site pilotés par les données de critiques de livres en production.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Télécharger la base de données Microsoft SQL Server 1.1 d’Assistant Publication](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Télécharger Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Précédent](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [Suivant](configuring-the-production-web-application-to-use-the-production-database-vb.md)
