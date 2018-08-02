---
title: Héberger ASP.NET Core sur Linux avec Nginx
author: rick-anderson
description: Découvrez comment configurer Nginx comme proxy inverse sur Ubuntu 16.04 pour transférer le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: aba9ed41ac3650d8c645d71fb772e2a8e4f32f02
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254855"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="3ff35-103">Héberger ASP.NET Core sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="3ff35-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="3ff35-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="3ff35-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="3ff35-105">Ce guide explique comment configurer un environnement ASP.NET Core prêt pour la production sur un serveur Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="3ff35-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="3ff35-106">Ces instructions fonctionnent normalement avec les versions d’Ubuntu plus récentes, mais elles n’ont pas fait l’objet de tests avec ces versions.</span><span class="sxs-lookup"><span data-stu-id="3ff35-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="3ff35-107">Pour Ubuntu 14.04, nous vous recommandons d’utiliser *supervisord* comme solution pour l’analyse du processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ff35-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="3ff35-108">*systemd* n’est pas disponible sur Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="3ff35-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="3ff35-109">Pour obtenir des instructions pour Ubuntu 14.04, consultez la [version précédente de cette rubrique](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="3ff35-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="3ff35-110">Ce guide montre comment effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="3ff35-110">This guide:</span></span>

* <span data-ttu-id="3ff35-111">Placer une application ASP.NET Core existante derrière un serveur proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="3ff35-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="3ff35-112">Configurer le serveur proxy inverse pour transférer les requêtes au serveur web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ff35-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="3ff35-113">S’assurer que l’application web s’exécute au démarrage en tant que démon.</span><span class="sxs-lookup"><span data-stu-id="3ff35-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="3ff35-114">Configurer un outil de gestion des processus pour aider à redémarrer l’application web.</span><span class="sxs-lookup"><span data-stu-id="3ff35-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ff35-115">Prérequis</span><span class="sxs-lookup"><span data-stu-id="3ff35-115">Prerequisites</span></span>

1. <span data-ttu-id="3ff35-116">Accédez à un serveur Ubuntu 16.04 avec un compte d’utilisateur standard disposant de privilèges sudo.</span><span class="sxs-lookup"><span data-stu-id="3ff35-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="3ff35-117">Installez le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3ff35-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="3ff35-118">Visitez la [page All Downloads de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="3ff35-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="3ff35-119">Dans la liste **Runtime**, sélectionnez le runtime le plus récent qui n’est pas une préversion.</span><span class="sxs-lookup"><span data-stu-id="3ff35-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="3ff35-120">Sélectionnez et suivez les instructions pour Ubuntu qui correspondent à la version Ubuntu du serveur.</span><span class="sxs-lookup"><span data-stu-id="3ff35-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="3ff35-121">Une application ASP.NET Core existante.</span><span class="sxs-lookup"><span data-stu-id="3ff35-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="3ff35-122">Publier et copier sur l’application</span><span class="sxs-lookup"><span data-stu-id="3ff35-122">Publish and copy over the app</span></span>

<span data-ttu-id="3ff35-123">Configurez l’application pour un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="3ff35-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="3ff35-124">Exécutez [dotnet publish](/dotnet/core/tools/dotnet-publish) à partir de l’environnement de développement pour empaqueter une application dans un répertoire (par exemple, *bin/Release/&lt;moniker_framework_target&gt;/publish*) exécutable sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="3ff35-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="3ff35-125">L’application peut également être publiée en tant que [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) si vous préférez ne pas gérer le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3ff35-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="3ff35-126">Copiez l’application ASP.NET Core sur le serveur à l’aide d’un outil qui s’intègre au flux de travail de l’organisation (par exemple, SCP ou SFTP).</span><span class="sxs-lookup"><span data-stu-id="3ff35-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="3ff35-127">Il est courant de placer les applications web sous le répertoire *var* (par exemple, *var/aspnetcore/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="3ff35-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="3ff35-128">Dans un scénario de déploiement en production, un workflow d’intégration continue effectue le travail de publication de l’application et de copie des composants sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3ff35-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="3ff35-129">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="3ff35-129">Test the app:</span></span>

