---
title: Héberger et déployer Razor Components
author: guardrex
description: Découvrez comment héberger et déployer des applications Razor Components et Blazor à l’aide d’ASP.NET Core, de réseaux de distribution de contenu (CDN), de serveurs de fichiers et de GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
ms.openlocfilehash: 9debd75128ceecb805fc673a8182a785fc9f7942
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667926"
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="887a3-103">Héberger et déployer Razor Components</span><span class="sxs-lookup"><span data-stu-id="887a3-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="887a3-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="887a3-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="887a3-105">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="887a3-105">Publish the app</span></span>

<span data-ttu-id="887a3-106">Les applications sont publiées pour le déploiement dans la configuration Release avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="887a3-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="887a3-107">Un IDE peut gérer l’exécution de la commande `dotnet publish` automatiquement à l’aide de ses fonctionnalités de publication intégrées. Ainsi, en fonction des outils de développement utilisés, il ne sera peut-être pas nécessaire d’exécuter manuellement la commande à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="887a3-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="887a3-108">`dotnet publish` déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [génère](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="887a3-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="887a3-109">Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="887a3-110">Le déploiement est créé dans le dossier */bin/Release/&lt;framework_cible&gt;/publish*.</span><span class="sxs-lookup"><span data-stu-id="887a3-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="887a3-111">Les ressources dans le dossier *publish* sont déployées sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="887a3-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="887a3-112">Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.</span><span class="sxs-lookup"><span data-stu-id="887a3-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="887a3-113">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="887a3-113">Host configuration values</span></span>

<span data-ttu-id="887a3-114">Les applications Razor Components qui utilisent le [modèle d’hébergement côté serveur](xref:razor-components/hosting-models#server-side-hosting-model) peuvent accepter des [valeurs de configuration d’hôte web](xref:fundamentals/host/web-host#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="887a3-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="887a3-115">Les applications Blazor qui utilisent le [modèle d’hébergement côté client](xref:razor-components/hosting-models#client-side-hosting-model) peuvent accepter les valeurs de configuration d’hôte suivantes en tant qu’arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="887a3-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="887a3-116">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="887a3-116">Content Root</span></span>

<span data-ttu-id="887a3-117">L’argument `--contentroot` définit le chemin absolu du répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="887a3-118">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="887a3-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="887a3-119">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="887a3-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="887a3-120">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="887a3-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="887a3-121">Ce paramètre est récupéré lors de l’exécution de l’application avec le débogueur Visual Studio et à partir d’une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="887a3-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="887a3-122">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="887a3-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="887a3-123">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="887a3-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="887a3-124">Base du chemin</span><span class="sxs-lookup"><span data-stu-id="887a3-124">Path base</span></span>

<span data-ttu-id="887a3-125">L’argument `--pathbase` définit le chemin de base de l’application pour une application s’exécutant localement avec un chemin virtuel non racine (le `href` de la balise `<base>` a comme valeur un chemin autre que `/` pour la préproduction et la production).</span><span class="sxs-lookup"><span data-stu-id="887a3-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="887a3-126">Pour plus d’informations, consultez la section [Chemin de base de l’application](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="887a3-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="887a3-127">Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="887a3-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="887a3-128">Si vous spécifiez `<base href="/CoolApp/" />` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="887a3-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="887a3-129">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="887a3-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="887a3-130">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="887a3-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="887a3-131">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="887a3-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="887a3-132">Ce paramètre est récupéré lors de l’exécution de l’application avec le débogueur Visual Studio et à partir d’une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="887a3-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="887a3-133">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="887a3-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="887a3-134">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="887a3-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="887a3-135">URL</span><span class="sxs-lookup"><span data-stu-id="887a3-135">URLs</span></span>

<span data-ttu-id="887a3-136">L’argument `--urls` indique les adresses IP ou adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les requêtes.</span><span class="sxs-lookup"><span data-stu-id="887a3-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="887a3-137">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="887a3-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="887a3-138">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="887a3-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="887a3-139">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="887a3-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="887a3-140">Ce paramètre est récupéré lors de l’exécution de l’application avec le débogueur Visual Studio et à partir d’une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="887a3-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="887a3-141">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="887a3-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="887a3-142">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="887a3-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="887a3-143">Déployer une application Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="887a3-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="887a3-144">Avec le [modèle d’hébergement côté client](xref:razor-components/hosting-models#client-side-hosting-model) :</span><span class="sxs-lookup"><span data-stu-id="887a3-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="887a3-145">L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="887a3-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="887a3-146">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="887a3-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="887a3-147">Les stratégies suivantes sont prises en charge :</span><span class="sxs-lookup"><span data-stu-id="887a3-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="887a3-148">L’application Blazor est fournie par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="887a3-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="887a3-149">Voir la section [Déploiement Blazor hébergé côté client avec ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="887a3-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="887a3-150">L’application Blazor est placée sur un service ou un serveur web d’hébergement statique, où .NET n’est pas utilisé pour servir l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="887a3-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="887a3-151">Voir la section [Déploiement Blazor autonome côté client](#client-side-blazor-standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="887a3-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>
  
### <a name="configure-the-linker"></a><span data-ttu-id="887a3-152">Configurer l'éditeur de liens</span><span class="sxs-lookup"><span data-stu-id="887a3-152">Configure the Linker</span></span>

<span data-ttu-id="887a3-153">Blazor effectue la liaison de langage intermédiaire (IL) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie.</span><span class="sxs-lookup"><span data-stu-id="887a3-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="887a3-154">Vous pouvez contrôler la liaison d’assembly lors de la génération.</span><span class="sxs-lookup"><span data-stu-id="887a3-154">You can control assembly linking on build.</span></span> <span data-ttu-id="887a3-155">Pour plus d'informations, consultez <xref:host-and-deploy/razor-components/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="887a3-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="887a3-156">Réécriture d’URL pour un routage correct</span><span class="sxs-lookup"><span data-stu-id="887a3-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="887a3-157">Le routage des requêtes pour les composants de la page dans une application côté client n’est pas aussi simple que le routage des requêtes vers une application hébergée côté serveur.</span><span class="sxs-lookup"><span data-stu-id="887a3-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="887a3-158">Considérez une application côté client avec deux pages :</span><span class="sxs-lookup"><span data-stu-id="887a3-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="887a3-159">**_Main.cshtml_** &ndash; Se charge à la racine de l’application et contient un lien vers la page À propos de (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="887a3-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="887a3-160">**_About.cshtml_** &ndash; Page À propos de.</span><span class="sxs-lookup"><span data-stu-id="887a3-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="887a3-161">Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :</span><span class="sxs-lookup"><span data-stu-id="887a3-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="887a3-162">Le navigateur effectue une requête.</span><span class="sxs-lookup"><span data-stu-id="887a3-162">The browser makes a request.</span></span>
1. <span data-ttu-id="887a3-163">La page par défaut est retournée, généralement *index.html*.</span><span class="sxs-lookup"><span data-stu-id="887a3-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="887a3-164">*index.HTML* amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="887a3-165">Le routeur Blazor se charge et la page principale Razor (*Main.cshtml*) s’affiche.</span><span class="sxs-lookup"><span data-stu-id="887a3-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="887a3-166">Dans la page principale, le fait de sélectionner le lien vers la page À propos de charge cette dernière.</span><span class="sxs-lookup"><span data-stu-id="887a3-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="887a3-167">La sélection du lien vers la page À propos de fonctionne sur le client, car le routeur Blazor empêche le navigateur d’effectuer une requête sur Internet à `www.contoso.com` pour `About` et fournit lui-même la page À propos de.</span><span class="sxs-lookup"><span data-stu-id="887a3-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="887a3-168">Toutes les requêtes pour des pages internes *au sein de l’application côté client* fonctionnent de manière identique : Les requêtes ne déclenchent pas de requêtes basées sur le navigateur pour des ressources hébergées par le serveur sur Internet.</span><span class="sxs-lookup"><span data-stu-id="887a3-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="887a3-169">Le routeur gère les requêtes en interne.</span><span class="sxs-lookup"><span data-stu-id="887a3-169">The router handles the requests internally.</span></span>

<span data-ttu-id="887a3-170">Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue.</span><span class="sxs-lookup"><span data-stu-id="887a3-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="887a3-171">Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 Non trouvé* est retournée.</span><span class="sxs-lookup"><span data-stu-id="887a3-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="887a3-172">Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="887a3-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="887a3-173">Quand *index.html* est retournée, le routeur côté client de l’application prend le relais et répond avec la ressource appropriée.</span><span class="sxs-lookup"><span data-stu-id="887a3-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="887a3-174">Chemin de base de l’application</span><span class="sxs-lookup"><span data-stu-id="887a3-174">App base path</span></span>

<span data-ttu-id="887a3-175">Le chemin de base de l’application est le chemin racine d’application virtuel sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="887a3-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="887a3-176">Par exemple, une application qui réside sur le serveur Contoso dans un dossier virtuel à `/CoolApp/` est accessible à `https://www.contoso.com/CoolApp` et son chemin de base virtuel est `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="887a3-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="887a3-177">Le fait d’affecter `CoolApp/` comme chemin de base de l’application lui permet de connaître l’endroit où elle réside virtuellement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="887a3-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="887a3-178">L’application peut utiliser le chemin de base de l’application pour construire des URL relatives à la racine d’application à partir d’un composant qui ne se trouve pas dans le répertoire racine.</span><span class="sxs-lookup"><span data-stu-id="887a3-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="887a3-179">Cela permet aux composants qui existent à différents niveaux de la structure de répertoires de générer des liens vers d’autres ressources à des emplacements dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="887a3-180">Le chemin de base de l’application sert également à intercepter les clics sur les liens hypertextes où la cible `href` du lien se trouve dans l’espace d’URI du chemin de base de l’application (le routeur Blazor gère la navigation interne).</span><span class="sxs-lookup"><span data-stu-id="887a3-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="887a3-181">Dans de nombreux scénarios d’hébergement, le chemin virtuel du serveur vers l’application est la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="887a3-182">Dans ce cas, le chemin de base de l’application est une barre oblique (`<base href="/" />`), ce qui correspond à la configuration par défaut pour une application.</span><span class="sxs-lookup"><span data-stu-id="887a3-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="887a3-183">Dans d’autres scénarios d’hébergement, tels que GitHub Pages et les sous-applications ou répertoires virtuels IIS, vous devez affecter le chemin virtuel du serveur vers l’application comme chemin de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="887a3-184">Pour définir le chemin de base de l’application, ajoutez ou mettez à jour la balise `<base>` dans *index.html* qui se trouve entre les éléments de balise `<head>`.</span><span class="sxs-lookup"><span data-stu-id="887a3-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="887a3-185">Affectez la valeur `<virtual-path>/` à l’attribut `href` (la barre oblique de fin est obligatoire), où `<virtual-path>/` est le chemin racine d’application virtuel complet sur le serveur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="887a3-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="887a3-186">Dans l’exemple précédent, le chemin virtuel est défini sur `CoolApp/` : `<base href="CoolApp/" />`.</span><span class="sxs-lookup"><span data-stu-id="887a3-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="887a3-187">Pour une application avec un chemin virtuel non racine configuré (par exemple `<base href="CoolApp/" />`), l’application ne parvient pas à trouver ses ressources *quand elle est exécutée localement*.</span><span class="sxs-lookup"><span data-stu-id="887a3-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="887a3-188">Pour surmonter ce problème pendant le développement local et les tests, vous pouvez fournir un argument de *base de chemin* qui correspond à la valeur `href` de la balise `<base>` au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="887a3-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="887a3-189">Pour passer l’argument de base de chemin avec le chemin racine (`/`) quand vous exécutez l’application localement, exécutez la commande suivante à partir du répertoire de l’application :</span><span class="sxs-lookup"><span data-stu-id="887a3-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="887a3-190">L’application répond localement à `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="887a3-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="887a3-191">Pour plus d’informations, consultez la section [Valeur de configuration d’hôte Base du chemin](#path-base).</span><span class="sxs-lookup"><span data-stu-id="887a3-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="887a3-192">Si une application utilise le [modèle d’hébergement côté client](xref:razor-components/hosting-models#client-side-hosting-model) (basé sur le modèle de projet **Blazor**) et est hébergée en tant que sous-application IIS dans une application ASP.NET Core, il convient de désactiver le gestionnaire de module ASP.NET Core hérité ou de s’assurer que la section `<handlers>` de l’application racine (parente) dans le fichier *web.config* n’est pas héritée par la sous-application.</span><span class="sxs-lookup"><span data-stu-id="887a3-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="887a3-193">Supprimez le gestionnaire dans le fichier *web.config* publié de l’application en ajoutant une section `<handlers>` au fichier :</span><span class="sxs-lookup"><span data-stu-id="887a3-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="887a3-194">En guise d’alternative, vous pouvez désactiver l’héritage de la section `<system.webServer>` de l’application racine (parente) en affectant la valeur `false` à l’attribut `inheritInChildApplications` d’un élément `<location>` :</span><span class="sxs-lookup"><span data-stu-id="887a3-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="887a3-195">La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de la configuration du chemin de base de l’application comme décrit dans cette section.</span><span class="sxs-lookup"><span data-stu-id="887a3-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="887a3-196">Dans le fichier *index.html* de l’application, définissez le chemin de base de l’application sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.</span><span class="sxs-lookup"><span data-stu-id="887a3-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="887a3-197">Déploiement Blazor hébergé côté client avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="887a3-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="887a3-198">Un *déploiement hébergé* fournit l’application Blazor côté client aux navigateurs à partir d’une [application ASP.NET Core](xref:index) qui s’exécute sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="887a3-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="887a3-199">L’application Blazor est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="887a3-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="887a3-200">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="887a3-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="887a3-201">Pour un déploiement hébergé, Visual Studio inclut le modèle de projet **Blazor (ASP.NET Core hébergé)** (modèle `blazorhosted` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="887a3-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="887a3-202">Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="887a3-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="887a3-203">Pour plus d’informations sur le déploiement sur Azure App Service, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="887a3-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="887a3-204">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="887a3-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="887a3-205">Déploiement Blazor autonome côté client</span><span class="sxs-lookup"><span data-stu-id="887a3-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="887a3-206">Un *déploiement autonome* fournit l’application Blazor côté client sous la forme d’un ensemble de fichiers statiques qui sont demandés directement par les clients.</span><span class="sxs-lookup"><span data-stu-id="887a3-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="887a3-207">Aucun serveur web n’est utilisé pour fournir l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="887a3-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="887a3-208">Hébergement Blazor autonome côté client avec IIS</span><span class="sxs-lookup"><span data-stu-id="887a3-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="887a3-209">IIS est un serveur de fichiers statiques compatible avec les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="887a3-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="887a3-210">Pour configurer IIS afin d’héberger Blazor, consultez [Générer un site web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="887a3-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="887a3-211">Les ressources publiées sont créées dans le dossier *\\bin\\version\\&lt;framework_cible&gt;\\publish*.</span><span class="sxs-lookup"><span data-stu-id="887a3-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="887a3-212">Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="887a3-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="887a3-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="887a3-213">**web.config**</span></span>

<span data-ttu-id="887a3-214">Quand un projet Blazor est publié, un fichier *web.config* est créé avec la configuration IIS suivante :</span><span class="sxs-lookup"><span data-stu-id="887a3-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="887a3-215">Les types MIME sont définis pour les extensions de fichiers suivantes :</span><span class="sxs-lookup"><span data-stu-id="887a3-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="887a3-216">*\*.dll* : `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="887a3-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="887a3-217">*\*.json* : `application/json`</span><span class="sxs-lookup"><span data-stu-id="887a3-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="887a3-218">*\*.wasm* : `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="887a3-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="887a3-219">*\*.woff* : `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="887a3-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="887a3-220">*\*.woff2* : `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="887a3-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="887a3-221">La compression HTTP est activée pour les types MIME suivants :</span><span class="sxs-lookup"><span data-stu-id="887a3-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="887a3-222">Des règles du module de réécriture d’URL sont établies :</span><span class="sxs-lookup"><span data-stu-id="887a3-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="887a3-223">Servir le sous-répertoire où résident les ressources statiques de l’application (*&lt;nom_assembly&gt;\\dist\\&lt;chemin_demandé&gt;*).</span><span class="sxs-lookup"><span data-stu-id="887a3-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="887a3-224">Créer un routage de secours SPA afin que les requêtes pour des ressources autres que des fichiers soient redirigées vers le document par défaut de l’application dans son dossier de ressources statiques (*&lt;nom_assembly&gt;\\dist\\index.html*).</span><span class="sxs-lookup"><span data-stu-id="887a3-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="887a3-225">**Installer le module de réécriture d’URL**</span><span class="sxs-lookup"><span data-stu-id="887a3-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="887a3-226">Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL.</span><span class="sxs-lookup"><span data-stu-id="887a3-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="887a3-227">Il n’est pas installé par défaut, et n’est pas disponible pour l’installation en tant que fonctionnalité de service de rôle Serveur Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="887a3-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="887a3-228">Vous devez le télécharger à partir du site web IIS.</span><span class="sxs-lookup"><span data-stu-id="887a3-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="887a3-229">Utilisez Web Platform Installer pour installer le module :</span><span class="sxs-lookup"><span data-stu-id="887a3-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="887a3-230">Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="887a3-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="887a3-231">Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI.</span><span class="sxs-lookup"><span data-stu-id="887a3-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="887a3-232">Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="887a3-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="887a3-233">Copiez le programme d’installation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="887a3-233">Copy the installer to the server.</span></span> <span data-ttu-id="887a3-234">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="887a3-234">Run the installer.</span></span> <span data-ttu-id="887a3-235">Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="887a3-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="887a3-236">Il n’est pas nécessaire de redémarrer le serveur après l’installation.</span><span class="sxs-lookup"><span data-stu-id="887a3-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="887a3-237">**Configurer le site web**</span><span class="sxs-lookup"><span data-stu-id="887a3-237">**Configure the website**</span></span>

<span data-ttu-id="887a3-238">Affectez le dossier de l’application comme **chemin physique** du site web.</span><span class="sxs-lookup"><span data-stu-id="887a3-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="887a3-239">Le dossier contient :</span><span class="sxs-lookup"><span data-stu-id="887a3-239">The folder contains:</span></span>

* <span data-ttu-id="887a3-240">Le fichier *web.config* utilisé par IIS pour configurer le site web, notamment les règles de redirection nécessaires et les types de contenu de fichiers</span><span class="sxs-lookup"><span data-stu-id="887a3-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="887a3-241">Le dossier de ressources statiques de l’application</span><span class="sxs-lookup"><span data-stu-id="887a3-241">The app's static asset folder.</span></span>

<span data-ttu-id="887a3-242">**Résolution des problèmes**</span><span class="sxs-lookup"><span data-stu-id="887a3-242">**Troubleshooting**</span></span>

<span data-ttu-id="887a3-243">Si un message *500 Erreur interne du serveur* est reçu et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé.</span><span class="sxs-lookup"><span data-stu-id="887a3-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="887a3-244">Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS.</span><span class="sxs-lookup"><span data-stu-id="887a3-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="887a3-245">Cela empêche le Gestionnaire IIS de charger la configuration du site web et empêche le site web de fournir les fichiers statiques Blazor.</span><span class="sxs-lookup"><span data-stu-id="887a3-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="887a3-246">Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="887a3-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="887a3-247">Hébergement Blazor autonome côté client avec Nginx</span><span class="sxs-lookup"><span data-stu-id="887a3-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="887a3-248">Le fichier *nginx.conf* suivant est simplifié afin d’illustrer comment configurer Nginx pour envoyer le fichier *Index.html* chaque fois qu’il ne trouve aucun fichier correspondant sur le disque.</span><span class="sxs-lookup"><span data-stu-id="887a3-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="887a3-249">Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).</span><span class="sxs-lookup"><span data-stu-id="887a3-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="887a3-250">Hébergement Blazor autonome côté client avec Nginx dans Docker</span><span class="sxs-lookup"><span data-stu-id="887a3-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="887a3-251">Pour héberger Blazor dans Docker à l’aide de Nginx, configurez le fichier Dockerfile pour utiliser l’image Nginx basée sur Alpine.</span><span class="sxs-lookup"><span data-stu-id="887a3-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="887a3-252">Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="887a3-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="887a3-253">Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="887a3-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="887a3-254">Hébergement Blazor autonome côté client avec GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="887a3-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="887a3-255">Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="887a3-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="887a3-256">Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="887a3-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="887a3-257">Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="887a3-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="887a3-258">Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*.</span><span class="sxs-lookup"><span data-stu-id="887a3-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="887a3-259">Affectez la valeur `<repository-name>/` à l’attribut `href`, où `<repository-name>/` est le nom du dépôt GitHub.</span><span class="sxs-lookup"><span data-stu-id="887a3-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="887a3-260">Déployer une application Razor Components côté serveur</span><span class="sxs-lookup"><span data-stu-id="887a3-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="887a3-261">Avec le [modèle d’hébergement côté serveur](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components est exécuté sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="887a3-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="887a3-262">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion SignalR.</span><span class="sxs-lookup"><span data-stu-id="887a3-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="887a3-263">L’application est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="887a3-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="887a3-264">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="887a3-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="887a3-265">Pour un déploiement côté serveur, Visual Studio inclut le modèle de projet **Blazor (côté serveur dans ASP.NET Core)** (modèle `blazorserver` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="887a3-265">For a server-side deployment, Visual Studio includes the **Blazor (Server-side in ASP.NET Core)** project template (`blazorserver` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="887a3-266">Quand l’application ASP.NET Core est publiée, l’application Razor Components est incluse dans la sortie publiée afin que l’application ASP.NET Core et l’application Razor Components puissent être déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="887a3-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="887a3-267">Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="887a3-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="887a3-268">Pour plus d’informations sur le déploiement sur Azure App Service, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="887a3-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="887a3-269">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="887a3-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
