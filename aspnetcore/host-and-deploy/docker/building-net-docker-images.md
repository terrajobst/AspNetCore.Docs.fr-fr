---
title: Images Docker pour ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser les images Docker .NET Core publiées à partir du Registre Docker. Extrayez des images et créez les vôtres.
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: 12665fb2e7a9c75747f5c83129a617aea6adfbbf
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317695"
---
# <a name="docker-images-for-aspnet-core"></a><span data-ttu-id="ac49e-104">Images Docker pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac49e-104">Docker images for ASP.NET Core</span></span>

<span data-ttu-id="ac49e-105">Ce tutoriel explique comment exécuter une application ASP.NET Core dans des conteneurs Docker.</span><span class="sxs-lookup"><span data-stu-id="ac49e-105">This tutorial shows how to run an ASP.NET Core app in Docker containers.</span></span>

<span data-ttu-id="ac49e-106">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ac49e-106">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ac49e-107">Découvrir les images Docker Microsoft .NET Core</span><span class="sxs-lookup"><span data-stu-id="ac49e-107">Learn about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="ac49e-108">Télécharger un exemple d’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac49e-108">Download an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="ac49e-109">Exécuter l’exemple d’application localement</span><span class="sxs-lookup"><span data-stu-id="ac49e-109">Run the sample app locally</span></span>
> * <span data-ttu-id="ac49e-110">Exécuter l’exemple d’application dans des conteneurs Linux</span><span class="sxs-lookup"><span data-stu-id="ac49e-110">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="ac49e-111">Exécuter l’exemple d’application dans des conteneurs Windows</span><span class="sxs-lookup"><span data-stu-id="ac49e-111">Run the sample app in Windows containers</span></span>
> * <span data-ttu-id="ac49e-112">Effectuer manuellement le build et le déploiement</span><span class="sxs-lookup"><span data-stu-id="ac49e-112">Build and deploy manually</span></span>

## <a name="aspnet-core-docker-images"></a><span data-ttu-id="ac49e-113">Images Docker ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac49e-113">ASP.NET Core Docker images</span></span>

<span data-ttu-id="ac49e-114">Dans ce tutoriel, vous allez télécharger un exemple d’application ASP.NET Core pour l’exécuter dans des conteneurs Docker.</span><span class="sxs-lookup"><span data-stu-id="ac49e-114">For this tutorial, you download an ASP.NET Core sample app and run it in Docker containers.</span></span> <span data-ttu-id="ac49e-115">L’exemple s’applique aux conteneurs Linux et Windows.</span><span class="sxs-lookup"><span data-stu-id="ac49e-115">The sample works with both Linux and Windows containers.</span></span>

<span data-ttu-id="ac49e-116">L’exemple de fichier Dockerfile utilise la [fonctionnalité de build en plusieurs étapes de Docker](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) pour séparer le build et l’exécution dans différents conteneurs.</span><span class="sxs-lookup"><span data-stu-id="ac49e-116">The sample Dockerfile uses the [Docker multi-stage build feature](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) to build and run in different containers.</span></span> <span data-ttu-id="ac49e-117">Les conteneurs de build et d’exécution sont créés à partir d’images fournies par Microsoft dans Docker Hub :</span><span class="sxs-lookup"><span data-stu-id="ac49e-117">The build and run containers are created from images that are provided in Docker Hub by Microsoft:</span></span>

* `dotnet/core/sdk`

  <span data-ttu-id="ac49e-118">L’exemple utilise cette image pour créer l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-118">The sample uses this image for building the app.</span></span> <span data-ttu-id="ac49e-119">Elle contient le Kit SDK .NET Core, qui inclut les outils en ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="ac49e-119">The image contains the .NET Core SDK, which includes the Command Line Tools (CLI).</span></span> <span data-ttu-id="ac49e-120">Elle est optimisée pour le développement, le débogage et le test unitaire en local.</span><span class="sxs-lookup"><span data-stu-id="ac49e-120">The image is optimized for local development, debugging, and unit testing.</span></span> <span data-ttu-id="ac49e-121">En raison des outils installés pour le développement et la compilation, elle est relativement volumineuse.</span><span class="sxs-lookup"><span data-stu-id="ac49e-121">The tools installed for development and compilation make this a relatively large image.</span></span> 

