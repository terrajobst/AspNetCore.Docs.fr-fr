---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : préparation pour le déploiement de la base de données | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388532"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Déploiement de Web ASP.NET à l’aide de Visual Studio : préparation pour le déploiement de la base de données
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel montre comment obtenir le projet est prêt pour le déploiement de la base de données. La structure de base de données et certains (pas tous) des données dans les deux de l’application bases de données doivent être déployés pour le test, intermédiaire et les environnements de production.

En règle générale, lorsque vous développez une application, vous devez entrer les données de test dans une base de données que vous ne souhaitez pas déployer sur un site de production. Toutefois, vous avez aussi des données de production que vous ne souhaitez pas déployer. Dans ce didacticiel, vous allez configurer le projet de Contoso University et préparer des scripts SQL afin que les données correctes sont incluses lorsque vous déployez.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

L’exemple d’application utilise SQL Server Express LocalDB. SQL Server Express est l’édition gratuite de SQL Server. Il est couramment utilisé pendant le développement, car il est basé sur le même moteur de base de données en tant que versions complètes de SQL Server. Vous pouvez tester avec SQL Server Express et être assuré que l’application se comporte en production, avec quelques exceptions pour les fonctionnalités qui varient entre les éditions de SQL Server.

LocalDB est un mode spécial de l’exécution de SQL Server Express qui vous permet de travailler avec les bases de données en tant que *.mdf* fichiers. En règle générale, les fichiers de base de données de base de données locale sont conservés dans le *application\_données* dossier d’un projet web. La fonctionnalité d’instance utilisateur dans SQL Server Express vous permet également de travailler avec *.mdf* fichiers, mais la fonctionnalité d’instance utilisateur est déconseillé ; par conséquent, il est recommandé LocalDB pour travailler avec *.mdf* fichiers.

SQL Server Express est généralement pas utilisé pour les applications web de production. Base de données locale en particulier est déconseillé pour la production avec une application web, car il n’est pas conçu pour fonctionner avec IIS.

Dans Visual Studio 2012, LocalDB est installé par défaut avec Visual Studio. Dans Visual Studio 2010 et versions antérieures, SQL Server Express (sans base de données locale) est installé par défaut avec Visual Studio. Autrement dit pourquoi vous l’installé en tant que les conditions préalables dans [le premier didacticiel de cette série](introduction.md).

