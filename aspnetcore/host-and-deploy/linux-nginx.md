---
title: Héberger ASP.NET Core sur Linux avec Nginx
author: rick-anderson
description: Découvrez comment configurer Nginx comme proxy inverse sur Ubuntu 16.04 pour transférer le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 8d3c158b44c9f30e7c0746398306aa1c0fd9e15b
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912114"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="f3c14-103">Héberger ASP.NET Core sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="f3c14-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="f3c14-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="f3c14-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="f3c14-105">Ce guide explique comment configurer un environnement ASP.NET Core prêt pour la production sur un serveur Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="f3c14-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="f3c14-106">Ces instructions fonctionnent normalement avec les versions d’Ubuntu plus récentes, mais elles n’ont pas fait l’objet de tests avec ces versions.</span><span class="sxs-lookup"><span data-stu-id="f3c14-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="f3c14-107">Pour plus d’informations sur les autres distributions Linux prises en charge par ASP.NET Core, consultez [Prérequis pour .NET Core sur Linux](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="f3c14-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="f3c14-108">Pour Ubuntu 14.04, nous vous recommandons d’utiliser *supervisord* comme solution pour l’analyse du processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f3c14-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="f3c14-109">*systemd* n’est pas disponible sur Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="f3c14-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="f3c14-110">Pour obtenir des instructions pour Ubuntu 14.04, consultez la [version précédente de cette rubrique](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="f3c14-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="f3c14-111">Ce guide montre comment effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="f3c14-111">This guide:</span></span>

* <span data-ttu-id="f3c14-112">Placer une application ASP.NET Core existante derrière un serveur proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="f3c14-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="f3c14-113">Configurer le serveur proxy inverse pour transférer les requêtes au serveur web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f3c14-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="f3c14-114">S’assurer que l’application web s’exécute au démarrage en tant que démon.</span><span class="sxs-lookup"><span data-stu-id="f3c14-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="f3c14-115">Configurer un outil de gestion des processus pour aider à redémarrer l’application web.</span><span class="sxs-lookup"><span data-stu-id="f3c14-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3c14-116">Prérequis</span><span class="sxs-lookup"><span data-stu-id="f3c14-116">Prerequisites</span></span>

1. <span data-ttu-id="f3c14-117">Accédez à un serveur Ubuntu 16.04 avec un compte d’utilisateur standard disposant de privilèges sudo.</span><span class="sxs-lookup"><span data-stu-id="f3c14-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="f3c14-118">Installez le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f3c14-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="f3c14-119">Visitez la [page All Downloads de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="f3c14-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="f3c14-120">Dans la liste **Runtime**, sélectionnez le runtime le plus récent qui n’est pas une préversion.</span><span class="sxs-lookup"><span data-stu-id="f3c14-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="f3c14-121">Sélectionnez et suivez les instructions pour Ubuntu qui correspondent à la version Ubuntu du serveur.</span><span class="sxs-lookup"><span data-stu-id="f3c14-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="f3c14-122">Une application ASP.NET Core existante.</span><span class="sxs-lookup"><span data-stu-id="f3c14-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="f3c14-123">Publier et copier sur l’application</span><span class="sxs-lookup"><span data-stu-id="f3c14-123">Publish and copy over the app</span></span>

<span data-ttu-id="f3c14-124">Configurez l’application pour un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="f3c14-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="f3c14-125">Exécutez [dotnet publish](/dotnet/core/tools/dotnet-publish) à partir de l’environnement de développement pour empaqueter une application dans un répertoire (par exemple, *bin/Release/&lt;moniker_framework_target&gt;/publish*) exécutable sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="f3c14-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="f3c14-126">L’application peut également être publiée en tant que [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) si vous préférez ne pas gérer le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f3c14-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="f3c14-127">Copiez l’application ASP.NET Core sur le serveur à l’aide d’un outil qui s’intègre au flux de travail de l’organisation (par exemple, SCP ou SFTP).</span><span class="sxs-lookup"><span data-stu-id="f3c14-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="f3c14-128">Il est courant de placer les applications web sous le répertoire *var* (par exemple, *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="f3c14-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="f3c14-129">Dans un scénario de déploiement en production, un workflow d’intégration continue effectue le travail de publication de l’application et de copie des composants sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f3c14-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="f3c14-130">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="f3c14-130">Test the app:</span></span>

1. <span data-ttu-id="f3c14-131">À partir de la ligne de commande, exécutez l’application : `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="f3c14-132">Dans un navigateur, accédez à `http://<serveraddress>:<port>` pour vérifier que l’application fonctionne sur Linux localement.</span><span class="sxs-lookup"><span data-stu-id="f3c14-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="f3c14-133">Configurer un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="f3c14-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="f3c14-134">Un proxy inverse est une configuration courante pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="f3c14-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="f3c14-135">Un proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3c14-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="f3c14-136">Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures.</span><span class="sxs-lookup"><span data-stu-id="f3c14-136">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="f3c14-137">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="f3c14-137">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="f3c14-138">Utiliser un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="f3c14-138">Use a reverse proxy server</span></span>

<span data-ttu-id="f3c14-139">Kestrel est idéal pour servir du contenu dynamique à partir d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f3c14-139">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="f3c14-140">Toutefois, les fonctionnalités de service web ne sont pas aussi complètes que les serveurs tels qu’IIS, Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="f3c14-140">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="f3c14-141">Un serveur proxy inverse peut décharger du travail tel que le traitement du contenu statique, la mise en cache des requêtes, la compression des requêtes et l’arrêt SSL à partir du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3c14-141">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="f3c14-142">Un serveur proxy inverse peut résider sur un ordinateur dédié ou peut être déployé à côté d’un serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3c14-142">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="f3c14-143">Pour les besoins de ce guide, nous utilisons une seule instance de Nginx.</span><span class="sxs-lookup"><span data-stu-id="f3c14-143">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="f3c14-144">Elle s’exécute sur le même serveur, en plus du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3c14-144">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="f3c14-145">Selon les exigences, un paramétrage différent peut être choisi.</span><span class="sxs-lookup"><span data-stu-id="f3c14-145">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="f3c14-146">Les requêtes étant transférées par le proxy inverse, utilisez le [middleware (intergiciel) des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) à partir du package [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="f3c14-146">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="f3c14-147">Le middleware met à jour le `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="f3c14-147">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="f3c14-148">Tout composant qui dépend du schéma, tel que l’authentification, la génération de lien, les redirections et la géolocalisation, doit être placé après l’appel du middleware des en-têtes transférés.</span><span class="sxs-lookup"><span data-stu-id="f3c14-148">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="f3c14-149">En règle générale, le middleware des en-têtes transférés doit être exécuté avant les autres middlewares, à l’exception des middlewares de gestion des erreurs et de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="f3c14-149">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="f3c14-150">Cet ordre permet au middleware qui repose sur les informations des en-têtes transférés d’utiliser les valeurs d’en-tête pour le traitement.</span><span class="sxs-lookup"><span data-stu-id="f3c14-150">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f3c14-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f3c14-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f3c14-152">Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou un middleware de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="f3c14-152">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="f3c14-153">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="f3c14-153">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f3c14-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f3c14-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f3c14-155">Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) et [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou un middleware de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="f3c14-155">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="f3c14-156">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="f3c14-156">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="f3c14-157">Si aucune option [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) n’est spécifiée au middleware, les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-157">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="f3c14-158">Seuls les proxys en cours d’exécution sur localhost (127.0.0.1, [::1]) sont approuvés par défaut.</span><span class="sxs-lookup"><span data-stu-id="f3c14-158">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="f3c14-159">Si d’autres proxys ou réseaux approuvés au sein de l’organisation gèrent les requêtes entre Internet et le serveur web, ajoutez-les à la liste des <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> ou des <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> avec <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="f3c14-159">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="f3c14-160">L’exemple suivant ajoute un serveur proxy approuvé avec l’adresse IP 10.0.0.100 au middleware des en-têtes transférés `KnownProxies` dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="f3c14-160">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="f3c14-161">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="f3c14-161">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="f3c14-162">Installer Nginx</span><span class="sxs-lookup"><span data-stu-id="f3c14-162">Install Nginx</span></span>

<span data-ttu-id="f3c14-163">Utilisez `apt-get` pour installer Nginx.</span><span class="sxs-lookup"><span data-stu-id="f3c14-163">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="f3c14-164">Le programme d’installation crée un script d’initialisation *systemd* qui exécute Nginx en tant que démon au démarrage du système.</span><span class="sxs-lookup"><span data-stu-id="f3c14-164">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="f3c14-165">Suivez les instructions d’installation pour Ubuntu sur le site [Nginx : Les packages officiels Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="f3c14-165">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="f3c14-166">Si des modules Nginx facultatifs sont requis, il peut s’avérer nécessaire de configurer Nginx à partir de la source.</span><span class="sxs-lookup"><span data-stu-id="f3c14-166">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="f3c14-167">Comme il s’agit de l’installation initiale de Nginx, vous devez le démarrer explicitement en exécutant :</span><span class="sxs-lookup"><span data-stu-id="f3c14-167">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="f3c14-168">Vérifiez qu’un navigateur affiche la page d’accueil par défaut de Nginx.</span><span class="sxs-lookup"><span data-stu-id="f3c14-168">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="f3c14-169">La page d’accueil est accessible à l’adresse `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-169">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="f3c14-170">Configurer Nginx</span><span class="sxs-lookup"><span data-stu-id="f3c14-170">Configure Nginx</span></span>

<span data-ttu-id="f3c14-171">Pour configurer Nginx en tant que proxy inverse pour transférer les requêtes à votre application ASP.NET Core, modifiez le fichier */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="f3c14-171">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="f3c14-172">Ouvrez-le dans un éditeur de texte et remplacez le contenu par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f3c14-172">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="f3c14-173">Si aucun `server_name` ne correspond, Nginx utilise le serveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f3c14-173">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="f3c14-174">Si aucun serveur par défaut n’est défini, le premier serveur dans le fichier de configuration est le serveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="f3c14-174">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="f3c14-175">En guise de bonne pratique, ajoutez un serveur par défaut spécifique qui retourne un code d’état 444 dans votre fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="f3c14-175">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="f3c14-176">Voici un exemple de configuration de serveur par défaut :</span><span class="sxs-lookup"><span data-stu-id="f3c14-176">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="f3c14-177">Avec les fichier de configuration et le serveur par défaut précédents, Nginx accepte le trafic public sur le port 80 avec un en-tête d’hôte `example.com` ou `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-177">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="f3c14-178">Les requêtes qui ne correspondent pas à ces hôtes ne sont pas transférées à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f3c14-178">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="f3c14-179">Nginx transfère les requêtes correspondantes à Kestrel à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-179">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="f3c14-180">Consultez [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) (Comment nginx traite une requête) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="f3c14-180">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="f3c14-181">Pour changer le port/l’adresse IP Kestrel, consultez [Kestrel : configuration du point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="f3c14-181">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="f3c14-182">La spécification d’une [directive server_name](https://nginx.org/docs/http/server_names.html) incorrecte expose votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="f3c14-182">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="f3c14-183">Une liaison générique de sous-domaine (par exemple, `*.example.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="f3c14-183">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="f3c14-184">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="f3c14-184">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="f3c14-185">Une fois la configuration de Nginx établie, exécutez `sudo nginx -t` pour vérifier la syntaxe des fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="f3c14-185">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="f3c14-186">Si le test de fichier de configuration réussit, forcez Nginx à appliquer les modifications en exécutant `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-186">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="f3c14-187">Pour exécuter directement l’application sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="f3c14-187">To directly run the app on the server:</span></span>

1. <span data-ttu-id="f3c14-188">Accédez au répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="f3c14-188">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="f3c14-189">Exécutez l’application : `dotnet <app_assembly.dll>`, où `app_assembly.dll` est le nom de fichier d’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="f3c14-189">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="f3c14-190">Si l’application s’exécute sur le serveur, mais ne répond pas sur Internet, vérifiez que le port 80 est ouvert sur le pare-feu du serveur.</span><span class="sxs-lookup"><span data-stu-id="f3c14-190">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="f3c14-191">Si vous utilisez une machine virtuelle Azure Ubuntu, ajoutez une règle de groupe de sécurité réseau (NSG) qui autorise le trafic entrant sur le port 80.</span><span class="sxs-lookup"><span data-stu-id="f3c14-191">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="f3c14-192">Il est inutile d’activer une règle de trafic sortant sur le port 80, car le trafic sortant est accordé automatiquement quand la règle de trafic entrant est activée.</span><span class="sxs-lookup"><span data-stu-id="f3c14-192">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="f3c14-193">Quand vous avez terminé de tester l’application, arrêtez-la avec `Ctrl+C` depuis l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="f3c14-193">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="f3c14-194">Surveillance de l’application</span><span class="sxs-lookup"><span data-stu-id="f3c14-194">Monitoring the app</span></span>

<span data-ttu-id="f3c14-195">Le serveur est configuré pour transférer les requêtes faites à `http://<serveraddress>:80` à l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-195">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="f3c14-196">Toutefois, Nginx n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f3c14-196">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="f3c14-197">*systemd* peut être utilisé pour créer un fichier de service afin de démarrer et de surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="f3c14-197">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="f3c14-198">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="f3c14-198">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="f3c14-199">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="f3c14-199">Create the service file</span></span>

<span data-ttu-id="f3c14-200">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="f3c14-200">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="f3c14-201">Voici un exemple de fichier de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="f3c14-201">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="f3c14-202">Si l’utilisateur *www-data* n’est pas utilisé par la configuration, vous devez d’abord créer l’utilisateur défini ici et l’affecter en tant que propriétaire des fichiers.</span><span class="sxs-lookup"><span data-stu-id="f3c14-202">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="f3c14-203">Utilisez `TimeoutStopSec` pour configurer la durée d’attente de l’arrêt de l’application après la réception du signal d’interruption initial.</span><span class="sxs-lookup"><span data-stu-id="f3c14-203">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="f3c14-204">Si l’application ne s’arrête pas pendant cette période, le signal SIGKILL est émis pour mettre fin à l’application.</span><span class="sxs-lookup"><span data-stu-id="f3c14-204">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="f3c14-205">Indiquez la valeur en secondes sans unité (par exemple, `150`), une valeur d’intervalle de temps (par exemple, `2min 30s`) ou `infinity` pour désactiver le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="f3c14-205">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="f3c14-206">`TimeoutStopSec` prend la valeur par défaut de `DefaultTimeoutStopSec` dans le fichier de configuration du gestionnaire (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*,  *user.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="f3c14-206">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="f3c14-207">Le délai d’expiration par défaut pour la plupart des distributions est de 90 secondes.</span><span class="sxs-lookup"><span data-stu-id="f3c14-207">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="f3c14-208">Linux possède un système de fichiers respectant la casse.</span><span class="sxs-lookup"><span data-stu-id="f3c14-208">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="f3c14-209">Si vous définissez ASPNETCORE_ENVIRONMENT sur « Production », c’est le fichier de configuration*appsettings.Production.json* qui est recherché, pas le fichier *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="f3c14-209">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="f3c14-210">Certaines valeurs (par exemple, les chaînes de connexion SQL) doivent être placées dans une séquence d’échappement afin que les fournisseurs de configuration puissent lire les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="f3c14-210">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="f3c14-211">Utilisez la commande suivante pour générer une valeur correctement placée dans une séquence d’échappement en vue de son utilisation dans le fichier de configuration :</span><span class="sxs-lookup"><span data-stu-id="f3c14-211">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="f3c14-212">Enregistrez le fichier et activez le service.</span><span class="sxs-lookup"><span data-stu-id="f3c14-212">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="f3c14-213">Démarrez le service et vérifiez qu’il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f3c14-213">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="f3c14-214">Le proxy inverse étant configuré et Kestrel étant géré via systemd, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur la machine locale à l’adresse `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-214">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="f3c14-215">Elle est également accessible à partir d’un ordinateur distant, sauf en cas de blocage par un pare-feu.</span><span class="sxs-lookup"><span data-stu-id="f3c14-215">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="f3c14-216">Si l’on inspecte les en-têtes de réponse, on constate que l’en-tête `Server` indique que l’application ASP.NET Core est traitée par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f3c14-216">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="f3c14-217">Affichage des journaux</span><span class="sxs-lookup"><span data-stu-id="f3c14-217">Viewing logs</span></span>

<span data-ttu-id="f3c14-218">Puisque l’application web utilisant Kestrel est gérée à l’aide de `systemd`, tous les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="f3c14-218">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="f3c14-219">Toutefois, ce journal inclut toutes les entrées pour tous les services et les processus gérés par `systemd`.</span><span class="sxs-lookup"><span data-stu-id="f3c14-219">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="f3c14-220">Pour afficher les éléments propres à `kestrel-helloapp.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f3c14-220">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="f3c14-221">Si vous voulez appliquer un filtrage supplémentaire, des options chronologiques, comme `--since today`, `--until 1 hour ago` ou une combinaison de ces options, peuvent réduire la quantité d’entrées retournées.</span><span class="sxs-lookup"><span data-stu-id="f3c14-221">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="f3c14-222">Protection des données</span><span class="sxs-lookup"><span data-stu-id="f3c14-222">Data protection</span></span>

<span data-ttu-id="f3c14-223">La [pile de protection des données ASP.NET Core](xref:security/data-protection/index) est utilisée par plusieurs [intergiciels (middleware)](xref:fundamentals/middleware/index) ASP.NET Core, notamment le middleware d’authentification (par exemple, le middleware cookie) et les protections de la falsification de requête intersites (CSRF, Cross Site Request Forgery).</span><span class="sxs-lookup"><span data-stu-id="f3c14-223">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="f3c14-224">Même si les API de protection des données ne sont pas appelées par le code de l’utilisateur, la protection des données doit être configurée pour créer un [magasin de clés](xref:security/data-protection/implementation/key-management) de chiffrement persistantes.</span><span class="sxs-lookup"><span data-stu-id="f3c14-224">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="f3c14-225">Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="f3c14-225">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="f3c14-226">Si le Key Ring est stocké en mémoire, au redémarrage de l’application :</span><span class="sxs-lookup"><span data-stu-id="f3c14-226">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="f3c14-227">tous les jetons d’authentification basés sur des cookies sont invalidés</span><span class="sxs-lookup"><span data-stu-id="f3c14-227">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="f3c14-228">Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande.</span><span class="sxs-lookup"><span data-stu-id="f3c14-228">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="f3c14-229">toutes les données protégées par le Key Ring ne peuvent plus être déchiffrées.</span><span class="sxs-lookup"><span data-stu-id="f3c14-229">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="f3c14-230">Ceci peut inclure des [jetons CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) et des [cookies TempData ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="f3c14-230">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="f3c14-231">Pour configurer la protection des données de façon à conserver et chiffrer le porte-clés (Key Ring), consultez :</span><span class="sxs-lookup"><span data-stu-id="f3c14-231">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="f3c14-232">Sécurisation de l’application</span><span class="sxs-lookup"><span data-stu-id="f3c14-232">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="f3c14-233">Activer AppArmor</span><span class="sxs-lookup"><span data-stu-id="f3c14-233">Enable AppArmor</span></span>

<span data-ttu-id="f3c14-234">Linux Security Modules (LSM) est un framework qui fait partie du noyau Linux depuis Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="f3c14-234">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="f3c14-235">LSM prend en charge différentes implémentations de modules de sécurité.</span><span class="sxs-lookup"><span data-stu-id="f3c14-235">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="f3c14-236">[AppArmor](https://wiki.ubuntu.com/AppArmor) est un LSM qui implémente un système de contrôle d’accès obligatoire permettant de confiner le programme à un ensemble limité de ressources.</span><span class="sxs-lookup"><span data-stu-id="f3c14-236">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="f3c14-237">Vérifiez qu’AppArmor est activé et configuré correctement.</span><span class="sxs-lookup"><span data-stu-id="f3c14-237">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="f3c14-238">Configuration du pare-feu</span><span class="sxs-lookup"><span data-stu-id="f3c14-238">Configuring the firewall</span></span>

<span data-ttu-id="f3c14-239">Fermez tous les ports externes qui ne sont pas en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="f3c14-239">Close off all external ports that are not in use.</span></span> <span data-ttu-id="f3c14-240">Uncomplicated firewall (ufw) fournit une interface de ligne de commande pour `iptables` afin de configurer le pare-feu.</span><span class="sxs-lookup"><span data-stu-id="f3c14-240">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="f3c14-241">Un pare-feu mal configuré bloque l’accès à l’ensemble du système.</span><span class="sxs-lookup"><span data-stu-id="f3c14-241">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="f3c14-242">Faute d’avoir spécifié le port SSH approprié, vous ne pourrez pas accéder au système si vous utilisez SSH pour vous y connecter.</span><span class="sxs-lookup"><span data-stu-id="f3c14-242">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="f3c14-243">Le numéro de port par défaut est 22.</span><span class="sxs-lookup"><span data-stu-id="f3c14-243">The default port is 22.</span></span> <span data-ttu-id="f3c14-244">Pour plus d’informations, consultez la [présentation d’ufw](https://help.ubuntu.com/community/UFW) et le [manuel](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="f3c14-244">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="f3c14-245">Installez `ufw` et configurez-le de façon à autoriser le trafic sur les ports nécessaires.</span><span class="sxs-lookup"><span data-stu-id="f3c14-245">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a><span data-ttu-id="f3c14-246">Sécurisation de Nginx</span><span class="sxs-lookup"><span data-stu-id="f3c14-246">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="f3c14-247">Changer le nom de la réponse Nginx</span><span class="sxs-lookup"><span data-stu-id="f3c14-247">Change the Nginx response name</span></span>

<span data-ttu-id="f3c14-248">Modifiez *src/http/ngx_http_header_filter_module.c* :</span><span class="sxs-lookup"><span data-stu-id="f3c14-248">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="f3c14-249">Configurer les options</span><span class="sxs-lookup"><span data-stu-id="f3c14-249">Configure options</span></span>

<span data-ttu-id="f3c14-250">Configurez le serveur avec les modules nécessaires supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="f3c14-250">Configure the server with additional required modules.</span></span> <span data-ttu-id="f3c14-251">Pour renforcer l’application, vous pouvez utiliser un pare-feu d’application web tel que [ModSecurity](https://www.modsecurity.org/).</span><span class="sxs-lookup"><span data-stu-id="f3c14-251">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="f3c14-252">Configurer le protocole SSL</span><span class="sxs-lookup"><span data-stu-id="f3c14-252">Configure SSL</span></span>

* <span data-ttu-id="f3c14-253">Configurez le serveur pour qu’il écoute le trafic HTTPS sur le port `443` en spécifiant un certificat valide émis par une autorité de certificat approuvée.</span><span class="sxs-lookup"><span data-stu-id="f3c14-253">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="f3c14-254">Renforcez la sécurité en appliquant certaines des pratiques mentionnées dans le fichier */etc/nginx/nginx.conf* suivant.</span><span class="sxs-lookup"><span data-stu-id="f3c14-254">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="f3c14-255">Vous pouvez par exemple choisir un chiffrement plus fort et rediriger tout le trafic sur HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f3c14-255">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="f3c14-256">L’ajout d’un en-tête `HTTP Strict-Transport-Security` (HSTS) garantit que toutes les requêtes ultérieures du client s’effectuent uniquement par le biais du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f3c14-256">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="f3c14-257">N’ajoutez pas l’en-tête Strict-Transport-Security ou choisissez une valeur `max-age` appropriée si le protocole SSL doit être désactivé ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="f3c14-257">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="f3c14-258">Ajoutez le fichier de configuration */etc/nginx/proxy.conf* :</span><span class="sxs-lookup"><span data-stu-id="f3c14-258">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="f3c14-259">Modifiez le fichier de configuration */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="f3c14-259">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="f3c14-260">Dans l’exemple, les sections `http` et `server` figurent dans un même fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="f3c14-260">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="f3c14-261">Sécuriser Nginx contre le détournement de clic</span><span class="sxs-lookup"><span data-stu-id="f3c14-261">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="f3c14-262">Le détournement de clic, ou clickjacking, est une technique malveillante visant à recueillir les clics d’un utilisateur infecté.</span><span class="sxs-lookup"><span data-stu-id="f3c14-262">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="f3c14-263">Elle pousse la victime (le visiteur) à cliquer sur un site infecté.</span><span class="sxs-lookup"><span data-stu-id="f3c14-263">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="f3c14-264">Utilisez X-FRAME-OPTIONS pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="f3c14-264">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="f3c14-265">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="f3c14-265">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="f3c14-266">Ajoutez la ligne `add_header X-Frame-Options "SAMEORIGIN";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="f3c14-266">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="f3c14-267">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="f3c14-267">MIME-type sniffing</span></span>

<span data-ttu-id="f3c14-268">Cet en-tête empêche la plupart des navigateurs de détourner le type MIME d’une réponse et de remplacer le type de contenu déclaré, car l’en-tête indique au navigateur qu’il ne doit pas substituer le type de contenu de la réponse.</span><span class="sxs-lookup"><span data-stu-id="f3c14-268">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="f3c14-269">Avec l’option `nosniff`, si le serveur indique que le contenu est « text/html », le navigateur le restitue en tant que « text/html ».</span><span class="sxs-lookup"><span data-stu-id="f3c14-269">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="f3c14-270">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="f3c14-270">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="f3c14-271">Ajoutez la ligne `add_header X-Content-Type-Options "nosniff";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="f3c14-271">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3c14-272">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f3c14-272">Additional resources</span></span>

* [<span data-ttu-id="f3c14-273">Prérequis pour .NET Core sur Linux</span><span class="sxs-lookup"><span data-stu-id="f3c14-273">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <span data-ttu-id="f3c14-274">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx : versions binaires : packages Debian/Ubuntu officiels).</span><span class="sxs-lookup"><span data-stu-id="f3c14-274">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* [<span data-ttu-id="f3c14-275">Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge</span><span class="sxs-lookup"><span data-stu-id="f3c14-275">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* <span data-ttu-id="f3c14-276">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (NGINX : utilisation de l’en-tête Forwarded)</span><span class="sxs-lookup"><span data-stu-id="f3c14-276">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)</span></span>