* `dotnet/core/aspnet` 

   <span data-ttu-id="ac49e-122">L’exemple utilise cette image pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-122">The sample uses this image for running the app.</span></span> <span data-ttu-id="ac49e-123">Elle contient le runtime ASP.NET Core et les bibliothèques. Elle est optimisée pour l’exécution d’applications en production.</span><span class="sxs-lookup"><span data-stu-id="ac49e-123">The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production.</span></span> <span data-ttu-id="ac49e-124">Conçue pour la vitesse de déploiement et de démarrage de l’application, elle est relativement petite afin d’optimiser les performances réseau du Registre Docker vers l’hôte Docker.</span><span class="sxs-lookup"><span data-stu-id="ac49e-124">Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized.</span></span> <span data-ttu-id="ac49e-125">Seuls les binaires et le contenu nécessaires pour exécuter une application sont copiés dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="ac49e-125">Only the binaries and content needed to run an app are copied to the container.</span></span> <span data-ttu-id="ac49e-126">Le contenu est prêt à s’exécuter, ce qui réduit le délai entre `Docker run` et le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-126">The contents are ready to run, enabling the fastest time from `Docker run` to app startup.</span></span> <span data-ttu-id="ac49e-127">La compilation de code dynamique n’est pas nécessaire dans le modèle Docker.</span><span class="sxs-lookup"><span data-stu-id="ac49e-127">Dynamic code compilation isn't needed in the Docker model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac49e-128">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ac49e-128">Prerequisites</span></span>

