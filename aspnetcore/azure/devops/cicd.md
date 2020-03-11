---
title: Intégration continue et déploiement - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Intégration continue et le déploiement dans DevOps avec ASP.NET Core et Azure
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: mvc, seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 5fdf52235b49119503885f92c370dc588e809ffe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655833"
---
# <a name="continuous-integration-and-deployment"></a>Intégration et déploiement continus

Dans le chapitre précédent, vous avez créé un référentiel Git local pour l’application de lecteur de flux Simple. Dans ce chapitre, vous allez publier ce code à un référentiel GitHub et construire un pipeline Azure DevOps Services à l’aide de Pipelines d’Azure. Le pipeline permet des déploiements de l’application et des builds continus. Toute validation vers le dépôt GitHub déclenche une build et un déploiement vers l’emplacement intermédiaire de l’application Web Azure.

Dans cette section, vous allez effectuer les tâches suivantes :

* Publier du code de l’application sur GitHub
* Déconnecter le déploiement Git local
* Création d’une organisation Azure DevOps
* Créer un projet d’équipe dans les Services Azure DevOps
* Créer une définition de build
* Créer un pipeline de mise en production
* Valider les modifications apportées à GitHub et les déployer automatiquement dans Azure
* Examiner le pipeline de Pipelines d’Azure

## <a name="publish-the-apps-code-to-github"></a>Publier du code de l’application sur GitHub

1. Ouvrez une fenêtre de navigateur et accédez à `https://github.com`.
1. Cliquez sur la liste déroulante **+** dans l’en-tête, puis sélectionnez **nouveau référentiel**:

    ![Option de référentiel GitHub](media/cicd/github-new-repo.png)

1. Sélectionnez votre compte dans la liste déroulante **propriétaire** , puis entrez *simple-Feed-Reader* dans la zone de texte **nom du dépôt** .
1. Cliquez sur le bouton **créer un référentiel** .
1. Ouvrez l’interface de commande de votre ordinateur local. Accédez au répertoire dans lequel est stocké le référentiel git du *lecteur de flux simple* .
1. Renommez l' *origine* existante à distance en *amont*. Exécutez la commande suivante :

    ```console
    git remote rename origin upstream
    ```

1. Ajoutez une nouvelle *origine* à distance pointant vers votre copie du référentiel sur GitHub. Exécutez la commande suivante :

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. Publier votre référentiel Git local vers le dépôt GitHub nouvellement créé. Exécutez la commande suivante :

    ```console
    git push -u origin master
    ```

1. Ouvrez une fenêtre de navigateur et accédez à `https://github.com/<GitHub_username>/simple-feed-reader/`. Vérifiez que votre code s’affiche dans le référentiel GitHub.

## <a name="disconnect-local-git-deployment"></a>Déconnecter le déploiement Git local

Supprimer le déploiement Git local en procédant comme suit. Les Pipelines Azure (un service Azure DevOps) à la fois remplace et étend cette fonctionnalité.