Pour plus d’informations sur les éditions de SQL Server, y compris LocalDB, consultez les ressources suivantes [utilisation des bases de données SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework et les fournisseurs universels

Pour un accès de base de données, l’application Contoso University nécessite les logiciels suivants doivent être déployés avec l’application, car il n’est pas inclus dans le .NET Framework :

- [Fournisseurs universels ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permet le système d’appartenance ASP.NET utiliser la base de données SQL Azure)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Étant donné que ce logiciel est inclus dans les packages NuGet, le projet est déjà configuré afin que les assemblys requis sont déployés avec le projet. (Les liens pointent vers les versions actuelles de ces packages, ce qui peuvent être plus récents que ce qui est installé dans le projet de démarrage que vous avez téléchargé pour ce didacticiel.)

Si vous déployez sur un fournisseur d’hébergement tiers au lieu d’Azure, assurez-vous que vous utilisez Entity Framework 5.0 ou version ultérieure. Les versions antérieures de Migrations Code First nécessitent la confiance totale, et la plupart des fournisseurs d’hébergement exécutera votre application dans Medium Trust. Pour plus d’informations sur Medium Trust, consultez le [déployer vers IIS comme un environnement de Test](deploying-to-iis.md) didacticiel.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurer les Migrations Code First pour le déploiement de base de données d’application

La base de données application Contoso University est géré par Code First, et vous allez la déployer à l’aide des Migrations Code First. Pour une vue d’ensemble du déploiement de base de données à l’aide des Migrations Code First, consultez [le premier didacticiel de cette série](introduction.md).

Lorsque vous déployez une base de données d’application, en général, vous ne simplement déployer votre base de données de développement avec toutes les données qu’il contient en production, car une grande partie des données qu’il contient existe-t-il probablement uniquement à des fins de test. Par exemple, les noms d’étudiants dans une base de données de test sont fictifs. En revanche, vous souvent impossible de déployer uniquement la structure de base de données sans données qu’il contient tout. Certaines des données dans votre base de données de test peuvent être des données réelles et doivent être il lorsque les utilisateurs commencent à utiliser l’application. Par exemple, votre base de données peut avoir une table qui contient les valeurs de niveau valide ou les noms de service réel.

Pour simuler ce scénario courant, vous allez configurer un Code First Migrations `Seed` méthode qui insère dans la base de données uniquement les données que vous souhaitez être il en production. Par exemple, si vous ajoutiez une colonne de Budget pour la table Department et que vous voulez initialiser tous les budgets de département à 1 000,00 $ dans le cadre d’une migration, vous pouvez ajouter la ligne suivant montre du code pour le `Seed` méthode pour que la migration :

Créer des scripts pour le déploiement de base de données d’appartenance L’application Contoso University utilise l’authentification de formulaires et de système de l’appartenance ASP.NET pour authentifier et autoriser les utilisateurs. Le `Seed`mise à jour crédits page est accessible uniquement aux utilisateurs qui se trouvent dans le rôle d’administrateur. Exécutez l’application et cliquez sur `enable Migrations. Then you'll update the `cours, puis cliquez sur crédits de la mise à jour.

Cliquez sur les crédits de mise à jour

[![Le [connectez-vous![ page s’affiche, car le ](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)crédits de la mise à jour page requiert des privilèges administratifs.](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Entrez `Student`administrateur`Enrollment` comme nom d’utilisateur et devpwd comme le mot de passe et cliquez sur connectez-vous. Page de connexion

### <a name="disable-the-initializer"></a>Le mise à jour crédits page s’affiche.

Page de crédits de mise à jour Les informations utilisateur et le rôle se trouvent dans le *aspnet-ContosoUniversity* base de données spécifiée par le DefaultConnection chaîne de connexion dans le Web.config fichier. Cette base de données n’est pas géré par Entity Framework Code First, vous ne pouvez pas utiliser les Migrations au pour déployer.

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Vous utiliserez le fournisseur dbDacFx pour déployer le schéma de base de données, et vous allez configurer le profil de publication pour exécuter un script qui insérera initiale des données dans des tables de base de données. Un nouveau système d’appartenance ASP.NET (maintenant appelé ASP.NET Identity) a été introduit avec Visual Studio 2013.

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Une autre façon de spécifier une classe d'initialisation est de le faire en appelant `Database.SetInitializer` dans la méthode `Application_Start` du fichier *Global.asax* L’exemple d’application utilise le système d’appartenance ASP.NET antérieures, ce qui ne peut pas être déployé à l’aide des Migrations Code First.


> [!NOTE]
> Les procédures de déploiement de cette base de données d’appartenance s’appliquent également à tout autre scénario dans lequel votre application a besoin déployer une base de données SQL Server qui n’est pas créé par Entity Framework Code First. Ici aussi, vous préfèrent généralement les mêmes données en production que vous avez dans le développement. Lorsque vous déployez un site pour la première fois, il est courant d’exclure la plupart ou tous les comptes d’utilisateur que vous créez pour le test.


### <a name="enable-code-first-migrations"></a>Par conséquent, le projet téléchargé a deux bases de données d’appartenance : aspnet-ContosoUniversity.mdf avec les utilisateurs de développement et aspnet-ContosoUniversity-Prod.mdf avec les utilisateurs en production.

1. Pour ce didacticiel, les noms d’utilisateur sont les mêmes dans les deux bases de données : administrateur et nonadmin. Les deux utilisateurs ont le mot de passe **devpwd** dans la base de données de développement et **prodpwd** dans la base de données de production. Vous allez déployer les utilisateurs de développement vers l’environnement de test et les utilisateurs de production à intermédiaire et de production.
2. Pour ce faire vous allez créer deux scripts SQL dans ce didacticiel, un pour le développement et un pour la production, et dans les didacticiels suivants, vous allez configurer le processus de publication pour les exécuter.

    ![La base de données d’appartenances stocke un hachage des mots de passe de compte.](preparing-databases/_static/image3.png)
3. En haut de la fenêtre **de la console du gestionnaire** de packages, sélectionnez ContosoUniversity.DAL comme projet par défaut, puis à l'invite `PM>`, entrez "enable-migrations".

    ![Ils génèrent les hachages mêmes lorsque vous utilisez les fournisseurs universels ASP.NET, tant que vous ne modifiez pas l’algorithme par défaut.](preparing-databases/_static/image4.png)

    L’algorithme par défaut est HMACSHA256 et qu’il est spécifié dans le *validation* attribut de la ** machineKey  élément dans le fichier Web.config.

    Vous pouvez créer des scripts de déploiement de données manuellement, à l’aide de SQL Server Management Studio (SSMS), ou à l’aide d’un outil tiers.

    ![Cette suite de ce didacticiel vous montrera comment le faire dans SSMS, mais si vous ne souhaitez pas installer et utiliser SSMS vous pouvez obtenir les scripts à partir de la version complète du projet et passer à la section où les stocker dans le dossier de solution.](preparing-databases/_static/image5.png)

    Pour installer SSMS, installez-le à partir de **centre de téléchargement : Microsoft SQL Server 2012 Express** en cliquant sur **ENU\x64\SQLManagementStudio**x64`enable-migrations`ENU.exe ou  ENU\x86\SQLManagementStudiox86ENU.exe. Si vous choisissez une autre pour votre système, il n’est pas installer et vous pouvez essayez-en un autre. (Notez qu’il s’agit d’un téléchargement de 600 mégaoctets. Il peut prendre beaucoup de temps à installer et nécessitera un redémarrage de votre ordinateur.) Dans la première page du centre d’Installation de SQL Server, cliquez sur `get-help enable-migrations`nouvelle installation SQL Server autonome ou ajout de fonctionnalités à une installation existanteet suivez les instructions en acceptant les choix par défaut.

    Créer le script de base de données de développement Exécutez SSMS. Dans le **se connecter au serveur** boîte de dialogue, entrez **(localdb) \v11.0** en tant que le nom du serveur, laissez authentification défini sur L’authentification Windows, puis cliquez sur Connect. SSMS se connecter au serveur Dans le Explorateur d’objets fenêtre, développez bases de données, avec le bouton droit aspnet-ContosoUniversity, cliquez sur tâches, puis cliquez sur Générer des Scripts.

### <a name="set-up-the-seed-method"></a>Générer des Scripts SSMS

Dans le `Seed`générer et publier des Scripts`Configuration` boîte de dialogue, cliquez sur définir les Options de script. Vous pouvez ignorer la `Seed`choisir objets étape car la valeur par défaut est base de données entière et tous les objets de base de données de Script et c’est ce que vous souhaitez.

Options de script de SSMS Dans le `AddOrUpdate`Options de script avancées boîte de dialogue, faites défiler jusqu'à Types de données à scripter, puis cliquez sur le uniquement les données option dans la liste déroulante. La méthode `AddOrUpdate` peut ne pas être le meilleur choix pour votre scénario. UTILISER des instructions ne sont pas valides pour la base de données SQL Azure et ne sont pas nécessaires pour le déploiement sur SQL Server Express dans l’environnement de test.

1. SSMS Script uniquement les données, aucune instruction USE

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Dans le `List`générer et publier des Scripts`using` boîte de dialogue, le nom de fichier zone spécifie où le script doit être créé. Modifier le chemin d’accès à votre dossier de solution (le dossier contenant votre fichier ContosoUniversity.sln) et le nom de fichier à `List`aspnet-data-dev.sql **.

    ![Cliquez sur suivant pour accéder à la Résumé onglet, puis cliquez sur suivant à nouveau pour créer le script.](preparing-databases/_static/image6.png)

    Script SSMS créé

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Créer le script de base de données de production

Étant donné que vous n’avez pas exécuté le projet avec la base de données de production, il n’est pas encore attaché à l’instance de base de données locale. Par conséquent, vous devez attacher la base de données tout d’abord.

> [!NOTE]
> Dans SSMS `Seed`Explorateur d’objets, avec le bouton droit bases de données et cliquez sur attacher. Attachement de SSMS Les méthodes `Up` et `Down` contiennent du code qui implémente les modifications de base de données. Vous en trouverez des exemples dans le didacticiel [Déploiement d'une base de données Update](deploying-a-database-update.md).
> 
> Suivez la même procédure que vous avez utilisé précédemment pour créer un script pour le fichier de production. Nommez le fichier de script `Up`aspnet-data-prod.sql.
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Les deux bases de données sont désormais prêtes à être déployée et vous avez deux scripts de déploiement de données dans votre dossier de solution.

Scripts de déploiement de données Dans ce didacticiel, vous configurez les paramètres de projet qui affectent le déploiement, et vous configurez automatique **Web.config** transformations de fichiers pour les paramètres qui doivent être différents dans l’application déployée.

Pour plus d’informations sur NuGet, consultez **gérer les bibliothèques de projets avec NuGet** et **Documentation de NuGet**.

![Si vous ne souhaitez pas utiliser NuGet, vous devrez apprendre à analyser un package NuGet pour déterminer ce qu’il fait lorsqu’il est installé.](preparing-databases/_static/image7.png)

(Par exemple, il peut configurer **Web.config** transformations, configurer des scripts PowerShell à exécuter au moment de la génération, etc..)

Pour en savoir plus sur le fonctionne de NuGet, consultez *créer et publier un Package* et **fichier de Configuration et des Transformations de Code Source**.

![Page de connexion](preparing-databases/_static/image8.png)

Le **mise à jour crédits** page s’affiche.

![Page de crédits de mise à jour](preparing-databases/_static/image9.png)

Les informations utilisateur et le rôle se trouvent dans le *aspnet-ContosoUniversity* base de données spécifiée par le **DefaultConnection** chaîne de connexion dans le *Web.config* fichier.

Cette base de données n’est pas géré par Entity Framework Code First, vous ne pouvez pas utiliser les Migrations au pour déployer. Vous utiliserez le fournisseur dbDacFx pour déployer le schéma de base de données, et vous allez configurer le profil de publication pour exécuter un script qui insérera initiale des données dans des tables de base de données.

> [!NOTE]
> Un nouveau système d’appartenance ASP.NET (maintenant appelé ASP.NET Identity) a été introduit avec Visual Studio 2013. Le nouveau système vous permet de conserver les applications et les tables d’appartenances dans la même base de données, et vous pouvez utiliser les Migrations Code First pour déployer les deux. L’exemple d’application utilise le système d’appartenance ASP.NET antérieures, ce qui ne peut pas être déployé à l’aide des Migrations Code First. Les procédures de déploiement de cette base de données d’appartenance s’appliquent également à tout autre scénario dans lequel votre application a besoin déployer une base de données SQL Server qui n’est pas créé par Entity Framework Code First.


Ici aussi, vous préfèrent généralement les mêmes données en production que vous avez dans le développement. Lorsque vous déployez un site pour la première fois, il est courant d’exclure la plupart ou tous les comptes d’utilisateur que vous créez pour le test. Par conséquent, le projet téléchargé a deux bases de données d’appartenance : *aspnet-ContosoUniversity.mdf* avec les utilisateurs de développement et *aspnet-ContosoUniversity-Prod.mdf* avec les utilisateurs en production. Pour ce didacticiel, les noms d’utilisateur sont les mêmes dans les deux bases de données : *administrateur* et *nonadmin*. Les deux utilisateurs ont le mot de passe *devpwd* dans la base de données de développement et *prodpwd* dans la base de données de production.

Vous allez déployer les utilisateurs de développement vers l’environnement de test et les utilisateurs de production à intermédiaire et de production. Pour ce faire vous allez créer deux scripts SQL dans ce didacticiel, un pour le développement et un pour la production, et dans les didacticiels suivants, vous allez configurer le processus de publication pour les exécuter.

> [!NOTE]
> La base de données d’appartenances stocke un hachage des mots de passe de compte. Pour déployer des comptes à partir d’un ordinateur à un autre, il se peut que vous devez vous assurer que les routines de hachage ne pas générer de hachages différents sur le serveur de destination qu’ils l’ordinateur source. Ils génèrent les hachages mêmes lorsque vous utilisez les fournisseurs universels ASP.NET, tant que vous ne modifiez pas l’algorithme par défaut. L’algorithme par défaut est HMACSHA256 et qu’il est spécifié dans le **validation** attribut de la **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** élément dans le fichier Web.config.


Vous pouvez créer des scripts de déploiement de données manuellement, à l’aide de SQL Server Management Studio (SSMS), ou à l’aide d’un outil tiers. Cette suite de ce didacticiel vous montrera comment le faire dans SSMS, mais si vous ne souhaitez pas installer et utiliser SSMS vous pouvez obtenir les scripts à partir de la version complète du projet et passer à la section où les stocker dans le dossier de solution.

Pour installer SSMS, installez-le à partir de [centre de téléchargement : Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) en cliquant sur [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) ou [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Si vous choisissez une autre pour votre système, il n’est pas installer et vous pouvez essayez-en un autre.

(Notez qu’il s’agit d’un téléchargement de 600 mégaoctets. Il peut prendre beaucoup de temps à installer et nécessitera un redémarrage de votre ordinateur.)

Dans la première page du centre d’Installation de SQL Server, cliquez sur **nouvelle installation SQL Server autonome ou ajout de fonctionnalités à une installation existante**et suivez les instructions en acceptant les choix par défaut.

### <a name="create-the-development-database-script"></a>Créer le script de base de données de développement

1. Exécutez SSMS.
2. Dans le **se connecter au serveur** boîte de dialogue, entrez *(localdb) \v11.0* en tant que le **nom du serveur**, laissez **authentification** défini sur **L’authentification Windows**, puis cliquez sur **Connect**.

    ![SSMS se connecter au serveur](preparing-databases/_static/image10.png)
3. Dans le **Explorateur d’objets** fenêtre, développez **bases de données**, avec le bouton droit **aspnet-ContosoUniversity**, cliquez sur **tâches**, puis cliquez sur **Générer des Scripts**.

    ![Générer des Scripts SSMS](preparing-databases/_static/image11.png)
4. Dans le **générer et publier des Scripts** boîte de dialogue, cliquez sur **définir les Options de script**.

    Vous pouvez ignorer la **choisir objets** étape car la valeur par défaut est **base de données entière et tous les objets de base de données de Script** et c’est ce que vous souhaitez.
5. Cliquez sur **Avancé**.

    ![Options de script de SSMS](preparing-databases/_static/image12.png)
6. Dans le **Options de script avancées** boîte de dialogue, faites défiler jusqu'à **Types de données à scripter**, puis cliquez sur le **uniquement les données** option dans la liste déroulante.
7. Modification **Script USE DATABASE** à **False**. UTILISER des instructions ne sont pas valides pour la base de données SQL Azure et ne sont pas nécessaires pour le déploiement sur SQL Server Express dans l’environnement de test.

    ![SSMS Script uniquement les données, aucune instruction USE](preparing-databases/_static/image13.png)
8. Cliquez sur **OK**.
9. Dans le **générer et publier des Scripts** boîte de dialogue, le **nom de fichier** zone spécifie où le script doit être créé. Modifier le chemin d’accès à votre dossier de solution (le dossier contenant votre fichier ContosoUniversity.sln) et le nom de fichier à *aspnet-data-dev.sql*.
10. Cliquez sur **suivant** pour accéder à la **Résumé** onglet, puis cliquez sur **suivant** à nouveau pour créer le script.

    ![Script SSMS créé](preparing-databases/_static/image14.png)
11. Cliquez sur **Terminer**.

### <a name="create-the-production-database-script"></a>Créer le script de base de données de production

Étant donné que vous n’avez pas exécuté le projet avec la base de données de production, il n’est pas encore attaché à l’instance de base de données locale. Par conséquent, vous devez attacher la base de données tout d’abord.

1. Dans SSMS **Explorateur d’objets**, avec le bouton droit **bases de données** et cliquez sur **attacher**.

    ![Attachement de SSMS](preparing-databases/_static/image15.png)
2. Dans le **attacher les bases de données** boîte de dialogue, cliquez sur **ajouter** , puis accédez à la *aspnet-ContosoUniversity-Prod.mdf* de fichiers dans le *application\_ Données* dossier.

     ![SSMS ajouter le fichier .mdf à attacher](preparing-databases/_static/image16.png)
3. Cliquez sur **OK**.
4. Suivez la même procédure que vous avez utilisé précédemment pour créer un script pour le fichier de production. Nommez le fichier de script *aspnet-data-prod.sql*.

## <a name="summary"></a>Récapitulatif

Les deux bases de données sont désormais prêtes à être déployée et vous avez deux scripts de déploiement de données dans votre dossier de solution.

![Scripts de déploiement de données](preparing-databases/_static/image17.png)

Dans ce didacticiel, vous configurez les paramètres de projet qui affectent le déploiement, et vous configurez automatique *Web.config* transformations de fichiers pour les paramètres qui doivent être différents dans l’application déployée.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur NuGet, consultez [gérer les bibliothèques de projets avec NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) et [Documentation de NuGet](http://docs.nuget.org/docs/start-here/overview). Si vous ne souhaitez pas utiliser NuGet, vous devrez apprendre à analyser un package NuGet pour déterminer ce qu’il fait lorsqu’il est installé. (Par exemple, il peut configurer *Web.config* transformations, configurer des scripts PowerShell à exécuter au moment de la génération, etc..) Pour en savoir plus sur le fonctionne de NuGet, consultez [créer et publier un Package](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) et [fichier de Configuration et des Transformations de Code Source](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Précédent](introduction.md)
> [Suivant](web-config-transformations.md)