* [<span data-ttu-id="ac49e-129">SDK .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="ac49e-129">.NET Core SDK 3.0</span></span>](https://dotnet.microsoft.com/download)

* <span data-ttu-id="ac49e-130">Client Docker 18.03 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ac49e-130">Docker client 18.03 or later</span></span>

  * <span data-ttu-id="ac49e-131">Distributions Linux</span><span class="sxs-lookup"><span data-stu-id="ac49e-131">Linux distributions</span></span>
    * [<span data-ttu-id="ac49e-132">CentOS</span><span class="sxs-lookup"><span data-stu-id="ac49e-132">CentOS</span></span>](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [<span data-ttu-id="ac49e-133">Debian</span><span class="sxs-lookup"><span data-stu-id="ac49e-133">Debian</span></span>](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [<span data-ttu-id="ac49e-134">Fedora</span><span class="sxs-lookup"><span data-stu-id="ac49e-134">Fedora</span></span>](https://docs.docker.com/install/linux/docker-ce/fedora/)
    * [<span data-ttu-id="ac49e-135">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="ac49e-135">Ubuntu</span></span>](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [<span data-ttu-id="ac49e-136">macOS</span><span class="sxs-lookup"><span data-stu-id="ac49e-136">macOS</span></span>](https://docs.docker.com/docker-for-mac/install/)
  * [<span data-ttu-id="ac49e-137">Windows</span><span class="sxs-lookup"><span data-stu-id="ac49e-137">Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

* [<span data-ttu-id="ac49e-138">Git</span><span class="sxs-lookup"><span data-stu-id="ac49e-138">Git</span></span>](https://git-scm.com/download)

## <a name="download-the-sample-app"></a><span data-ttu-id="ac49e-139">Télécharger l’exemple d’application</span><span class="sxs-lookup"><span data-stu-id="ac49e-139">Download the sample app</span></span>

* <span data-ttu-id="ac49e-140">Téléchargez l’exemple en clonant le [référentiel Docker .NET Core](https://github.com/dotnet/dotnet-docker) :</span><span class="sxs-lookup"><span data-stu-id="ac49e-140">Download the sample by cloning the [.NET Core Docker repository](https://github.com/dotnet/dotnet-docker):</span></span> 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a><span data-ttu-id="ac49e-141">Exécuter l’application localement</span><span class="sxs-lookup"><span data-stu-id="ac49e-141">Run the app locally</span></span>

* <span data-ttu-id="ac49e-142">Accédez au dossier de projet à l’adresse *dotnet-docker/samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="ac49e-142">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="ac49e-143">Exécutez la commande suivante pour générer et exécuter l’application localement :</span><span class="sxs-lookup"><span data-stu-id="ac49e-143">Run the following command to build and run the app locally:</span></span>

  ```dotnetcli
  dotnet run
  ```

* <span data-ttu-id="ac49e-144">Accédez à `http://localhost:5000` dans un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-144">Go to `http://localhost:5000` in a browser to test the app.</span></span>

* <span data-ttu-id="ac49e-145">Appuyez sur Ctrl+C dans l’invite de commande pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-145">Press Ctrl+C at the command prompt to stop the app.</span></span>

## <a name="run-in-a-linux-container"></a><span data-ttu-id="ac49e-146">Exécuter dans un conteneur Linux</span><span class="sxs-lookup"><span data-stu-id="ac49e-146">Run in a Linux container</span></span>

* <span data-ttu-id="ac49e-147">Dans le client Docker, basculez vers les conteneurs Linux.</span><span class="sxs-lookup"><span data-stu-id="ac49e-147">In the Docker client, switch to Linux containers.</span></span>

* <span data-ttu-id="ac49e-148">Accédez au dossier Dockerfile à l’adresse *dotnet-docker/samples/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="ac49e-148">Navigate to the Dockerfile folder at *dotnet-docker/samples/aspnetapp*.</span></span>

* <span data-ttu-id="ac49e-149">Exécutez les commandes suivantes pour générer et exécuter l’exemple dans Docker :</span><span class="sxs-lookup"><span data-stu-id="ac49e-149">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  <span data-ttu-id="ac49e-150">Voici le rôle des arguments de la commande `build` :</span><span class="sxs-lookup"><span data-stu-id="ac49e-150">The `build` command arguments:</span></span>
  * <span data-ttu-id="ac49e-151">nommer l’image aspnetapp ;</span><span class="sxs-lookup"><span data-stu-id="ac49e-151">Name the image aspnetapp.</span></span>
  * <span data-ttu-id="ac49e-152">rechercher le fichier Dockerfile dans le dossier actif (le point final).</span><span class="sxs-lookup"><span data-stu-id="ac49e-152">Look for the Dockerfile in the current folder (the period at the end).</span></span>

  <span data-ttu-id="ac49e-153">Voici le rôle des arguments de la commande run :</span><span class="sxs-lookup"><span data-stu-id="ac49e-153">The run command arguments:</span></span>
  * <span data-ttu-id="ac49e-154">allouer un pseudoterminal TTY et le laisser ouvert même s’il n’est pas attaché</span><span class="sxs-lookup"><span data-stu-id="ac49e-154">Allocate a pseudo-TTY and keep it open even if not attached.</span></span> <span data-ttu-id="ac49e-155">(même effet que `--interactive --tty`) ;</span><span class="sxs-lookup"><span data-stu-id="ac49e-155">(Same effect as `--interactive --tty`.)</span></span>
  * <span data-ttu-id="ac49e-156">supprimer automatiquement le conteneur lorsqu’il se ferme ;</span><span class="sxs-lookup"><span data-stu-id="ac49e-156">Automatically remove the container when it exits.</span></span>
  * <span data-ttu-id="ac49e-157">mapper le port 5000 de l’ordinateur local avec le port 80 du conteneur ;</span><span class="sxs-lookup"><span data-stu-id="ac49e-157">Map port 5000 on the local machine to port 80 in the container.</span></span>
  * <span data-ttu-id="ac49e-158">nommer le conteneur aspnetcore_sample ;</span><span class="sxs-lookup"><span data-stu-id="ac49e-158">Name the container aspnetcore_sample.</span></span>
  * <span data-ttu-id="ac49e-159">spécifier l’image aspnetapp.</span><span class="sxs-lookup"><span data-stu-id="ac49e-159">Specify the aspnetapp image.</span></span>

* <span data-ttu-id="ac49e-160">Accédez à `http://localhost:5000` dans un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-160">Go to `http://localhost:5000` in a browser to test the app.</span></span>

## <a name="run-in-a-windows-container"></a><span data-ttu-id="ac49e-161">Exécuter dans un conteneur Windows</span><span class="sxs-lookup"><span data-stu-id="ac49e-161">Run in a Windows container</span></span>

* <span data-ttu-id="ac49e-162">Dans le client Docker, basculez vers les conteneurs Windows.</span><span class="sxs-lookup"><span data-stu-id="ac49e-162">In the Docker client, switch to Windows containers.</span></span>

<span data-ttu-id="ac49e-163">Accédez au dossier de fichiers Dockerfile à l’adresse `dotnet-docker/samples/aspnetapp`.</span><span class="sxs-lookup"><span data-stu-id="ac49e-163">Navigate to the docker file folder at `dotnet-docker/samples/aspnetapp`.</span></span>

* <span data-ttu-id="ac49e-164">Exécutez les commandes suivantes pour générer et exécuter l’exemple dans Docker :</span><span class="sxs-lookup"><span data-stu-id="ac49e-164">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* <span data-ttu-id="ac49e-165">Pour les conteneurs Windows, l’adresse IP du conteneur est nécessaire (accéder à `http://localhost:5000` ne fonctionnera pas) :</span><span class="sxs-lookup"><span data-stu-id="ac49e-165">For Windows containers, you need the IP address of the container (browsing to `http://localhost:5000` won't work):</span></span>
  * <span data-ttu-id="ac49e-166">Ouvrez une autre invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ac49e-166">Open up another command prompt.</span></span>
  * <span data-ttu-id="ac49e-167">Exécutez `docker ps` pour voir les conteneurs en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ac49e-167">Run `docker ps` to see the running containers.</span></span> <span data-ttu-id="ac49e-168">Vérifiez que le conteneur « aspnetcore_sample » en fait partie.</span><span class="sxs-lookup"><span data-stu-id="ac49e-168">Verify that the "aspnetcore_sample" container is there.</span></span>
  * <span data-ttu-id="ac49e-169">Exécutez `docker exec aspnetcore_sample ipconfig` pour afficher l’adresse IP du conteneur.</span><span class="sxs-lookup"><span data-stu-id="ac49e-169">Run `docker exec aspnetcore_sample ipconfig` to display the IP address of the container.</span></span> <span data-ttu-id="ac49e-170">La sortie de la commande se présente ainsi :</span><span class="sxs-lookup"><span data-stu-id="ac49e-170">The output from the command looks like this example:</span></span>

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* <span data-ttu-id="ac49e-171">Copiez l’adresse IPv4 du conteneur (par exemple, 172.29.245.43) et collez-la dans la barre d’adresse du navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-171">Copy the container IPv4 address (for example, 172.29.245.43) and paste into the browser address bar to test the app.</span></span>

## <a name="build-and-deploy-manually"></a><span data-ttu-id="ac49e-172">Effectuer manuellement le build et le déploiement</span><span class="sxs-lookup"><span data-stu-id="ac49e-172">Build and deploy manually</span></span>

<span data-ttu-id="ac49e-173">Dans certains scénarios, il peut être intéressant de déployer une application sur un conteneur en y copiant les fichiers d’application nécessaires à l’exécution.</span><span class="sxs-lookup"><span data-stu-id="ac49e-173">In some scenarios, you might want to deploy an app to a container by copying to it the application files that are needed at run time.</span></span> <span data-ttu-id="ac49e-174">Cette section montre comment effectuer un déploiement manuel.</span><span class="sxs-lookup"><span data-stu-id="ac49e-174">This section shows how to deploy manually.</span></span>

* <span data-ttu-id="ac49e-175">Accédez au dossier de projet à l’adresse *dotnet-docker/samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="ac49e-175">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="ac49e-176">Exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) :</span><span class="sxs-lookup"><span data-stu-id="ac49e-176">Run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

  ```dotnetcli
  dotnet publish -c Release -o published
  ```

  <span data-ttu-id="ac49e-177">Voici le rôle des arguments de la commande :</span><span class="sxs-lookup"><span data-stu-id="ac49e-177">The command arguments:</span></span>
  * <span data-ttu-id="ac49e-178">Générez l’application en mode version finale (la valeur par défaut est le mode débogage).</span><span class="sxs-lookup"><span data-stu-id="ac49e-178">Build the application in release mode (the default is debug mode).</span></span>
  * <span data-ttu-id="ac49e-179">Créer les fichiers dans le dossier *published*.</span><span class="sxs-lookup"><span data-stu-id="ac49e-179">Create the files in the *published* folder.</span></span>

* <span data-ttu-id="ac49e-180">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="ac49e-180">Run the application.</span></span>

  * <span data-ttu-id="ac49e-181">Windows :</span><span class="sxs-lookup"><span data-stu-id="ac49e-181">Windows:</span></span>

    ```dotnetcli
    dotnet published\aspnetapp.dll
    ```

  * <span data-ttu-id="ac49e-182">Linux :</span><span class="sxs-lookup"><span data-stu-id="ac49e-182">Linux:</span></span>

    ```dotnetcli
    dotnet published/aspnetapp.dll
    ```

* <span data-ttu-id="ac49e-183">Accédez à `http://localhost:5000` pour afficher la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="ac49e-183">Browse to `http://localhost:5000` to see the home page.</span></span>

<span data-ttu-id="ac49e-184">Pour utiliser l’application publiée manuellement dans un conteneur Docker, créez un nouveau fichier Dockerfile et utilisez la commande `docker build .` pour générer le conteneur.</span><span class="sxs-lookup"><span data-stu-id="ac49e-184">To use the manually published application within a Docker container, create a new Dockerfile and use the `docker build .` command to build the container.</span></span>

```console
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY published/aspnetapp.dll ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

### <a name="the-dockerfile"></a><span data-ttu-id="ac49e-185">Le fichier Dockerfile</span><span class="sxs-lookup"><span data-stu-id="ac49e-185">The Dockerfile</span></span>

<span data-ttu-id="ac49e-186">Voici le *fichier dockerfile* utilisé par la `docker build` commande que vous avez exécutée précédemment.</span><span class="sxs-lookup"><span data-stu-id="ac49e-186">Here's the *Dockerfile* used by the `docker build` command you ran earlier.</span></span>  <span data-ttu-id="ac49e-187">Il utilise `dotnet publish` comme nous l’avons fait dans cette section pour le build et le déploiement.</span><span class="sxs-lookup"><span data-stu-id="ac49e-187">It uses `dotnet publish` the same way you did in this section to build and deploy.</span></span>  

```dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY --from=build /app/aspnetapp/out ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

## <a name="additional-resources"></a><span data-ttu-id="ac49e-188">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ac49e-188">Additional resources</span></span>

* [<span data-ttu-id="ac49e-189">Commande docker build</span><span class="sxs-lookup"><span data-stu-id="ac49e-189">Docker build command</span></span>](https://docs.docker.com/engine/reference/commandline/build)
* [<span data-ttu-id="ac49e-190">Commande docker run</span><span class="sxs-lookup"><span data-stu-id="ac49e-190">Docker run command</span></span>](https://docs.docker.com/engine/reference/commandline/run)
* <span data-ttu-id="ac49e-191">[Exemple Docker ASP.NET Core](https://github.com/dotnet/dotnet-docker) (utilisé dans ce tutoriel)</span><span class="sxs-lookup"><span data-stu-id="ac49e-191">[ASP.NET Core Docker sample](https://github.com/dotnet/dotnet-docker) (The one used in this tutorial.)</span></span>
* [<span data-ttu-id="ac49e-192">Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge</span><span class="sxs-lookup"><span data-stu-id="ac49e-192">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="ac49e-193">Utilisation des outils Docker dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac49e-193">Working with Visual Studio Docker Tools</span></span>](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [<span data-ttu-id="ac49e-194">Débogage avec Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ac49e-194">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a><span data-ttu-id="ac49e-195">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ac49e-195">Next steps</span></span>

<span data-ttu-id="ac49e-196">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ac49e-196">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ac49e-197">Découvrir les images Docker Microsoft .NET Core</span><span class="sxs-lookup"><span data-stu-id="ac49e-197">Learned about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="ac49e-198">Télécharger un exemple d’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac49e-198">Downloaded an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="ac49e-199">Exécuter l’exemple d’application localement</span><span class="sxs-lookup"><span data-stu-id="ac49e-199">Run the sample app locally</span></span>
> * <span data-ttu-id="ac49e-200">Exécuter l’exemple d’application dans des conteneurs Linux</span><span class="sxs-lookup"><span data-stu-id="ac49e-200">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="ac49e-201">Exécuter l’exemple dans des conteneurs Windows</span><span class="sxs-lookup"><span data-stu-id="ac49e-201">Run the sample with in Windows containers</span></span>
> * <span data-ttu-id="ac49e-202">Effectuer un build et un déploiement manuels</span><span class="sxs-lookup"><span data-stu-id="ac49e-202">Built and deployed manually</span></span>

<span data-ttu-id="ac49e-203">Le référentiel Git qui contient l’exemple d’application comporte également une documentation.</span><span class="sxs-lookup"><span data-stu-id="ac49e-203">The Git repository that contains the sample app also includes documentation.</span></span> <span data-ttu-id="ac49e-204">Pour une vue d’ensemble des ressources disponibles dans le référentiel, voir le [fichier Lisez-moi](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span><span class="sxs-lookup"><span data-stu-id="ac49e-204">For an overview of the resources available in the repository, see [the README file](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span></span> <span data-ttu-id="ac49e-205">En particulier, découvrez comment implémenter le protocole HTTPS :</span><span class="sxs-lookup"><span data-stu-id="ac49e-205">In particular, learn how to implement HTTPS:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ac49e-206">Développer des applications ASP.NET Core avec Docker sur le protocole HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac49e-206">Developing ASP.NET Core Applications with Docker over HTTPS</span></span>](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)