1. Ouvrez la [portail Azure](https://portal.azure.com/)et accédez à l’application Web *intermédiaire (mywebapp\<unique_number\>/staging)* . L’application Web peut être rapidement localisée en entrant *intermédiaire* dans la zone de recherche du portail :

    ![terme de recherche application Web intermédiaire](media/cicd/portal-search-box.png)

1. Cliquez sur **Centre de déploiement**. Un nouveau panneau s’affiche. Cliquez sur **déconnecter** pour supprimer la configuration du contrôle de code source git locale qui a été ajoutée dans le chapitre précédent. Confirmez l’opération de suppression en cliquant sur le bouton **Oui** .
1. Accédez au *< mywebapp unique_number >* App service. En guise de rappel, zone de recherche du portail peut être utilisé pour localiser rapidement le Service d’application.
1. Cliquez sur **Centre de déploiement**. Un nouveau panneau s’affiche. Cliquez sur **déconnecter** pour supprimer la configuration du contrôle de code source git locale qui a été ajoutée dans le chapitre précédent. Confirmez l’opération de suppression en cliquant sur le bouton **Oui** .

## <a name="create-an-azure-devops-organization"></a>Création d’une organisation Azure DevOps

1. Ouvrez un navigateur et accédez à la page de création de l' [organisation Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Tapez un nom unique dans la zone de texte **choisir un nom mémorable** pour former l’URL d’accès à votre organisation Azure DevOps.
1. Sélectionnez la case d’option **git** , car le code est hébergé dans un référentiel github.
1. Cliquez sur le bouton **Continuer**. Après une brève attente, un compte et un projet d’équipe, nommés *MyFirstProject*, sont créés.

    ![Page de création d’organisation DevOps Azure](media/cicd/vsts-account-creation.png)

1. Ouvrez l’e-mail de confirmation indiquant que le projet et l’organisation d’Azure DevOps sont prêtes pour une utilisation. Cliquez sur le bouton **Démarrer votre projet** :

    ![Démarrez votre bouton de projet](media/cicd/vsts-start-project.png)

1. Un navigateur s’ouvre pour *\<account_name\>. VisualStudio.com*. Cliquez sur le lien *MyFirstProject* pour commencer à configurer le pipeline DevOps du projet.

## <a name="configure-the-azure-pipelines-pipeline"></a>Configurer le pipeline de Pipelines d’Azure

Il existe trois étapes distinctes pour terminer. Les étapes dans les résultats de trois sections suivantes dans un pipeline DevOps opérationnels.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>Accès Grant Azure DevOps dans le référentiel GitHub

1. Développez le **Code de build ou à partir d’un accordéon de référentiel externe** . Cliquez sur le bouton **configurer la build** :

    ![Bouton Configurer la build](media/cicd/vsts-setup-build.png)

1. Sélectionnez l’option **GitHub** dans la section **Sélectionner une source** :

    ![Sélectionnez une source - GitHub](media/cicd/vsts-select-source.png)

1. L’autorisation est requise que Azure DevOps puisse accéder à votre référentiel GitHub. Entrez *< GitHub_username > connexion GitHub* dans la zone de texte nom de la **connexion** . Par exemple :

    ![Nom de la connexion GitHub](media/cicd/vsts-repo-authz.png)

1. Si l’authentification à deux facteurs est activée sur votre compte GitHub, un jeton d’accès personnel est requis. Dans ce cas, cliquez sur le lien **autoriser avec un jeton d’accès personnel GitHub** . Pour obtenir de l’aide, consultez les [instructions officielles de création d’un jeton d’accès personnel GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) . Seule l’étendue *référentiel* des autorisations est nécessaire. Dans le cas contraire, cliquez sur le bouton **autoriser à l’aide d’OAuth** .
1. Lorsque vous y êtes invité, connectez-vous à votre compte GitHub. Ensuite, sélectionnez Autoriser pour accorder l’accès à votre organisation d’Azure DevOps. Si l’opération réussit, un nouveau point de terminaison de service est créé.
1. Cliquez sur le bouton de sélection en regard du bouton **référentiel** . Sélectionnez le *< GitHub_username > référentiel/simple-Feed-Reader* dans la liste. Cliquez sur le bouton **Sélectionner**.
1. Sélectionnez la branche *principale* à partir de la branche par défaut pour la liste déroulante **manuelle et planifiée des builds** . Cliquez sur le bouton **Continuer**. La page Sélection du modèle s’affiche.

### <a name="create-the-build-definition"></a>Créer la définition de build

1. Dans la page sélection du modèle, entrez *ASP.net Core* dans la zone de recherche :

    ![Recherche d’ASP.NET Core sur la page de modèle](media/cicd/vsts-template-selection.png)

1. Les résultats de recherche de modèle s’affichent. Pointez sur le modèle **ASP.net Core** , puis cliquez sur le bouton **appliquer** .
1. L’onglet **tâches** de la définition de build s’affiche. Cliquez sur l'onglet **Déclencheurs** .
1. Cochez la case **activer l’intégration continue** . Dans la section **filtres de branche** , vérifiez que la liste déroulante **type** est définie sur *include*. Définissez la liste déroulante **spécification de branche** sur *Master*.

    ![Activer les paramètres d’intégration continue](media/cicd/vsts-enable-ci.png)

    Ces paramètres provoquent le déclenchement d’une génération lorsqu’une modification est envoyée à la branche *maître* du référentiel github. L’intégration continue est testée dans la section [valider les modifications apportées à GitHub et déployer automatiquement sur Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) .

1. Cliquez sur le bouton **enregistrer la file d’attente &** , puis sélectionnez l’option **Enregistrer** :

    ![Bouton Enregistrer](media/cicd/vsts-save-build.png)

1. La boîte de dialogue modale s’affiche :

    ![Enregistrer de définition de build - boîte de dialogue modale](media/cicd/vsts-save-modal.png)

    Utilisez le dossier par défaut de *\\* , puis cliquez sur le bouton **Enregistrer** .

### <a name="create-the-release-pipeline"></a>Créer le pipeline de mise en production

1. Cliquez sur l’onglet **mises** en production de votre projet d’équipe. Cliquez sur le bouton **nouveau pipeline** .

    ![Onglet versions - nouveau bouton de définition](media/cicd/vsts-new-release-definition.png)

    Le volet de sélection de modèle s’affiche.

1. Dans la page sélection du modèle, entrez *app service* dans la zone de recherche :

    ![Zone de recherche de modèle de pipeline mise en production](media/cicd/vsts-release-template-search.png)

1. Les résultats de recherche de modèle s’affichent. Pointez sur le modèle **Azure App service déploiement avec emplacement** , puis cliquez sur le bouton **appliquer** . L’onglet **pipeline** du pipeline de mise en sortie s’affiche.

    ![Pipeline de versions onglet Pipeline](media/cicd/vsts-release-definition-pipeline.png)

1. Cliquez sur le bouton **Ajouter** dans la zone **artefacts** . Le panneau **Ajouter un artefact** s’affiche :

    ![Pipeline de versions - ajouter panneau d’artefact](media/cicd/vsts-release-add-artifact.png)

1. Sélectionnez la vignette **Build** dans la section **type de source** . Ce type permet de lier le pipeline de mise en production pour la définition de build.
1. Sélectionnez *MyFirstProject* dans la liste déroulante **projet** .
1. Sélectionnez le nom de la définition de build, *MyFirstProject-ASP.net Core-ci*, dans la liste déroulante **source (définition de Build)** .
1. Sélectionnez *dernier* dans la liste déroulante **version par défaut** . Cette option génère les artefacts générés par la dernière exécution de la définition de build.
1. Remplacez le texte de la zone de texte **alias source** par *Drop*.
1. Cliquez sur le bouton **Add** . La section **artefacts** est mise à jour pour afficher les modifications.
1. Cliquez sur l’icône d’éclair pour activer les déploiements continus :

    ![Pipeline de versions artefacts - icône d’éclair](media/cicd/vsts-artifacts-lightning-bolt.png)

    Avec cette option est activée, un déploiement se produit chaque fois qu’une nouvelle build est disponible.
1. Un panneau **déclencheur de déploiement continu** s’affiche à droite. Cliquez sur le bouton bascule pour activer la fonctionnalité. Il n’est pas nécessaire d’activer le **déclencheur de requête de tirage**.
1. Cliquez sur la liste déroulante **Ajouter** dans la section **créer des filtres de branche** . Choisissez l’option **de branche par défaut de la définition de build** . Ce filtre entraîne le déclenchement de la mise en sortie uniquement pour une build à partir de la branche *maître* du référentiel github.
1. Cliquez sur le bouton **Enregistrer** . Cliquez sur le bouton **OK** dans la boîte de dialogue d' **enregistrement** modal.
1. Cliquez sur la zone **environnement 1** . Un panneau d' **environnement** s’affiche à droite. Modifiez le texte de l' *environnement 1* dans la zone de texte nom de l' **environnement** en *production*.

   ![Pipeline de versions - zone de texte Nom environnement](media/cicd/vsts-environment-name-textbox.png)

1. Cliquez sur le lien **1 phase, 2 tâches** dans la zone **production** :

    ![Pipeline de versions - link.png d’environnement de Production](media/cicd/vsts-production-link.png)

    L’onglet **tâches** de l’environnement s’affiche.
1. Cliquez sur la tâche **déployer Azure App service à l’emplacement** . Ses paramètres s’affichent dans le panneau vers la droite.
1. Sélectionnez l’abonnement Azure associé à la App Service dans la liste déroulante **abonnement Azure** . Une fois sélectionné, cliquez sur le bouton **autoriser** .
1. Sélectionnez *application Web* dans la liste déroulante **type d’application** .
1. Sélectionnez *myWebApp/< unique_number/>* dans la liste déroulante nom de l' **app service** .
1. Sélectionnez *AzureTutorial* dans la liste déroulante **groupe de ressources** .
1. Sélectionnez *intermédiaire* dans la liste déroulante **emplacement** .
1. Cliquez sur le bouton **Enregistrer** .
1. Pointez sur le nom de pipeline de version par défaut. Cliquez sur l’icône de crayon pour le modifier. Utilisez *MyFirstProject-ASP.net Core-CD* comme nom.

    ![Nom de pipeline de mise en production](media/cicd/vsts-release-definition-name.png)

1. Cliquez sur le bouton **Enregistrer** .

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Valider les modifications apportées à GitHub et les déployer automatiquement dans Azure

1. Ouvrez *SimpleFeedReader. sln* dans Visual Studio.
1. Dans Explorateur de solutions, ouvrez *Pages\Index.cshtml*. Remplacez `<h2>Simple Feed Reader - V3</h2>` par `<h2>Simple Feed Reader - V4</h2>`.
1. Appuyez sur **Ctrl**+**MAJ**+**B** pour générer l’application.
1. Valider le fichier vers le dépôt GitHub. Utilisez la page **modifications** dans l’onglet *Team Explorer* de Visual Studio, ou exécutez la commande suivante à l’aide de l’interface de commande de l’ordinateur local :

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. Transmettent la modification dans la branche *principale* à l' *origine* distante de votre référentiel GitHub :

    ```console
    git push origin master
    ```

    La validation s’affiche dans la branche *principale* du dépôt github :

    ![Validation de GitHub dans la branche principale](media/cicd/github-commit.png)

    La build est déclenchée, car l’intégration continue est activée dans l’onglet **déclencheurs** de la définition de build :

    ![Activer l’intégration continue](media/cicd/enable-ci.png)

1. Accédez à l’onglet en **file d’attente** de la page **Azure pipelines** > **Builds** dans Azure DevOps services. La build en file d’attente affiche la branche et la validation qui a déclenché la génération :

    ![build en file d’attente](media/cicd/build-queued.png)

1. Une fois la génération terminée, un déploiement vers Azure se produit. Accédez à l’application dans le navigateur. Notez que le texte « V4 » s’affiche dans l’en-tête :

    ![application mise à jour](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Examiner le pipeline de Pipelines d’Azure

### <a name="build-definition"></a>Définition de build

Une définition de Build a été créée avec le nom *MyFirstProject-ASP.net Core-ci*. Une fois l’opération terminée, la génération produit un fichier *. zip* incluant les ressources à publier. Le pipeline de mise en production déploie ces ressources sur Azure.

L’onglet **tâches** de la définition de build répertorie les étapes individuelles en cours d’utilisation. Il existe cinq tâches de génération.

![tâches de définition de build](media/cicd/build-definition-tasks.png)

1. **Restore** &mdash; exécute la commande `dotnet restore` pour restaurer les packages NuGet de l’application. Le package par défaut utilisé un flux est nuget.org.
1. **Build** &mdash; exécute la commande `dotnet build --configuration release` pour compiler le code de l’application. Cette option `--configuration` est utilisée pour produire une version optimisée du code, qui convient au déploiement dans un environnement de production. Modifiez la variable *BuildConfiguration* sous l’onglet **variables** de la définition de build si, par exemple, une configuration Debug est nécessaire.
1. **Test** &mdash; exécute la commande `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` pour exécuter les tests unitaires de l’application. Les tests unitaires sont exécutés dans n’importe quel C# projet correspondant au modèle `**/*Tests/*.csproj` glob. Les résultats des tests sont enregistrés dans un fichier *. trx* à l’emplacement spécifié par l’option `--results-directory`. Si tous les tests échouent, la build échoue et n’est pas déployée.

    > [!NOTE]
    > Pour vérifier le fonctionnement des tests unitaires, modifiez *SimpleFeedReader. Tests\Services\NewsServiceTests.cs* afin de rompre intentionnellement l’un des tests. Par exemple, remplacez `Assert.True(result.Count > 0);` par `Assert.False(result.Count > 0);` dans la méthode `Returns_News_Stories_Given_Valid_Uri`. Validez et envoyez la modification à GitHub. La build est déclenchée et échoue. L’état du pipeline de build passe à **échec**. Rétablir la modification, valider et l’envoyer à nouveau. La build réussit.

1. L' &mdash; de **publication** exécute la commande `dotnet publish --configuration release --output <local_path_on_build_agent>` pour générer un fichier *. zip* avec les artefacts à déployer. L’option `--output` spécifie l’emplacement de publication du fichier *. zip* . Cet emplacement est spécifié en passant une [variable prédéfinie](/azure/devops/pipelines/build/variables) nommée `$(build.artifactstagingdirectory)`. Cette variable se développe en un chemin d’accès local, tel que *c:\agent\_work\1\a*, sur l’agent de Build.
1. **Publier l’artefact** &mdash; publie le fichier *. zip* produit par la tâche de **publication** . La tâche accepte l’emplacement du fichier *. zip* en tant que paramètre, qui est la variable prédéfinie `$(build.artifactstagingdirectory)`. Le fichier *. zip* est publié sous la forme d’un dossier nommé *Drop*.

Cliquez sur le lien **Résumé** de la définition de build pour afficher un historique des builds avec la définition :

![Historique de capture d’écran montrant build définition](media/cicd/build-definition-summary.png)

Dans la page résultante, cliquez sur le lien correspondant au numéro de build unique :

![Page de résumé de définition de capture d’écran montrant build](media/cicd/build-definition-completed.png)

Un résumé de cette build spécifique s’affiche. Cliquez sur l’onglet **artefacts** , et notez que le dossier de *dépôt* produit par la build est listé :

![Capture d’écran montrant les artefacts de définition de build - dossier de dépôt](media/cicd/build-definition-artifacts.png)

Utilisez les liens **Télécharger** et **Explorer** pour inspecter les artefacts publiés.

### <a name="release-pipeline"></a>Pipeline de mise en production

Un pipeline de version a été créé avec le nom *MyFirstProject-ASP.net Core-CD*:

![Vue d’ensemble du pipeline capture d’écran montrant mise en production](media/cicd/release-definition-overview.png)

Les deux principaux composants du pipeline de mise en version sont les **artefacts** et les **environnements**. Le fait de cliquer sur la zone dans la section **artefacts** affiche le panneau suivant :

![Artefacts de capture d’écran montrant version pipeline](media/cicd/release-definition-artifacts.png)

La valeur **source (définition de Build)** représente la définition de build à laquelle ce pipeline de mise en version est lié. Le fichier *. zip* produit par une exécution réussie de la définition de build est fourni à l’environnement de *production* pour le déploiement sur Azure. Cliquez sur le lien *1 phase, 2 tâches* dans la zone environnement de production pour afficher les tâches de pipeline de mise en *production* :

![Capture d’écran affichant les tâches pipeline mise en production](media/cicd/release-definition-tasks.png)

Le pipeline de mise en version est constitué de deux tâches : *déployer des Azure App service sur l’emplacement* et gérer l' *échange d’emplacement Azure App service*. En cliquant sur la première tâche, vous affichez la configuration de la tâche suivante :

![Tâche de déploiement du pipeline de versions de capture d’écran montrant](media/cicd/release-definition-task1.png)

Abonnement Azure, type de service, nom de l’application web, groupe de ressources et emplacement de déploiement sont définies dans la tâche de déploiement. La zone de texte **package ou dossier** contient le chemin d’accès du fichier *. zip* à extraire et à déployer dans l’emplacement *intermédiaire* de l' *\<myWebApp unique_number\>* application Web.

En cliquant sur la tâche de permutation d’emplacement révèle la configuration de la tâche suivante :

![Tâche de capture d’écran montrant version pipeline slot swap](media/cicd/release-definition-task2.png)

L’abonnement, groupe de ressources, type de service, nom de l’application web et les détails d’emplacement de déploiement sont fournis. La case à cocher **swap with production** est activée. Par conséquent, les bits déployés sur l’emplacement *intermédiaire* sont échangés dans l’environnement de production.

## <a name="additional-reading"></a>Documentation supplémentaire

* [Créer son premier pipeline avec Azure Pipelines](/azure/devops/pipelines/get-started-yaml)
* [Build et projet .NET Core](/azure/devops/pipelines/languages/dotnet-core)
* [Déployer une application Web avec Azure Pipelines](/azure/devops/pipelines/targets/webapp)
