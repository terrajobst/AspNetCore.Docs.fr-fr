---
title: Déployer une application dans App Service - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Déployer une application ASP.NET Core sur Azure App Service, la première étape pour DevOps avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: 4424d3d15cbd234357c8265fa276834cb9abf352
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121218"
---
# <a name="deploy-an-app-to-app-service"></a>Déployer une application dans App Service

[Azure App Service](/azure/app-service/) est Azure web plateforme d’hébergement. Déploiement d’une application web sur Azure App Service peut être effectuée manuellement ou par un processus automatisé. Cette section du guide décrit les méthodes de déploiement qui peuvent être déclenchées manuellement ou par script à l’aide de la ligne de commande, ou déclenchement manuellement à l’aide de Visual Studio.

Dans cette section, vous allez effectuer les tâches suivantes :

* Téléchargez et générez l’exemple d’application.
* Créer une application Web Azure App Service à l’aide d’Azure Cloud Shell.
* Déployer l’exemple d’application dans Azure à l’aide de Git.
* Déployer une modification dans l’application à l’aide de Visual Studio.
* Ajouter un emplacement intermédiaire à l’application web.
* Déployer une mise à jour dans l’emplacement intermédiaire.
* Échanger les emplacements intermédiaires et de production.

## <a name="download-and-test-the-app"></a>Télécharger et tester l’application

