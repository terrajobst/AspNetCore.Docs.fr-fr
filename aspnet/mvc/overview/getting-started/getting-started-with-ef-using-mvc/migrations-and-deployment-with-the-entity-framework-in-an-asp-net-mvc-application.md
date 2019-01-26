---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutoriel : Utiliser des Migrations Entity Framework dans une application ASP.NET MVC et le déployer dans Azure'
author: tdykstra
description: Dans ce didacticiel, vous activez les migrations Code First et déployez l’application sur le cloud dans Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dd6bf5d8eb8a05dad1d230ef40c9b863e2af7094
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889910"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Tutoriel : Utiliser des Migrations Entity Framework dans une application ASP.NET MVC et le déployer dans Azure

Jusqu'à présent, l’exemple d’application web Contoso University a été exécuté localement dans IIS Express sur votre ordinateur de développement. Pour rendre une application réelle disponible pour d’autres personnes à utiliser sur Internet, vous devez déployer sur un fournisseur d’hébergement web. Dans ce didacticiel, vous activez des migrations Code First et déployez l’application sur le cloud dans Azure :

- Activer les Migrations Code First. La fonctionnalité de Migrations vous permet de modifier le modèle de données et de déployer vos modifications en production en mettant à jour le schéma de base de données sans devoir supprimer et recréer la base de données.
- Déployer sur Azure. Cette étape est facultative. Vous pouvez continuer avec les didacticiels restants sans avoir déployé le projet.

