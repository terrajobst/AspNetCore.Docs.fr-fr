---
title: Héberger et déployer ASP.NET Core Blazor webassembly
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor à l’aide de ASP.NET Core, de réseaux de distribution de contenu (CDN), de serveurs de fichiers et de pages GitHub.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: eae12b266e91a30a47daf63ac77ba082c25225aa
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664100"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="beefb-103">Héberger et déployer ASP.NET Core Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="beefb-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="beefb-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="beefb-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="beefb-105">Avec le [modèle d’hébergement WebassemblyBlazor](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="beefb-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="beefb-106">L’application Blazor, ses dépendances et le Runtime .NET sont téléchargés dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="beefb-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="beefb-107">L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.</span><span class="sxs-lookup"><span data-stu-id="beefb-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="beefb-108">Les stratégies de déploiement suivantes sont prises en charge :</span><span class="sxs-lookup"><span data-stu-id="beefb-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="beefb-109">L’application Blazor est fournie par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="beefb-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="beefb-110">Cette stratégie est abordée dans la section [Déploiement hébergé avec ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="beefb-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="beefb-111">L’application Blazor est placée sur un service ou un serveur Web d’hébergement statique, où .NET n’est pas utilisé pour traiter l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="beefb-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="beefb-112">Cette stratégie est traitée dans la section [Déploiement autonome](#standalone-deployment) , qui comprend des informations sur l’hébergement d’une application Blazor webassembly en tant que sous-application IIS.</span><span class="sxs-lookup"><span data-stu-id="beefb-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="beefb-113">Réécriture d’URL pour un routage correct</span><span class="sxs-lookup"><span data-stu-id="beefb-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="beefb-114">Les requêtes de routage pour les composants de page dans une application Blazor webassembly ne sont pas aussi simples que les demandes de routage dans un serveur de Blazor, une application hébergée.</span><span class="sxs-lookup"><span data-stu-id="beefb-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="beefb-115">Prenons l’exemple d’une application Blazor webassembly avec deux composants :</span><span class="sxs-lookup"><span data-stu-id="beefb-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="beefb-116">*Main. razor* &ndash; se charge à la racine de l’application et contient un lien vers le composant `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="beefb-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="beefb-117">*À propos de* la &ndash; du composant `About`. Razor.</span><span class="sxs-lookup"><span data-stu-id="beefb-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="beefb-118">Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :</span><span class="sxs-lookup"><span data-stu-id="beefb-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="beefb-119">Le navigateur effectue une requête.</span><span class="sxs-lookup"><span data-stu-id="beefb-119">The browser makes a request.</span></span>
1. <span data-ttu-id="beefb-120">La page par défaut est retournée, généralement *index.html*.</span><span class="sxs-lookup"><span data-stu-id="beefb-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="beefb-121">*index.HTML* amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="beefb-121">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="beefb-122">le routeur de Blazorest chargé et le composant de `Main` Razor est rendu.</span><span class="sxs-lookup"><span data-stu-id="beefb-122">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="beefb-123">Dans la page principale, le fait de sélectionner le lien vers le composant `About` fonctionne sur le client, car le routeur Blazor empêche le navigateur de faire une demande sur Internet pour `www.contoso.com` pour `About` et sert le composant `About` rendu lui-même.</span><span class="sxs-lookup"><span data-stu-id="beefb-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="beefb-124">Toutes les requêtes pour les points de terminaison internes *au sein de l’application Blazor Webassembly* fonctionnent de la même façon : les demandes ne déclenchent pas de demandes basées sur un navigateur vers des ressources hébergées sur le serveur sur Internet.</span><span class="sxs-lookup"><span data-stu-id="beefb-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="beefb-125">Le routeur gère les requêtes en interne.</span><span class="sxs-lookup"><span data-stu-id="beefb-125">The router handles the requests internally.</span></span>

<span data-ttu-id="beefb-126">Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue.</span><span class="sxs-lookup"><span data-stu-id="beefb-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="beefb-127">Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 – Non trouvé* est retournée.</span><span class="sxs-lookup"><span data-stu-id="beefb-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="beefb-128">Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="beefb-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="beefb-129">Quand *index. html* est retourné, le routeur de Blazor de l’application prend le relais et répond avec la ressource correcte.</span><span class="sxs-lookup"><span data-stu-id="beefb-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="beefb-130">Lors du déploiement sur un serveur IIS, vous pouvez utiliser le module de réécriture d’URL avec le fichier *Web. config* publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="beefb-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="beefb-131">Pour plus d’informations, consultez la section [IIS](#iis) .</span><span class="sxs-lookup"><span data-stu-id="beefb-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="beefb-132">Déploiement hébergé avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="beefb-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="beefb-133">Un *déploiement hébergé* sert l’application Blazor webassembly aux navigateurs à partir d’une [application ASP.net Core](xref:index) qui s’exécute sur un serveur Web.</span><span class="sxs-lookup"><span data-stu-id="beefb-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="beefb-134">L’application Blazor est incluse avec l’application ASP.NET Core dans la sortie publiée afin que les deux applications soient déployées ensemble.</span><span class="sxs-lookup"><span data-stu-id="beefb-134">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="beefb-135">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="beefb-135">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="beefb-136">Pour un déploiement hébergé, Visual Studio comprend le modèle de projet d' **applicationBlazor Webassembly** (`blazorwasm` modèle lors de l’utilisation de la commande [dotnet New](/dotnet/core/tools/dotnet-new) ) avec l’option **hébergée** sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="beefb-136">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected.</span></span>

<span data-ttu-id="beefb-137">Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="beefb-137">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="beefb-138">Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="beefb-138">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="beefb-139">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="beefb-139">Standalone deployment</span></span>

<span data-ttu-id="beefb-140">Un *Déploiement autonome* sert l’application Blazor webassembly sous la forme d’un ensemble de fichiers statiques demandés directement par les clients.</span><span class="sxs-lookup"><span data-stu-id="beefb-140">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="beefb-141">Tout serveur de fichiers statique peut traiter l’application Blazor.</span><span class="sxs-lookup"><span data-stu-id="beefb-141">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="beefb-142">Les ressources d’un déploiement autonome sont publiées dans le dossier *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist*.</span><span class="sxs-lookup"><span data-stu-id="beefb-142">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="beefb-143">IIS</span><span class="sxs-lookup"><span data-stu-id="beefb-143">IIS</span></span>

<span data-ttu-id="beefb-144">IIS est un serveur de fichiers statiques permettant de Blazor des applications.</span><span class="sxs-lookup"><span data-stu-id="beefb-144">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="beefb-145">Pour configurer IIS pour héberger des Blazor, consultez [créer un site Web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="beefb-145">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="beefb-146">Les ressources publiées sont créées dans le dossier */bin/Release/{FRAMEWORK CIBLE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="beefb-146">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="beefb-147">Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="beefb-147">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="beefb-148">web.config</span><span class="sxs-lookup"><span data-stu-id="beefb-148">web.config</span></span>

<span data-ttu-id="beefb-149">Quand un projet de Blazor est publié, un fichier *Web. config* est créé avec la configuration IIS suivante :</span><span class="sxs-lookup"><span data-stu-id="beefb-149">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="beefb-150">Les types MIME sont définis pour les extensions de fichiers suivantes :</span><span class="sxs-lookup"><span data-stu-id="beefb-150">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="beefb-151">*. dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="beefb-151">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="beefb-152">&ndash; *. json* `application/json`</span><span class="sxs-lookup"><span data-stu-id="beefb-152">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="beefb-153">*wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="beefb-153">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="beefb-154">*woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="beefb-154">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="beefb-155">*woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="beefb-155">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="beefb-156">La compression HTTP est activée pour les types MIME suivants :</span><span class="sxs-lookup"><span data-stu-id="beefb-156">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="beefb-157">Des règles du module de réécriture d’URL sont établies :</span><span class="sxs-lookup"><span data-stu-id="beefb-157">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="beefb-158">Servir le sous-répertoire où résident les ressources statiques de l’application ( *{NOM ASSEMBLY}/dist/{CHEMIN DEMANDÉ}* ).</span><span class="sxs-lookup"><span data-stu-id="beefb-158">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="beefb-159">Créer un routage de secours SPA afin que les demandes de ressources autres que des fichiers soient redirigées vers le document par défaut de l’application dans son dossier de ressources statiques ( *{NOM ASSEMBLY}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="beefb-159">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="beefb-160">Installer le module de réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="beefb-160">Install the URL Rewrite Module</span></span>

<span data-ttu-id="beefb-161">Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL.</span><span class="sxs-lookup"><span data-stu-id="beefb-161">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="beefb-162">Il n’est pas installé par défaut, et ne peut pas l’être en tant que fonctionnalité de service de rôle Serveur Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="beefb-162">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="beefb-163">Vous devez le télécharger à partir du site web IIS.</span><span class="sxs-lookup"><span data-stu-id="beefb-163">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="beefb-164">Utilisez Web Platform Installer pour installer le module :</span><span class="sxs-lookup"><span data-stu-id="beefb-164">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="beefb-165">Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="beefb-165">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="beefb-166">Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI.</span><span class="sxs-lookup"><span data-stu-id="beefb-166">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="beefb-167">Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="beefb-167">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="beefb-168">Copiez le programme d’installation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="beefb-168">Copy the installer to the server.</span></span> <span data-ttu-id="beefb-169">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="beefb-169">Run the installer.</span></span> <span data-ttu-id="beefb-170">Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="beefb-170">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="beefb-171">Il n’est pas nécessaire de redémarrer le serveur après l’installation.</span><span class="sxs-lookup"><span data-stu-id="beefb-171">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="beefb-172">Configurer le site web</span><span class="sxs-lookup"><span data-stu-id="beefb-172">Configure the website</span></span>

<span data-ttu-id="beefb-173">Affectez le dossier de l’application comme **chemin physique** du site web.</span><span class="sxs-lookup"><span data-stu-id="beefb-173">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="beefb-174">Le dossier contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="beefb-174">The folder contains:</span></span>

* <span data-ttu-id="beefb-175">Le fichier *web.config* utilisé par IIS pour configurer le site web, notamment les règles de redirection nécessaires et les types de contenu de fichiers</span><span class="sxs-lookup"><span data-stu-id="beefb-175">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="beefb-176">Le dossier de ressources statiques de l’application</span><span class="sxs-lookup"><span data-stu-id="beefb-176">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="beefb-177">Héberger en tant que sous-application IIS</span><span class="sxs-lookup"><span data-stu-id="beefb-177">Host as an IIS sub-app</span></span>

<span data-ttu-id="beefb-178">Si une application autonome est hébergée en tant que sous-application IIS, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="beefb-178">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="beefb-179">Désactivez le gestionnaire de module ASP.NET Core hérité.</span><span class="sxs-lookup"><span data-stu-id="beefb-179">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="beefb-180">Supprimez le gestionnaire dans le fichier *Web. config* publié de Blazor application en ajoutant une section `<handlers>` au fichier :</span><span class="sxs-lookup"><span data-stu-id="beefb-180">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="beefb-181">Désactivez l’héritage de la section `<system.webServer>` de l’application racine (parent) à l’aide d’un élément `<location>` avec `inheritInChildApplications` défini sur `false`:</span><span class="sxs-lookup"><span data-stu-id="beefb-181">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="beefb-182">La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de [la configuration du chemin d’accès de base de l’application](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="beefb-182">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="beefb-183">Dans le fichier *index.html* de l’application, définissez le chemin de base de l’application sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.</span><span class="sxs-lookup"><span data-stu-id="beefb-183">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="beefb-184">Dépannage</span><span class="sxs-lookup"><span data-stu-id="beefb-184">Troubleshooting</span></span>

<span data-ttu-id="beefb-185">Si vous recevez un message *500 – Erreur interne du serveur* et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé.</span><span class="sxs-lookup"><span data-stu-id="beefb-185">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="beefb-186">Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS.</span><span class="sxs-lookup"><span data-stu-id="beefb-186">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="beefb-187">Cela empêche le gestionnaire des services Internet de charger la configuration du site Web et le site Web de servir les fichiers statiques de Blazor.</span><span class="sxs-lookup"><span data-stu-id="beefb-187">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="beefb-188">Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="beefb-188">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="beefb-189">Stockage Azure</span><span class="sxs-lookup"><span data-stu-id="beefb-189">Azure Storage</span></span>

<span data-ttu-id="beefb-190">L’hébergement de fichiers statiques [Azure Storage](/azure/storage/) permet l’hébergement d’applications Blazor sans serveur.</span><span class="sxs-lookup"><span data-stu-id="beefb-190">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="beefb-191">Les noms de domaine personnalisé, le réseau de distribution de contenu Azure (CDN) et HTTPS sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="beefb-191">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="beefb-192">Lorsque le service blob est activé pour l’hébergement de site Web statique sur un compte de stockage :</span><span class="sxs-lookup"><span data-stu-id="beefb-192">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="beefb-193">Définissez le **nom du document d’index** sur `index.html`.</span><span class="sxs-lookup"><span data-stu-id="beefb-193">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="beefb-194">Définissez le **chemin d’accès au document d’erreur** sur `index.html`.</span><span class="sxs-lookup"><span data-stu-id="beefb-194">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="beefb-195">Les composants de Razor et autres points de terminaison non-fichier ne se trouvent sur les chemins d’accès physiques dans le contenu statique stocké par le service blob.</span><span class="sxs-lookup"><span data-stu-id="beefb-195">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="beefb-196">Lorsqu’une demande pour l’une de ces ressources est reçue que le routeur Blazor doit gérer, l’erreur *404-introuvable* générée par le service BLOB achemine la requête vers le **chemin d’accès du document d’erreur**.</span><span class="sxs-lookup"><span data-stu-id="beefb-196">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="beefb-197">L’objet BLOB *index. html* est retourné et le routeur Blazor charge et traite le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="beefb-197">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="beefb-198">Pour plus d’informations, consultez [Hébergement de sites web statiques dans le service Stockage Azure](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="beefb-198">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="beefb-199">Nginx</span><span class="sxs-lookup"><span data-stu-id="beefb-199">Nginx</span></span>

<span data-ttu-id="beefb-200">Le fichier *nginx.conf* suivant est simplifié afin de montrer comment configurer Nginx pour envoyer le fichier *index.html* chaque fois qu’il ne trouve aucun fichier correspondant sur le disque.</span><span class="sxs-lookup"><span data-stu-id="beefb-200">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="beefb-201">Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).</span><span class="sxs-lookup"><span data-stu-id="beefb-201">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="beefb-202">Nginx dans Docker</span><span class="sxs-lookup"><span data-stu-id="beefb-202">Nginx in Docker</span></span>

<span data-ttu-id="beefb-203">Pour héberger des Blazor dans l’Ancreur à l’aide de nginx, configurez le fichier dockerfile pour utiliser l’image Nginx alpine.</span><span class="sxs-lookup"><span data-stu-id="beefb-203">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="beefb-204">Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="beefb-204">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="beefb-205">Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="beefb-205">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="beefb-206">Apache</span><span class="sxs-lookup"><span data-stu-id="beefb-206">Apache</span></span>

<span data-ttu-id="beefb-207">Pour déployer une application Blazor webassembly sur CentOS 7 ou version ultérieure :</span><span class="sxs-lookup"><span data-stu-id="beefb-207">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="beefb-208">Créez le fichier de configuration Apache.</span><span class="sxs-lookup"><span data-stu-id="beefb-208">Create the Apache configuration file.</span></span> <span data-ttu-id="beefb-209">L’exemple suivant est un fichier de configuration simplifié (*blazorapp. config*) :</span><span class="sxs-lookup"><span data-stu-id="beefb-209">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. <span data-ttu-id="beefb-210">Placez le fichier de configuration Apache dans le répertoire `/etc/httpd/conf.d/`, qui est le répertoire de configuration Apache par défaut dans CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="beefb-210">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="beefb-211">Placez les fichiers de l’application dans le répertoire `/var/www/blazorapp` (l’emplacement spécifié pour `DocumentRoot` dans le fichier de configuration).</span><span class="sxs-lookup"><span data-stu-id="beefb-211">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="beefb-212">Redémarrez le service Apache.</span><span class="sxs-lookup"><span data-stu-id="beefb-212">Restart the Apache service.</span></span>

<span data-ttu-id="beefb-213">Pour plus d’informations, consultez [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) et [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span><span class="sxs-lookup"><span data-stu-id="beefb-213">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="beefb-214">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="beefb-214">GitHub Pages</span></span>

<span data-ttu-id="beefb-215">Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*.</span><span class="sxs-lookup"><span data-stu-id="beefb-215">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="beefb-216">Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="beefb-216">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="beefb-217">Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="beefb-217">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="beefb-218">Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*.</span><span class="sxs-lookup"><span data-stu-id="beefb-218">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="beefb-219">Définissez la valeur d’attribut `href` sur le nom du dépôt GitHub avec une barre oblique (par exemple, `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="beefb-219">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="beefb-220">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="beefb-220">Host configuration values</span></span>

<span data-ttu-id="beefb-221">[Blazor applications Webassembly](xref:blazor/hosting-models#blazor-webassembly) peuvent accepter les valeurs de configuration d’hôte suivantes comme arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="beefb-221">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="beefb-222">Racine de contenu</span><span class="sxs-lookup"><span data-stu-id="beefb-222">Content root</span></span>

<span data-ttu-id="beefb-223">L’argument `--contentroot` définit le chemin d’accès absolu au répertoire qui contient les fichiers de contenu de l’application ([racine du contenu](xref:fundamentals/index#content-root)).</span><span class="sxs-lookup"><span data-stu-id="beefb-223">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="beefb-224">Dans les exemples suivants, `/content-root-path` est le chemin racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="beefb-224">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="beefb-225">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="beefb-225">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="beefb-226">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="beefb-226">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="beefb-227">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="beefb-227">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="beefb-228">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="beefb-228">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="beefb-229">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="beefb-229">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="beefb-230">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="beefb-230">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="beefb-231">Base du chemin</span><span class="sxs-lookup"><span data-stu-id="beefb-231">Path base</span></span>

<span data-ttu-id="beefb-232">L’argument `--pathbase` définit le chemin d’accès de base d’application pour une application exécutée localement avec un chemin d’URL relatif non racine (la balise `<base>` `href` est définie sur un chemin d’accès autre que `/` pour la mise en lots et la production).</span><span class="sxs-lookup"><span data-stu-id="beefb-232">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="beefb-233">Dans les exemples suivants, `/relative-URL-path` est la base du chemin de l’application.</span><span class="sxs-lookup"><span data-stu-id="beefb-233">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="beefb-234">Pour plus d’informations, consultez chemin de la base de l' [application](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="beefb-234">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="beefb-235">Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="beefb-235">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="beefb-236">Si vous spécifiez `<base>` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base href="/CoolApp/">`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="beefb-236">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="beefb-237">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="beefb-237">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="beefb-238">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="beefb-238">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="beefb-239">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="beefb-239">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="beefb-240">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="beefb-240">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="beefb-241">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="beefb-241">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="beefb-242">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="beefb-242">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="beefb-243">URLs</span><span class="sxs-lookup"><span data-stu-id="beefb-243">URLs</span></span>

<span data-ttu-id="beefb-244">L’argument `--urls` définit les adresses IP ou les adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les demandes.</span><span class="sxs-lookup"><span data-stu-id="beefb-244">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="beefb-245">Passez l’argument lors de l’exécution de l’application localement à une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="beefb-245">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="beefb-246">À partir du répertoire de l’application, exécutez :</span><span class="sxs-lookup"><span data-stu-id="beefb-246">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="beefb-247">Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="beefb-247">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="beefb-248">Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="beefb-248">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="beefb-249">Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**.</span><span class="sxs-lookup"><span data-stu-id="beefb-249">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="beefb-250">Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="beefb-250">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="beefb-251">Configurer l'éditeur de liens</span><span class="sxs-lookup"><span data-stu-id="beefb-251">Configure the Linker</span></span>

Blazor<span data-ttu-id="beefb-252"> effectue une liaison IL (Intermediate Language) sur chaque Build pour supprimer l’IL inutile des assemblys de sortie.</span><span class="sxs-lookup"><span data-stu-id="beefb-252"> performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="beefb-253">La liaison d’assembly peut être contrôlée pendant le processus de build.</span><span class="sxs-lookup"><span data-stu-id="beefb-253">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="beefb-254">Pour plus d’informations, consultez <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="beefb-254">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