L’application utilisée dans ce guide est une application ASP.NET Core prédéfinie, [lecteur de flux Simple](https://github.com/Azure-Samples/simple-feed-reader/). Il s’agit d’une application Pages Razor qui utilise le `Microsoft.SyndicationFeed.ReaderWriter` API pour récupérer un flux RSS/Atom et afficher les éléments d’informations dans une liste.

N’hésitez pas à examiner le code, mais il est important de comprendre qu’il n’a rien de spécial à cette application. Il est simplement une application ASP.NET Core simple à titre d’illustration.

À partir d’un interpréteur de commandes, vous pouvez télécharger le code, générez le projet et exécutez-le comme suit.

> *Remarque : Linux/Mac OS les utilisateurs doivent apporter les modifications appropriées pour les chemins d’accès, par exemple, à l’aide de la barre oblique (`/`) au lieu de la barre oblique inverse (`\`).*

1. Clonez le code dans un dossier sur votre ordinateur local.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Modifier votre dossier de travail pour le *lecture de flux simple* dossier a été créé.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Restaurer les packages et générez la solution.

    ```console
    dotnet build
    ```

4. Exécuter l’application.

    ```console
    dotnet run
    ```

    ![La commande dotnet run a réussi](./media/deploying-to-app-service/dotnet-run.png)

5. Ouvrez un navigateur et accédez à `http://localhost:5000`. L’application vous permet de taper ou coller un URL de flux de syndication et afficher une liste d’éléments d’actualités.

     ![L’application Affichage du contenu d’un flux RSS](./media/deploying-to-app-service/app-in-browser.png)

6. Une fois que vous êtes satisfait de l’application fonctionne correctement, l’arrêter en appuyant sur **Ctrl**+**C** dans l’interface de commande.

## <a name="create-the-azure-app-service-web-app"></a>Créer l’application Web Azure App Service

Pour déployer l’application, vous devez créer un Service d’application [application Web](/azure/app-service/app-service-web-overview). Après la création de l’application Web, vous allez déployer à partir de votre ordinateur local à l’aide de Git à celle-ci.

1. Se connecter à la [Azure Cloud Shell](https://shell.azure.com/bash). Remarque : Lorsque vous connectez pour la première fois, Cloud Shell vous invite à créer un compte de stockage pour les fichiers de configuration. Acceptez les valeurs par défaut ou fournissez un nom unique.

2. Utiliser Cloud Shell pour les étapes suivantes.

    a. Déclarez une variable pour stocker le nom de votre application web. Le nom doit être unique à utiliser dans l’URL par défaut. À l’aide de la `$RANDOM` fonction Bash pour construire le nom garantit l’unicité et des résultats dans le format `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Créer un groupe de ressources. Groupes de ressources fournissent un moyen pour agréger les ressources Azure pour être gérés en tant que groupe.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    Le `az` commande appelle le [Azure CLI](/cli/azure/). L’interface CLI peut être exécuté localement, mais l’utiliser dans l’interpréteur de gagner du temps et configuration.

    c. Créer un plan App Service dans le niveau S1. Un plan App Service est un regroupement d’applications web qui partagent le même niveau tarifaire. Le niveau S1 n’est pas gratuit, mais il est requis pour la fonctionnalité d’emplacements intermédiaire.

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. Créer la ressource d’application web à l’aide du plan App Service dans le même groupe de ressources.

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. Définir les informations d’identification de déploiement. Ces informations d’identification de déploiement s’appliquent à toutes les applications web dans votre abonnement. N’utilisez pas les caractères spéciaux dans le nom d’utilisateur.

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. Configurer l’application web pour accepter les déploiements à partir de Git local et l’affichage le *URL de déploiement Git*. **Notez cette URL pour faire référence ultérieurement**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Afficher le *URL d’application web*. Accédez à cette URL pour voir l’application web vide. **Notez cette URL pour faire référence ultérieurement**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. À l’aide d’une invite de commandes sur votre ordinateur local, accédez au dossier de projet de l’application web (par exemple, `.\simple-feed-reader\SimpleFeedReader`). Exécutez les commandes suivantes pour configurer Git pour pousser vers l’URL de déploiement :

    a. Ajoutez l’URL distante dans le référentiel local.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Push local *master* créer une branche vers le *azure-prod* à distance *master* branche.

    ```console
    git push azure-prod master
    ```

    Vous êtes invité pour les informations d’identification de déploiement que vous avez créé précédemment. Observez la sortie dans l’interface de commande. Azure génère l’application ASP.NET Core à distance.

4. Dans un navigateur, accédez à la *URL d’application Web* et notez l’application a été créée et déployée. Des modifications supplémentaires peuvent être validées sur le référentiel Git local avec `git commit`. Ces modifications sont envoyées vers Azure avec l’exemple précédent `git push` commande.

## <a name="deployment-with-visual-studio"></a>Déploiement avec Visual Studio

> *Remarque : Cette section s’applique à Windows uniquement. Les utilisateurs de Linux et macOS doivent apporter la modification décrite à l’étape 2 ci-dessous. Enregistrez le fichier et valider la modification dans le référentiel local avec `git commit`. Enfin, transmettre la modification avec `git push`, comme illustré dans la première section.*

L’application a déjà été déployée à partir de l’interface de commande. Nous allons utiliser les outils intégrés de Visual Studio pour déployer une mise à jour à l’application. Dans les coulisses, Visual Studio effectue la même opération en tant que l’outils en ligne de commande, mais au sein de l’interface familière de Visual Studio.

1. Ouvrez *SimpleFeedReader.sln* dans Visual Studio.
2. Dans l’Explorateur de solutions, ouvrez *Pages\Index.cshtml*. Modification `<h2>Simple Feed Reader</h2>` à `<h2>Simple Feed Reader - V2</h2>`.
3. Appuyez sur **Ctrl**+**MAJ**+**B** pour générer l’application.
4. Dans l’Explorateur de solutions, avec le bouton droit sur le projet, puis cliquez sur **publier**.

    ![Capture d’écran montrant le clic droit, publier](./media/deploying-to-app-service/publish.png)
5. Visual Studio peut créer une ressource App Service, mais cette mise à jour sera publiée sur le déploiement existant. Dans le **choisir une cible de publication** boîte de dialogue, sélectionnez **App Service** dans la liste sur la gauche, puis sélectionnez **sélectionner**. Cliquez sur **Publier**.
6. Dans le **App Service** boîte de dialogue, vérifiez que Microsoft ou compte professionnel utilisé pour créer votre abonnement Azure est affiché dans le coin supérieur droit. Si elle n’est pas le cas, cliquez sur la liste déroulante et ajoutez-le.
7. Vérifiez que le Azure correct **abonnement** est sélectionné. Pour **vue**, sélectionnez **groupe de ressources**. Développez le **AzureTutorial** groupe de ressources et sélectionnez l’application web existante. Cliquez sur **OK**.

    ![Boîte de dialogue Publier le Service application capture d’écran](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio génère et déploie l’application sur Azure. Accédez à l’URL de l’application web. Vérifier que le `<h2>` modification de l’élément est en ligne.

![L’application avec le titre a été modifié](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Emplacements de déploiement

Emplacements de déploiement prend en charge la mise en lots de modifications sans affecter l’application en cours d’exécution en production. Une fois que la version intermédiaire de l’application est validée par une équipe d’assurance qualité, la production et emplacements intermédiaires peuvent être échangés. L’application dans un environnement intermédiaire est promue en production de cette manière. Les étapes suivantes créer un emplacement intermédiaire, déploiement des modifications à ce dernier et échanger l’emplacement intermédiaire avec production après vérification.

1. Se connecter à la [Azure Cloud Shell](https://shell.azure.com/bash), si pas déjà connecté.
2. Créez l’emplacement intermédiaire.

    a. Créer un emplacement de déploiement avec le nom *intermédiaire*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Configurer l’emplacement intermédiaire pour utiliser un déploiement à partir de Git local et get le **intermédiaire** URL de déploiement. **Notez cette URL pour faire référence ultérieurement**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Afficher l’URL de l’emplacement intermédiaire. Accédez à l’URL pour afficher l’emplacement intermédiaire vide. **Notez cette URL pour faire référence ultérieurement**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. Dans un éditeur de texte ou de Visual Studio, modifiez *pages/index.cshtml* à nouveau afin que le `<h2>` élément lit `<h2>Simple Feed Reader - V3</h2>` et enregistrez le fichier.

4. Valider le fichier dans le référentiel Git local, à l’aide du **modifications** page dans Visual Studio *Team Explorer* onglet, ou en entrant ce qui suit à l’aide de shell de commande de l’ordinateur local :

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. À l’aide du shell de commande de l’ordinateur local, ajoutez l’URL de déploiement intermédiaire en tant que Git distant et push les modifications validées :

    a. Ajoutez l’URL distante pour l’environnement intermédiaire dans le référentiel Git local.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Push local *master* créer une branche vers le *intermédiaire d’azure* à distance *master* branche.

    ```console
    git push azure-staging master
    ```

    Patientez pendant que Azure génère et déploie l’application.

6. Pour vérifier que V3 a été déployé sur l’emplacement intermédiaire, ouvrez deux fenêtres de navigateur. Dans une fenêtre, accédez à l’URL d’origine de l’application web. Dans l’autre fenêtre, accédez à l’URL d’application web intermédiaire. L’URL de production sert V2 de l’application. L’URL intermédiaire sert V3 de l’application.

    ![Capture d’écran comparant les fenêtres du navigateur](./media/deploying-to-app-service/ready-to-swap.png)

7. Dans l’interpréteur de commandes, remplacez l’emplacement intermédiaire vérifiée/chaud, les en production.

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. Vérifiez que l’échange s’est produite en actualisant les fenêtres de deux navigateur.

    ![Comparaison des fenêtres de navigateur après la permutation](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>Récapitulatif

Dans cette section, les tâches suivantes ont été effectuées :

* Téléchargé et généré l’exemple d’application.
* Créé une application Web Azure App Service à l’aide d’Azure Cloud Shell.
* Déployé l’exemple d’application sur Azure à l’aide de Git.
* Déployé une modification à l’application à l’aide de Visual Studio.
* Ajouter un emplacement intermédiaire à l’application web.
* Déployé une mise à jour dans l’emplacement intermédiaire.
* Échanger les emplacements intermédiaires et de production.

Dans la section suivante, vous allez apprendre à créer un pipeline DevOps avec les Pipelines d’Azure.

## <a name="additional-reading"></a>Lecture supplémentaire

* [Vue d’ensemble des applications Web](/azure/app-service/app-service-web-overview)
* [Créer une application web .NET Core et SQL Database dans Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Configurer les informations d’identification de déploiement pour Azure App Service](/azure/app-service/app-service-deployment-credentials)
* [Configurer des environnements intermédiaires dans Azure App Service](/azure/app-service/web-sites-staged-publishing)
