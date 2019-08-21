---
title: Héberger et déployer ASP.NET Core Blazor côté client
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor avec ASP.NET Core, des réseaux de distribution de contenu (CDN), des serveurs de fichiers et GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: e9a42bd4e8511d426761746047fed2d4f7dfc6dd
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994090"
---
# <a name="host-and-deploy-aspnet-core-blazor-client-side"></a><span data-ttu-id="84ed2-103">Héberger et déployer ASP.NET Core Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="84ed2-103">Host and deploy ASP.NET Core Blazor client-side</span></span>

<span data-ttu-id="84ed2-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="84ed2-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="84ed2-105">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="84ed2-105">Host configuration values</span></span>

<span data-ttu-id="84ed2-106">Les applications Blazor qui utilisent le [modèle d’hébergement côté client](xref:blazor/hosting-models#client-side) peuvent accepter les valeurs de configuration d’hôte suivantes en tant qu’arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="84ed2-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="84ed2-107">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="84ed2-107">Content Root</span></span>

<span data-ttu-id="84ed2-108">L’argument `--contentroot` définit le chemin absolu du répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="84ed2-109">Dans les exemples suivants, `/content-root-path` est le chemin racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="84ed2-110">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="84ed2-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="84ed2-111">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="84ed2-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="84ed2-112">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="84ed2-113">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="84ed2-114">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="84ed2-115">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="84ed2-116">Base du chemin</span><span class="sxs-lookup"><span data-stu-id="84ed2-116">Path base</span></span>

<span data-ttu-id="84ed2-117">L’argument `--pathbase` définit le chemin de base de l’application pour une application s’exécutant localement avec un chemin virtuel non racine (le `href` de la balise `<base>` a comme valeur un chemin autre que `/` pour la préproduction et la production).</span><span class="sxs-lookup"><span data-stu-id="84ed2-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="84ed2-118">Dans les exemples suivants, `/virtual-path` est la base du chemin de l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="84ed2-119">Pour plus d’informations, consultez la section [Chemin de base de l’application](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="84ed2-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84ed2-120">Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="84ed2-121">Si vous spécifiez `<base href="/CoolApp/">` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="84ed2-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="84ed2-122">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="84ed2-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="84ed2-123">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="84ed2-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="84ed2-124">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="84ed2-125">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="84ed2-126">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="84ed2-127">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="84ed2-128">URL</span><span class="sxs-lookup"><span data-stu-id="84ed2-128">URLs</span></span>

<span data-ttu-id="84ed2-129">L’argument `--urls` définit les adresses IP ou les adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les demandes.</span><span class="sxs-lookup"><span data-stu-id="84ed2-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="84ed2-130">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="84ed2-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="84ed2-131">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="84ed2-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="84ed2-132">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="84ed2-133">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="84ed2-134">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="84ed2-135">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="84ed2-136">Déploiement</span><span class="sxs-lookup"><span data-stu-id="84ed2-136">Deployment</span></span>

<span data-ttu-id="84ed2-137">Avec le [modèle d’hébergement côté client](xref:blazor/hosting-models#client-side) :</span><span class="sxs-lookup"><span data-stu-id="84ed2-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="84ed2-138">L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="84ed2-139">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="84ed2-140">Les stratégies suivantes sont prises en charge :</span><span class="sxs-lookup"><span data-stu-id="84ed2-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="84ed2-141">L’application Blazor est fournie par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84ed2-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="84ed2-142">Cette stratégie est abordée dans la section [Déploiement hébergé avec ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="84ed2-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="84ed2-143">L’application Blazor est placée sur un service ou un serveur web d’hébergement statique, où .NET n’est pas utilisé pour servir l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="84ed2-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="84ed2-144">Cette stratégie est abordée dans la section [Déploiement autonome](#standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="84ed2-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="84ed2-145">Configurer l'éditeur de liens</span><span class="sxs-lookup"><span data-stu-id="84ed2-145">Configure the Linker</span></span>

<span data-ttu-id="84ed2-146">Blazor effectue la liaison de langage intermédiaire (IL) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie.</span><span class="sxs-lookup"><span data-stu-id="84ed2-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="84ed2-147">La liaison d’assembly peut être contrôlée pendant le processus de build.</span><span class="sxs-lookup"><span data-stu-id="84ed2-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="84ed2-148">Pour plus d’informations, consultez <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="84ed2-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="84ed2-149">Réécriture d’URL pour un routage correct</span><span class="sxs-lookup"><span data-stu-id="84ed2-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="84ed2-150">Le routage des requêtes pour les composants de la page dans une application côté client n’est pas aussi simple que le routage des requêtes vers une application hébergée côté serveur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="84ed2-151">Considérez une application côté client avec deux composants :</span><span class="sxs-lookup"><span data-stu-id="84ed2-151">Consider a client-side app with two components:</span></span>

* <span data-ttu-id="84ed2-152">*Main.razor* &ndash; Se charge à la racine de l’application et contient un lien vers le composant `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="84ed2-152">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="84ed2-153">Composant *About.Razor* &ndash; `About`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-153">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="84ed2-154">Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :</span><span class="sxs-lookup"><span data-stu-id="84ed2-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="84ed2-155">Le navigateur effectue une requête.</span><span class="sxs-lookup"><span data-stu-id="84ed2-155">The browser makes a request.</span></span>
1. <span data-ttu-id="84ed2-156">La page par défaut est retournée, généralement *index.html*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="84ed2-157">*index.HTML* amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="84ed2-158">Le routeur de Blazor se charge et le composant `Main` Razor est rendu.</span><span class="sxs-lookup"><span data-stu-id="84ed2-158">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="84ed2-159">Dans la page principale, la sélection du composant `About` fonctionne sur le client, car le routeur Blazor empêche le navigateur d’effectuer une requête sur Internet à `www.contoso.com` pour `About` et fournit lui-même le composant `About` rendu.</span><span class="sxs-lookup"><span data-stu-id="84ed2-159">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="84ed2-160">Toutes les requêtes pour des points de terminaison internes *au sein de l’application côté client* fonctionnent de manière identique : Les requêtes ne déclenchent pas de requêtes basées sur le navigateur pour des ressources hébergées par le serveur sur Internet.</span><span class="sxs-lookup"><span data-stu-id="84ed2-160">All of the requests for internal endpoints *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="84ed2-161">Le routeur gère les requêtes en interne.</span><span class="sxs-lookup"><span data-stu-id="84ed2-161">The router handles the requests internally.</span></span>

<span data-ttu-id="84ed2-162">Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue.</span><span class="sxs-lookup"><span data-stu-id="84ed2-162">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="84ed2-163">Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 – Non trouvé* est retournée.</span><span class="sxs-lookup"><span data-stu-id="84ed2-163">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="84ed2-164">Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-164">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="84ed2-165">Quand *index.html* est retournée, le routeur côté client de l’application prend le relais et répond avec la ressource appropriée.</span><span class="sxs-lookup"><span data-stu-id="84ed2-165">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="84ed2-166">Chemin de base de l’application</span><span class="sxs-lookup"><span data-stu-id="84ed2-166">App base path</span></span>

<span data-ttu-id="84ed2-167">Le *chemin de base de l’application* est le chemin racine d’application virtuel sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-167">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="84ed2-168">Par exemple, une application qui réside sur le serveur Contoso dans un dossier virtuel à `/CoolApp/` est accessible à `https://www.contoso.com/CoolApp` et son chemin de base virtuel est `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-168">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="84ed2-169">Le fait d’affecter `<base href="/CoolApp/">` comme chemin de base de l’application au chemin virtuel lui permet de connaître l’endroit où elle réside virtuellement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-169">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="84ed2-170">L’application peut utiliser le chemin de base de l’application pour construire des URL relatives à la racine d’application à partir d’un composant qui ne se trouve pas dans le répertoire racine.</span><span class="sxs-lookup"><span data-stu-id="84ed2-170">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="84ed2-171">Cela permet aux composants qui existent à différents niveaux de la structure de répertoires de générer des liens vers d’autres ressources à des emplacements dans toute l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-171">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="84ed2-172">Le chemin de base de l’application sert également à intercepter les clics sur les liens hypertextes où la cible `href` du lien se trouve dans l’espace d’URI du chemin de base de l’application (le routeur Blazor gère la navigation interne).</span><span class="sxs-lookup"><span data-stu-id="84ed2-172">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="84ed2-173">Dans de nombreux scénarios d’hébergement, le chemin virtuel du serveur vers l’application est la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-173">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="84ed2-174">Dans ce cas, le chemin de base de l’application est une barre oblique (`<base href="/" />`), ce qui correspond à la configuration par défaut pour une application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-174">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="84ed2-175">Dans d’autres scénarios d’hébergement, tels que GitHub Pages et les sous-applications ou répertoires virtuels IIS, vous devez affecter le chemin virtuel du serveur vers l’application comme chemin de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-175">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="84ed2-176">Pour définir le chemin de base de l’application, mettez à jour la balise `<base>` dans les éléments de balise `<head>` du fichier *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-176">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="84ed2-177">Affectez la valeur `/virtual-path/` à l’attribut `href` (la barre oblique de fin est obligatoire), où `/virtual-path/` est le chemin racine d’application virtuel complet sur le serveur pour l’application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-177">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="84ed2-178">Dans l’exemple précédent, le chemin virtuel est défini sur `/CoolApp/` : `<base href="/CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-178">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="84ed2-179">Pour une application avec un chemin virtuel non racine configuré (par exemple `<base href="/CoolApp/">`), l’application ne parvient pas à trouver ses ressources *quand elle est exécutée localement*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-179">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="84ed2-180">Pour surmonter ce problème pendant le développement local et les tests, vous pouvez fournir un argument de *base de chemin* qui correspond à la valeur `href` de la balise `<base>` au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="84ed2-180">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="84ed2-181">Pour passer l’argument de base de chemin avec le chemin racine (`/`) quand vous exécutez l’application localement, exécutez la commande `dotnet run` à partir du répertoire de l’application avec l’option `--pathbase` :</span><span class="sxs-lookup"><span data-stu-id="84ed2-181">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="84ed2-182">Pour une application avec un chemin virtuel de base de `/CoolApp/` (`<base href="/CoolApp/">`), la commande est :</span><span class="sxs-lookup"><span data-stu-id="84ed2-182">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="84ed2-183">L’application répond localement à `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-183">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="84ed2-184">Pour plus d’informations, voir la section [Valeur de configuration d’hôte de base du chemin](#path-base).</span><span class="sxs-lookup"><span data-stu-id="84ed2-184">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="84ed2-185">Si une application utilise le [modèle d’hébergement côté client](xref:blazor/hosting-models#client-side) (basé sur le modèle de projet **Application Blazor WebAssembly** ou le modèle `blazorwasm` quand la commande [dotnet new](/dotnet/core/tools/dotnet-new) est utilisée) et est hébergée en tant que sous-application IIS dans une application ASP.NET Core, il est important de désactiver le gestionnaire de module ASP.NET Core hérité ou de vérifier que la section `<handlers>` de l’application racine (parente) dans le fichier *web.config* n’est pas héritée par la sous-application.</span><span class="sxs-lookup"><span data-stu-id="84ed2-185">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor WebAssembly App** project template, the `blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-app in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="84ed2-186">Supprimez le gestionnaire dans le fichier *web.config* publié de l’application en ajoutant une section `<handlers>` au fichier :</span><span class="sxs-lookup"><span data-stu-id="84ed2-186">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="84ed2-187">En guise d’alternative, vous pouvez désactiver l’héritage de la section `<system.webServer>` de l’application racine (parente) en affectant la valeur `false` à l’attribut `inheritInChildApplications` d’un élément `<location>` :</span><span class="sxs-lookup"><span data-stu-id="84ed2-187">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="84ed2-188">La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de la configuration du chemin de base de l’application comme décrit dans cette section.</span><span class="sxs-lookup"><span data-stu-id="84ed2-188">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="84ed2-189">Dans le fichier *index.html* de l’application, définissez le chemin de base de l’application sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.</span><span class="sxs-lookup"><span data-stu-id="84ed2-189">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="84ed2-190">Déploiement hébergé avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84ed2-190">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="84ed2-191">Un *déploiement hébergé* fournit l’application Blazor côté client aux navigateurs à partir d’une [application ASP.NET Core](xref:index) qui s’exécute sur un serveur web.</span><span class="sxs-lookup"><span data-stu-id="84ed2-191">A *hosted deployment* serves the Blazor client-side app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="84ed2-192">L’application Blazor est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="84ed2-192">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="84ed2-193">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="84ed2-193">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="84ed2-194">Pour un déploiement hébergé, Visual Studio inclut le modèle de projet **Application Blazor WebAssembly** (modèle `blazorwasm` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)) avec l’option **Hébergé** sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="84ed2-194">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="84ed2-195">Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="84ed2-195">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="84ed2-196">Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="84ed2-196">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="84ed2-197">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="84ed2-197">Standalone deployment</span></span>

<span data-ttu-id="84ed2-198">Un *déploiement autonome* fournit l’application Blazor côté client sous la forme d’un ensemble de fichiers statiques qui sont demandés directement par les clients.</span><span class="sxs-lookup"><span data-stu-id="84ed2-198">A *standalone deployment* serves the Blazor client-side app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="84ed2-199">N’importe quel serveur de fichiers statiques est capable de servir l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="84ed2-199">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="84ed2-200">Les ressources d’un déploiement autonome sont publiées dans le dossier *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-200">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="84ed2-201">IIS</span><span class="sxs-lookup"><span data-stu-id="84ed2-201">IIS</span></span>

<span data-ttu-id="84ed2-202">IIS est un serveur de fichiers statiques compatible avec les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="84ed2-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="84ed2-203">Pour configurer IIS afin d’héberger Blazor, consultez [Générer un site web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="84ed2-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="84ed2-204">Les ressources publiées sont créées dans le dossier */bin/Release/{FRAMEWORK CIBLE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="84ed2-205">Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="84ed2-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="84ed2-206">web.config</span><span class="sxs-lookup"><span data-stu-id="84ed2-206">web.config</span></span>

<span data-ttu-id="84ed2-207">Quand un projet Blazor est publié, un fichier *web.config* est créé avec la configuration IIS suivante :</span><span class="sxs-lookup"><span data-stu-id="84ed2-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="84ed2-208">Les types MIME sont définis pour les extensions de fichiers suivantes :</span><span class="sxs-lookup"><span data-stu-id="84ed2-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="84ed2-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="84ed2-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="84ed2-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="84ed2-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="84ed2-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="84ed2-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="84ed2-212">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="84ed2-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="84ed2-213">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="84ed2-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="84ed2-214">La compression HTTP est activée pour les types MIME suivants :</span><span class="sxs-lookup"><span data-stu-id="84ed2-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="84ed2-215">Des règles du module de réécriture d’URL sont établies :</span><span class="sxs-lookup"><span data-stu-id="84ed2-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="84ed2-216">Servir le sous-répertoire où résident les ressources statiques de l’application ( *{NOM ASSEMBLY}/dist/{CHEMIN DEMANDÉ}* ).</span><span class="sxs-lookup"><span data-stu-id="84ed2-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="84ed2-217">Créer un routage de secours SPA afin que les demandes de ressources autres que des fichiers soient redirigées vers le document par défaut de l’application dans son dossier de ressources statiques ( *{NOM ASSEMBLY}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="84ed2-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="84ed2-218">Installer le module de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="84ed2-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="84ed2-219">Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL.</span><span class="sxs-lookup"><span data-stu-id="84ed2-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="84ed2-220">Il n’est pas installé par défaut, et ne peut pas l’être en tant que fonctionnalité de service de rôle Serveur Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="84ed2-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="84ed2-221">Vous devez le télécharger à partir du site web IIS.</span><span class="sxs-lookup"><span data-stu-id="84ed2-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="84ed2-222">Utilisez Web Platform Installer pour installer le module :</span><span class="sxs-lookup"><span data-stu-id="84ed2-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="84ed2-223">Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="84ed2-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="84ed2-224">Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI.</span><span class="sxs-lookup"><span data-stu-id="84ed2-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="84ed2-225">Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="84ed2-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="84ed2-226">Copiez le programme d’installation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-226">Copy the installer to the server.</span></span> <span data-ttu-id="84ed2-227">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="84ed2-227">Run the installer.</span></span> <span data-ttu-id="84ed2-228">Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="84ed2-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="84ed2-229">Il n’est pas nécessaire de redémarrer le serveur après l’installation.</span><span class="sxs-lookup"><span data-stu-id="84ed2-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="84ed2-230">Configurer le site web</span><span class="sxs-lookup"><span data-stu-id="84ed2-230">Configure the website</span></span>

<span data-ttu-id="84ed2-231">Affectez le dossier de l’application comme **chemin physique** du site web.</span><span class="sxs-lookup"><span data-stu-id="84ed2-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="84ed2-232">Le dossier contient :</span><span class="sxs-lookup"><span data-stu-id="84ed2-232">The folder contains:</span></span>

* <span data-ttu-id="84ed2-233">Le fichier *web.config* utilisé par IIS pour configurer le site web, notamment les règles de redirection nécessaires et les types de contenu de fichiers</span><span class="sxs-lookup"><span data-stu-id="84ed2-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="84ed2-234">Le dossier de ressources statiques de l’application</span><span class="sxs-lookup"><span data-stu-id="84ed2-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="84ed2-235">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="84ed2-235">Troubleshooting</span></span>

<span data-ttu-id="84ed2-236">Si vous recevez un message *500 – Erreur interne du serveur* et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé.</span><span class="sxs-lookup"><span data-stu-id="84ed2-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="84ed2-237">Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS.</span><span class="sxs-lookup"><span data-stu-id="84ed2-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="84ed2-238">Cela empêche le Gestionnaire IIS de charger la configuration du site web et empêche le site web de fournir les fichiers statiques Blazor.</span><span class="sxs-lookup"><span data-stu-id="84ed2-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="84ed2-239">Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="84ed2-239">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="84ed2-240">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="84ed2-240">Azure Storage</span></span>

<span data-ttu-id="84ed2-241">L’hébergement d’un fichier statique de stockage Azure permet l’hébergement d’applications serverless Blazor.</span><span class="sxs-lookup"><span data-stu-id="84ed2-241">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="84ed2-242">Les noms de domaine personnalisé, le réseau de distribution de contenu Azure (CDN) et HTTPS sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="84ed2-242">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="84ed2-243">Lorsque le service blob est activé pour l’hébergement de site Web statique sur un compte de stockage :</span><span class="sxs-lookup"><span data-stu-id="84ed2-243">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="84ed2-244">Définissez le **nom du document d’index** sur `index.html`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-244">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="84ed2-245">Définissez le **chemin d’accès au document d’erreur** sur `index.html`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-245">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="84ed2-246">Les composants de Razor et autres points de terminaison non-fichier ne se trouvent sur les chemins d’accès physiques dans le contenu statique stocké par le service blob.</span><span class="sxs-lookup"><span data-stu-id="84ed2-246">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="84ed2-247">Lors de la réception d’une requête pour l’une de ces ressources que le routeur Blazor doit gérer, l’erreur *404 - introuvable* générée par le service blob achemine la requête vers le **chemin d’accès au document d’erreur**.</span><span class="sxs-lookup"><span data-stu-id="84ed2-247">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="84ed2-248">Le blob *index.html* est retourné et le routeur Blazor charge et traite le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="84ed2-248">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="84ed2-249">Pour plus d’informations, consultez [Hébergement de site Web statique dans le stockage Azure](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="84ed2-249">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="84ed2-250">Nginx</span><span class="sxs-lookup"><span data-stu-id="84ed2-250">Nginx</span></span>

<span data-ttu-id="84ed2-251">Le fichier *nginx.conf* suivant est simplifié afin de montrer comment configurer Nginx pour envoyer le fichier *index.html* chaque fois qu’il ne trouve aucun fichier correspondant sur le disque.</span><span class="sxs-lookup"><span data-stu-id="84ed2-251">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="84ed2-252">Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).</span><span class="sxs-lookup"><span data-stu-id="84ed2-252">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="84ed2-253">Nginx dans Docker</span><span class="sxs-lookup"><span data-stu-id="84ed2-253">Nginx in Docker</span></span>

<span data-ttu-id="84ed2-254">Pour héberger Blazor dans Docker à l’aide de Nginx, configurez le fichier Dockerfile pour utiliser l’image Nginx basée sur Alpine.</span><span class="sxs-lookup"><span data-stu-id="84ed2-254">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="84ed2-255">Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="84ed2-255">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="84ed2-256">Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="84ed2-256">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="84ed2-257">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="84ed2-257">GitHub Pages</span></span>

<span data-ttu-id="84ed2-258">Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-258">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="84ed2-259">Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="84ed2-259">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="84ed2-260">Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="84ed2-260">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="84ed2-261">Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*.</span><span class="sxs-lookup"><span data-stu-id="84ed2-261">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="84ed2-262">Définissez la valeur d’attribut `href` sur le nom du dépôt GitHub avec une barre oblique (par exemple, `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="84ed2-262">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
