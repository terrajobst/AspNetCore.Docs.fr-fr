---
title: Visual Studio Tools pour Docker avec ASP.NET Core
author: spboyer
description: "Découvrez comment utiliser les outils Visual Studio 2017 et Docker pour Windows pour mettre une application ASP.NET Core dans un conteneur."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="424bc-103">Visual Studio Tools pour Docker avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="424bc-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="424bc-104">[Visual Studio 2017](https://www.visualstudio.com/) prend en charge la création, le débogage et exécute des applications ciblant le .NET Core en conteneur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="424bc-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="424bc-105">Les conteneurs Windows et Linux sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="424bc-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="424bc-106">Prérequis</span><span class="sxs-lookup"><span data-stu-id="424bc-106">Prerequisites</span></span>

* <span data-ttu-id="424bc-107">[Visual Studio 2017](https://www.visualstudio.com/) avec la charge de travail **Développement multiplateforme .NET Core**</span><span class="sxs-lookup"><span data-stu-id="424bc-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="424bc-108">Docker pour Windows</span><span class="sxs-lookup"><span data-stu-id="424bc-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="424bc-109">Installation et configuration</span><span class="sxs-lookup"><span data-stu-id="424bc-109">Installation and setup</span></span>

<span data-ttu-id="424bc-110">Pour l’installation de Docker, passez en revue les informations contenues dans [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) et installez [Docker pour Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="424bc-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="424bc-111">Il est nécessaire de configurer les **[lecteurs partagés](https://docs.docker.com/docker-for-windows/#shared-drives)** dans Docker pour Windows pour prendre en charge le mappage de volume et le débogage.</span><span class="sxs-lookup"><span data-stu-id="424bc-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="424bc-112">Cliquez sur l’icône de Docker, sélectionnez **paramètres...** , puis sélectionnez **des lecteurs partagés**.</span><span class="sxs-lookup"><span data-stu-id="424bc-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="424bc-113">Sélectionnez le lecteur où Docker stocke les fichiers.</span><span class="sxs-lookup"><span data-stu-id="424bc-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="424bc-114">Sélectionnez **appliquer**.</span><span class="sxs-lookup"><span data-stu-id="424bc-114">Select **Apply**.</span></span>

![Shared Drives](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="424bc-116">Demander à Visual Studio 2017 15,6 et versions ultérieures lorsque **des lecteurs partagés** ne sont pas configurés.</span><span class="sxs-lookup"><span data-stu-id="424bc-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="424bc-117">Ajouter la prise en charge de Docker à une application</span><span class="sxs-lookup"><span data-stu-id="424bc-117">Add Docker support to an app</span></span>

<span data-ttu-id="424bc-118">Pour ajouter la prise en charge Docker à un projet ASP.NET Core, le projet doit cibler .NET Core.</span><span class="sxs-lookup"><span data-stu-id="424bc-118">In order to add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="424bc-119">Les conteneurs Linux et Windows sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="424bc-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="424bc-120">Lorsque vous ajoutez la prise en charge Docker à un projet, choisissez Windows ou un conteneur de Linux.</span><span class="sxs-lookup"><span data-stu-id="424bc-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="424bc-121">L’hôte Docker doit exécuter le même type de conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="424bc-122">Pour changer le type de conteneur dans l’instance de Docker en cours d’exécution, cliquez avec le bouton droit sur l’icône Docker de la zone de notification, puis choisissez **Basculer vers les conteneurs Windows...** ou **Basculer vers les conteneurs Linux...**.</span><span class="sxs-lookup"><span data-stu-id="424bc-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="424bc-123">Nouvelle application</span><span class="sxs-lookup"><span data-stu-id="424bc-123">New app</span></span>

<span data-ttu-id="424bc-124">Quand vous créez une application avec les modèles de projet **Application web ASP.NET Core**, cochez la case **Activer la prise en charge de Docker** :</span><span class="sxs-lookup"><span data-stu-id="424bc-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Case Activer la prise en charge de Docker](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="424bc-126">Si le framework cible est .NET Core, la **système d’exploitation** déroulante permet la sélection d’un type de conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="424bc-127">Application existante</span><span class="sxs-lookup"><span data-stu-id="424bc-127">Existing app</span></span>

<span data-ttu-id="424bc-128">Visual Studio Tools pour Docker ne prend pas en charge l’ajout de Docker à un projet ASP.NET Core existant ciblant le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="424bc-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="424bc-129">Pour les projets ASP.NET Core ciblant .NET Core, il existe deux options pour ajouter la prise en charge de Docker via les outils.</span><span class="sxs-lookup"><span data-stu-id="424bc-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="424bc-130">Ouvrez le projet dans Visual Studio et choisissez l’une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="424bc-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="424bc-131">Sélectionnez **Prise en charge de Docker** dans le menu **Projet**.</span><span class="sxs-lookup"><span data-stu-id="424bc-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="424bc-132">Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez **Ajouter** > **Prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="424bc-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="424bc-133">Vue d’ensemble des ressources de Docker</span><span class="sxs-lookup"><span data-stu-id="424bc-133">Docker assets overview</span></span>

<span data-ttu-id="424bc-134">Visual Studio Tools pour Docker ajoute un projet *docker-compose* à la solution, qui contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="424bc-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="424bc-135">*.dockerignore* : contient une liste de modèles de fichiers et de répertoires à exclure lors de la génération d’un contexte de build.</span><span class="sxs-lookup"><span data-stu-id="424bc-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="424bc-136">*docker-compose.yml* : fichier [Docker Compose](https://docs.docker.com/compose/overview/) de base utilisé pour définir la collection d’images à générer et à exécuter avec `docker-compose build` et `docker-compose run`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="424bc-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="424bc-137">*docker-compose.override.yml* : fichier facultatif, lu par Docker Compose, contenant les remplacements de configuration pour les services.</span><span class="sxs-lookup"><span data-stu-id="424bc-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="424bc-138">Visual Studio exécute `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` pour fusionner ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="424bc-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="424bc-139">Un fichier *Dockerfile*, la recette de la création d’une image Docker finale, est ajouté à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="424bc-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="424bc-140">Reportez-vous à [Informations de référence sur Dockerfile](https://docs.docker.com/engine/reference/builder/) pour comprendre les commandes qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="424bc-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="424bc-141">Ce fichier *Dockerfile* particulier utilise une [build en plusieurs étapes](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) contenant quatre étapes de build nommées distinctes :</span><span class="sxs-lookup"><span data-stu-id="424bc-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="424bc-142">Le fichier *Dockerfile* est basé sur l’image [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="424bc-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="424bc-143">Cette image de base inclut les packages NuGet ASP.NET Core, qui ont subi un traitement pré-JIT pour améliorer les performances au démarrage.</span><span class="sxs-lookup"><span data-stu-id="424bc-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="424bc-144">Le *docker-compose.yml* fichier contient le nom de l’image qui est créé lors de l’exécution du projet :</span><span class="sxs-lookup"><span data-stu-id="424bc-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="424bc-145">Dans l’exemple précédent, `image: hellodockertools` génère l’image `hellodockertools:dev` quand l’application est exécutée en mode **débogage**.</span><span class="sxs-lookup"><span data-stu-id="424bc-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="424bc-146">L’image `hellodockertools:latest` est générée quand l’application est exécutée en mode **mise en production**.</span><span class="sxs-lookup"><span data-stu-id="424bc-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="424bc-147">Préfixe du nom de l’image avec le [Hub Docker](https://hub.docker.com/) nom d’utilisateur (par exemple, `dockerhubusername/hellodockertools`) si l’image est poussée vers le Registre.</span><span class="sxs-lookup"><span data-stu-id="424bc-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="424bc-148">Vous pouvez également modifier le nom de l’image à inclure l’URL de registres privés (par exemple, `privateregistry.domain.com/hellodockertools`) selon la configuration.</span><span class="sxs-lookup"><span data-stu-id="424bc-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="424bc-149">Débogage</span><span class="sxs-lookup"><span data-stu-id="424bc-149">Debug</span></span>

<span data-ttu-id="424bc-150">Sélectionnez **Docker** dans la liste déroulante de débogage dans la barre d’outils et démarrez le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="424bc-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="424bc-151">La vue **Docker** de la fenêtre **Sortie** affiche les actions suivantes qui se déroulent :</span><span class="sxs-lookup"><span data-stu-id="424bc-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="424bc-152">Le *microsoft/aspnetcore* acquisition d’image d’exécution (si pas déjà dans le cache).</span><span class="sxs-lookup"><span data-stu-id="424bc-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="424bc-153">Le *aspnetcore/microsoft-build* compilation/publier l’image est acquis (si pas déjà dans le cache).</span><span class="sxs-lookup"><span data-stu-id="424bc-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="424bc-154">La variable d’environnement *ASPNETCORE_ENVIRONMENT* a la valeur `Development` dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="424bc-155">Le port 80 est exposé et mappé à un port affecté dynamiquement pour localhost.</span><span class="sxs-lookup"><span data-stu-id="424bc-155">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="424bc-156">Le port est déterminé par l’hôte Docker et peut être interrogé avec la commande `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="424bc-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="424bc-157">L’application est copiée dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-157">The app is copied to the container.</span></span>
* <span data-ttu-id="424bc-158">Le navigateur par défaut est lancé avec le débogueur attaché au conteneur via le port attribué de manière dynamique.</span><span class="sxs-lookup"><span data-stu-id="424bc-158">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="424bc-159">L’image Docker qui en résulte est la *dev* l’image de l’application avec le *microsoft/aspnetcore* images en tant que l’image de base.</span><span class="sxs-lookup"><span data-stu-id="424bc-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="424bc-160">Exécutez la commande `docker images` dans la fenêtre **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="424bc-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="424bc-161">Les images sur l’ordinateur s’affichent :</span><span class="sxs-lookup"><span data-stu-id="424bc-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="424bc-162">L’image de développement n’a pas le contenu de l’application, en tant que **déboguer** configurations utilisent le montage de volume pour fournir une expérience itérative.</span><span class="sxs-lookup"><span data-stu-id="424bc-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="424bc-163">Pour placer une image, utilisez la configuration **release**.</span><span class="sxs-lookup"><span data-stu-id="424bc-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="424bc-164">Exécutez la commande `docker ps` dans la console du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="424bc-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="424bc-165">Notez que l’application s’exécute à l’aide du conteneur :</span><span class="sxs-lookup"><span data-stu-id="424bc-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="424bc-166">Modifier & Continuer</span><span class="sxs-lookup"><span data-stu-id="424bc-166">Edit and continue</span></span>

<span data-ttu-id="424bc-167">Les modifications apportées à des fichiers statiques et à des vues Razor sont automatiquement mises à jour sans qu’une étape de compilation ne soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="424bc-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="424bc-168">Apportez la modification, enregistrez et actualisez le navigateur pour afficher la mise à jour.</span><span class="sxs-lookup"><span data-stu-id="424bc-168">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="424bc-169">Les modifications apportées aux fichiers de code nécessitent une compilation et un redémarrage de Kestrel au sein du conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-169">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="424bc-170">Après avoir effectué la modification, utilisez Ctrl+F5 pour exécuter le processus et démarrer l’application au sein du conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-170">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="424bc-171">Le conteneur Docker n’est pas reconstruit ou arrêté.</span><span class="sxs-lookup"><span data-stu-id="424bc-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="424bc-172">Exécutez la commande `docker ps` dans la console du Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="424bc-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="424bc-173">Notez que le conteneur d’origine est toujours en cours d’exécution comme 10 minutes auparavant :</span><span class="sxs-lookup"><span data-stu-id="424bc-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="424bc-174">Publier des images Docker</span><span class="sxs-lookup"><span data-stu-id="424bc-174">Publish Docker images</span></span>

<span data-ttu-id="424bc-175">Une fois le cycle de développement et de débogage de l’application terminé, Visual Studio Tools pour Docker vous aider à créer l’image de production de l’application.</span><span class="sxs-lookup"><span data-stu-id="424bc-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="424bc-176">Changez la liste déroulante de configuration en **Release** et générez l’application.</span><span class="sxs-lookup"><span data-stu-id="424bc-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="424bc-177">Les outils de produit de l’image avec le *dernière* balise, qui peut être appliquée sur le Registre privé ou le Hub d’ancrage.</span><span class="sxs-lookup"><span data-stu-id="424bc-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="424bc-178">Exécutez la commande `docker images` dans la console du Gestionnaire de package pour afficher la liste des images :</span><span class="sxs-lookup"><span data-stu-id="424bc-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="424bc-179">La commande `docker images` retourne des images intermédiaires avec des noms de dépôt et des balises identifiées comme *\<none>* (non répertoriées ci-dessus).</span><span class="sxs-lookup"><span data-stu-id="424bc-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="424bc-180">Ces images sans nom sont produites par la [build en plusieurs étapes](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="424bc-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="424bc-181">Elles améliorent l’efficacité de la création de l’image finale ; seules les couches nécessaires sont regénérées en cas de modifications.</span><span class="sxs-lookup"><span data-stu-id="424bc-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="424bc-182">Lorsque les images intermédiaires ne sont plus nécessaires, supprimez-les à l’aide de la [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) commande.</span><span class="sxs-lookup"><span data-stu-id="424bc-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="424bc-183">Vous pourriez vous attendre à ce que l’image de production ou de publication ait une taille inférieure à l’image de *développement*.</span><span class="sxs-lookup"><span data-stu-id="424bc-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="424bc-184">En raison du mappage de volume, le débogueur et l’application en cours d’exécution de l’ordinateur local et non dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="424bc-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="424bc-185">L’image *latest* a empaqueté le code de l’application nécessaire pour l’exécuter sur un ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="424bc-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="424bc-186">Par conséquent, le fichier delta est la taille du code d’application.</span><span class="sxs-lookup"><span data-stu-id="424bc-186">Therefore, the delta is the size of the app code.</span></span>
