---
title: Héberger et déployer ASP.NET Core éblouissant webassembly
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor avec ASP.NET Core, des réseaux de distribution de contenu (CDN), des serveurs de fichiers et GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 06316cacb02d9d7619ff7a210bd596696f86021b
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964253"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="65836-103">Héberger et déployer ASP.NET Core éblouissant webassembly</span><span class="sxs-lookup"><span data-stu-id="65836-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="65836-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="65836-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="65836-105">Avec le [modèle d’hébergement de Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="65836-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="65836-106">L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.</span><span class="sxs-lookup"><span data-stu-id="65836-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="65836-107">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="65836-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="65836-108">Les stratégies de déploiement suivantes sont prises en charge :</span><span class="sxs-lookup"><span data-stu-id="65836-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="65836-109">L’application Blazor est fournie par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65836-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="65836-110">Cette stratégie est abordée dans la section [Déploiement hébergé avec ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="65836-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="65836-111">L’application Blazor est placée sur un service ou un serveur web d’hébergement statique, où .NET n’est pas utilisé pour servir l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="65836-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="65836-112">Cette stratégie est traitée dans la section [Déploiement autonome](#standalone-deployment) , qui comprend des informations sur l’hébergement d’une application de webassembly éblouissante en tant que sous-application IIS.</span><span class="sxs-lookup"><span data-stu-id="65836-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="65836-113">Réécriture d’URL pour un routage correct</span><span class="sxs-lookup"><span data-stu-id="65836-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="65836-114">Le routage des requêtes pour les composants de page dans une application de webassembly éblouissante n’est pas aussi simple que le routage des demandes dans un serveur éblouissant, une application hébergée.</span><span class="sxs-lookup"><span data-stu-id="65836-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="65836-115">Prenons l’exemple d’une application de webassembly éblouissant avec deux composants :</span><span class="sxs-lookup"><span data-stu-id="65836-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="65836-116">*Main.razor* &ndash; Se charge à la racine de l’application et contient un lien vers le composant `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="65836-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="65836-117">Composant *About.Razor* &ndash; `About`.</span><span class="sxs-lookup"><span data-stu-id="65836-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="65836-118">Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :</span><span class="sxs-lookup"><span data-stu-id="65836-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="65836-119">Le navigateur effectue une requête.</span><span class="sxs-lookup"><span data-stu-id="65836-119">The browser makes a request.</span></span>
1. <span data-ttu-id="65836-120">La page par défaut est retournée, généralement *index.html*.</span><span class="sxs-lookup"><span data-stu-id="65836-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="65836-121">*index.HTML* amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="65836-121">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="65836-122">Le routeur de Blazor se charge et le composant `Main` Razor est rendu.</span><span class="sxs-lookup"><span data-stu-id="65836-122">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="65836-123">Dans la page principale, la sélection du composant `About` fonctionne sur le client, car le routeur Blazor empêche le navigateur d’effectuer une requête sur Internet à `www.contoso.com` pour `About` et fournit lui-même le composant `About` rendu.</span><span class="sxs-lookup"><span data-stu-id="65836-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="65836-124">Toutes les requêtes pour les points de terminaison internes de *l’application de Webassembly éblouissant* fonctionnent de la même façon : Les requêtes ne déclenchent pas de requêtes basées sur le navigateur pour des ressources hébergées par le serveur sur Internet.</span><span class="sxs-lookup"><span data-stu-id="65836-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="65836-125">Le routeur gère les requêtes en interne.</span><span class="sxs-lookup"><span data-stu-id="65836-125">The router handles the requests internally.</span></span>

<span data-ttu-id="65836-126">Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue.</span><span class="sxs-lookup"><span data-stu-id="65836-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="65836-127">Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 – Non trouvé* est retournée.</span><span class="sxs-lookup"><span data-stu-id="65836-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="65836-128">Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="65836-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="65836-129">Quand *index. html* est retourné, le routeur éblouissant de l’application prend le relais et répond avec la ressource correcte.</span><span class="sxs-lookup"><span data-stu-id="65836-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="65836-130">Lors du déploiement sur un serveur IIS, vous pouvez utiliser le module de réécriture d’URL avec le fichier *Web. config* publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="65836-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="65836-131">Pour plus d’informations, consultez la section [IIS](#iis) .</span><span class="sxs-lookup"><span data-stu-id="65836-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="65836-132">Déploiement hébergé avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65836-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="65836-133">Un *déploiement hébergé* sert l’application de webassembly éblouissant à des navigateurs à partir d’une [application ASP.net Core](xref:index) qui s’exécute sur un serveur Web.</span><span class="sxs-lookup"><span data-stu-id="65836-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="65836-134">L’application Blazor est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="65836-134">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="65836-135">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="65836-135">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="65836-136">Pour un déploiement hébergé, Visual Studio inclut le modèle de projet **Application Blazor WebAssembly** (modèle `blazorwasm` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)) avec l’option **Hébergé** sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="65836-136">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="65836-137">Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="65836-137">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="65836-138">Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="65836-138">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="65836-139">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="65836-139">Standalone deployment</span></span>

<span data-ttu-id="65836-140">Un *Déploiement autonome* sert l’application de webassembly éblouissant sous la forme d’un ensemble de fichiers statiques demandés directement par les clients.</span><span class="sxs-lookup"><span data-stu-id="65836-140">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="65836-141">N’importe quel serveur de fichiers statiques est capable de servir l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="65836-141">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="65836-142">Les ressources d’un déploiement autonome sont publiées dans le dossier *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist*.</span><span class="sxs-lookup"><span data-stu-id="65836-142">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="65836-143">IIS</span><span class="sxs-lookup"><span data-stu-id="65836-143">IIS</span></span>

<span data-ttu-id="65836-144">IIS est un serveur de fichiers statiques compatible avec les applications Blazor.</span><span class="sxs-lookup"><span data-stu-id="65836-144">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="65836-145">Pour configurer IIS afin d’héberger Blazor, consultez [Générer un site web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="65836-145">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="65836-146">Les ressources publiées sont créées dans le dossier */bin/Release/{FRAMEWORK CIBLE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="65836-146">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="65836-147">Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="65836-147">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="65836-148">web.config</span><span class="sxs-lookup"><span data-stu-id="65836-148">web.config</span></span>

<span data-ttu-id="65836-149">Quand un projet Blazor est publié, un fichier *web.config* est créé avec la configuration IIS suivante :</span><span class="sxs-lookup"><span data-stu-id="65836-149">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="65836-150">Les types MIME sont définis pour les extensions de fichiers suivantes :</span><span class="sxs-lookup"><span data-stu-id="65836-150">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="65836-151">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="65836-151">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="65836-152">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="65836-152">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="65836-153">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="65836-153">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="65836-154">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="65836-154">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="65836-155">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="65836-155">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="65836-156">La compression HTTP est activée pour les types MIME suivants :</span><span class="sxs-lookup"><span data-stu-id="65836-156">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="65836-157">Des règles du module de réécriture d’URL sont établies :</span><span class="sxs-lookup"><span data-stu-id="65836-157">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="65836-158">Servir le sous-répertoire où résident les ressources statiques de l’application ( *{NOM ASSEMBLY}/dist/{CHEMIN DEMANDÉ}* ).</span><span class="sxs-lookup"><span data-stu-id="65836-158">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="65836-159">Créer un routage de secours SPA afin que les demandes de ressources autres que des fichiers soient redirigées vers le document par défaut de l’application dans son dossier de ressources statiques ( *{NOM ASSEMBLY}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="65836-159">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="65836-160">Installer le module de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="65836-160">Install the URL Rewrite Module</span></span>

<span data-ttu-id="65836-161">Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL.</span><span class="sxs-lookup"><span data-stu-id="65836-161">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="65836-162">Il n’est pas installé par défaut, et ne peut pas l’être en tant que fonctionnalité de service de rôle Serveur Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="65836-162">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="65836-163">Vous devez le télécharger à partir du site web IIS.</span><span class="sxs-lookup"><span data-stu-id="65836-163">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="65836-164">Utilisez Web Platform Installer pour installer le module :</span><span class="sxs-lookup"><span data-stu-id="65836-164">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="65836-165">Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="65836-165">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="65836-166">Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI.</span><span class="sxs-lookup"><span data-stu-id="65836-166">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="65836-167">Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="65836-167">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="65836-168">Copiez le programme d’installation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="65836-168">Copy the installer to the server.</span></span> <span data-ttu-id="65836-169">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="65836-169">Run the installer.</span></span> <span data-ttu-id="65836-170">Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="65836-170">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="65836-171">Il n’est pas nécessaire de redémarrer le serveur après l’installation.</span><span class="sxs-lookup"><span data-stu-id="65836-171">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="65836-172">Configurer le site web</span><span class="sxs-lookup"><span data-stu-id="65836-172">Configure the website</span></span>

<span data-ttu-id="65836-173">Affectez le dossier de l’application comme **chemin physique** du site web.</span><span class="sxs-lookup"><span data-stu-id="65836-173">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="65836-174">Le dossier contient :</span><span class="sxs-lookup"><span data-stu-id="65836-174">The folder contains:</span></span>

* <span data-ttu-id="65836-175">Le fichier *web.config* utilisé par IIS pour configurer le site web, notamment les règles de redirection nécessaires et les types de contenu de fichiers</span><span class="sxs-lookup"><span data-stu-id="65836-175">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="65836-176">Le dossier de ressources statiques de l’application</span><span class="sxs-lookup"><span data-stu-id="65836-176">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="65836-177">Héberger en tant que sous-application IIS</span><span class="sxs-lookup"><span data-stu-id="65836-177">Host as an IIS sub-app</span></span>

<span data-ttu-id="65836-178">Si une application autonome est hébergée en tant que sous-application IIS, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="65836-178">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="65836-179">Désactivez le gestionnaire de module ASP.NET Core hérité.</span><span class="sxs-lookup"><span data-stu-id="65836-179">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="65836-180">Supprimez le gestionnaire dans le fichier *Web. config* publié de l’application éblouissant en ajoutant `<handlers>` une section au fichier :</span><span class="sxs-lookup"><span data-stu-id="65836-180">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="65836-181">Désactivez l’héritage de la section de l' `<system.webServer>` application racine (parent) `inheritInChildApplications` à l' `false`aide d’un `<location>` élément dont la valeur est :</span><span class="sxs-lookup"><span data-stu-id="65836-181">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="65836-182">La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de [la configuration du chemin d’accès de base de l’application](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="65836-182">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="65836-183">Dans le fichier *index.html* de l’application, définissez le chemin de base de l’application sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.</span><span class="sxs-lookup"><span data-stu-id="65836-183">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="65836-184">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="65836-184">Troubleshooting</span></span>

<span data-ttu-id="65836-185">Si vous recevez un message *500 – Erreur interne du serveur* et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé.</span><span class="sxs-lookup"><span data-stu-id="65836-185">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="65836-186">Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS.</span><span class="sxs-lookup"><span data-stu-id="65836-186">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="65836-187">Cela empêche le Gestionnaire IIS de charger la configuration du site web et empêche le site web de fournir les fichiers statiques Blazor.</span><span class="sxs-lookup"><span data-stu-id="65836-187">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="65836-188">Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="65836-188">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="65836-189">Stockage Azure</span><span class="sxs-lookup"><span data-stu-id="65836-189">Azure Storage</span></span>

<span data-ttu-id="65836-190">L’hébergement de fichiers statiques [Azure Storage](/azure/storage/) permet l’hébergement d’applications éblouissantes sans serveur.</span><span class="sxs-lookup"><span data-stu-id="65836-190">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="65836-191">Les noms de domaine personnalisé, le réseau de distribution de contenu Azure (CDN) et HTTPS sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="65836-191">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="65836-192">Lorsque le service blob est activé pour l’hébergement de site Web statique sur un compte de stockage :</span><span class="sxs-lookup"><span data-stu-id="65836-192">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="65836-193">Définissez le **nom du document d’index** sur `index.html`.</span><span class="sxs-lookup"><span data-stu-id="65836-193">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="65836-194">Définissez le **chemin d’accès au document d’erreur** sur `index.html`.</span><span class="sxs-lookup"><span data-stu-id="65836-194">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="65836-195">Les composants de Razor et autres points de terminaison non-fichier ne se trouvent sur les chemins d’accès physiques dans le contenu statique stocké par le service blob.</span><span class="sxs-lookup"><span data-stu-id="65836-195">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="65836-196">Lors de la réception d’une requête pour l’une de ces ressources que le routeur Blazor doit gérer, l’erreur *404 - introuvable* générée par le service blob achemine la requête vers le **chemin d’accès au document d’erreur**.</span><span class="sxs-lookup"><span data-stu-id="65836-196">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="65836-197">Le blob *index.html* est retourné et le routeur Blazor charge et traite le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="65836-197">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="65836-198">Pour plus d’informations, consultez [Hébergement de site Web statique dans le stockage Azure](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="65836-198">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="65836-199">Nginx</span><span class="sxs-lookup"><span data-stu-id="65836-199">Nginx</span></span>

<span data-ttu-id="65836-200">Le fichier *nginx.conf* suivant est simplifié afin de montrer comment configurer Nginx pour envoyer le fichier *index.html* chaque fois qu’il ne trouve aucun fichier correspondant sur le disque.</span><span class="sxs-lookup"><span data-stu-id="65836-200">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="65836-201">Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).</span><span class="sxs-lookup"><span data-stu-id="65836-201">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="65836-202">Nginx dans Docker</span><span class="sxs-lookup"><span data-stu-id="65836-202">Nginx in Docker</span></span>

<span data-ttu-id="65836-203">Pour héberger Blazor dans Docker à l’aide de Nginx, configurez le fichier Dockerfile pour utiliser l’image Nginx basée sur Alpine.</span><span class="sxs-lookup"><span data-stu-id="65836-203">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="65836-204">Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="65836-204">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="65836-205">Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="65836-205">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="65836-206">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="65836-206">GitHub Pages</span></span>

<span data-ttu-id="65836-207">Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="65836-207">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="65836-208">Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="65836-208">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="65836-209">Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="65836-209">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="65836-210">Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*.</span><span class="sxs-lookup"><span data-stu-id="65836-210">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="65836-211">Définissez la valeur d’attribut `href` sur le nom du dépôt GitHub avec une barre oblique (par exemple, `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="65836-211">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="65836-212">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="65836-212">Host configuration values</span></span>

<span data-ttu-id="65836-213">Les [applications Webassembly éblouissantes](xref:blazor/hosting-models#blazor-webassembly) peuvent accepter les valeurs de configuration d’hôte suivantes comme arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="65836-213">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="65836-214">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="65836-214">Content Root</span></span>

<span data-ttu-id="65836-215">L’argument `--contentroot` définit le chemin absolu du répertoire qui contient les fichiers de contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="65836-215">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="65836-216">Dans les exemples suivants, `/content-root-path` est le chemin racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="65836-216">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="65836-217">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="65836-217">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="65836-218">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="65836-218">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="65836-219">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="65836-219">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="65836-220">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="65836-220">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="65836-221">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="65836-221">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="65836-222">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="65836-222">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="65836-223">Base du chemin</span><span class="sxs-lookup"><span data-stu-id="65836-223">Path base</span></span>

<span data-ttu-id="65836-224">L' `--pathbase` argument définit le chemin d’accès de base d’application pour une application exécutée localement avec un chemin d’URL relatif `href` non racine (la balise est `/` définie sur un chemin d’accès autre que pour la mise en lots et la `<base>` production).</span><span class="sxs-lookup"><span data-stu-id="65836-224">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="65836-225">Dans les exemples suivants, `/relative-URL-path` est la base du chemin de l’application.</span><span class="sxs-lookup"><span data-stu-id="65836-225">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="65836-226">Pour plus d’informations, consultez chemin de la base de l' [application](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="65836-226">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65836-227">Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="65836-227">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="65836-228">Si vous spécifiez `<base href="/CoolApp/">` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="65836-228">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="65836-229">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="65836-229">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="65836-230">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="65836-230">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="65836-231">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="65836-231">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="65836-232">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="65836-232">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="65836-233">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="65836-233">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="65836-234">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="65836-234">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="65836-235">URL</span><span class="sxs-lookup"><span data-stu-id="65836-235">URLs</span></span>

<span data-ttu-id="65836-236">L’argument `--urls` définit les adresses IP ou les adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les demandes.</span><span class="sxs-lookup"><span data-stu-id="65836-236">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="65836-237">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="65836-237">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="65836-238">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="65836-238">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="65836-239">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="65836-239">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="65836-240">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="65836-240">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="65836-241">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="65836-241">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="65836-242">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="65836-242">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="65836-243">Configurer l'éditeur de liens</span><span class="sxs-lookup"><span data-stu-id="65836-243">Configure the Linker</span></span>

<span data-ttu-id="65836-244">Blazor effectue la liaison de langage intermédiaire (IL) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie.</span><span class="sxs-lookup"><span data-stu-id="65836-244">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="65836-245">La liaison d’assembly peut être contrôlée pendant le processus de build.</span><span class="sxs-lookup"><span data-stu-id="65836-245">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="65836-246">Pour plus d'informations, consultez <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="65836-246">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
