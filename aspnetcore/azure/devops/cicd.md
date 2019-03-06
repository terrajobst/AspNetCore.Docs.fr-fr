---
title: Intégration continue et déploiement - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Intégration continue et le déploiement dans DevOps avec ASP.NET Core et Azure
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 906aae3fd4b4abd0becc8847b0f54c372bda300a
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346305"
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
* Valider les modifications à GitHub et déployer automatiquement vers Azure
* Examiner le pipeline de Pipelines d’Azure

## <a name="publish-the-apps-code-to-github"></a>Publier du code de l’application sur GitHub

1. Ouvrez une fenêtre de navigateur et accédez à `https://github.com`.
1. Cliquez sur le **+** dans l’en-tête de liste déroulante, puis sélectionnez **nouveau référentiel**:

    ![Option de référentiel GitHub](media/cicd/github-new-repo.png)

1. Sélectionnez votre compte dans le **propriétaire** liste déroulante, puis entrez *lecture de flux simple* dans le **nom du référentiel** zone de texte.
1. Cliquez sur le **créer référentiel** bouton.
1. Ouvrez l’interface de commande de votre ordinateur local. Accédez au répertoire dans lequel le *lecture de flux simple* référentiel Git est stocké.
1. Renommez les fichiers *origine* distant à *en amont*. Exécutez la commande suivante :
    ```console
    git remote rename origin upstream
    ```
1. Ajouter un nouveau *origine* distant qui pointe vers votre copie du référentiel sur GitHub. Exécutez la commande suivante :
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

