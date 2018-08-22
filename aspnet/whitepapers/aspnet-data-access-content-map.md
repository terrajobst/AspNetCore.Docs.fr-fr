---
uid: whitepapers/aspnet-data-access-content-map
title: Accès aux données ASP.NET - ressources recommandées pour | Microsoft Docs
author: rick-anderson
description: Cette rubrique fournit des liens vers des ressources de documentation sur l’accès aux données dans les applications web ASP.NET, en utilisant Entity Framework et SQL Se principalement...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 6993c17c8de890cbaa40c619bcd20f494bfd2f90
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830175"
---
<a name="aspnet-data-access---recommended-resources"></a>Accès aux données ASP.NET - ressources recommandées
====================
> Cette rubrique fournit des liens vers des ressources de documentation sur l’accès aux données dans les applications web ASP.NET, principalement par à l’aide d’Entity Framework et SQL Server.
> 
> Si vous connaissez un excellent blog valider, [stackoverflow](http://stackoverflow.com) thread ou un autre lien qui serait utile, [nous envoyer un e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) avec le lien.
> 
> Dernière mise à jour 4 3 / / 2014


La rubrique contient les sections suivantes :

- [Mise en route avec l’accès aux données dans ASP.NET](#gettingstarted)
- [À l’aide d’Entity Framework](#ef)

    - [Tout d’abord à l’aide de Code d’Entity Framework](#cf)
    - [À l’aide d’Entity Framework Code First Migrations](#efcfmigrations)
    - [En utilisant Entity Framework Database First ou Model First (Concepteur d’Entity Framework)](#efdbf)
    - [Chargement des données associées dans Entity Framework (chargement différé, le chargement hâtif et le chargement explicite)](#efrelateddata)
    - [Optimisation des performances de Entity Framework](#optimizingef)
    - [Gestion des accès concurrentiels dans une Application Entity Framework](#efconcurrency)
    - [Documentation sur Entity Framework](#efbooks)
    - [Ressources supplémentaires de Entity Framework](#otherefresources)
- [Applications de liaison de données dans ASP.NET Web Forms](#wfdatabinding)

    - [À l’aide de Web Forms de liaison de modèle](#wfmodelbinding)
    - [À l’aide de Web Forms des contrôles de Source de données](#wfdsc)
    - [À l’aide de Web Forms des contrôles liés aux données et les Expressions de liaison de données](#wfdbc)
- [Utilisation des bases de données SQL Server](#sqlserver)

    - [Utilisation des bases de données SQL Server Express LocalDB](#sslocaldb)
    - [Utilisation des bases de données SQL Server Express](#sse)
    - [Utilisation de Windows Azure SQL Database](#ssdb)
    - [Choix entre SQL Server et Windows Azure SQL Database](#ssdbchoosing)
- [Utilisation des systèmes de gestion de base de données NoSQL](#nosql)
- [À l’aide de requêtes LINQ dans les Applications ASP.NET](#linq)
- [À l’aide de la structure de données dynamiques](#dd)
- [Sécurisation des accès aux données](#securing)
- [Optimisation des performances accès aux données](#optimizingdataaccess)
- [Déploiement d’une base de données](#deploying)
- [L’accès aux données via un Service Web](#webservice)
- [Ressources supplémentaires pour MSBuild](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Mise en route avec l’accès aux données dans ASP.NET

- [Options de stockage de données (création d’applications Cloud réalistes avec Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Chapitre du livre électronique sur le développement pour le cloud. Présente les bases de données NoSQL comme alternative que de nombreux développeurs familiarisés avec les bases de données relationnelles ont tendance à négliger. Présente des conseils sur ce qu’il faut penser lors du choix relationnelle ou NoSQL ou en choisissant une plateforme spécifique.
- [Options d’accès aux données ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Présentation d’options d’accès aux données pour les bases de données relationnelles pour ASP.NET et des conseils sur le choix des plateformes et accéder aux méthodes appropriées pour votre scénario.
- [Base de données relationnelle](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Si vous n’avez jamais travaillé avec des bases de données relationnelles, consultez cette page pour une introduction aux concepts et terminologie de base de données relationnelle. Pour une introduction à SQL Server en particulier consultez [des bases de données SQL Server](#sqlserver) plus loin dans cette rubrique.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>À l’aide d’Entity Framework

- [Approches de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Conseils sur la façon de choisir une Entity Framework développement approche Database First, Model First ou Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Tout d’abord à l’aide de Code d’Entity Framework
  

Les didacticiels suivants offrent des exemples d’applications téléchargeables :

- [Bien démarrer avec EF 6 avec MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Couvre un large éventail de scénarios Entity Framework Code First, y compris les Migrations et EF 6 fonctionnalités telles que la résilience des connexions et interception des commandes async. Il s’agit d’une version mise à jour de la [EF 5 / série de MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La série précédente contient un didacticiel sur le référentiel et des modèles d’unité de travail qui n’est pas inclus dans la nouvelle série.
- [Introduction à ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Couvre une plage de Code d’Entity Framework rétrécir les scénarios première mais fait un travail plus complète de la présentation des fonctionnalités de MVC.
- [Liaison de modèle et les Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Utilise Code First dans une application Web Forms.
- [Bien démarrer avec Web Forms ASP.NET 4.5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introduction à Web Forms avec une couverture de Code First. Utilise la liaison de modèle.
- [Store de musique MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utilise Code First dans une application MVC 3 de commerce électronique qui implémente également l’appartenance et l’autorisation. La version MVC et ASP.NET (authentification et autorisation) système d’appartenance utilisé ici sont obsolètes ; Pour plus d’informations à jour sur l’appartenance d’ASP.NET, consultez [ https://asp.net/identity ](https://asp.net/identity).

Autres ressources :

- [Entity Framework - Code First pour une base de données existante](https://msdn.microsoft.com/data/jj200620). MSDN. Vidéo et procédure pas à pas qui montre comment utiliser Code First avec une base de données existante.
- [Centre de développement de données - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Pour obtenir un guide pour la documentation d’Entity Framework qui a été créé et géré par l’équipe Entity Framework, consultez le [prise en main](https://msdn.microsoft.com/data/ee712907) lien.

Voir aussi [la documentation sur Entity Framework](#efbooks) et [supplémentaires ressources Entity Framework](#otherefresources) plus loin dans cette rubrique.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>À l’aide d’Entity Framework Code First Migrations
  

Plupart des didacticiels Code First répertoriées ci-dessus migrations de garde. Consultez également les ressources suivantes.

- [Déploiement de Web ASP.NET à l’aide de Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2-série de didacticiels qui montre comment utiliser les Migrations Code First pour déployer une base de données.
- [Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et base de données SQL sur un Site Web Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Comment utiliser des migrations pour déployer les données d’appartenance et d’application dans Azure.
- [Vue d’ensemble du déploiement de Web pour Visual Studio et ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consultez le **configuration du déploiement de base de données dans Visual Studio** section pour savoir comment les Migrations Code First s’intègre à des fonctionnalités de déploiement web Visual Studio.
- [Centre de développement de données - Migrations Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentation de Migrations de l’équipe Entity Framework.
- [Série de screencasts migrations](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog de l’Entity Framework). Trois des vidéos sur les rubriques avancées dans les Migrations Code First.
- [Code First Migrations avec les Sites ASP.NET Web Pages](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog de Mikesdotnetting). Montre comment utiliser les migrations Code First avec un site ASP.NET Web Pages en plaçant le contexte de données dans un projet de bibliothèque de classes Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>En utilisant Entity Framework Database First ou Model First (Concepteur d’Entity Framework)

- [Bien démarrer avec Entity Framework 6 Database First avec MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Exécuter un script dans l’Explorateur de serveurs pour créer une base de données et ensuite utiliser le Concepteur d’Entity Framework pour créer le modèle de données. Montre comment web CRUD simple de créer des pages et pour d’autres données que vous pouvez suivre un des didacticiels Code First étant donné que tous les workflows EF utilisent la même API DbContext de fonctions de gestion.

Les ressources suivantes sont plus anciens. Elles sont utiles si vous souhaitez utiliser la version 4.0 d’Entity Framework, et que vous souhaitez utiliser un contrôle de source de données pour la liaison de données dans une application Web Forms.

- [Bien démarrer avec Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Montre comment utiliser le **EntityDataSource** contrôle.
- [Continuer avec Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(montre comment utiliser le **ObjectDataSource** contrôle. Inclut un didacticiel sur la gestion d’accès concurrentiel, un didacticiel sur les performances d’EF et un didacticiel sur les nouveautés dans EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Gestion des données liées dans Entity Framework (chargement différé, le chargement hâtif et le chargement explicite)

- [Lecture des données connexes avec Entity Framework dans une Application ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, exemple d’application MVC. Les méthodes indiquées s’appliquent également à la liaison de modèle Web Forms et le flux de travail première base de données.
- [Centre de développement de données - chargement des entités connexes](https://msdn.microsoft.com/data/jj574232) (MSDN). Documentation de l’équipe Entity Framework sur le chargement des données associées.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimisation des performances d’Entity Framework

- [Advanced scénarios Entity Framework pour une Application ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Montre comment exécuter vos propres instructions SQL ou vos procédures stockées, comment désactiver la détection des modifications et comment désactiver la validation lors de l’enregistrement des modifications.
- [Considérations sur les performances d’Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considérations sur les performances (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Optimisation des performances avec Entity Framework dans une Application Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). S’applique à Entity Framework 4.0.
- Voir aussi [accès aux données ASP.NET optimisation](#optimizingdataaccess) plus loin dans cette rubrique.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Gestion des accès concurrentiels dans une Application Entity Framework

- [Gestion des accès concurrentiels avec Entity Framework dans une Application ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, API DbContext, à l’aide d’un exemple d’application MVC.
- [Centre de développement de données – modèles de concurrence optimiste](https://msdn.microsoft.cus/data/jj592904) (MSDN). Documentation de concurrence de l’équipe Entity Framework.
- [Gestion des accès concurrentiels avec Entity Framework dans une Application Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). S’applique à Entity Framework 4.0. La base de données tout d’abord, les API ObjectContext, à l’aide d’un exemple d’application Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Documentation sur Entity Framework

- [Programming Entity Framework : DbContext](http://shop.oreilly.com/product/0636920022237.do) par Julie Lerman et Rowan Miller.
- [Programming Entity Framework : Code First](http://shop.oreilly.com/product/0636920022220.do) par Julie Lerman et Rowan Miller.

Les deux de ces livres sont à jour avec les techniques recommandées en cours. Ils fournissent une présentation plus complète encore faciles à suivre pour Entity Framework que quoi que ce soit disponible sur Internet. Un autre livre, [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) par Julie Lerman, est plus grande et plus complète, mais il sont plus anciennes et la plupart des techniques il couvre ne sont plus la méthode recommandée pour utiliser Entity Framework. Consultez également la liste de livres recommandés par l’équipe Entity Framework chez [Data Developer Center - la documentation](https://msdn.microsoft.com/data/aa937716) sur le site MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Autres ressources Entity Framework

- [Blog de l’équipe Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Une des meilleures ressources pour les informations et les annonces des nouvelles améliorations les plus récentes. Pour les autres blogs associées à Entity Framework, consultez le blog favori à [bien démarrer avec Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Consultez le **Points de données** colonne, qui est fréquemment sur les rubriques liées à Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Applications de liaison de données dans ASP.NET Web Forms

- [Options d’accès aux données de formulaires Web ASP.NET](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>À l’aide de Web Forms de liaison de modèle

- [Liaison de modèle et les Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Série de didacticiels utilisant EF Code First.
- [Web Forms partie de liaison de modèle 1 : Sélection de données](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog Guthrie). Dans ces anciens billets de blog, la propriété qui est nommée actuellement ItemType s’appelait ModelType, mais sinon les informations qu’ils contiennent sont valides.
- [Web Forms modèle liaison 2ème partie : Filtrage des données](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog Guthrie).
- [Web Forms modèle liaison 3ème partie : Mise à jour et Validation](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog Guthrie).
- [Liaison de modèle Web Forms ASP.NET 4.5](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vidéo).
- [Liaison 1ère partie-sélection des données de modèle](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vidéo).
- [Modèle de liaison 2ème partie - filtrage](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vidéo).
- [Prise en main de Web Forms ASP.NET 4.5 - afficher les données des éléments et les détails](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>À l’aide de Web Forms des contrôles de Source de données

- [Contrôles de serveur Web de données Source](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Annonce de la version du fournisseur de données dynamique et de contrôle EntityDataSource pour Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de développement Web de Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>À l’aide de Web Forms des contrôles liés aux données et les Expressions de liaison de données

- [Liaison de modèle et les Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Série de didacticiels qui utilise EF Code First.
- [Prise en main de Web Forms ASP.NET 4.5 - afficher les données des éléments et les détails](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Fortement typé les contrôles de données](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog Guthrie).
- [Fortement typé les contrôles de données](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vidéo).
- [ASP.NET 4.5 contrôles de données typés de Web Forms fort](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vidéo).
- [Les contrôles serveur Web lié aux données](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Vue d’ensemble des Expressions de liaison de données](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Cette page couvre uniquement **Eval** et **lier**; il n’a pas été mis à jour pour inclure **élément** et **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Utilisation des bases de données SQL Server

- [Fonctionnalités de base de données SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Pour une introduction générale à une grande variété de sujets de SQL Server, consultez les entrées sous celle-ci dans la table des matières.
- [Éditions de SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un résumé des éditions de SQL Server disponibles, avec des liens vers plus d’informations sur chacune d’elles).
- [Chaînes de connexion SQL Server pour les Applications Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Pour les Applications Web ASP.NET à l’aide de SQL Server Compact](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server : Exemples de produits de base de données](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Exemples de bases de données AdventureWorks.
- [Installation des bases de données exemple](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Outre les méthodes indiquées ici, vous pouvez également télécharger un des exemples de fichiers .mdf l’application\_dossier de données d’un projet web, de convertir la base de données de la base de données locale et de créer une chaîne de connexion de base de données locale. Pour savoir comment procéder, consultez [Comment : mettre à niveau vers LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Consultez également les sections suivantes sur l’utilisation de SQL Server Express et la base de données locale et en choisissant entre SQL Server et SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Utilisation des bases de données SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). L’introduction de MSDN officielle pour la base de données locale.
- [Chaînes de connexion SQL Server pour les Applications Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Comment : mettre à niveau vers LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Comment migrer un fichier .mdf à partir d’une version antérieure de SQL Server Express à la base de données locale. Vous devez également exécuter ce processus si vous téléchargez un de la [bases de données SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Présentation de base de données locale, une instruction SQL Express amélioré](https://go.microsoft.com/fwlink/?LinkId=234375) (blog SQL Server Express). A plus d’informations sur pourquoi la base de données locale a été créée qu’est incluse dans MSDN.
- [LocalDB : Where is My Database ?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Savoir où sont créés les fichiers de base de données de base de données locale.
- [Utilisation de LocalDB avec IIS complet, partie 1 : profil utilisateur](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog SQL Server Express). Base de données locale n’est pas conçu pour fonctionner avec IIS. Cette série de billets de blog explique les problèmes et des solutions.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Utilisation des bases de données SQL Server Express

- [Chaînes de connexion SQL Server pour les Applications Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si vous utilisez le paramètre de chaîne de connexion AttachDBFileName avec SQL Server Express, consultez en particulier la section de l’Instance utilisateur de cette page.
- [Comment prendre possession de votre local SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog SQL Server Express). Un problème courant est de ne pas pouvoir travailler avec les bases de données SQL Server Express, car vous n’êtes pas administrateur sur l’instance SQL Server Express. Par défaut, seule la personne qui a installé SQL Server Express est un administrateur. Ce billet de blog explique comment effectuer vous-même un administrateur SQL Server Express si vous êtes un administrateur sur l’ordinateur.
- [Mon application de web ASP.NET peut utiliser d’une base de données SQL Server Express en production ?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Utilisation de Windows Azure SQL Database

- [Déployer une application ASP.NET MVC sécurisée avec appartenance, OAuth et base de données SQL à un Site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (site Microsoft Azure).
- [Bases de données SQL](https://docs.microsoft.com/azure/sql-database/) (site Microsoft Azure). Obtention des didacticiels et guides de procédures.
- [Microsoft Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Nœud de niveau supérieur de la table des matières de la base de données SQL dans MSDN.
- [Index Articles Wiki de la base de données SQL Windows Azure TechNet](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (site Microsoft TechNet).
- [Bloc d’Application de gestion des erreurs temporaires](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Une infrastructure qui vous permet de gérer les défaillances réseau temporaires et les erreurs de connexion qui résultent de la limitation. Disponible dans un package NuGet : [Enterprise Library 5.0 - bloc applicatif de pannes gestion](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Prise en main de la base de données SQL et Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de formation Azure Windows](https://www.microsoft.com/download/details.aspx?id=8396) (centre de téléchargement Microsoft). Comprend des laboratoires pratiques pour SQL Database.
- [Forum de communauté Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Passage à Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Chapitre d’un scénario de bout en bout complète par l’équipe Microsoft Patterns and Practices. Arrière-plan pourquoi vous pouvez choisir de migrer et comment migrer à partir de SQL Server vers la base de données SQL.
- [Migration des bases de données SQL Server vers Microsoft Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Assistant de Migration de base de données SQL](http://sqlazuremw.codeplex.com/). Un outil open source pour la migration des bases de données vers et à partir de la base de données SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Choix entre SQL Server et Windows Azure SQL Database

- [Comparer SQL Server avec Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (site Microsoft TechNet).
- [Migration des données vers Microsoft Azure SQL Database : outils et Techniques](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Comprend les sections qui comparent les SQL Server vers SQL Database et de fournissent des conseils migrer à partir de SQL Server vers SQL Database.
- [Guide de remise de base de données Windows Azure SQL](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (site Microsoft TechNet).
- [Limitations des fonctionnalités SQL Server (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Stockage Table Microsoft Azure et Windows Azure SQL Database - comparaison et différences](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Pour une application que vous déployez sur Windows Azure, le stockage de Table Windows Azure peut être une alternative à la base de données SQL Windows Azure. Cette rubrique vous aide à choisir entre ces alternatives.
- [Microsoft Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Instructions et Limitations (Microsoft Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Utilisation des systèmes de gestion de base de données NoSQL

- [Services de données Windows Azure](https://www.windowsazure.com/develop/net/data/) (site Microsoft Azure). Consultez le [guide sur la fonctionnalité Service de Table](https://docs.microsoft.com/azure/) et **Big Data** section de la page.
- [Tables de stockage à l’aide de ASP.NET multiniveau Application, les files d’attente et les objets BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site Microsoft Azure). Didacticiel de bout en bout avec l’exemple téléchargeable d’application qui utilise les tables NoSQL Windows Azure storage.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>À l’aide de requêtes LINQ dans les Applications ASP.NET

- [Options d’accès aux données ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Inclut une présentation de LINQ.
- [Vidéos de formation LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [Thread de Forum de ASP.NET avec des liens vers des ressources LINQ dynamiques](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>À l’aide de la structure de données dynamiques

- [Modèles de projet Dynamic Data](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Conseils sur l’utilisation des projets de Dynamic Data.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Sécurisation des accès aux données

- [Sécurisation des accès aux données dans ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considérations sur la sécurité (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Comment : Sécuriser des chaînes de connexion lors de l’utilisation de contrôles de Source de données](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimisation des performances accès aux données

- [Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [La mise en cache ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Amélioration des performances ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Un avertissement s’affiche « Contenu retiré » en haut de cette page, mais la plupart des informations sont toujours applicables, et il n’existe aucune ressource de mise à jour comparable.
- [Améliorer les performances de SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Commentaire même en tant que le lien précédent.

Voir aussi [performances d’optimisation Entity Framework](#optimizingef) plus haut dans cette rubrique.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Déploiement d’une base de données

- [Ressources recommandées de déploiement Web ASP.NET -](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>L’accès aux données via un Service Web

- [L’accès aux données via un Service Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Conseils utiliser l’API Web et WCF.
- [Bien démarrer avec ASP.NET Web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.NET Data Access FAQ](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Web Forms ASP.NET didacticiels - données](../web-forms/overview/data-access/index.md). La plupart de ces didacticiels sont relativement ancienne ; Assurez-vous que vous lire [Options d’accès ASP.NET données](https://msdn.microsoft.com/library/ms178359.aspx) et [Options de stockage de données (développement réel d’applications Cloud avec Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) premier afin que vous n’obtenez pas trop loin dans une méthode d’accès de données qui ne vous convient pas pour votre scénario.
- [ASP.NET MVC Content Map](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Didacticiels - données de Pages Web ASP.NET](../web-pages/overview/data/index.md).
- [L’accès aux données dans Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fournit une liste de liens similaire à ce plan de contenu, mais en se concentrant sur Visual Studio plutôt que dans ASP.NET.