1. <span data-ttu-id="3ff35-130">À partir de la ligne de commande, exécutez l’application : `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="3ff35-131">Dans un navigateur, accédez à `http://<serveraddress>:<port>` pour vérifier que l’application fonctionne sur Linux localement.</span><span class="sxs-lookup"><span data-stu-id="3ff35-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="3ff35-132">Configurer un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="3ff35-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="3ff35-133">Un proxy inverse est une configuration courante pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="3ff35-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="3ff35-134">Un proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ff35-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="3ff35-135">Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures.</span><span class="sxs-lookup"><span data-stu-id="3ff35-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="3ff35-136">Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="3ff35-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="3ff35-137">Utiliser un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="3ff35-137">Use a reverse proxy server</span></span>

<span data-ttu-id="3ff35-138">Kestrel est idéal pour servir du contenu dynamique à partir d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ff35-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="3ff35-139">Toutefois, les fonctionnalités de service web ne sont pas aussi complètes que les serveurs tels qu’IIS, Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="3ff35-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="3ff35-140">Un serveur proxy inverse peut décharger du travail tel que le traitement du contenu statique, la mise en cache des requêtes, la compression des requêtes et l’arrêt SSL à partir du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="3ff35-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="3ff35-141">Un serveur proxy inverse peut résider sur un ordinateur dédié ou peut être déployé à côté d’un serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="3ff35-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="3ff35-142">Pour les besoins de ce guide, nous utilisons une seule instance de Nginx.</span><span class="sxs-lookup"><span data-stu-id="3ff35-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="3ff35-143">Elle s’exécute sur le même serveur, en plus du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="3ff35-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="3ff35-144">Selon les exigences, un paramétrage différent peut être choisi.</span><span class="sxs-lookup"><span data-stu-id="3ff35-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="3ff35-145">Les requêtes étant transférées par le proxy inverse, utilisez le [middleware (intergiciel) des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) à partir du package [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="3ff35-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="3ff35-146">Le middleware met à jour le `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="3ff35-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="3ff35-147">Tout composant qui dépend du schéma, tel que l’authentification, la génération de lien, les redirections et la géolocalisation, doit être placé après l’appel du middleware des en-têtes transférés.</span><span class="sxs-lookup"><span data-stu-id="3ff35-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="3ff35-148">En règle générale, le middleware des en-têtes transférés doit être exécuté avant les autres middlewares, à l’exception des middlewares de gestion des erreurs et de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="3ff35-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="3ff35-149">Cet ordre permet au middleware qui repose sur les informations des en-têtes transférés d’utiliser les valeurs d’en-tête pour le traitement.</span><span class="sxs-lookup"><span data-stu-id="3ff35-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3ff35-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3ff35-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3ff35-151">Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou un middleware de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="3ff35-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="3ff35-152">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="3ff35-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3ff35-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3ff35-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3ff35-154">Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) et [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou un middleware de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="3ff35-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="3ff35-155">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="3ff35-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="3ff35-156">Si aucune option [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) n’est spécifiée au middleware, les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="3ff35-157">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge.</span><span class="sxs-lookup"><span data-stu-id="3ff35-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="3ff35-158">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="3ff35-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="3ff35-159">Installer Nginx</span><span class="sxs-lookup"><span data-stu-id="3ff35-159">Install Nginx</span></span>

<span data-ttu-id="3ff35-160">Utilisez `apt-get` pour installer Nginx.</span><span class="sxs-lookup"><span data-stu-id="3ff35-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="3ff35-161">Le programme d’installation crée un script d’initialisation *systemd* qui exécute Nginx en tant que démon au démarrage du système.</span><span class="sxs-lookup"><span data-stu-id="3ff35-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="3ff35-162">L’archive Ubuntu PPA (Personal Package Archive) est gérée par des volontaires et n’est pas distribuée par [nginx.org](https://nginx.org/). Pour plus d’informations, consultez [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx : versions binaires : packages Debian/Ubuntu officiels).</span><span class="sxs-lookup"><span data-stu-id="3ff35-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="3ff35-163">Si des modules Nginx facultatifs sont requis, il peut s’avérer nécessaire de configurer Nginx à partir de la source.</span><span class="sxs-lookup"><span data-stu-id="3ff35-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="3ff35-164">Comme il s’agit de l’installation initiale de Nginx, vous devez le démarrer explicitement en exécutant :</span><span class="sxs-lookup"><span data-stu-id="3ff35-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="3ff35-165">Vérifiez qu’un navigateur affiche la page d’accueil par défaut de Nginx.</span><span class="sxs-lookup"><span data-stu-id="3ff35-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="3ff35-166">La page d’accueil est accessible à l’adresse `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="3ff35-167">Configurer Nginx</span><span class="sxs-lookup"><span data-stu-id="3ff35-167">Configure Nginx</span></span>

<span data-ttu-id="3ff35-168">Pour configurer Nginx en tant que proxy inverse pour transférer les requêtes à votre application ASP.NET Core, modifiez le fichier */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="3ff35-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="3ff35-169">Ouvrez-le dans un éditeur de texte et remplacez le contenu par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="3ff35-169">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="3ff35-170">Si aucun `server_name` ne correspond, Nginx utilise le serveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="3ff35-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="3ff35-171">Si aucun serveur par défaut n’est défini, le premier serveur dans le fichier de configuration est le serveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="3ff35-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="3ff35-172">En guise de bonne pratique, ajoutez un serveur par défaut spécifique qui retourne un code d’état 444 dans votre fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="3ff35-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="3ff35-173">Voici un exemple de configuration de serveur par défaut :</span><span class="sxs-lookup"><span data-stu-id="3ff35-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="3ff35-174">Avec les fichier de configuration et le serveur par défaut précédents, Nginx accepte le trafic public sur le port 80 avec un en-tête d’hôte `example.com` ou `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="3ff35-175">Les requêtes qui ne correspondent pas à ces hôtes ne sont pas transférées à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ff35-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="3ff35-176">Nginx transfère les requêtes correspondantes à Kestrel à l’adresse `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="3ff35-177">Consultez [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) (Comment nginx traite une requête) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="3ff35-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="3ff35-178">Pour changer le port/l’adresse IP Kestrel, consultez [Kestrel : configuration du point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="3ff35-178">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="3ff35-179">La spécification d’une [directive server_name](https://nginx.org/docs/http/server_names.html) incorrecte expose votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="3ff35-179">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="3ff35-180">Une liaison générique de sous-domaine (par exemple, `*.example.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="3ff35-180">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="3ff35-181">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="3ff35-181">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="3ff35-182">Une fois la configuration de Nginx établie, exécutez `sudo nginx -t` pour vérifier la syntaxe des fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="3ff35-182">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="3ff35-183">Si le test de fichier de configuration réussit, forcez Nginx à appliquer les modifications en exécutant `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-183">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="3ff35-184">Pour exécuter directement l’application sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="3ff35-184">To directly run the app on the server:</span></span>

1. <span data-ttu-id="3ff35-185">Accédez au répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ff35-185">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="3ff35-186">Exécutez le fichier exécutable de l’application : `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-186">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="3ff35-187">Si une erreur d’autorisation se produit, changez les autorisations :</span><span class="sxs-lookup"><span data-stu-id="3ff35-187">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="3ff35-188">Si l’application s’exécute sur le serveur, mais ne répond pas sur Internet, vérifiez que le port 80 est ouvert sur le pare-feu du serveur.</span><span class="sxs-lookup"><span data-stu-id="3ff35-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="3ff35-189">Si vous utilisez une machine virtuelle Azure Ubuntu, ajoutez une règle de groupe de sécurité réseau (NSG) qui autorise le trafic entrant sur le port 80.</span><span class="sxs-lookup"><span data-stu-id="3ff35-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="3ff35-190">Il est inutile d’activer une règle de trafic sortant sur le port 80, car le trafic sortant est accordé automatiquement quand la règle de trafic entrant est activée.</span><span class="sxs-lookup"><span data-stu-id="3ff35-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="3ff35-191">Quand vous avez terminé de tester l’application, arrêtez-la avec `Ctrl+C` depuis l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="3ff35-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="3ff35-192">Surveillance de l’application</span><span class="sxs-lookup"><span data-stu-id="3ff35-192">Monitoring the app</span></span>

<span data-ttu-id="3ff35-193">Le serveur est configuré pour transférer les requêtes faites à `http://<serveraddress>:80` à l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="3ff35-194">Toutefois, Nginx n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ff35-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="3ff35-195">*systemd* peut être utilisé pour créer un fichier de service afin de démarrer et de surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="3ff35-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="3ff35-196">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="3ff35-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="3ff35-197">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="3ff35-197">Create the service file</span></span>

<span data-ttu-id="3ff35-198">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="3ff35-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="3ff35-199">Voici un exemple de fichier de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="3ff35-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="3ff35-200">Si l’utilisateur *www-data* n’est pas utilisé par la configuration, vous devez d’abord créer l’utilisateur défini ici et l’affecter en tant que propriétaire des fichiers.</span><span class="sxs-lookup"><span data-stu-id="3ff35-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="3ff35-201">Linux possède un système de fichiers respectant la casse.</span><span class="sxs-lookup"><span data-stu-id="3ff35-201">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="3ff35-202">Si vous définissez ASPNETCORE_ENVIRONMENT sur « Production », c’est le fichier de configuration*appsettings.Production.json* qui est recherché, pas le fichier *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="3ff35-202">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="3ff35-203">Certaines valeurs (par exemple, les chaînes de connexion SQL) doivent être placées dans une séquence d’échappement afin que les fournisseurs de configuration puissent lire les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="3ff35-203">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="3ff35-204">Utilisez la commande suivante pour générer une valeur correctement placée dans une séquence d’échappement en vue de son utilisation dans le fichier de configuration :</span><span class="sxs-lookup"><span data-stu-id="3ff35-204">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="3ff35-205">Enregistrez le fichier et activez le service.</span><span class="sxs-lookup"><span data-stu-id="3ff35-205">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="3ff35-206">Démarrez le service et vérifiez qu’il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="3ff35-206">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="3ff35-207">Le proxy inverse étant configuré et Kestrel étant géré via systemd, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur la machine locale à l’adresse `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-207">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="3ff35-208">Elle est également accessible à partir d’un ordinateur distant, sauf en cas de blocage par un pare-feu.</span><span class="sxs-lookup"><span data-stu-id="3ff35-208">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="3ff35-209">Si l’on inspecte les en-têtes de réponse, on constate que l’en-tête `Server` indique que l’application ASP.NET Core est traitée par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ff35-209">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="3ff35-210">Affichage des journaux</span><span class="sxs-lookup"><span data-stu-id="3ff35-210">Viewing logs</span></span>

<span data-ttu-id="3ff35-211">Puisque l’application web utilisant Kestrel est gérée à l’aide de `systemd`, tous les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="3ff35-211">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="3ff35-212">Toutefois, ce journal inclut toutes les entrées pour tous les services et les processus gérés par `systemd`.</span><span class="sxs-lookup"><span data-stu-id="3ff35-212">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="3ff35-213">Pour afficher les éléments propres à `kestrel-hellomvc.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="3ff35-213">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="3ff35-214">Si vous voulez appliquer un filtrage supplémentaire, des options chronologiques, comme `--since today`, `--until 1 hour ago` ou une combinaison de ces options, peuvent réduire la quantité d’entrées retournées.</span><span class="sxs-lookup"><span data-stu-id="3ff35-214">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="3ff35-215">Protection des données</span><span class="sxs-lookup"><span data-stu-id="3ff35-215">Data protection</span></span>

<span data-ttu-id="3ff35-216">La [pile de protection des données ASP.NET Core](xref:security/data-protection/index) est utilisée par plusieurs [intergiciels (middleware)](xref:fundamentals/middleware/index) ASP.NET Core, notamment le middleware d’authentification (par exemple, le middleware cookie) et les protections de la falsification de requête intersites (CSRF, Cross Site Request Forgery).</span><span class="sxs-lookup"><span data-stu-id="3ff35-216">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="3ff35-217">Même si les API de protection des données ne sont pas appelées par le code de l’utilisateur, la protection des données doit être configurée pour créer un [magasin de clés](xref:security/data-protection/implementation/key-management) de chiffrement persistantes.</span><span class="sxs-lookup"><span data-stu-id="3ff35-217">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="3ff35-218">Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="3ff35-218">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="3ff35-219">Si le Key Ring est stocké en mémoire, au redémarrage de l’application :</span><span class="sxs-lookup"><span data-stu-id="3ff35-219">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="3ff35-220">tous les jetons d’authentification basés sur des cookies sont invalidés</span><span class="sxs-lookup"><span data-stu-id="3ff35-220">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="3ff35-221">Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande.</span><span class="sxs-lookup"><span data-stu-id="3ff35-221">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="3ff35-222">toutes les données protégées par le Key Ring ne peuvent plus être déchiffrées.</span><span class="sxs-lookup"><span data-stu-id="3ff35-222">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="3ff35-223">Ceci peut inclure des [jetons CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) et des [cookies TempData ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="3ff35-223">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="3ff35-224">Pour configurer la protection des données de façon à conserver et chiffrer le porte-clés (Key Ring), consultez :</span><span class="sxs-lookup"><span data-stu-id="3ff35-224">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="3ff35-225">Sécurisation de l’application</span><span class="sxs-lookup"><span data-stu-id="3ff35-225">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="3ff35-226">Activer AppArmor</span><span class="sxs-lookup"><span data-stu-id="3ff35-226">Enable AppArmor</span></span>

<span data-ttu-id="3ff35-227">Linux Security Modules (LSM) est un framework qui fait partie du noyau Linux depuis Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="3ff35-227">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="3ff35-228">LSM prend en charge différentes implémentations de modules de sécurité.</span><span class="sxs-lookup"><span data-stu-id="3ff35-228">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="3ff35-229">[AppArmor](https://wiki.ubuntu.com/AppArmor) est un LSM qui implémente un système de contrôle d’accès obligatoire permettant de confiner le programme à un ensemble limité de ressources.</span><span class="sxs-lookup"><span data-stu-id="3ff35-229">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="3ff35-230">Vérifiez qu’AppArmor est activé et configuré correctement.</span><span class="sxs-lookup"><span data-stu-id="3ff35-230">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="3ff35-231">Configuration du pare-feu</span><span class="sxs-lookup"><span data-stu-id="3ff35-231">Configuring the firewall</span></span>

<span data-ttu-id="3ff35-232">Fermez tous les ports externes qui ne sont pas en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="3ff35-232">Close off all external ports that are not in use.</span></span> <span data-ttu-id="3ff35-233">Uncomplicated firewall (ufw) fournit une interface de ligne de commande pour `iptables` afin de configurer le pare-feu.</span><span class="sxs-lookup"><span data-stu-id="3ff35-233">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="3ff35-234">Un pare-feu mal configuré bloque l’accès à l’ensemble du système.</span><span class="sxs-lookup"><span data-stu-id="3ff35-234">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="3ff35-235">Faute d’avoir spécifié le port SSH approprié, vous ne pourrez pas accéder au système si vous utilisez SSH pour vous y connecter.</span><span class="sxs-lookup"><span data-stu-id="3ff35-235">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="3ff35-236">Le numéro de port par défaut est 22.</span><span class="sxs-lookup"><span data-stu-id="3ff35-236">The default port is 22.</span></span> <span data-ttu-id="3ff35-237">Pour plus d’informations, consultez la [présentation d’ufw](https://help.ubuntu.com/community/UFW) et le [manuel](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="3ff35-237">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="3ff35-238">Installez `ufw` et configurez-le de façon à autoriser le trafic sur les ports nécessaires.</span><span class="sxs-lookup"><span data-stu-id="3ff35-238">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a><span data-ttu-id="3ff35-239">Sécurisation de Nginx</span><span class="sxs-lookup"><span data-stu-id="3ff35-239">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="3ff35-240">Changer le nom de la réponse Nginx</span><span class="sxs-lookup"><span data-stu-id="3ff35-240">Change the Nginx response name</span></span>

<span data-ttu-id="3ff35-241">Modifiez *src/http/ngx_http_header_filter_module.c* :</span><span class="sxs-lookup"><span data-stu-id="3ff35-241">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="3ff35-242">Configurer les options</span><span class="sxs-lookup"><span data-stu-id="3ff35-242">Configure options</span></span>

<span data-ttu-id="3ff35-243">Configurez le serveur avec les modules nécessaires supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="3ff35-243">Configure the server with additional required modules.</span></span> <span data-ttu-id="3ff35-244">Pour renforcer l’application, vous pouvez utiliser un pare-feu d’application web tel que [ModSecurity](https://www.modsecurity.org/).</span><span class="sxs-lookup"><span data-stu-id="3ff35-244">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="3ff35-245">Configurer le protocole SSL</span><span class="sxs-lookup"><span data-stu-id="3ff35-245">Configure SSL</span></span>

* <span data-ttu-id="3ff35-246">Configurez le serveur pour qu’il écoute le trafic HTTPS sur le port `443` en spécifiant un certificat valide émis par une autorité de certificat approuvée.</span><span class="sxs-lookup"><span data-stu-id="3ff35-246">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="3ff35-247">Renforcez la sécurité en appliquant certaines des pratiques mentionnées dans le fichier */etc/nginx/nginx.conf* suivant.</span><span class="sxs-lookup"><span data-stu-id="3ff35-247">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="3ff35-248">Vous pouvez par exemple choisir un chiffrement plus fort et rediriger tout le trafic sur HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3ff35-248">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="3ff35-249">L’ajout d’un en-tête `HTTP Strict-Transport-Security` (HSTS) garantit que toutes les requêtes ultérieures du client s’effectuent uniquement par le biais du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3ff35-249">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="3ff35-250">N’ajoutez pas l’en-tête Strict-Transport-Security ou choisissez une valeur `max-age` appropriée si le protocole SSL doit être désactivé ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="3ff35-250">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="3ff35-251">Ajoutez le fichier de configuration */etc/nginx/proxy.conf* :</span><span class="sxs-lookup"><span data-stu-id="3ff35-251">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="3ff35-252">Modifiez le fichier de configuration */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="3ff35-252">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="3ff35-253">Dans l’exemple, les sections `http` et `server` figurent dans un même fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="3ff35-253">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="3ff35-254">Sécuriser Nginx contre le détournement de clic</span><span class="sxs-lookup"><span data-stu-id="3ff35-254">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="3ff35-255">Le détournement de clic, ou clickjacking, est une technique malveillante visant à recueillir les clics d’un utilisateur infecté.</span><span class="sxs-lookup"><span data-stu-id="3ff35-255">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="3ff35-256">Elle pousse la victime (le visiteur) à cliquer sur un site infecté.</span><span class="sxs-lookup"><span data-stu-id="3ff35-256">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="3ff35-257">Utilisez X-FRAME-OPTIONS pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="3ff35-257">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="3ff35-258">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="3ff35-258">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="3ff35-259">Ajoutez la ligne `add_header X-Frame-Options "SAMEORIGIN";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="3ff35-259">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="3ff35-260">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="3ff35-260">MIME-type sniffing</span></span>

<span data-ttu-id="3ff35-261">Cet en-tête empêche la plupart des navigateurs de détourner le type MIME d’une réponse et de remplacer le type de contenu déclaré, car l’en-tête indique au navigateur qu’il ne doit pas substituer le type de contenu de la réponse.</span><span class="sxs-lookup"><span data-stu-id="3ff35-261">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="3ff35-262">Avec l’option `nosniff`, si le serveur indique que le contenu est « text/html », le navigateur le restitue en tant que « text/html ».</span><span class="sxs-lookup"><span data-stu-id="3ff35-262">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="3ff35-263">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="3ff35-263">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="3ff35-264">Ajoutez la ligne `add_header X-Content-Type-Options "nosniff";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="3ff35-264">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ff35-265">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3ff35-265">Additional resources</span></span>

* <span data-ttu-id="3ff35-266">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages) (Nginx : versions binaires : packages Debian/Ubuntu officiels).</span><span class="sxs-lookup"><span data-stu-id="3ff35-266">[Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)</span></span>
* [<span data-ttu-id="3ff35-267">Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge</span><span class="sxs-lookup"><span data-stu-id="3ff35-267">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* <span data-ttu-id="3ff35-268">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (NGINX : utilisation de l’en-tête Forwarded)</span><span class="sxs-lookup"><span data-stu-id="3ff35-268">[NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)</span></span>