1. Ouvrir le [portail Azure](https://portal.azure.com/)et accédez à la *intermédiaire (mywebapp\<unique_number\>/intermédiaire)* application Web. L’application Web peut se trouver rapidement en entrant *intermédiaire* dans la zone de recherche du portail :

    ![terme de recherche application Web intermédiaire](media/cicd/portal-search-box.png)

1. Cliquez sur **Deployment Center**. Un nouveau panneau s’affiche. Cliquez sur **déconnexion** pour supprimer la configuration de contrôle d’une source de Git locale qui a été ajoutée dans le chapitre précédent. Confirmer l’opération de suppression en cliquant sur le **Oui** bouton.
1. Accédez à la *mywebapp < unique_number >* App Service. En guise de rappel, zone de recherche du portail peut être utilisé pour localiser rapidement le Service d’application.
1. Cliquez sur **Deployment Center**. Un nouveau panneau s’affiche. Cliquez sur **déconnexion** pour supprimer la configuration de contrôle d’une source de Git locale qui a été ajoutée dans le chapitre précédent. Confirmer l’opération de suppression en cliquant sur le **Oui** bouton.

## <a name="create-an-azure-devops-organization"></a>Création d’une organisation Azure DevOps

1. Ouvrez un navigateur et accédez à la [page de création Azure DevOps organisation](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Tapez un nom unique dans le **choisir un nom facile à mémoriser** zone de texte pour former l’URL pour accéder à votre organisation d’Azure DevOps.
1. Sélectionnez le **Git** bouton radio, étant donné que le code est hébergé dans un référentiel GitHub.
1. Cliquez sur le bouton **Continuer**. Après un court délai d’attente, un compte et un projet d’équipe, nommé *MyFirstProject*, sont créés.

    ![Page de création d’organisation DevOps Azure](media/cicd/vsts-account-creation.png)

1. Ouvrez l’e-mail de confirmation indiquant que le projet et l’organisation d’Azure DevOps sont prêtes pour une utilisation. Cliquez sur le **démarrez votre projet** bouton :

    ![Démarrez votre bouton de projet](media/cicd/vsts-start-project.png)

1. Un navigateur s’ouvre à  *\<account_name\>. visualstudio.com*. Cliquez sur le *MyFirstProject* lien pour commencer à configurer le pipeline de DevOps du projet.

## <a name="configure-the-azure-pipelines-pipeline"></a>Configurer le pipeline de Pipelines d’Azure

Il existe trois étapes distinctes pour terminer. Les étapes dans les résultats de trois sections suivantes dans un pipeline DevOps opérationnels.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>Accès Grant Azure DevOps dans le référentiel GitHub

1. Développez le **ou générer le code à partir d’un dépôt externe** accordion. Cliquez sur le **générer le programme d’installation** bouton :

    ![Bouton de générer le programme d’installation](media/cicd/vsts-setup-build.png)

1. Sélectionnez le **GitHub** option à partir de la **sélectionner une source de** section :

    ![Sélectionnez une source - GitHub](media/cicd/vsts-select-source.png)

1. L’autorisation est requise que Azure DevOps puisse accéder à votre référentiel GitHub. Entrez *< GitHub_username > connexion GitHub* dans le **nom de la connexion** zone de texte. Exemple :

    ![Nom de la connexion GitHub](media/cicd/vsts-repo-authz.png)

1. Si l’authentification à deux facteurs est activée sur votre compte GitHub, un jeton d’accès personnel est requis. Dans ce cas, cliquez sur le **Authorize avec un jeton d’accès personnel GitHub** lien. Consultez le [instructions de création d’un jeton accès personnel GitHub officielles](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) de l’aide. Uniquement les *référentiel* étendue des autorisations est nécessaire. Sinon, cliquez sur le **autoriser l’utilisation d’OAuth** bouton.
1. Lorsque vous y êtes invité, connectez-vous à votre compte GitHub. Ensuite, sélectionnez Autoriser pour accorder l’accès à votre organisation d’Azure DevOps. Si l’opération réussit, un nouveau point de terminaison de service est créé.
1. Cliquez sur le bouton de sélection en regard du **référentiel** bouton. Sélectionnez le *< GitHub_username > / lecture de flux simple* référentiel à partir de la liste. Cliquez sur le **sélectionnez** bouton.
1. Sélectionnez le *master* créer une branche à partir de la **branche par défaut pour les builds manuelles et planifiées** liste déroulante. Cliquez sur le bouton **Continuer**. La page Sélection du modèle s’affiche.

### <a name="create-the-build-definition"></a>Créer la définition de build

1. À partir de la page Sélection du modèle, entrez *ASP.NET Core* dans la zone de recherche :

    ![Recherche d’ASP.NET Core sur la page de modèle](media/cicd/vsts-template-selection.png)

1. Les résultats de recherche de modèle s’affichent. Placez le curseur sur le **ASP.NET Core** modèle, puis cliquez sur le **appliquer** bouton.
1. Le **tâches** onglet de la définition de build s’affiche. Cliquez sur l'onglet **Déclencheurs** .
1. Vérifier le **activer l’intégration continue** boîte. Sous le **filtres de branche** section, vérifiez que le **Type** liste déroulante est définie sur *Include*. Définir le **spécification de branche** liste déroulante pour *master*.

    ![Activer les paramètres d’intégration continue](media/cicd/vsts-enable-ci.png)

    Ces paramètres entraînent une build à déclencher lorsqu’une modification est envoyée vers le *master* branche du référentiel GitHub. Intégration continue est testée dans le [valider des modifications à GitHub et déployer automatiquement vers Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.

1. Cliquez sur le **Enregistrer & file d’attente** bouton, puis sélectionnez le **enregistrer** option :

    ![Bouton Enregistrer](media/cicd/vsts-save-build.png)

1. La boîte de dialogue modale s’affiche :

    ![Enregistrer de définition de build - boîte de dialogue modale](media/cicd/vsts-save-modal.png)

    Utiliser le dossier par défaut de *\\*, puis cliquez sur le **enregistrer** bouton.

### <a name="create-the-release-pipeline"></a>Créer le pipeline de mise en production

1. Cliquez sur le **versions** onglet de votre projet d’équipe. Cliquez sur le **nouveau pipeline** bouton.

    ![Onglet versions - nouveau bouton de définition](media/cicd/vsts-new-release-definition.png)

    Le volet de sélection de modèle s’affiche.

1. À partir de la page Sélection du modèle, entrez *App Service* dans la zone de recherche :

    ![Zone de recherche de modèle de pipeline mise en production](media/cicd/vsts-release-template-search.png)

1. Les résultats de recherche de modèle s’affichent. Placez le curseur sur le **déploiement d’Azure App Service avec l’emplacement** modèle, puis cliquez sur le **appliquer** bouton. Le **Pipeline** onglet du pipeline de versions s’affiche.

    ![Pipeline de versions onglet Pipeline](media/cicd/vsts-release-definition-pipeline.png)

1. Cliquez sur le **ajouter** situé dans le **artefacts** boîte. Le **ajouter un artefact** panneau s’affiche :

    ![Pipeline de versions - ajouter panneau d’artefact](media/cicd/vsts-release-add-artifact.png)

1. Sélectionnez le **Build** vignette à partir de la **type de Source** section. Ce type permet de lier le pipeline de mise en production pour la définition de build.
1. Sélectionnez *MyFirstProject* à partir de la **projet** liste déroulante.
1. Sélectionnez le nom de définition de build, *MyFirstProject-ASP.NET Core-CI*, à partir de la **Source (définition de Build)** liste déroulante.
1. Sélectionnez *dernière* à partir de la **version par défaut** liste déroulante. Cette option génère les artefacts générés par la dernière exécution de la définition de build.
1. Remplacez le texte dans le **alias Source** textbox avec *Drop*.
1. Cliquez sur le bouton **Ajouter**. Le **artefacts** section mises à jour pour afficher les modifications.
1. Cliquez sur l’icône d’éclair pour activer les déploiements continus :

    ![Pipeline de versions artefacts - icône d’éclair](media/cicd/vsts-artifacts-lightning-bolt.png)

    Avec cette option est activée, un déploiement se produit chaque fois qu’une nouvelle build est disponible.
1. Un **déclencheur de déploiement continu** panneau s’affiche à droite. Cliquez sur le bouton bascule pour activer la fonctionnalité. Il n’est pas nécessaire d’activer la **déclencheur de requête de tirage**.
1. Cliquez sur le **ajouter** liste déroulante dans le **générer des filtres de branche** section. Choisissez le **branche par défaut de la définition de Build** option. Ce filtre provoque la mise en production déclencher uniquement pour une build à partir du référentiel GitHub *master* branche.
1. Cliquez sur le bouton **Enregistrer**. Cliquez sur le **OK** bouton dans résultant **enregistrer** boîte de dialogue modale.
1. Cliquez sur le **Environment 1** boîte. Un **environnement** panneau s’affiche à droite. Modifier le *Environment 1* texte dans le **nom de l’environnement** zone de texte pour *Production*.

   ![Pipeline de versions - zone de texte Nom environnement](media/cicd/vsts-environment-name-textbox.png)

1. Cliquez sur le **1 phase, 2 tâches** lien dans le **Production** zone :

    ![Pipeline de versions - link.png d’environnement de Production](media/cicd/vsts-production-link.png)

    Le **tâches** onglet de l’environnement s’affiche.
1. Cliquez sur le **déployer Azure App Service à l’emplacement** tâche. Ses paramètres s’affichent dans le panneau vers la droite.
1. Sélectionnez l’abonnement Azure associé au Service d’application à partir de la **abonnement Azure** liste déroulante. Une fois sélectionné, cliquez sur le **Authorize** bouton.
1. Sélectionnez *application Web* à partir de la **type d’application** liste déroulante.
1. Sélectionnez *mywebapp / < unique_number / >* à partir de la **Nom App service** liste déroulante.
1. Sélectionnez *AzureTutorial* à partir de la **groupe de ressources** liste déroulante.
1. Sélectionnez *intermédiaire* à partir de la **emplacement** liste déroulante.
1. Cliquez sur le bouton **Enregistrer**.
1. Pointez sur le nom de pipeline de version par défaut. Cliquez sur l’icône de crayon pour le modifier. Utilisez *MyFirstProject-ASP.NET Core-CD* comme nom.

    ![Nom de pipeline de mise en production](media/cicd/vsts-release-definition-name.png)

1. Cliquez sur le bouton **Enregistrer**.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Valider les modifications à GitHub et déployer automatiquement vers Azure

1. Ouvrez *SimpleFeedReader.sln* dans Visual Studio.
1. Dans l’Explorateur de solutions, ouvrez *Pages\Index.cshtml*. Modification `<h2>Simple Feed Reader - V3</h2>` à `<h2>Simple Feed Reader - V4</h2>`.
1. Appuyez sur **Ctrl**+**MAJ**+**B** pour générer l’application.
1. Valider le fichier vers le dépôt GitHub. Utilisez le **modifications** page dans Visual Studio *Team Explorer* onglet, ou exécutez la commande suivante à l’aide du shell de commande de l’ordinateur local :

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. Transmettre la modification le *master* créer une branche vers le *origine* à distance de votre référentiel GitHub :

    ```console
    git push origin master
    ```

    La validation s’affiche dans le référentiel GitHub *master* branche :

    ![Validation de GitHub dans la branche principale](media/cicd/github-commit.png)

    La build est déclenchée, étant donné que l’intégration continue est activée dans la définition de build **déclencheurs** onglet :

    ![Activer l’intégration continue](media/cicd/enable-ci.png)

1. Accédez à la **en file d’attente** onglet de la **Azure Pipelines** > **génère** page dans les Services Azure DevOps. La build en file d’attente affiche la branche et la validation qui a déclenché la génération :

    ![build en file d’attente](media/cicd/build-queued.png)

1. Une fois la génération terminée, un déploiement vers Azure se produit. Accédez à l’application dans le navigateur. Notez que le texte « V4 » s’affiche dans l’en-tête :

    ![application mise à jour](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Examiner le pipeline de Pipelines d’Azure

### <a name="build-definition"></a>Définition de build

Une définition de build a été créée avec le nom *MyFirstProject-ASP.NET Core-CI*. Une fois terminée, la build génère un *.zip* fichier, y compris les ressources à publier. Le pipeline de mise en production déploie ces ressources sur Azure.

La définition de build **tâches** onglet répertorie les étapes individuelles utilisées. Il existe cinq tâches de génération.

![tâches de définition de build](media/cicd/build-definition-tasks.png)

1. **Restaurer** &mdash; exécute le `dotnet restore` commande pour restaurer les packages NuGet de l’application. Le package par défaut utilisé un flux est nuget.org.
1. **Build** &mdash; exécute le `dotnet build --configuration release` commande pour compiler le code de l’application. Cela `--configuration` option est utilisée pour produire une version optimisée du code, qui est appropriée pour le déploiement dans un environnement de production. Modifier le *BuildConfiguration* variable sur la définition de build **Variables** onglet si, par exemple, une configuration debug est nécessaire.
1. **Test** &mdash; exécute le `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` commande pour exécuter des tests unitaires de l’application. Tests unitaires sont exécutés dans n’importe quel langage c# projet correspondant à la `**/*Tests/*.csproj` modèle glob. Résultats des tests sont enregistrés dans un *.trx* fichier à l’emplacement spécifié par le `--results-directory` option. Si tous les tests échouent, la build échoue et n’est pas déployée.

    > [!NOTE]
    > Pour vérifier le travail de tests unitaires, modifiez *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* délibérément rompre un des tests. Par exemple, remplacez `Assert.True(result.Count > 0);` à `Assert.False(result.Count > 0);` dans le `Returns_News_Stories_Given_Valid_Uri` (méthode). Validez et envoyez la modification à GitHub. La build est déclenchée et échoue. L’état de pipeline de build devient **échec**. Rétablir la modification, valider et l’envoyer à nouveau. La build réussit.

1. **Publier** &mdash; exécute le `dotnet publish --configuration release --output <local_path_on_build_agent>` commande pour produire un *.zip* fichier avec les artefacts à déployer. Le `--output` option spécifie l’emplacement de publication de la *.zip* fichier. Que l’emplacement est spécifié en passant une [variable prédéfinie](/azure/devops/pipelines/build/variables) nommé `$(build.artifactstagingdirectory)`. Cette variable est passée à un chemin d’accès local, tel que *c:\agent\_work\1\a*, sur l’agent de build.
1. **Publier l’artefact** &mdash; Publishes le *.zip* fichier produit par le **publier** tâche. La tâche accepte le *.zip* emplacement_fichier en tant que paramètre, qui est la variable prédéfinie `$(build.artifactstagingdirectory)`. Le *.zip* fichier est publié comme un dossier nommé *drop*.

Cliquez sur la définition de build **Résumé** lien pour afficher un historique des builds avec la définition de :

![Historique de capture d’écran montrant build définition](media/cicd/build-definition-summary.png)

Dans la page résultante, cliquez sur le lien correspondant au numéro de build unique :

![Page de résumé de définition de capture d’écran montrant build](media/cicd/build-definition-completed.png)

Un résumé de cette build spécifique s’affiche. Cliquez sur le **artefacts** onglet et notez le *drop* dossier produit par la build est répertorié :

![Capture d’écran montrant les artefacts de définition de build - dossier de dépôt](media/cicd/build-definition-artifacts.png)

Utilisez le **télécharger** et **Explorer** des liens pour inspecter les artefacts publiés.

### <a name="release-pipeline"></a>Pipeline de versions

Un pipeline de mise en production a été créé avec le nom *MyFirstProject-ASP.NET Core-CD*:

![Vue d’ensemble du pipeline capture d’écran montrant mise en production](media/cicd/release-definition-overview.png)

Les deux principaux composants du pipeline de versions sont les **artefacts** et **environnements**. Clic sur la zone dans le **artefacts** section, vous affichez le panneau de configuration suivante :

![Artefacts de capture d’écran montrant version pipeline](media/cicd/release-definition-artifacts.png)

Le **Source (définition de Build)** valeur représente la définition de build à laquelle est lié ce pipeline de mise en production. Le *.zip* fichier produit par une exécution réussie de la définition de build est fourni pour le *Production* environnement pour le déploiement vers Azure. Cliquez sur le *1 phase, 2 tâches* lien dans le *Production* boîte d’environnement pour afficher les tâches de pipeline de mise en production :

![Capture d’écran affichant les tâches pipeline mise en production](media/cicd/release-definition-tasks.png)

Le pipeline de mise en production est composé de deux tâches : *Déployer Azure App Service à l’emplacement* et *gérer Azure App Service - Slot Swap*. En cliquant sur la première tâche, vous affichez la configuration de la tâche suivante :

![Tâche de déploiement du pipeline de versions de capture d’écran montrant](media/cicd/release-definition-task1.png)

Abonnement Azure, type de service, nom de l’application web, groupe de ressources et emplacement de déploiement sont définies dans la tâche de déploiement. Le **Package ou dossier** conserve de la zone de texte le *.zip* chemin d’accès du fichier à extraire et déployée sur le *intermédiaire* emplacement de la *mywebapp\<unique _numéro\>*  application web.

En cliquant sur la tâche de permutation d’emplacement révèle la configuration de la tâche suivante :

![Tâche de capture d’écran montrant version pipeline slot swap](media/cicd/release-definition-task2.png)

L’abonnement, groupe de ressources, type de service, nom de l’application web et les détails d’emplacement de déploiement sont fournis. Le **échanger avec Production** case à cocher est activée. Par conséquent, les bits déploiement sur le *intermédiaire* emplacement sont échangé avec l’environnement de production.

## <a name="additional-reading"></a>Lecture supplémentaire

* [Créer son premier pipeline avec Azure Pipelines](/azure/devops/pipelines/get-started-yaml)
* [Projet de build et de .NET Core](/azure/devops/pipelines/languages/dotnet-core)
* [Déployer une application web avec les Pipelines d’Azure](/azure/devops/pipelines/targets/webapp)
