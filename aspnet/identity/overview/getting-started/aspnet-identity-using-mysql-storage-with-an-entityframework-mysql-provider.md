---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity : Utilisation du stockage MySQL avec un fournisseur MySQL Entity Framework (c#) | Microsoft Docs'
author: maumar
description: Ce didacticiel vous montre comment remplacer le mécanisme de stockage de données par défaut pour ASP.NET Identity avec Entity Framework (fournisseur SQL client) avec un être accrus ou MySQL...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: f510c9bcaf83c6a68e835a7d82555653459df856
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912369"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity : Utilisation du stockage MySQL avec un fournisseur MySQL Entity Framework (c#)
====================
par [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Ce didacticiel vous montre comment remplacer le mécanisme de stockage de données par défaut pour [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) avec Entity Framework (fournisseur SQL client) avec un fournisseur de MySQL.


Les rubriques suivantes sont traitées dans ce didacticiel :

- Création d’une base de données MySQL sur Azure
- Création d’une application MVC à l’aide du modèle de Visual Studio 2013 MVC
- Configuration d’Entity Framework pour travailler avec un fournisseur de base de données MySQL
- Exécution de l’application pour vérifier les résultats

À la fin de ce didacticiel, vous disposerez une application MVC avec l’identité ASP.NET stocker qui utilise une base de données MySQL qui est hébergé dans Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Création d’une instance de base de données MySQL sur Azure

1. Connectez-vous au [portail Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Cliquez sur **NEW** en bas de la page, puis sélectionnez **STORE**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. Dans le **choisir et du module complémentaire** Assistant, sélectionnez **base de données MySQL ClearDB**, puis cliquez sur le **suivant** flèche en bas de l’image :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Conservez la valeur par défaut **gratuit** planifier, de modifier le **nom** à **IdentityMySQLDatabase**, sélectionnez la région qui est plus proche de vous, puis cliquez sur le **suivant** flèche en bas de l’image :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Cliquez sur le **achat** coche pour terminer la création de la base de données.

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Une fois que votre base de données a été créé, vous pouvez le gérer à partir de la **modules complémentaires** onglet dans le portail de gestion. Pour récupérer les informations de connexion pour votre base de données, cliquez sur **informations de connexion** en bas de la page :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copiez la chaîne de connexion en cliquant sur le bouton de copie par le **CONNECTIONSTRING** champ et enregistrez-le ; vous utiliserez plus loin dans ce didacticiel pour votre application MVC :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Création d’un projet d’application MVC

Pour effectuer les étapes décrites dans cette section du didacticiel, vous devez d’abord installer [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Une fois que Visual Studio a été installé, procédez comme suit pour créer un nouveau projet d’application MVC :

1. Ouvrez Visual Studio 2103.
2. Cliquez sur **nouveau projet** à partir de la **Démarrer** page, ou vous pouvez cliquer sur le **fichier** menu, puis **nouveau projet**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Lorsque le **nouveau projet** boîte de dialogue, développez **Visual C#** dans la liste des modèles, puis cliquez sur **Web**, puis sélectionnez **ASP.NET Web Application**. Nommez votre projet **IdentityMySQLDemo** puis cliquez sur **OK**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **MVC** templatewith les options par défaut ; cela va configurer **comptes d’utilisateur individuels** comme méthode d’authentification. Cliquez sur **OK** :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurer Entity Framework pour travailler avec une base de données MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Mettre à jour de l’assembly d’Entity Framework pour votre projet

L’application MVC qui a été créée à partir du modèle Visual Studio 2013 contienne une référence à la [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) du package, mais il ont été mises à jour à cet assembly depuis sa version qui contiennent significatif améliorations des performances. Pour pouvoir utiliser ces dernières mises à jour dans votre application, procédez comme suit.

1. Ouvrez votre projet MVC dans Visual Studio.
2. Cliquez sur **outils**, puis cliquez sur **Gestionnaire de Package NuGet**, puis cliquez sur **Console du Gestionnaire de Package**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. Le **Console du Gestionnaire de Package** apparaîtra dans la section inférieure de Visual Studio. Type &quot; **Update-Package EntityFramework** &quot; et appuyez sur ENTRÉE :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installez le fournisseur MySQL pour Entity Framework

Dans l’ordre pour Entity Framework pour se connecter à la base de données MySQL, vous devez installer un fournisseur de MySQL. Pour ce faire, ouvrez le **Console du Gestionnaire de Package** et type &quot; **MySql.Data.Entity d’Install-Package - Pre**&quot;, puis appuyez sur ENTRÉE.

> [!NOTE]
> Il s’agit d’une version préliminaire de l’assembly, et par conséquent il peut contenir des bogues. Vous ne devez pas utiliser une version préliminaire du fournisseur en production.


[Cliquez sur l’image suivante pour la développer.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Apporter des modifications de configuration de projet au fichier Web.config pour votre application

Dans cette section, que vous allez configurer Entity Framework pour utiliser le fournisseur MySQL que vous venez d’installer, inscrit la fabrique de fournisseur MySQL et ajoutez votre chaîne de connexion à partir d’Azure.

> [!NOTE]
> Les exemples suivants contiennent une version d’assembly spécifique pour MySql.Data.dll. Si la version d’assembly change, vous devez modifier les paramètres de configuration approprié avec la version correcte.


1. Ouvrez le fichier Web.config de votre projet dans Visual Studio 2013.
2. Recherchez les paramètres de configuration suivants, qui définissent le fournisseur de base de données par défaut et la fabrique pour Entity Framework :

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Remplacez ces paramètres de configuration avec la commande suivante, qui configurera l’Entity Framework pour utiliser le fournisseur MySQL :

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Recherchez le &lt;connectionStrings&gt; section et remplacez-le par le code suivant, qui définit la chaîne de connexion pour votre base de données MySQL est hébergée sur Azure (Notez que providerName valeur a également été modifiée à partir de la d’origine) :

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Ajout de contexte MigrationHistory personnalisé

Entity Framework Code First utilise un **MigrationHistory** tableau pour effectuer le suivi des modifications apportées au modèle et de garantir la cohérence entre le schéma de base de données et le schéma conceptuel. Toutefois, cette table est inopérante pour MySQL par défaut, car la clé primaire est trop volumineuse. Pour remédier à cette situation, vous devez réduire la taille de clé pour cette table. Pour ce faire, procédez comme suit :

1. Les informations de schéma pour cette table sont capturées dans un **HistoryContext**, ce qui peut être modifié comme n’importe quel autre **DbContext**. Pour ce faire, ajoutez un nouveau fichier de classe nommé **MySqlHistoryContext.cs** au projet et remplacez son contenu par le code suivant :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Vous devrez ensuite configurer Entity Framework pour utiliser modifié **HistoryContext**, plutôt que celui par défaut. Cela est possible en tirant parti des fonctionnalités de configuration basée sur le code. Pour ce faire, ajoutez le nouveau fichier de classe nommé **MySqlConfiguration.cs** à votre projet et remplacez son contenu avec :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Création d’un initialiseur d’Entity Framework personnalisé pour ApplicationDbContext

Le fournisseur MySQL qui sera proposé dans ce didacticiel ne gère pas actuellement les migrations Entity Framework, vous devez utiliser des initialiseurs de modèle pour vous connecter à la base de données. Étant donné que ce didacticiel utilise une instance de MySQL sur Azure, vous devez créer un initialiseur d’Entity Framework personnalisé.

> [!NOTE]
> Cette étape n’est pas requise si vous vous connectez à une instance de SQL Server sur Azure ou si vous utilisez une base de données qui est hébergé localement.


Pour créer un initialiseur d’Entity Framework personnalisé pour MySQL, utilisez les étapes suivantes :

1. Ajoutez un nouveau fichier de classe nommé **MySqlInitializer.cs** pour le projet, puis remplacez, il est contenu par le code suivant :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Ouvrez le **IdentityModels.cs** fichier de votre projet, ce qui se trouve dans le **modèles** directory et remplacez son contenu par ce qui suit :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Exécution de l’application et la vérification de la base de données

Une fois que vous avez terminé les étapes décrites dans les sections précédentes, vous devez tester votre base de données. Pour ce faire, procédez comme suit :

1. Appuyez sur **Ctrl + F5** pour générer et exécuter l’application web.
2. Cliquez sur le **inscrire** onglet en haut de la page :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Entrez un nouveau nom d’utilisateur et le mot de passe, puis cliquez sur **inscrire**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. À ce stade, les tables d’ASP.NET Identity sont créés sur la base de données MySQL, et l’utilisateur est inscrit et connecté à l’application :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installation de l’outil MySQL Workbench pour vérifier les données

1. Installer le **MySQL Workbench** outil à partir de la [page des téléchargements de MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Dans l’Assistant installation : **sélection des fonctionnalités** onglet, sélectionnez **MySQL Workbench** sous **applications** section.
3. Lancez l’application et ajouter une nouvelle connexion en utilisant les données de chaîne de connexion à partir de la base de données Azure MySQL que vous avez créé à la lacune de ce didacticiel.
4. Après avoir établi la connexion, inspecter la **ASP.NET Identity** tables créées sur le **IdentityMySQLDatabase.**
5. Vous verrez que tous les ASP.NET nécessaires à l’identité tables sont créées comme indiqué dans l’image ci-dessous :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspecter le **aspnetusers** table par exemple pour vérifier les entrées que vous inscrivez des nouveaux utilisateurs.

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