Nous recommandons que vous utilisez un processus d’intégration continue avec contrôle de code source pour le déploiement, mais ce didacticiel ne couvre pas ces rubriques. Pour plus d’informations, consultez le [contrôle de code source](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) et [intégration continue](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) chapitres de [développement d’applications Cloud réalistes avec Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Activer les migrations Code First
> * Déployer l’application dans Azure (facultatif)

## <a name="prerequisites"></a>Prérequis

- [Résilience des connexions et interception des commandes](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Activer les migrations Code First

Quand vous développez une nouvelle application, votre modèle de données change fréquemment et, chaque fois que le modèle change, il n’est plus en synchronisation avec la base de données. Vous avez configuré l’Entity Framework pour supprimer et recréer la base de données chaque fois que vous modifiez le modèle de données automatiquement. Lorsque vous ajouter, supprimer ou modifier des classes d’entité ou modifier votre `DbContext` classe, la prochaine fois que vous exécutez l’application elle supprime de votre base de données existante, crée un nouveau qui correspond au modèle et l’alimente avec les données de test d’automatiquement.

Cette méthode pour conserver la base de données en synchronisation avec le modèle de données fonctionne bien jusqu’au déploiement de l’application en production. Lorsque l’application s’exécute en production, elle stocke généralement les données que vous souhaitez conserver, et vous ne souhaitez pas tout perdre chaque fois que vous apportez une modification telles que l’ajout d’une nouvelle colonne. Le [Migrations Code First](https://msdn.microsoft.com/data/jj591621) fonctionnalité résout ce problème en activant Code First mettre à jour le schéma de base de données au lieu de devoir supprimer et recréer la base de données. Dans ce didacticiel, vous allez déployer l’application, et pour préparer pour ce faire, vous allez activer les Migrations.

1. Désactiver l’initialiseur que vous avez configurés précédemment en supprimant ou en supprimant le `contexts` élément que vous avez ajouté au fichier Web.config de l’application.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Également dans l’application *Web.config* , modifiez le nom de la base de données dans la chaîne de connexion par contosouniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Cette modification configure le projet afin que la première migration crée une nouvelle base de données. Cela n’est pas obligatoire, mais vous verrez plus tard pourquoi c’est une bonne idée.
3. Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.

1. À la `PM>` invite Entrez les commandes suivantes :

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    Le `enable-migrations` commande crée un *Migrations* dossier dans le projet ContosoUniversity et il place dans ce dossier un *Configuration.cs* fichier que vous pouvez modifier pour configurer les Migrations.

    (Si vous avez manqué l’étape précédente qui vous indique comment modifier le nom de la base de données, Migrations recherche la base de données existante et effectuer automatiquement la `add-migration` commande. C’est OK, cela signifie simplement que vous n’exécutez un test du code de migrations avant de déployer la base de données. Par la suite lorsque vous exécutez le `update-database` commande rien ne se produira, car la base de données existe déjà.)

    Ouvrez le *ContosoUniversity\Migrations\Configuration.cs* fichier. Tout comme la classe d’initialiseur qui vous l’avez vu précédemment, le `Configuration` classe inclut un `Seed` (méthode).

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    L’objectif de la [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) méthode consiste à vous permettre d’insérer ou mettre à jour des données de test après le Code First crée ou met à jour de la base de données. La méthode est appelée lorsque la base de données est créé et chaque fois que le schéma de base de données est mis à jour après un modèle de données est modifié.

### <a name="set-up-the-seed-method"></a>Configurer la méthode Seed

Lorsque vous supprimez et recréez la base de données pour chaque modification de modèle de données, vous utilisez la classe d’initialiseur `Seed` méthode pour insérer des données de test, car après chaque modification de modèle, la base de données est supprimée et toutes les données de test est perdue. Avec les Migrations Code First, test, les données sont conservées après les modifications de la base de données, par conséquent, y compris les données de test dans le [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) méthode n’est généralement pas nécessaire. En fait, vous ne souhaitez pas la `Seed` méthode pour insérer des données de test si vous allez utiliser les Migrations à déployer la base de données en production, car le `Seed` méthode s’exécute en production. Dans ce cas, vous souhaitez que le `Seed` (méthode) à insérer dans la base de données uniquement les données dont vous avez besoin en production. Par exemple, vous pouvez choisir la base de données à inclure les noms de service réel dans le `Department` table lorsque l’application est disponible en production.

Pour ce didacticiel, vous allez utiliser Migrations pour le déploiement, mais que votre `Seed` méthode insère les données de test tout de même pour le rendre plus facile à observer le fonctionne de la fonctionnalité de l’application sans avoir à insérer manuellement une grande quantité de données.

1. Remplacez le contenu de la *Configuration.cs* fichier avec le code suivant, qui charge les données de test dans la base de données.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Le [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) méthode prend l’objet de contexte de base de données comme paramètre d’entrée, et le code dans la méthode utilise cet objet pour ajouter de nouvelles entités à la base de données. Pour chaque type d’entité, le code crée une collection de nouvelles entités, les ajoute à approprié [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) propriété, puis enregistre les modifications apportées à la base de données. Il n’est pas nécessaire d’appeler le [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) méthode après chaque groupe d’entités, comme le fait ici, mais cela vous permet de localiser la source d’un problème si une exception se produit pendant que le code écrit pour la base de données.

    Certaines des instructions qui insèrent des données utilisent le [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode pour effectuer une opération « upsert ». Étant donné que le `Seed` méthode s’exécute chaque fois que vous exécutez le `update-database` de commande, généralement après chaque migration, vous ne pouvez pas simplement insérer des données, car les lignes que vous essayez d’ajouter seront déjà il après la première migration qui crée la base de données. L’opération « upsert » empêche les erreurs qui se passerait si vous essayez d’insérer une ligne qui existe déjà, mais il ***substitue*** les modifications apportées aux données que vous avez apportées lors du test de l’application. Avec les données de test dans certaines tables vous ne souhaitez pas que cela se produise : dans certains cas lorsque vous modifiez des données lors du test vous voulez que vos modifications restant après les mises à jour de la base de données. Dans ce cas, vous souhaitez effectuer une opération d’insertion conditionnel : insérer une ligne uniquement si elle n’existe pas déjà. La méthode Seed utilise les deux approches.

    Le premier paramètre passé à la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) méthode spécifie la propriété à utiliser pour vérifier si une ligne existe déjà. Pour les données d’étudiant de test que vous fournissez, le `LastName` propriété peut être utilisée à cet effet dans la mesure où chaque nom de famille dans la liste est unique :

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Ce code suppose que les noms sont uniques. Si vous ajoutez manuellement un étudiant ayant un nom en double, vous obtiendrez l’exception suivante la prochaine fois que vous effectuez une migration :

    **Séquence contient plusieurs éléments**

    Pour plus d’informations sur la gestion des données redondantes, tels que les deux étudiants nommés « Alexander Carson », consultez [amorçage et débogage Entity Framework (EF) des bases de données](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sur le blog de Rick Anderson. Pour plus d’informations sur la `AddOrUpdate` (méthode), consultez [prendre en charge avec une méthode EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sur le blog de Julie.

    Le code qui crée `Enrollment` entités suppose que vous avez le `ID` valeur dans les entités dans le `students` collection, même si vous n’avez pas défini cette propriété dans le code qui crée la collection.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Vous pouvez utiliser la `ID` propriété ici, car le `ID` a la valeur lorsque vous appelez `SaveChanges` pour le `students` collection. Entity Framework obtient automatiquement la valeur de clé primaire quand il insère une entité dans la base de données, et il met à jour le `ID` propriété de l’entité dans la mémoire.

    Le code qui ajoute chacun d’eux `Enrollment` entité à la `Enrollments` jeu d’entités n’utilise pas le `AddOrUpdate` (méthode). Il vérifie si une entité existe déjà et qu’il insère l’entité si elle n’existe pas. Cette approche conserve les modifications que vous apportez à un niveau d’inscription à l’aide de l’interface utilisateur de l’application. Le code effectue une itération sur chaque membre de la `Enrollment` [liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) et si l’inscription est introuvable dans la base de données, il ajoute l’inscription à la base de données. La première fois que vous mettez à jour la base de données, la base de données sera vide, donc il ajoutera chaque inscription.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Générez le projet.

### <a name="execute-the-first-migration"></a>Exécutez la première migration

Lorsque vous avez exécuté le `add-migration` Migrations a généré le code qui crée la base de données à partir de zéro de la commande. Ce code est également dans le *Migrations* dossier, dans le fichier nommé  *&lt;timestamp&gt;\_InitialCreate.cs*. Le `Up` méthode de la `InitialCreate` classe crée les tables de base de données qui correspondent aux jeux d’entités de modèle de données, et le `Down` méthode les supprime.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La fonctionnalité Migrations appelle la méthode `Up` pour implémenter les modifications du modèle de données pour une migration. Quand vous entrez une commande pour annuler la mise à jour, Migrations appelle la méthode `Down`.

Il s’agit la migration initiale qui a été créée lorsque vous avez entré le `add-migration InitialCreate` commande. Le paramètre (`InitialCreate` dans l’exemple) est utilisé pour le fichier de nom et peut être tout ce que vous voulez ; vous choisissez généralement un mot ou une expression qui résume ce qui est effectué lors de la migration. Par exemple, vous pouvez nommer une migration ultérieure &quot;Ajoutertabledépartement&quot;.

Si vous avez créé la migration initiale alors que la base de données existait déjà, le code de création de la base de données est généré, mais il n’est pas nécessaire de l’exécuter, car la base de données correspond déjà au modèle de données. Quand vous déployez l’application sur un autre environnement où la base de données n’existe pas encore, ce code est exécuté pour créer votre base de données : il est donc judicieux de le tester au préalable. C’est pourquoi vous avez modifié le nom de la base de données dans la chaîne de connexion précédemment&mdash;afin que les migrations peuvent créer un nouveau à partir de zéro.

1. Dans le **Console du Gestionnaire de Package** fenêtre, entrez la commande suivante :

    `update-database`

    Le `update-database` commande exécute le `Up` méthode pour créer la base de données, puis il exécute la `Seed` méthode pour remplir la base de données. Le même processus s’exécute automatiquement en production après avoir déployé l’application, comme vous le verrez dans la section suivante.
2. Utilisez **Explorateur de serveurs** pour inspecter la base de données comme vous le faisiez dans le premier didacticiel, exécutez l’application pour vérifier que tout fonctionne toujours comme avant.

## <a name="deploy-to-azure"></a>Déployer sur Azure

Jusqu'à présent, l’application a été exécuté localement dans IIS Express sur votre ordinateur de développement. Pour le rendre disponible pour d’autres personnes à utiliser sur Internet, vous devez déployer sur un fournisseur d’hébergement web. Dans cette section du didacticiel, vous allez la déployer vers Azure. Cette section est facultative. Vous pouvez ignorer cette étape et passez au tutoriel suivant, ou vous pouvez adapter les instructions de cette section pour un autre fournisseur d’hébergement de votre choix.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Utiliser des migrations Code First pour déployer la base de données

Pour déployer la base de données, vous allez utiliser les Migrations Code First. Lorsque vous créez le profil de publication que vous utilisez pour configurer les paramètres de déploiement à partir de Visual Studio, vous allez sélectionner une case à cocher intitulée **mise à jour la base de données**. Ce paramètre entraîne le processus de déploiement configurer automatiquement l’application *Web.config* des fichiers sur le serveur de destination afin qu’utilise Code First le `MigrateDatabaseToLatestVersion` classe d’initialiseur.

Visual Studio ne fait rien avec la base de données pendant le processus de déploiement pendant qu’il est copie votre projet sur le serveur de destination. Lorsque vous exécutez l’application déployée et qu’il accède à la base de données pour la première fois après le déploiement, Code First vérifie de si la base de données correspond au modèle de données. S’il existe une incompatibilité, Code First crée automatiquement la base de données (si elle n’existe pas encore) ou met à jour le schéma de base de données vers la dernière version (si une base de données existe mais ne correspond pas au modèle). Si l’application implémente un Migrations `Seed` méthode, l’exécution de la méthode après la création de la base de données ou le schéma est mis à jour.

Vos Migrations `Seed` méthode insère des données de test. Si vous déployez dans un environnement de production, vous seriez obligé de modifier le `Seed` méthode afin qu’il insère uniquement des données que vous souhaitez insérer dans votre base de données de production. Par exemple, dans votre modèle de données actuel, vous souhaiterez avoir des cours réels, mais les étudiants fictives dans la base de données de développement. Vous pouvez écrire un `Seed` méthode pour charger les deux dans le développement et commentez les étudiants fictives avant de déployer en production. Ou vous pouvez écrire un `Seed` méthode pour charger uniquement les cours et entrer manuellement des étudiants fictives dans la base de données de test à l’aide de l’interface utilisateur de l’application.

### <a name="get-an-azure-account"></a>Obtenir un compte Azure

Vous aurez besoin d’un compte Azure. Si vous n’en avez pas déjà, mais vous avez un abonnement Visual Studio, vous pouvez [activer vos avantages d’abonnement](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). Sinon, vous pouvez créer un compte d’essai gratuit en quelques minutes. Pour plus d’informations, consultez [version d’évaluation gratuite Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Créer un site web et une base de données SQL dans Azure

Votre application web dans Azure s’exécutera dans un environnement d’hébergement partagé, ce qui signifie qu’il s’exécute sur des machines virtuelles (VM) qui sont partagés avec d’autres clients Azure. Un environnement d’hébergement partagé est un moyen économique de prise en main dans le cloud. Plus tard, si votre trafic web augmente, l’application peut mettre à l’échelle pour répondre aux besoins en exécutant sur des machines virtuelles dédiées. Pour en savoir plus sur les Options de tarification pour Azure App Service, consultez [tarification App Service](https://azure.microsoft.com/pricing/details/app-service/).

Vous allez déployer la base de données à base de données SQL Azure. Base de données SQL est un service de base de données relationnelle en nuage qui repose sur les technologies SQL Server. Outils et applications qui fonctionnent avec SQL Server fonctionnent également avec la base de données SQL.

1. Dans le [portail de gestion](https://portal.azure.com), choisissez **créer une ressource** dans gauche onglet, puis choisissez **afficher tout** sur le **New** volet (ou *panneau*) pour afficher toutes les ressources disponibles. Choisissez **application Web + SQL** dans le **Web** section de la **tout** panneau. Enfin, choisissez **créer**.

    ![Créer une ressource dans le portail Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Le formulaire pour créer un nouveau **nouvelle application Web + SQL** ouvre de ressources.

2. Entrez une chaîne dans le **nom de l’application** zone à utiliser comme URL unique pour votre application. L’URL complète se compose de ce que vous entrez ici ainsi que le domaine par défaut d’Azure App Services (. azurewebsites.net). Si le **nom de l’application** est déjà utilisé, l’Assistant vous avertit d’une croix rouge *le nom de l’application n’est pas disponible* message. Si le **nom de l’application** est disponible, vous voyez une coche verte.

3. Dans le **abonnement** , sélectionnez l’abonnement Azure dans lequel vous souhaitez le **App Service** réside.

4. Dans le **groupe de ressources** zone de texte, choisissez un groupe de ressources ou créez-en un. Ce paramètre spécifie votre site web s’exécutera dans le centre de données. Pour plus d’informations sur les groupes de ressources, consultez [groupes de ressources](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Créer un nouveau **Plan App Service** en cliquant sur le *section App Service*, **créer un nouveau**et renseignez **plan App Service** (peut être identique à celui App Service), **emplacement**, et **niveau tarifaire** (il existe une option gratuite).

6. Cliquez sur **base de données SQL**, puis choisissez **créer une base de données** ou sélectionnez une base de données existante.

7. Dans le **nom** , entrez un nom pour votre base de données.
8. Cliquez sur le **serveur cible** zone, puis sélectionnez **créer un nouveau serveur**. Vous pouvez également, si vous avez créé précédemment un serveur, vous pouvez sélectionner ce serveur à partir de la liste des serveurs disponibles.
9. Choisissez **niveau tarifaire** , choisissez *gratuit*. Si des ressources supplémentaires sont nécessaires, la base de données peut être mis à l’échelle à tout moment. Pour en savoir plus sur la tarification de SQL Azure, consultez [tarification d’Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modifier [classement](/sql/relational-databases/collations/collation-and-unicode-support) en fonction des besoins.
11. Entrez un administrateur **nom d’utilisateur administrateur de SQL** et **mot de passe administrateur SQL**.

   - Si vous avez sélectionné **serveur de base de données SQL**, définir un nouveau nom et mot de passe que vous utiliserez plus tard lorsque vous accédez à la base de données.
   - Si vous avez sélectionné un serveur que vous avez créé précédemment, entrez les informations d’identification pour ce serveur.

12. Collecte de données de télémétrie peut être activée pour App Service à l’aide d’Application Insights. Avec peu de configuration, Application Insights collecte les événements précieux, exception, dépendance, demande et les informations de trace. Pour en savoir plus sur Application Insights, consultez [Azure Monitor](https://azure.microsoft.com/services/monitor/).
13. Cliquez sur **créer** en bas pour indiquer que vous avez terminé.

    Le portail de gestion retourne à la page tableau de bord et le **Notifications** zone en haut de la page indique que le site est en cours de création. Après un certain temps (généralement inférieur à une minute), il existe une notification indiquant que le déploiement a réussi. Dans la barre de navigation à gauche, le nouveau Service d’application s’affiche dans le **App Services** section et la nouvelle base de données SQL s’affiche dans le **bases de données SQL** section.

### <a name="deploy-the-app-to-azure"></a>Déployer l’application sur Azure

1. Dans Visual Studio, cliquez sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier** dans le menu contextuel.

2. Sur le **choisir une cible de publication** page, choisissez **App Service** , puis **sélectionner**, puis choisissez **publier**.

    ![Choisir une page cible de publication](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Si vous n’avez pas précédemment ajouté à votre abonnement Azure dans Visual Studio, effectuez les opérations sur l’écran. Ces étapes permettent à Visual Studio pour vous connecter à votre abonnement Azure ainsi que la liste des **App Services** inclura votre site web.

4. Sur le **App Service** page, sélectionnez le **abonnement** vous avez ajouté le Service à l’application. Sous **vue**, sélectionnez **groupe de ressources**. Développez le groupe de ressources que vous avez ajouté le Service à l’application, puis sélectionnez le Service d’application. Choisissez **OK** pour publier l’application.

5. Le **sortie** fenêtre montre les actions de déploiement ont été effectuées et signale la réussite du déploiement.

6. Après un déploiement réussi, le navigateur par défaut ouvre automatiquement l’URL du site web déployé.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    Votre application est maintenant en cours d’exécution dans le cloud.

À ce stade, le *SchoolContext* base de données a été créé dans la base de données SQL Azure, car vous avez sélectionné **exécuter les Migrations Code First (s’exécute sur le démarrage de l’application)**. Le *Web.config* fichier dans le site web déployé a été modifié afin que le [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) initialiseur s’exécute la première fois que votre code lit ou écrit des données dans la base de données (qui s’est produite Lorsque vous avez sélectionné le **étudiants** onglet) :

![Extrait du fichier Web.config](https://asp.net/media/4367421/mig.png)

Le processus de déploiement créé également une nouvelle chaîne de connexion *(SchoolContext\_DatabasePublish*) pour les Migrations Code First à utiliser pour l’amorçage de la base de données et de mise à jour le schéma de base de données.

![Chaîne de connexion dans le fichier Web.config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Vous trouverez la version déployée du fichier Web.config sur votre ordinateur dans *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Vous pouvez accéder à la version déployée *Web.config* fichier lui-même à l’aide de FTP. Pour obtenir des instructions, consultez [déploiement de Web ASP.NET à l’aide de Visual Studio : Déploiement d’une mise à jour du Code](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Suivez les instructions qui commencent par « pour utiliser un outil FTP, vous avez besoin de trois choses : l’URL de FTP, le nom d’utilisateur et le mot de passe. »

> [!NOTE]
> L’application web n’implémente pas la sécurité, toute personne trouvant l’URL permettant de changer les données. Pour obtenir des instructions sur la façon de sécuriser le site web, consultez [déployer une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL database dans Azure](/aspnet/core/security/authorization/secure-data). Vous pouvez empêcher les autres personnes de l’utilisation du site en arrêtant le service à l’aide du portail de gestion Azure ou **Explorateur de serveurs** dans Visual Studio.

![Arrêter l’élément de menu app service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Scénarios de migrations avancées

Si vous déployez une base de données en exécutant des migrations automatiquement comme indiqué dans ce didacticiel, et que vous déployez sur un site web qui s’exécute sur plusieurs serveurs, vous pouvez obtenir plusieurs serveurs essaie d’exécuter des migrations en même temps. Les migrations sont atomiques, donc si vous essaient d’exécuter la même migration deux serveurs, un réussira et l’autre échoue (en supposant que les opérations ne peuvent pas être effectuées deux fois). Dans ce scénario si vous souhaitez éviter ces problèmes, vous pouvez appeler manuellement les migrations et définir votre propre code afin qu’il se produit uniquement une fois. Pour plus d’informations, consultez [en cours d’exécution et d’écriture de scripts des Migrations à partir du Code](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) sur le blog de Rowan Miller et [Migrate.exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (pour l’exécution des migrations à partir de la ligne de commande).

Pour plus d’informations sur les autres scénarios de migration, consultez [série de screencasts Migrations](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>Initialiseurs First de code

Dans la section déploiement, vous avez vu le [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) initialiseur utilisé. Code tout d’abord fournit également des autres initialiseurs, y compris [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (la valeur par défaut), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (que vous avez utilisées précédemment) et [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Le `DropCreateAlways` initialiseur peut être utile pour configurer des conditions pour les tests unitaires. Vous pouvez également écrire vos propres initialiseurs, et vous pouvez appeler un initialiseur explicitement si vous ne souhaitez pas attendre jusqu'à ce que l’application lit ou écrit dans la base de données.

Pour plus d’informations sur les initialiseurs, consultez [compréhension des initialiseurs de base de données dans Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) et le chapitre 6 du livre [Programming Entity Framework : Code First](http://shop.oreilly.com/product/0636920022220.do) par Julie Lerman et Rowan Miller.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources Entity Framework dans [accès aux données ASP.NET - ressources recommandées](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Migrations Code First est activées
> * Déploiement de l’application dans Azure (facultatif)

Passez à l’article suivant pour apprendre à créer un modèle de données plus complexe pour une Application ASP.NET MVC.
> [!div class="nextstepaction"]
> [Créer un modèle de données plus complexe](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
