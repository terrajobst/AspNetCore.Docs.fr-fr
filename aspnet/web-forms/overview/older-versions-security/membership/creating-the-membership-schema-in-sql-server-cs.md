---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Création du schéma d’appartenance dans SQL Server (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel commence par examiner des techniques permettant d’ajouter le schéma nécessaire à la base de données pour pouvoir utiliser SqlMembershipProvider. Ensuite, nous wi...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: c28e9735884586c43be4cf25fb2a3e5fa597832c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816650"
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Création du schéma d’appartenance dans SQL Server (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Ce didacticiel commence par examiner des techniques permettant d’ajouter le schéma nécessaire à la base de données pour pouvoir utiliser SqlMembershipProvider. Ensuite, nous examiner les tables de clés dans le schéma et discuter de leur utilité et leur importance. Ce didacticiel se termine par examiner comment indiquer à quel fournisseur de l’infrastructure de l’appartenance doit utiliser une application ASP.NET.


## <a name="introduction"></a>Introduction

Les deux les didacticiels précédents examinés à l’aide de l’authentification par formulaire pour identifier les visiteurs du site Web. L’infrastructure d’authentification forms rend plus facile pour les développeurs à connecter un utilisateur à un site Web et se les rappeler une visite de page grâce à l’utilisation des tickets d’authentification. Le `FormsAuthentication` classe inclut des méthodes pour générer le ticket et son ajout à des cookies du visiteur. Le `FormsAuthenticationModule` examine toutes les demandes entrantes et, pour ceux avec un ticket d’authentification valide, crée et associe un `GenericPrincipal` et un `FormsIdentity` objet avec la requête actuelle. L’authentification par formulaire est simplement un mécanisme permettant d’accorder un ticket d’authentification pour un visiteur lors de la connexion dans et, pour les requêtes ultérieures, l’analyse de ce ticket pour déterminer l’identité de l’utilisateur. Pour une application web prendre en charge les comptes d’utilisateur, nous devons toujours implémenter un magasin d’utilisateurs et ajouter des fonctionnalités pour valider les informations d’identification, puis inscrire de nouveaux utilisateurs et la myriade d’autres tâches relatives aux comptes d’utilisateur.

Avant ASP.NET 2.0, les développeurs étaient sur le raccordement pour l’implémentation de toutes ces tâches relatives aux comptes d’utilisateur. Heureusement, l’équipe ASP.NET reconnu cette lacune et présenté l’infrastructure d’appartenance avec ASP.NET 2.0. L’infrastructure de l’appartenance est un ensemble de classes dans le .NET Framework qui fournissent une interface de programmation pour accomplir des tâches de relatives aux comptes utilisateur core. Cette infrastructure est construite sur le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), ce qui permet aux développeurs d’incorporer une implémentation personnalisée dans une API normalisée.

Comme indiqué dans le <a id="Tutorial1"> </a> [ *principes élémentaires de sécurité et la prise en charge ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) didacticiel, le .NET Framework est livré avec deux fournisseurs d’appartenances intégrés : [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) et [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Comme son nom l’indique, le `SqlMembershipProvider` utilise une base de données Microsoft SQL Server comme magasin de l’utilisateur. Pour utiliser ce fournisseur dans une application, nous devons indiquer au fournisseur de base de données à utiliser comme magasin. Comme vous pouvez l’imaginer, la `SqlMembershipProvider` attend la base de données du magasin utilisateur d’avoir certaines tables de base de données, les vues et les procédures stockées. Nous devons ajouter ce schéma attendu à la base de données sélectionnée.

Ce didacticiel commence par examiner des techniques permettant d’ajouter le schéma nécessaire à la base de données pour pouvoir utiliser le `SqlMembershipProvider`. Ensuite, nous examiner les tables de clés dans le schéma et discuter de leur utilité et leur importance. Ce didacticiel se termine par examiner comment indiquer à quel fournisseur de l’infrastructure de l’appartenance doit utiliser une application ASP.NET.

C’est parti !

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Étape 1 : Vous décidez où placer le Store de l’utilisateur

Données d’une application ASP.NET sont généralement stockées dans un nombre de tables dans une base de données. Lorsque vous implémentez le `SqlMembershipProvider` schéma de base de données nous devons déterminer s’il faut placer du schéma d’appartenance dans la même base de données en tant que les données d’application ou dans une autre base de données.

Je vous recommande de localisation du schéma d’appartenance dans la même base de données en tant que les données d’application pour les raisons suivantes :

- **La facilité de maintenance** une application dont les données sont encapsulées dans une base de données est plus facile à comprendre, gérer et déployer une application qui a deux bases de données distinctes.
- **L’intégrité relationnelle** en recherchant les tables liées à l’appartenance dans la même base de données en tant que tables de l’application qu’il est possible d’établir [contraintes de clé étrangère](http://en.wikipedia.org/wiki/Foreign_key) entre les clés primaires dans le Tables liées à l’appartenance et les tables d’application associé.

Découplage les données de magasin et l’application utilisateur dans les bases de données distinctes convient uniquement si vous possédez plusieurs applications que chacun utilise des bases de données distinctes, mais doivent partager un magasin utilisateur commun.

### <a name="creating-a-database"></a>Création d’une base de données

L’application que nous avons créé dans la mesure où le deuxième didacticiel n’a pas encore nécessaire d’une base de données. Nous avons besoin une maintenant, toutefois, pour le magasin d’utilisateurs. Nous allons créer un et puis lui ajouter le schéma requis par le `SqlMembershipProvider` fournisseur (voir étape 2).

> [!NOTE]
> Tout au long de cette série de didacticiels nous utiliserons un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) base de données pour stocker nos tables de l’application et le `SqlMembershipProvider` schéma. Cette décision a été effectuée pour deux raisons : tout d’abord, en raison de son coût - gratuit - Express Edition est la version la plus lisiblement accessible de SQL Server 2005 ; Deuxièmement, les bases de données SQL Server 2005 Express Edition peuvent être placés directement dans l’application web `App_Data` dossier, en rendant un jeu d’enfant pour la base de données du package et une application web ensemble dans un seul fichier ZIP et redéployer sans les instructions de configuration spéciale options de configuration ou. Si vous préférez suivre la procédure à l’aide d’une version non - Express Edition de SQL Server, n’hésitez pas. Les étapes sont pratiquement identiques. Le `SqlMembershipProvider` schéma travaillera avec n’importe quelle version de Microsoft SQL Server 2000 et le haut.


À partir de l’Explorateur de solutions, cliquez sur le `App_Data` dossier et choisir d’ajouter un nouvel élément. (Si vous ne voyez pas un `App_Data` dossier dans votre projet, avec le bouton droit sur le projet dans l’Explorateur de solutions, sélectionnez Ajouter le dossier ASP.NET et choisir `App_Data`.) À partir de la boîte de dialogue Ajouter un nouvel élément, choisissez d’ajouter une nouvelle base de données SQL nommée `SecurityTutorials.mdf`. Dans ce didacticiel, nous allons ajouter le `SqlMembershipProvider` schéma pour cette base de données ; dans les didacticiels suivants, nous allons créer supplémentaires tables pour capturer des données de notre application.


[![Ajouter une nouvelle base de données SQL nommée SecurityTutorials.mdf de base de données dans le dossier App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figure 1**: ajouter une nouvelle base de données SQL nommée `SecurityTutorials.mdf` base de données vers le `App_Data` dossier ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Ajout d’une base de données pour le `App_Data` dossier inclut automatiquement dans l’Explorateur de base de données. (Dans la version non - Express Edition de Visual Studio, l’Explorateur de base de données est appelée l’Explorateur de serveurs). Accédez à l’Explorateur de base de données et développez la juste ajouté `SecurityTutorials` base de données. Si vous ne voyez pas l’Explorateur de base de données à l’écran, accédez au menu Affichage et choisissez Explorateur de base de données ou appuyez sur Ctrl + Alt + S. Comme le montre la Figure 2, le `SecurityTutorials` base de données est vide ; il contient aucune table, aucune vue et aucune procédure stockée.


[![La base de données SecurityTutorials est actuellement vide](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figure 2**: le `SecurityTutorials` base de données est actuellement vide ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Étape 2 : Ajout de la`SqlMembershipProvider`schéma à la base de données

Le `SqlMembershipProvider` requiert un jeu de tables, vues et procédures stockées doit être installé dans la base de données du magasin utilisateur particulier. Ces objets de base de données requis peuvent être ajoutés à l’aide de la [ `aspnet_regsql.exe` outil](https://msdn.microsoft.com/library/ms229862.aspx). Ce fichier se trouve dans le `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` dossier.

> [!NOTE]
> Le `aspnet_regsql.exe` outil propose des fonctionnalités de la ligne de commande et une interface utilisateur graphique. L’interface graphique plus convivial et est ce que nous allons examiner dans ce didacticiel. L’interface de ligne de commande est utile lorsque l’ajout de la `SqlMembershipProvider` schéma doit être automatisée, comme dans la génération de scripts ou automatisée des scénarios de test.


Le `aspnet_regsql.exe` outil est utilisé pour ajouter ou supprimer des *services d’application ASP.NET* à une base de données SQL Server spécifiée. Les services d’application ASP.NET englobent les schémas pour le `SqlMembershipProvider` et `SqlRoleProvider`, ainsi que les schémas pour les fournisseurs basé sur SQL pour d’autres frameworks d’ASP.NET 2.0. Nous devons fournir les deux bits d’information pour le `aspnet_regsql.exe` outil :

- Si nous souhaitons ajouter ou supprimer des services d’application, et
- La base de données à partir duquel ajouter ou supprimer le schéma de services d’application

Dans l’invite pour la base de données à utiliser, le `aspnet_regsql.exe` outil nous demande de fournir le nom du serveur sur la base de données réside sur les informations d’identification de sécurité pour la connexion à la base de données et le nom de la base de données. Si vous utilisez les non - Express Edition de SQL Server, vous devez déjà savoir ces informations, car il s’agit des mêmes informations que vous devez fournir via une chaîne de connexion lorsque vous travaillez avec la base de données via une page web ASP.NET. Détermination du nom de serveur et base de données lors de l’utilisation d’une base de données SQL Server 2005 Express Edition dans la `App_Data` dossier, cependant, est un peu plus complexe.

La section suivante examine une méthode simple pour spécifier le nom de serveur et base de données pour une base de données SQL Server 2005 Express Edition dans la `App_Data` dossier. Si vous n’utilisez pas SQL Server 2005 Express Edition hésitez à passer directement à l’installation de la section de Services d’Application.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Déterminer le nom du serveur et base de données pour une base de données SQL Server 2005 Express Edition dans la`App_Data`dossier

Pour pouvoir utiliser le `aspnet_regsql.exe` outil que nous avons besoin de connaître les noms de serveur et base de données. Le nom du serveur est `localhost\InstanceName`. Vraisemblablement, le *InstanceName* est `SQLExpress`. Toutefois, si vous avez installé SQL Server 2005 Express Edition manuellement (autrement dit, vous n’avez pas installé il automatiquement lors de l’installation de Visual Studio), il est possible que vous avez sélectionné un autre nom d’instance.

Le nom de la base de données est un peu plus difficile à déterminer. Bases de données dans le `App_Data` dossier ont généralement un nom de base de données qui inclut un [identificateur global unique](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) , ainsi que le chemin d’accès au fichier de base de données. Nous devons déterminer ce nom de base de données afin d’ajouter le schéma de services d’application via `aspnet_regsql.exe`.

Pour déterminer le nom de la base de données le plus simple consiste à examiner via SQL Server Management Studio. SQL Server Management Studio fournit une interface graphique pour la gestion des bases de données SQL Server 2005, mais il n’est pas fourni avec le Express Edition de SQL Server 2005. La bonne nouvelle est que [vous pouvez télécharger](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) le libre Express Edition de SQL Server Management Studio.

> [!NOTE]
> Si vous avez également une version non - Express Edition de SQL Server 2005 est installé sur votre ordinateur de bureau, la version complète de Management Studio est probablement installée. Vous pouvez utiliser la version complète pour déterminer le nom de la base de données, suivant les mêmes étapes, comme indiqué ci-dessous pour l’édition Express.


Commencez par fermer Visual Studio pour vous assurer que tous les verrous imposées par Visual Studio sur le fichier de base de données sont fermées. Ensuite, lancez SQL Server Management Studio et connectez-vous à la `localhost\InstanceName` base de données pour SQL Server 2005 Express Edition. Comme indiqué précédemment, il est probable qu’est le nom d’instance est `SQLExpress`. Pour l’option d’authentification, sélectionnez l’authentification Windows.


[![Se connecter à l’Instance SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figure 3**: se connecter à SQL Server 2005 Express Edition Instance ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Après vous être connecté à l’instance de SQL Server 2005 Express Edition, Management Studio affiche les dossiers pour les bases de données, les paramètres de sécurité, les objets de serveur et ainsi de suite. Si vous développez l’onglet bases de données vous verrez que le `SecurityTutorials.mdf` base de données est *pas* inscrits dans l’instance de la base de données - nous avons besoin attacher la base de données tout d’abord.

Avec le bouton droit sur le dossier bases de données et cliquez sur Attacher dans le menu contextuel. Cela affiche la boîte de dialogue Attacher les bases de données. À ce stade, cliquez sur le bouton Ajouter, accédez à la `SecurityTutorials.mdf` de base de données, puis cliquez sur OK. Figure 4 montre la boîte de dialogue Attacher les bases de données après le `SecurityTutorials.mdf` base de données a été sélectionnée. La figure 5 illustre l’Explorateur d’objets de Management Studio une fois que la base de données a été correctement attachée.


[![Attacher la base de données SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figure 4**: attacher le `SecurityTutorials.mdf` base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![La base de données SecurityTutorials.mdf apparaît dans le dossier de bases de données](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figure 5**: le `SecurityTutorials.mdf` base de données apparaît dans le dossier de bases de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Comme le montre la Figure 5, le `SecurityTutorials.mdf` base de données a un nom plutôt abstruse. Nous allons le modifier pour une plus facile à mémoriser (et plus facile à taper) nom. Avec le bouton droit sur la base de données, cliquez sur Renommer dans le menu contextuel et renommez-le `SecurityTutorialsDatabase`. Cela ne modifie pas le nom de fichier, simplement le nom de la base de données utilise pour s’identifier sur SQL Server.


[![Renommez la base de données en SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figure 6**: renommer la base de données `SecurityTutorialsDatabase`([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


À ce stade nous savons que les noms de serveur et base de données pour le `SecurityTutorials.mdf` fichier de base de données : `localhost\InstanceName` et `SecurityTutorialsDatabase`, respectivement. Nous sommes maintenant prêts à installer les services d’application via le `aspnet_regsql.exe` outil.

### <a name="installing-the-application-services"></a>L’installation des Services d’Application

Pour lancer le `aspnet_regsql.exe` outil, accédez au menu Démarrer et choisissez Exécuter. Entrez `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` dans la zone de texte et cliquez sur OK. Vous pouvez également utiliser l’Explorateur Windows pour Explorer le dossier approprié et double-cliquez sur le `aspnet_regsql.exe` fichier. Chacune de ces approches ne supprime pas les mêmes résultats.

En cours d’exécution le `aspnet_regsql.exe` outil sans argument de ligne de commande lance l’interface utilisateur graphique d’Assistant ASP.NET SQL Server. L’Assistant rend plus facile ajouter ou supprimer les services d’application ASP.NET sur une base de données spécifiée. Le premier écran de l’Assistant, illustrée à la Figure 7, décrit l’objectif de l’outil.


[![Utilisez l’Assistant d’installation ASP.NET SQL Server simplifie pour ajouter le schéma d’appartenance](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figure 7**: permet du ASP.NET SQL Server Configuration Assistant permet d’ajouter le schéma d’abonnement ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


La deuxième étape de l’Assistant vous demande de nous si nous souhaitons ajouter les services d’application ou les supprimer. Étant donné que nous souhaitons ajouter des tables, vues et procédures stockées nécessaires pour le `SqlMembershipProvider`, choisissez la configuration de SQL Server pour l’option de services d’application. Ultérieurement, si vous souhaitez supprimer ce schéma de votre base de données, réexécutez cet Assistant, mais à la place choisir les informations de services d’application supprimer à partir d’une option de base de données existante.


[![Choisissez la configuration de SQL Server pour l’Option de Services d’Application](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figure 8**: choisissez de configurer SQL Server pour l’Option de Services d’Application ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


La troisième étape invite à entrer les informations de base de données : le nom du serveur, les informations d’authentification et le nom de la base de données. Si vous avez suivi ce didacticiel et que vous avez ajouté le `SecurityTutorials.mdf` à la base de données `App_Data`, attaché à `localhost\InstanceName`et renommé en `SecurityTutorialsDatabase`, puis utilisez les valeurs suivantes :

- Serveur : `localhost\InstanceName`
- Authentification Windows
- Base de données : `SecurityTutorialsDatabase`


[![Entrez les informations de base de données](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figure 9**: entrez les informations de base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Après avoir entré les informations de base de données, cliquez sur Suivant. La dernière étape résume les étapes à entreprendre. Cliquez sur Suivant pour installer les services d’application, puis sur Terminer pour terminer l’Assistant.

> [!NOTE]
> Si vous utilisez Management Studio pour attacher la base de données et renommez le fichier de base de données, veillez à détacher la base de données et fermez Management Studio avant de rouvrir Visual Studio. Pour détacher le `SecurityTutorialsDatabase` de base de données, avec le bouton droit sur le nom de la base de données et, dans le menu tâches, cliquez sur Détacher.


À l’achèvement de l’Assistant, revenez à Visual Studio et accédez à l’Explorateur de base de données. Développez le dossier Tables. Vous devez voir une série de tables dont les noms commencent par le préfixe `aspnet_`. De même, vous pouvez trouver une variété de vues et procédures stockées dans les dossiers de vues et procédures stockées. Ces objets de base de données constituent le schéma de services d’application. Nous allons examiner les objets de base de données d’appartenance et de rôle spécifique à l’étape 3.


[![Une variété de Tables, vues et procédures stockées ont été ajoutées à la base de données](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figure 10**: une variété de Tables, vues et stockées procédures ont été ajoutées à la base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> Le `aspnet_regsql.exe` interface utilisateur graphique de l’outil installe le schéma de services d’application dans son intégralité. Mais lors de l’exécution `aspnet_regsql.exe` à partir de la ligne de commande, vous pouvez spécifier quelle application particulière services de composants pour installer (ou supprimer). Par conséquent, si vous souhaitez ajouter uniquement les tables, vues, nécessaire pour des procédures stockées et les `SqlMembershipProvider` et `SqlRoleProvider` fournisseurs, exécutez `aspnet_regsql.exe` à partir de la ligne de commande. Ou bien, vous pouvez exécuter manuellement le sous-ensemble de T-SQL créer les scripts utilisés par `aspnet_regsql.exe`. Ces scripts se trouvent dans le `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` dossier avec des noms tels que `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`, et ainsi de suite.


À ce stade, nous avons créé les objets de base de données requis par le `SqlMembershipProvider`. Toutefois, nous avons besoin indiquer à l’infrastructure de l’appartenance qu’il doit utiliser le `SqlMembershipProvider` (versus, par exemple, le `ActiveDirectoryMembershipProvider`) et que le `SqlMembershipProvider` doit utiliser le `SecurityTutorials` base de données. Nous allons examiner comment spécifier quel fournisseur à utiliser et comment personnaliser les paramètres du fournisseur sélectionné à l’étape 4. Mais tout d’abord, jetons un œil plus approfondie sur les objets de base de données qui viennent d’être créés.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Étape 3 : Examinez les Tables de base du schéma

Lorsque vous travaillez avec les infrastructures d’appartenance et des rôles dans une application ASP.NET, les détails d’implémentation sont encapsulées par le fournisseur. Dans les futures didacticiels nous devra interagir avec ces infrastructures par le biais du .NET Framework `Membership` et `Roles` classes. Lorsque vous utilisez ces API de haut niveau nous n’avez pas besoin de nous-mêmes concernent avec les détails de bas niveau, tels que les requêtes sont exécutées ou quelles tables sont modifiées par le `SqlMembershipProvider` et `SqlRoleProvider`.

Pour cette raison, nous pourrions utiliser en toute confiance les infrastructures d’appartenance et des rôles sans avoir exploré le schéma de base de données créée à l’étape 2. Toutefois, lors de la création de tables pour stocker les données d’application nous pouvons besoin créer des entités qui se rapportent aux utilisateurs ou rôles. Vous pour devez avoir une connaissance de la `SqlMembershipProvider` et `SqlRoleProvider` schémas lors de l’établissement étrangère clé contraintes entre les tables de données d’application et les tables créées à l’étape 2. En outre, dans certaines circonstances rares nous devons interagir avec l’utilisateur et rôle stocke directement au niveau de la base de données (plutôt que par le `Membership` ou `Roles` classes).

### <a name="partitioning-the-user-store-into-applications"></a>Partitionnement Store de l’utilisateur dans des Applications

Les frameworks d’appartenance et rôles soient conçus pour un seul magasin de l’utilisateur et le rôle peut être partagé entre différentes applications. Une application ASP.NET qui utilise les infrastructures d’appartenance ou de rôles doit spécifier quelle partition d’application à utiliser. En bref, plusieurs applications web peuvent utiliser les mêmes magasins d’utilisateur et le rôle. Figure 11 illustre l’utilisateur et le rôle des magasins sont partitionnés en trois applications : HRSite, CustomerSite et SalesSite. Ces applications de trois web ont leurs propres utilisateurs uniques et les rôles, mais ils tous les physiquement stockent leurs informations de compte et le rôle d’utilisateur dans les mêmes tables de base de données.


[![Comptes d’utilisateur peuvent être partitionnées entre plusieurs Applications](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figure 11**: comptes peuvent être partitionnées entre plusieurs Applications utilisateur ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


Le `aspnet_Applications` table est ce qui définit ces partitions. Chaque application qui utilise la base de données pour stocker les informations de compte d’utilisateur est représentée par une ligne dans cette table. Le `aspnet_Applications` table comporte quatre colonnes : `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, et `Description`. `ApplicationId` est de type [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) et est la clé primaire de la table ; `ApplicationName` fournit un nom convivial unique pour chaque application.

Les autres tables d’appartenance et de rôle liés à un lien vers le `ApplicationId` champ `aspnet_Applications`. Par exemple, le `aspnet_Users` table, qui contient un enregistrement pour chaque compte d’utilisateur, a un `ApplicationId` champ de clé étrangère ; même chose pour le `aspnet_Roles` table. Le `ApplicationId` champ dans ces tables spécifie la partition d’application le compte d’utilisateur ou rôle appartient.

### <a name="storing-user-account-information"></a>Stockage des informations de compte d’utilisateur

Informations de compte utilisateur sont hébergées dans deux tables : `aspnet_Users` et `aspnet_Membership`. Le `aspnet_Users` table contient des champs qui contiennent les informations de compte utilisateur essentiels. Les trois colonnes les plus pertinents sont :

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` est la clé primaire (et de type `uniqueidentifier`). `UserName` est de type `nvarchar(256)` et, ainsi que le mot de passe, informations d’identification de l’utilisateur. (Un mot de passe utilisateur est stockée dans le `aspnet_Membership` table.) `ApplicationId` lie le compte d’utilisateur à une application particulière dans `aspnet_Applications`. Il existe un composite [ `UNIQUE` contrainte](https://msdn.microsoft.com/library/ms191166.aspx) sur le `UserName` et `ApplicationId` colonnes. Cela garantit que dans une application donnée, chaque nom d’utilisateur est unique, mais elle permet de la même `UserName` à utiliser dans différentes applications.

Le `aspnet_Membership` table inclut des informations de compte d’utilisateur supplémentaires, telles que le mot de passe utilisateur, adresse de messagerie, la dernière date de connexion et heure et ainsi de suite. Il existe une correspondance univoque entre les enregistrements dans le `aspnet_Users` et `aspnet_Membership` tables. Cette relation est assurée par le `UserId` champ `aspnet_Membership`, qui sert de clé primaire de la table. Comme le `aspnet_Users` table, `aspnet_Membership` inclut un `ApplicationId` champ qui lie ces informations à une partition d’application particulière.

### <a name="securing-passwords"></a>Sécurisation des mots de passe

Informations de mot de passe sont stockées dans le `aspnet_Membership` table. Le `SqlMembershipProvider` permet des mots de passe soient stockés dans la base de données en utilisant l’une des trois techniques suivantes :

- **Effacer** -le mot de passe est stocké dans la base de données en tant que texte brut. Je déconseille fortement à l’aide de cette option. Si la base de données est compromise - qu’il s’agisse par un pirate qui recherche une porte dérobée ou un employé mécontent qui a accès de base de données - informations d’identification de chaque utilisateur unique sont présents pour l’adoption.
- **Haché** -les mots de passe sont hachés à l’aide d’un algorithme de hachage à sens unique et une valeur salt générée de manière aléatoire. Cette valeur hachée (ainsi que la valeur salt) est stockée dans la base de données.
- **Chiffré** -une version chiffrée du mot de passe est stockée dans la base de données.

La technique de stockage de mot de passe utilisée varie selon le `SqlMembershipProvider` paramètres spécifiés dans `Web.config`. Nous allons examiner de personnalisation de le `SqlMembershipProvider` paramètres à l’étape 4. Le comportement par défaut consiste à stocker le hachage du mot de passe.

Les colonnes responsables pour stocker le mot de passe sont `Password`, `PasswordFormat`, et `PasswordSalt`. `PasswordFormat` est un champ de type `int` dont la valeur indique la technique utilisée pour stocker le mot de passe : 0 pour effacer ; 1 pour Hashed ; 2 pour le chiffrement. `PasswordSalt` est affecté à une chaîne générée de manière aléatoire, quel que soit la technique de stockage de mot de passe utilisée ; la valeur de `PasswordSalt` est utilisé uniquement lors du calcul du hachage du mot de passe. Enfin, le `Password` colonne contient les données de mot de passe réel, que ce soit le mot de passe en texte brut, le hachage de mot de passe ou le mot de passe chiffré.

Le tableau 1 illustre ce que ces trois colonnes peut se présenter comme pour les différentes techniques de stockage lorsque vous stockez le mot de passe MySecret ! .

| **Technique de stockage&lt;\_o3a\_p /&gt;** | **Mot de passe&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt du client&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Effacer | MySecret ! | 0 | tTnkPlesqissc2y2SMEygA== |
| Haché | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ == |
| Chiffré | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tableau 1**: exemples de valeurs pour les champs liés au mot de passe lorsque vous stockez le mot de passe MySecret !

> [!NOTE]
> Le chiffrement particulier ou un algorithme de hachage utilisé par le `SqlMembershipProvider` est déterminée par les paramètres dans le `<machineKey>` élément. Nous avons abordé cet élément de configuration à l’étape 3 de la <a id="Tutorial3"> </a> [ *Configuration de l’authentification de formulaires et des sujets avancés* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) didacticiel.


### <a name="storing-roles-and-role-associations"></a>Stockage des rôles et les Associations de rôle

L’infrastructure de rôles permet aux développeurs de définir un ensemble de rôles et de spécifier quels utilisateurs appartiennent à quels rôles. Ces informations sont capturées dans la base de données dans les deux tables : `aspnet_Roles` et `aspnet_UsersInRoles`. Chaque enregistrement dans le `aspnet_Roles` table représente un rôle pour une application particulière. Comme beaucoup le `aspnet_Users` table, le `aspnet_Roles` comporte trois colonnes pertinentes pour notre discussion :

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` est la clé primaire (et de type `uniqueidentifier`). `RoleName` est de type `nvarchar(256)`. Et `ApplicationId` lie le compte d’utilisateur à une application particulière dans `aspnet_Applications`. Il existe un composite `UNIQUE` contrainte sur la `RoleName` et `ApplicationId` colonnes, en garantissant que chaque nom de rôle est unique dans une application donnée.

Le `aspnet_UsersInRoles` table sert un mappage entre les utilisateurs et rôles. Il existe seulement deux colonnes - `UserId` et `RoleId` - et elles constituent une clé primaire composite.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Étape 4 : Spécifier le fournisseur et personnaliser ses paramètres

Tous les frameworks qui prennent en charge le modèle de fournisseur, par exemple les infrastructures d’appartenance et de rôles - ne disposent pas de détails d’implémentation eux-mêmes et à la place de déléguer cette responsabilité à une classe de fournisseur. Dans le cas de l’infrastructure de l’appartenance, la `Membership` classe définit l’API de gestion des comptes d’utilisateur, mais il n’interagit pas directement avec n’importe quel magasin de l’utilisateur. Au lieu de cela, le `Membership` rassemblerez de méthodes de la classe à la demande au fournisseur configuré - nous utiliserons le `SqlMembershipProvider`. Lorsque nous appelons une des méthodes dans le `Membership` (classe), comment l’infrastructure Membership sait pour déléguer l’appel à la `SqlMembershipProvider`?

Le `Membership` classe a un [ `Providers` propriété](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) qui contient une référence à toutes les classes de fournisseur inscrit disponibles pour une utilisation par l’infrastructure d’appartenance. Chaque fournisseur inscrit a un nom associé et le type. Le nom offre un moyen de conviviale pour référencer un fournisseur particulier dans le `Providers` collection, tandis que le type identifie la classe de fournisseur. En outre, chaque fournisseur inscrit peut inclure des paramètres de configuration. Incluent des paramètres de configuration de l’infrastructure de l’appartenance `passwordFormat` et `requiresUniqueEmail`, entre autres. Reportez-vous au tableau 2 pour obtenir la liste complète des paramètres de configuration utilisés par le `SqlMembershipProvider`.

Le `Providers` contenu de la propriété est spécifiés via les paramètres de configuration de l’application web. Par défaut, toutes les applications web ont un fournisseur nommé `AspNetSqlMembershipProvider` de type `SqlMembershipProvider`. Ce fournisseur d’appartenances par défaut est inscrit dans `machine.config` (situé dans `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

En tant que le balisage ci-dessus, le [ `<membership>` élément](https://msdn.microsoft.com/library/1b9hw62f.aspx) définit les paramètres de configuration de l’infrastructure de l’appartenance lors de la [ `<providers>` élément enfant](https://msdn.microsoft.com/library/6d4936ht.aspx) spécifie inscrit fournisseurs. Les fournisseurs peuvent être ajoutés ou supprimés à l’aide la [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) ou [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) éléments ; Utilisez le [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) élément à supprimer tous les actuellement fournisseurs inscrits. En tant que le balisage ci-dessus, `machine.config` ajoute un fournisseur nommé `AspNetSqlMembershipProvider` de type `SqlMembershipProvider`.

Outre le `name` et `type` attributs, la `<add>` élément contient des attributs qui définissent les valeurs de configuration des paramètres différents. Le tableau 2 répertorie les informations disponibles `SqlMembershipProvider`-paramètres de configuration spécifiques, ainsi qu’une description de chacun d’eux.

> [!NOTE]
> Toutes les valeurs par défaut indiqués dans le tableau 2 font référence aux valeurs par défaut définies dans le `SqlMembershipProvider` classe. Notez que not tous les paramètres de configuration dans `AspNetSqlMembershipProvider` correspondent aux valeurs par défaut de la `SqlMembershipProvider` classe. Par exemple, si non spécifié dans un fournisseur d’appartenances, le `requiresUniqueEmail` définissant les valeurs par défaut sur true. Toutefois, le `AspNetSqlMembershipProvider` remplace cette valeur par défaut en spécifiant explicitement une valeur de `false`.


| **Paramètre&lt;\_o3a\_p /&gt;** | **Description&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Rappel qui permet à l’infrastructure de l’appartenance d’un magasin utilisateur unique d’être partitionnées entre plusieurs applications. Ce paramètre indique le nom de la partition d’application utilisé par le fournisseur d’appartenances. Si cette valeur n’est explicitement spécifiée, elle est définie, lors de l’exécution, à la valeur de chemin d’accès virtuel racine de l’application. |
| `commandTimeout` | Spécifie la valeur de délai d’attente de commande SQL (en secondes). La valeur par défaut est 30. |
| `connectionStringName` | Le nom de la chaîne de connexion dans le `<connectionStrings>` élément à utiliser pour se connecter à la base de données utilisateur. Cette valeur est obligatoire. |
| `description` | Fournit une description conviviale du fournisseur inscrit. |
| `enablePasswordRetrieval` | Spécifie si les utilisateurs peuvent récupérer leur mot de passe oublié. La valeur par défaut est `false`. |
| `enablePasswordReset` | Indique si les utilisateurs sont autorisés à réinitialiser leur mot de passe. La valeur par défaut est `true`. |
| `maxInvalidPasswordAttempts` | Le nombre maximal de tentatives de connexion ayant échoué qui peuvent se produire pour un utilisateur donné au cours de le `passwordAttemptWindow` avant que l’utilisateur est verrouillé. La valeur par défaut est 5. |
| `minRequiredNonalphanumericCharacters` | Le nombre minimal de caractères non alphanumériques qui doivent apparaître dans un mot de passe utilisateur. Cette valeur doit être comprise entre 0 et 128 ; la valeur par défaut est 1. |
| `minRequiredPasswordLength` | Le nombre minimal de caractères requis dans un mot de passe. Cette valeur doit être comprise entre 0 et 128 ; la valeur par défaut est 7. |
| `name` | Le nom du fournisseur inscrit. Cette valeur est obligatoire. |
| `passwordAttemptWindow` | Le nombre de minutes pendant lesquelles Échec de connexion tentatives sont suivies. Si un utilisateur fournit des informations d’identification de connexion non valide `maxInvalidPasswordAttempts` fois au cours de cette spécifié fenêtre, ils sont verrouillés. La valeur par défaut est 10. |
| `PasswordFormat` | Le format de stockage du mot de passe : `Clear`, `Hashed`, ou `Encrypted`. La valeur par défaut est `Hashed`. |
| `passwordStrengthRegularExpression` | S’il est fourni, cette expression régulière est utilisée pour évaluer la force de mot de passe de l’utilisateur sélectionné lors de la création d’un nouveau compte ou lors de la modification de leur mot de passe. La valeur par défaut est une chaîne vide. |
| `requiresQuestionAndAnswer` | Spécifie si un utilisateur doit répondre à sa question de sécurité lors de la récupération ou de réinitialiser son mot de passe. La valeur par défaut est `true`. |
| `requiresUniqueEmail` | Indique si tous les comptes d’utilisateur dans une partition d’application donnée doivent avoir une adresse de messagerie unique. La valeur par défaut est `true`. |
| `type` | Spécifie le type du fournisseur. Cette valeur est obligatoire. |

**Tableau 2**: appartenance et `SqlMembershipProvider` paramètres de Configuration

En plus de `AspNetSqlMembershipProvider`, autres fournisseurs d’appartenance peuvent être enregistrés sur une base de l’application par application en ajoutant un balisage semblable à la `Web.config` fichier.

> [!NOTE]
> Le framework de rôles fonctionne à peu près la même façon : il existe un fournisseur de rôle inscrit par défaut dans `machine.config` et fournisseurs inscrits peuvent être personnalisés sur une base de l’application par application dans `Web.config`. Nous allons examiner l’infrastructure de rôles et de son balisage de configuration en détail dans un futur didacticiel.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Personnalisation de le`SqlMembershipProvider`paramètres

La valeur par défaut `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) a sa `connectionStringName` attribut la valeur `LocalSqlServer`. Comme le `AspNetSqlMembershipProvider` fournisseur, le nom de chaîne de connexion `LocalSqlServer` est défini dans `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Comme vous pouvez le voir, cette chaîne de connexion définit la base de données SQL 2005 Express Edition | DataDirectory|aspnetdb.mdf. La chaîne | DataDirectory | est traduite lors de l’exécution pour pointer vers le `~/App_Data/` directory, par conséquent, le chemin d’accès de la base de données | Se traduit par DataDirectory|aspnetdb.mdf » `~/App_Data` / `aspnet.mdf`.

Si nous ne spécifiait pas les informations de fournisseur d’appartenance dans de notre application `Web.config` fichier, l’application utilise le fournisseur d’appartenances par défaut inscrit, `AspNetSqlMembershipProvider`. Si le `~/App_Data/aspnet.mdf` base de données n’existe pas, le runtime ASP.NET crée automatiquement et ajoutez le schéma de services d’application. Toutefois, nous ne souhaitez pas utiliser le `aspnet.mdf` base de données ; au lieu de cela, nous souhaitons utiliser la `SecurityTutorials.mdf` base de données créées à l’étape 2. Cette modification peut être accomplie de deux manières :

- <strong>Spécifiez une valeur pour le</strong><strong>`LocalSqlServer`</strong><strong>nom de chaîne de connexion dans</strong><strong>`Web.config`</strong><strong>.</strong> En remplaçant le `LocalSqlServer` valeur du nom de chaîne de connexion `Web.config`, nous pouvons utiliser le fournisseur d’appartenances par défaut inscrit (`AspNetSqlMembershipProvider`) et que cela fonctionne correctement avec le `SecurityTutorials.mdf` base de données. Cette approche est adaptée si vous êtes contenu avec les paramètres de configuration spécifiés par `AspNetSqlMembershipProvider`. Pour plus d’informations sur cette technique, consultez [Scott Guthrie](https://weblogs.asp.net/scottgu/)du billet de blog, [configuration des Services ASP.NET 2.0 pour utiliser SQL Server 2000 ou SQL Server 2005 Application](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Ajouter un nouveau fournisseur inscrit du type</strong><strong>`SqlMembershipProvider`</strong><strong>et configurer ses</strong><strong>`connectionStringName`</strong><strong>paramètre pour pointer vers le</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de données.</strong> Cette approche est utile dans les scénarios où vous souhaitez personnaliser les autres propriétés de configuration en plus de la chaîne de connexion de base de données. Dans mes propres projets, j’utilise toujours cette approche en raison de sa flexibilité et une meilleure lisibilité.

Avant de pouvoir ajouter un nouveau fournisseur inscrit qui fait référence à la `SecurityTutorials.mdf` base de données, nous devons d’abord ajouter une valeur de chaîne de connexion approprié dans le `<connectionStrings>` section `Web.config`. Le balisage suivant ajoute une nouvelle chaîne de connexion nommée `SecurityTutorialsConnectionString` qui fait référence à SQL Server 2005 Express Edition `SecurityTutorials.mdf` de base de données dans le `App_Data` dossier.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Si vous utilisez un fichier de l’autre base de données, mettre à jour la chaîne de connexion en fonction des besoins. Pour plus d’informations sur la formation de la chaîne de connexion correcte, reportez-vous à [ConnectionStrings.com](http://www.connectionstrings.com/).

Ensuite, ajoutez le balisage suivant de configuration d’appartenance pour le `Web.config` fichier. Ce balisage inscrit un nouveau fournisseur nommé `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Outre l’enregistrement le `SecurityTutorialsSqlMembershipProvider` fournisseur, le balisage ci-dessus définit le `SecurityTutorialsSqlMembershipProvider` en tant que le fournisseur par défaut (via le `defaultProvider` d’attribut dans le `<membership>` élément). Rappelez-vous que l’infrastructure de l’appartenance peut avoir plusieurs fournisseurs enregistrés. Dans la mesure où `AspNetSqlMembershipProvider` est inscrit en tant que premier fournisseur dans `machine.config`, il constitue le fournisseur par défaut, sauf si nous indiquer dans le cas contraire.

Actuellement, notre application a deux fournisseurs enregistrés : `AspNetSqlMembershipProvider` et `SecurityTutorialsSqlMembershipProvider`. Toutefois, avant d’enregistrer le `SecurityTutorialsSqlMembershipProvider` fournisseur nous pourrions effaciez tous précédemment inscrit les fournisseurs en ajoutant un [ `<clear />` élément](https://msdn.microsoft.com/library/t062y6yc.aspx) immédiatement avant notre `<add>` élément. Cela serait effacer la `AspNetSqlMembershipProvider` à partir de la liste des fournisseurs enregistrés, ce qui signifie que le `SecurityTutorialsSqlMembershipProvider` serait le seul fournisseur d’appartenances inscrit. Si nous avons utilisé cette approche, nous devons pas marquer la `SecurityTutorialsSqlMembershipProvider` en tant que le fournisseur par défaut, car il serait le seul fournisseur d’appartenances inscrit. Pour plus d’informations sur l’utilisation de `<clear />`, consultez [Using `<clear />` lorsque des fournisseurs ajout](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Notez que le `SecurityTutorialsSqlMembershipProvider`de `connectionStringName` définition de références le juste ajouté `SecurityTutorialsConnectionString` nom de chaîne de connexion et que son `applicationName` paramètre a été défini sur une valeur de SecurityTutorials. En outre, le `requiresUniqueEmail` paramètre a été défini sur `true`. Toutes les autres options de configuration sont identiques aux valeurs dans `AspNetSqlMembershipProvider`. N’hésitez pas à apporter des modifications de configuration ici, si vous le souhaitez. Par exemple, vous pourrez renforcer la force de mot de passe en demandant à deux caractères non alphanumériques plutôt qu’un seul ou en augmentant la longueur de mot de passe à huit caractères au lieu de sept.

> [!NOTE]
> Rappel qui permet à l’infrastructure de l’appartenance d’un magasin utilisateur unique d’être partitionnées entre plusieurs applications. Le fournisseur d’appartenances `applicationName` paramètre indique quelle application que le fournisseur utilise lorsque vous travaillez avec le magasin d’utilisateurs. Il est important que vous définissez explicitement une valeur pour le `applicationName` paramètre de configuration, car si le `applicationName` n’est pas défini explicitement, il est affecté à un chemin d’accès virtuel racine de l’application web lors de l’exécution. Cela fonctionne bien tant que chemin d’accès virtuel racine de l’application ne change pas, mais si vous déplacez l’application vers un autre chemin d’accès, le `applicationName` paramètre est modifiées. Dans ce cas, le fournisseur d’appartenances travaillerons avec une partition d’application autre que celui utilisé précédemment. Comptes d’utilisateur créés avant le déplacement résidera dans une partition d’application différents, et ces utilisateurs ne seront plus en mesure de vous connecter au site. Pour obtenir une explication plus approfondie sur ce sujet, consultez [toujours définir le `applicationName` propriété lors de la configuration ASP.NET 2.0 de l’appartenance et les autres fournisseurs](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Récapitulatif

À ce stade, nous avons une base de données avec les services d’application configuré (`SecurityTutorials.mdf`) et avez configuré notre application web afin que l’infrastructure d’appartenance utilise le `SecurityTutorialsSqlMembershipProvider` fournisseur nous venez d’inscrire. Ce fournisseur inscrit est de type `SqlMembershipProvider` et a son `connectionStringName` défini sur la chaîne de connexion approprié (`SecurityTutorialsConnectionString`) et son `applicationName` valeur explicitement.

Nous sommes maintenant prêts à utiliser l’infrastructure de l’appartenance à partir de notre application. Dans le didacticiel suivant, nous allons examiner comment créer de nouveaux comptes d’utilisateur. Après que nous explorerons l’authentification des utilisateurs, effectue une autorisation basée sur l’utilisateur et le stockage des informations supplémentaires relatives à l’utilisateur dans la base de données.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Toujours défini le `applicationName` propriété lors de la configuration d’ASP.NET 2.0 appartenance et autres fournisseurs](https://weblogs.asp.net/scottgu/443634)
- [Configuration d’ASP.NET 2.0 des Services d’Application pour utiliser SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Télécharger SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examen d’ASP.NET 2.0 s appartenance, rôles et profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Le `<add>` élément de Providers pour Membership](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Le `<membership>` élément](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Le `<providers>` élément pour l’appartenance](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [À l’aide de `<clear />` lorsque vous ajoutez les fournisseurs](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Travailler directement avec le `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vidéos de formation sur les rubriques contenues dans ce didacticiel

- [Présentation des appartenances d’ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configuration de SQL pour l’utilisation de schémas d’appartenance](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modification des paramètres d’appartenance dans le schéma d’appartenance par défaut](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Alicja Maziarz. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Next](creating-user-accounts-cs.md)
