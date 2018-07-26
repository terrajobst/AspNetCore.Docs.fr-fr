---
title: Visual Studio Tools pour Docker avec ASP.NET Core
author: spboyer
description: Découvrez comment utiliser les outils Visual Studio 2017 et Docker pour Windows pour mettre une application ASP.NET Core dans un conteneur.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146882"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools pour Docker avec ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) prend en charge la création, le débogage et l’exécution d’applications ASP.NET Core en conteneur ciblant .NET Core. Les conteneurs Windows et Linux sont pris en charge.

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017](https://www.visualstudio.com/) avec la charge de travail **Développement multiplateforme .NET Core**
* [Docker pour Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Installation et configuration

Pour l’installation de Docker, passez en revue les informations contenues dans [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) et installez [Docker pour Windows](https://docs.docker.com/docker-for-windows/install/).

Il est nécessaire de configurer les **[lecteurs partagés](https://docs.docker.com/docker-for-windows/#shared-drives)** dans Docker pour Windows pour prendre en charge le mappage de volume et le débogage. Cliquez avec le bouton droit sur l’icône Docker de la zone de notification, sélectionnez **Paramètres...** et sélectionnez **Shared Drives** (Lecteurs partagés). Sélectionnez le lecteur où Docker stocke les fichiers. Sélectionnez **Appliquer**.

![Shared Drives](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 versions 15.6 et ultérieures sollicitent l’utilisateur quand les **lecteurs partagés** ne sont pas configurés.

## <a name="add-docker-support-to-an-app"></a>Ajouter la prise en charge de Docker à une application

Pour qu’un projet ASP.NET Core prenne en charge Docker, il doit cibler .NET Core. Les conteneurs Linux et Windows sont pris en charge.

Quand vous ajoutez la prise en charge de Docker à un projet, choisissez un conteneur Windows ou Linux. L’hôte Docker doit exécuter le même type de conteneur. Pour changer le type de conteneur dans l’instance de Docker en cours d’exécution, cliquez avec le bouton droit sur l’icône Docker de la zone de notification, puis choisissez **Basculer vers les conteneurs Windows...** ou **Basculer vers les conteneurs Linux...**.

### <a name="new-app"></a>Nouvelle application

Quand vous créez une application avec les modèles de projet **Application web ASP.NET Core**, cochez la case **Activer la prise en charge de Docker** :

![Case Activer la prise en charge de Docker](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

Si la version cible du .NET Framework est .NET Core, la liste déroulante **OS** (SE) permet la sélection d’un type de conteneur.

### <a name="existing-app"></a>Application existante

Visual Studio Tools pour Docker ne prend pas en charge l’ajout de Docker à un projet ASP.NET Core existant ciblant le .NET Framework. Pour les projets ASP.NET Core ciblant .NET Core, il existe deux options pour ajouter la prise en charge de Docker via les outils. Ouvrez le projet dans Visual Studio et choisissez l’une des options suivantes :

* Sélectionnez **Prise en charge de Docker** dans le menu **Projet**.
* Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez **Ajouter** > **Prise en charge de Docker**.

## <a name="docker-assets-overview"></a>Vue d’ensemble des ressources de Docker

Visual Studio Tools pour Docker ajoute un projet *docker-compose* à la solution, qui contient les fichiers suivants :

* *.dockerignore* : contient une liste de modèles de fichiers et de répertoires à exclure lors de la génération d’un contexte de build.
* *docker-compose.yml* : fichier [Docker Compose](https://docs.docker.com/compose/overview/) de base utilisé pour définir la collection d’images à générer et à exécuter avec `docker-compose build` et `docker-compose run`, respectivement.
* *docker-compose.override.yml* : fichier facultatif, lu par Docker Compose, contenant les remplacements de configuration pour les services. Visual Studio exécute `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` pour fusionner ces fichiers.

Un fichier *Dockerfile*, la recette de la création d’une image Docker finale, est ajouté à la racine du projet. Reportez-vous à [Informations de référence sur Dockerfile](https://docs.docker.com/engine/reference/builder/) pour comprendre les commandes qu’il contient. Ce fichier *Dockerfile* particulier utilise une [build en plusieurs étapes](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) contenant quatre étapes de build nommées distinctes :

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

Le fichier *Dockerfile* est basé sur l’image [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore). Cette image de base inclut les packages NuGet ASP.NET Core, qui ont subi un traitement pré-JIT pour améliorer les performances au démarrage.

Le fichier *docker-compose.yml* contient le nom de l’image qui est créée au moment de l’exécution du projet :

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

Dans l’exemple précédent, `image: hellodockertools` génère l’image `hellodockertools:dev` quand l’application est exécutée en mode **débogage**. L’image `hellodockertools:latest` est générée quand l’application est exécutée en mode **mise en production**.

Ajoutez comme préfixe au nom de l’image le nom d’utilisateur [Docker Hub](https://hub.docker.com/) (par exemple, `dockerhubusername/hellodockertools`) si l’image est destinée à être envoyée (push) au Registre. Vous pouvez aussi changer le nom de l’image pour inclure l’URL de Registre privé (par exemple, `privateregistry.domain.com/hellodockertools`) en fonction de la configuration.

## <a name="debug"></a>Débogage

Sélectionnez **Docker** dans la liste déroulante de débogage dans la barre d’outils et démarrez le débogage de l’application. La vue **Docker** de la fenêtre **Sortie** affiche les actions suivantes qui se déroulent :

* L’image d’exécution *microsoft/aspnetcore* est acquise (si elle n’est pas déjà dans le cache).
* L’image de compilation/publication *microsoft/aspnetcore-build* est acquise (si elle n’est pas déjà dans le cache).
* La variable d’environnement *ASPNETCORE_ENVIRONMENT* a la valeur `Development` dans le conteneur.
* Le port 80 est exposé et mappé à un port attribué dynamiquement pour localhost. Le port est déterminé par l’hôte Docker et peut être interrogé avec la commande `docker ps`.
* L’application est copiée dans le conteneur.
* Le navigateur par défaut est lancé avec le débogueur attaché au conteneur, en utilisant le port attribué dynamiquement.

L’image Docker obtenue est l’image de *développement* de l’application avec les images *microsoft/aspnetcore* comme image de base. Exécutez la commande `docker images` dans la fenêtre **Console du Gestionnaire de package**. Les images sur la machine s’affichent :

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> L’image de développement n’inclut pas le contenu de l’application, car les configurations de **débogage** utilisent le montage de volume pour fournir l’expérience itérative. Pour placer une image, utilisez la configuration **release**.

Exécutez la commande `docker ps` dans la console du Gestionnaire de package. Notez que l’application s’exécute à l’aide du conteneur :

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Modifier & Continuer

Les modifications apportées à des fichiers statiques et à des vues Razor sont automatiquement mises à jour sans qu’une étape de compilation ne soit nécessaire. Apportez la modification, enregistrez et actualisez le navigateur pour afficher la mise à jour.

Les modifications de fichiers de code nécessitent une compilation et un redémarrage de Kestrel au sein du conteneur. Après avoir effectué la modification, utilisez `CTRL+F5` pour exécuter le processus et démarrer l’application au sein du conteneur. Le conteneur Docker n’est pas regénéré ni arrêté. Exécutez la commande `docker ps` dans la console du Gestionnaire de package. Notez que le conteneur d’origine est toujours en cours d’exécution comme 10 minutes auparavant :

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publier des images Docker

Une fois terminé le cycle de développement et de débogage de l’application, Visual Studio Tools pour Docker aide à créer l’image de production de l’application. Changez la liste déroulante de configuration en **Release** et générez l’application. Les outils produisent l’image avec la balise *latest*, qui peut être envoyée (push) au Registre privé ou à Docker Hub.

Exécutez la commande `docker images` dans la console du Gestionnaire de package pour afficher la liste des images :

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> La commande `docker images` retourne des images intermédiaires avec des noms de dépôt et des balises identifiées comme *\<none>* (non répertoriées ci-dessus). Ces images sans nom sont produites par la [build en plusieurs étapes](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*. Elles améliorent l’efficacité de la création de l’image finale ; seules les couches nécessaires sont regénérées en cas de modifications. Quand les images intermédiaires ne sont plus nécessaires, supprimez-les à l’aide de la commande [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/).

Vous pourriez vous attendre à ce que l’image de production ou de publication ait une taille inférieure à l’image de *développement*. En raison de l’utilisation du mappage de volume, le débogueur et l’application étaient exécutés à partir de l’ordinateur local et non dans le conteneur. L’image *latest* a empaqueté le code de l’application nécessaire pour l’exécuter sur un ordinateur hôte. Le delta correspond donc à la taille du code de l’application.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Résoudre les problèmes de développement dans Visual Studio 2017 avec Docker](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Visual Studio Tools pour référentiel Docker GitHub](https://github.com/Microsoft/DockerTools)
