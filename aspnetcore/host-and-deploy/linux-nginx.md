---
title: "Héberger ASP.NET Core sur Linux avec Nginx"
author: rick-anderson
description: "Décrit comment le programme d’installation de Nginx comme un proxy inverse sur 16.04 Ubuntu pour transférer le trafic HTTP vers une application web ASP.NET Core sur Kestrel."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: a1de177fcd41c925a85e5aab9a0d236249b7da0b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="42ca2-103">Héberger ASP.NET Core sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="42ca2-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="42ca2-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="42ca2-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="42ca2-105">Ce guide explique comment configurer un environnement ASP.NET Core prêt pour la production sur un serveur Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="42ca2-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="42ca2-106">Pour Ubuntu 14.04, *supervisord* est recommandé d’utiliser une solution pour l’analyse du processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="42ca2-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="42ca2-107">*systemd* n’est pas disponible sur Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="42ca2-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="42ca2-108">[Consultez la version précédente de ce document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="42ca2-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="42ca2-109">Ce guide montre comment effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="42ca2-109">This guide:</span></span>

* <span data-ttu-id="42ca2-110">Place une application ASP.NET Core existante derrière un serveur proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="42ca2-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="42ca2-111">Configure le serveur de proxy inverse pour transférer les demandes au serveur web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="42ca2-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="42ca2-112">Garantit que l’application web s’exécute au démarrage en tant que démon.</span><span class="sxs-lookup"><span data-stu-id="42ca2-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="42ca2-113">Configure un outil de gestion des processus permettant de redémarrer l’application web.</span><span class="sxs-lookup"><span data-stu-id="42ca2-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42ca2-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="42ca2-114">Prerequisites</span></span>

1. <span data-ttu-id="42ca2-115">Accès à un serveur Ubuntu 16.04 avec un compte d’utilisateur standard disposant de privilèges sudo</span><span class="sxs-lookup"><span data-stu-id="42ca2-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="42ca2-116">Une application ASP.NET Core existante</span><span class="sxs-lookup"><span data-stu-id="42ca2-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="42ca2-117">Copier sur l’application</span><span class="sxs-lookup"><span data-stu-id="42ca2-117">Copy over the app</span></span>

<span data-ttu-id="42ca2-118">Exécutez [dotnet publier](/dotnet/core/tools/dotnet-publish) à partir de l’environnement de développement pour empaqueter une application dans un répertoire autonome pouvant s’exécuter sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="42ca2-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="42ca2-119">Copie de l’application ASP.NET Core pour le serveur à l’aide de n’importe quel outil s’intègre dans le flux de travail de l’organisation (par exemple, SCP, FTP).</span><span class="sxs-lookup"><span data-stu-id="42ca2-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="42ca2-120">Testez l’application, par exemple :</span><span class="sxs-lookup"><span data-stu-id="42ca2-120">Test the app, for example:</span></span>

* <span data-ttu-id="42ca2-121">À partir de la ligne de commande, exécutez `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="42ca2-122">Dans un navigateur, accédez à `http://<serveraddress>:<port>` pour vérifier que l’application fonctionne sur Linux.</span><span class="sxs-lookup"><span data-stu-id="42ca2-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="42ca2-123">Configurer un serveur proxy inverse</span><span class="sxs-lookup"><span data-stu-id="42ca2-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="42ca2-124">Un proxy inverse est une installation commune pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="42ca2-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="42ca2-125">Un proxy inverse met fin à la requête HTTP et le transmet à l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42ca2-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="42ca2-126">Pourquoi utiliser un serveur proxy inverse ?</span><span class="sxs-lookup"><span data-stu-id="42ca2-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="42ca2-127">Kestrel est idéal pour héberger un contenu dynamique à partir de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="42ca2-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="42ca2-128">Toutefois, les fonctionnalités de service web ne sont pas en tant que riche en tant que serveurs tels que IIS, Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="42ca2-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="42ca2-129">Un serveur proxy inverse peut décharger du travail, tels que fournit du contenu statique, la mise en cache des demandes, la compression des demandes et l’arrêt SSL à partir du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="42ca2-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="42ca2-130">Un serveur proxy inverse peut résider sur un ordinateur dédié ou peut être déployé à côté d’un serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="42ca2-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="42ca2-131">Pour les besoins de ce guide, nous utilisons une seule instance de Nginx.</span><span class="sxs-lookup"><span data-stu-id="42ca2-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="42ca2-132">Elle s’exécute sur le même serveur, en plus du serveur HTTP.</span><span class="sxs-lookup"><span data-stu-id="42ca2-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="42ca2-133">Selon la configuration requise, un paramétrage différent peut être choisi.</span><span class="sxs-lookup"><span data-stu-id="42ca2-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="42ca2-134">Étant donné que les demandes sont transmises par le proxy inverse, utilisez l’intergiciel en-têtes transférés à partir de la [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span><span class="sxs-lookup"><span data-stu-id="42ca2-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="42ca2-135">Les mises à jour de l’intergiciel (middleware) le `Request.Scheme`, à l’aide du `X-Forwarded-Proto` en-tête, afin qu’URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="42ca2-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="42ca2-136">Lorsque vous utilisez n’importe quel type d’intergiciel (middleware) d’authentification, les en-têtes transféré intergiciel (middleware) doit exécuter en premier.</span><span class="sxs-lookup"><span data-stu-id="42ca2-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="42ca2-137">Cette commande garantit que l’intergiciel (middleware) d’authentification permettre consommer les valeurs d’en-tête et générer l’URI de redirection correcte.</span><span class="sxs-lookup"><span data-stu-id="42ca2-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42ca2-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42ca2-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="42ca2-139">Appeler le [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) méthode dans `Startup.Configure` avant d’appeler [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou intergiciel (middleware) du schéma d’authentification similaire :</span><span class="sxs-lookup"><span data-stu-id="42ca2-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42ca2-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42ca2-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="42ca2-141">Appeler le [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) méthode dans `Startup.Configure` avant d’appeler [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) et [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou le schéma d’authentification similaires intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="42ca2-141">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="42ca2-142">Si aucun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sont spécifiés à l’intergiciel (middleware), les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-142">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="42ca2-143">Installer Nginx</span><span class="sxs-lookup"><span data-stu-id="42ca2-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="42ca2-144">Si des modules Nginx facultatifs sont installés, il peut être nécessaire de génération Nginx à partir de la source.</span><span class="sxs-lookup"><span data-stu-id="42ca2-144">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="42ca2-145">Utilisez `apt-get` pour installer Nginx.</span><span class="sxs-lookup"><span data-stu-id="42ca2-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="42ca2-146">Le programme d’installation crée un script d’initialisation System V qui exécute Nginx en tant que démon au démarrage du système.</span><span class="sxs-lookup"><span data-stu-id="42ca2-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="42ca2-147">Comme il s’agit de l’installation initiale de Nginx, vous devez le démarrer explicitement en exécutant :</span><span class="sxs-lookup"><span data-stu-id="42ca2-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="42ca2-148">Vérifiez qu’un navigateur affiche la page d’accueil par défaut de Nginx.</span><span class="sxs-lookup"><span data-stu-id="42ca2-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="42ca2-149">Configurer Nginx</span><span class="sxs-lookup"><span data-stu-id="42ca2-149">Configure Nginx</span></span>

<span data-ttu-id="42ca2-150">Pour configurer Nginx comme un proxy inverse pour transférer les demandes à votre application ASP.NET Core, modifiez */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="42ca2-150">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="42ca2-151">Ouvrez-le dans un éditeur de texte et remplacez le contenu par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="42ca2-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="42ca2-152">Lorsqu’aucun `server_name` correspondances, Nginx utilise le serveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="42ca2-152">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="42ca2-153">Si aucun serveur par défaut n’est défini, le premier serveur dans le fichier de configuration est le serveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="42ca2-153">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="42ca2-154">En tant que meilleure pratique, ajouter un serveur de valeur par défaut spécifique qui retourne un code d’état de 444 dans votre fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="42ca2-154">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="42ca2-155">Un exemple de configuration de serveur par défaut est :</span><span class="sxs-lookup"><span data-stu-id="42ca2-155">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="42ca2-156">Avec le précédent serveur de configuration par défaut et le fichier, Nginx accepte le trafic public sur le port 80 avec un en-tête d’hôte `example.com` ou `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-156">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="42ca2-157">Demande ne correspond ne pas à ces hôtes ne sont transmis à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="42ca2-157">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="42ca2-158">Nginx transfère les demandes correspondants à Kestrel à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-158">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="42ca2-159">Consultez [comment nginx traite une demande](https://nginx.org/docs/http/request_processing.html) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="42ca2-159">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="42ca2-160">Échec de spécification de manière adéquate [nom_serveur directive](https://nginx.org/docs/http/server_names.html) expose votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="42ca2-160">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="42ca2-161">Liaison de caractère générique de sous-domaine (par exemple, `*.example.com`) ne présente ce risque de sécurité si vous contrôlez le domaine parent entière (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="42ca2-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="42ca2-162">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="42ca2-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="42ca2-163">Une fois la configuration de Nginx est établie, exécutez `sudo nginx -t` pour vérifier la syntaxe des fichiers de configuration.</span><span class="sxs-lookup"><span data-stu-id="42ca2-163">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="42ca2-164">Si le test de fichier de configuration a réussi, forcer Nginx pour appliquer les modifications en exécutant `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-164">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="42ca2-165">Surveillance de l’application</span><span class="sxs-lookup"><span data-stu-id="42ca2-165">Monitoring the app</span></span>

<span data-ttu-id="42ca2-166">Le serveur est configuré pour transférer les demandes faites à `http://<serveraddress>:80` une session sur l’application ASP.NET Core s’exécutant sur Kestrel à `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-166">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="42ca2-167">Toutefois, Nginx n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="42ca2-167">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="42ca2-168">*systemd* peut être utilisé pour créer un fichier de service pour démarrer et surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="42ca2-168">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="42ca2-169">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="42ca2-169">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="42ca2-170">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="42ca2-170">Create the service file</span></span>

<span data-ttu-id="42ca2-171">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="42ca2-171">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="42ca2-172">Voici un exemple de fichier de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="42ca2-172">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="42ca2-173">**Remarque :** si l’utilisateur *www-données* n’est pas utilisé par la configuration, l’utilisateur défini ici doit être créé en premier et étant donné la propriété appropriée pour les fichiers.</span><span class="sxs-lookup"><span data-stu-id="42ca2-173">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="42ca2-174">**Remarque :** Linux possède un système de fichiers respectant la casse.</span><span class="sxs-lookup"><span data-stu-id="42ca2-174">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="42ca2-175">La ASPNETCORE_ENVIRONMENT produit « Production » dans la recherche du fichier de configuration *appsettings. Production.JSON*, et non *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="42ca2-175">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="42ca2-176">Enregistrez le fichier et activez le service.</span><span class="sxs-lookup"><span data-stu-id="42ca2-176">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="42ca2-177">Démarrez le service et vérifiez qu’il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="42ca2-177">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="42ca2-178">Avec la configuration de proxy inverse et le Kestrel gérés via systemd, l’application web est entièrement configurée et sont accessibles à partir d’un navigateur sur l’ordinateur local à `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-178">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="42ca2-179">Il est également accessible à partir d’un ordinateur distant, la restriction de pare-feu pouvant bloquer.</span><span class="sxs-lookup"><span data-stu-id="42ca2-179">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="42ca2-180">Inspecter les en-têtes de réponse, le `Server` en-tête indique à l’application ASP.NET Core sont traitée par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="42ca2-180">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="42ca2-181">Affichage des journaux</span><span class="sxs-lookup"><span data-stu-id="42ca2-181">Viewing logs</span></span>

<span data-ttu-id="42ca2-182">Depuis l’application web à l’aide de Kestrel est géré à l’aide de `systemd`, tous les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="42ca2-182">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="42ca2-183">Toutefois, ce journal inclut toutes les entrées pour tous les services et les processus gérés par `systemd`.</span><span class="sxs-lookup"><span data-stu-id="42ca2-183">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="42ca2-184">Pour afficher les éléments propres à `kestrel-hellomvc.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="42ca2-184">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="42ca2-185">Si vous voulez appliquer un filtrage supplémentaire, des options chronologiques, comme `--since today`, `--until 1 hour ago` ou une combinaison de ces options, peuvent réduire la quantité d’entrées retournées.</span><span class="sxs-lookup"><span data-stu-id="42ca2-185">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="42ca2-186">Sécurisation de l’application</span><span class="sxs-lookup"><span data-stu-id="42ca2-186">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="42ca2-187">Activer AppArmor</span><span class="sxs-lookup"><span data-stu-id="42ca2-187">Enable AppArmor</span></span>

<span data-ttu-id="42ca2-188">Modules de sécurité Linux (LSM) est une infrastructure qui fait partie du noyau Linux depuis Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="42ca2-188">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="42ca2-189">LSM prend en charge différentes implémentations de modules de sécurité.</span><span class="sxs-lookup"><span data-stu-id="42ca2-189">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="42ca2-190">[AppArmor](https://wiki.ubuntu.com/AppArmor) est un LSM qui implémente un système de contrôle d’accès obligatoire permettant de confiner le programme à un ensemble limité de ressources.</span><span class="sxs-lookup"><span data-stu-id="42ca2-190">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="42ca2-191">Vérifiez qu’AppArmor est activé et configuré correctement.</span><span class="sxs-lookup"><span data-stu-id="42ca2-191">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="42ca2-192">Configuration du pare-feu</span><span class="sxs-lookup"><span data-stu-id="42ca2-192">Configuring the firewall</span></span>

<span data-ttu-id="42ca2-193">Fermez tous les ports externes qui ne sont pas en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="42ca2-193">Close off all external ports that are not in use.</span></span> <span data-ttu-id="42ca2-194">Uncomplicated firewall (ufw) fournit une interface de ligne de commande pour `iptables` afin de configurer le pare-feu.</span><span class="sxs-lookup"><span data-stu-id="42ca2-194">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="42ca2-195">Vérifiez que `ufw` est configuré pour autoriser le trafic sur les ports requis.</span><span class="sxs-lookup"><span data-stu-id="42ca2-195">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="42ca2-196">Sécurisation de Nginx</span><span class="sxs-lookup"><span data-stu-id="42ca2-196">Securing Nginx</span></span>

<span data-ttu-id="42ca2-197">La distribution par défaut de Nginx n’active pas le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="42ca2-197">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="42ca2-198">Pour activer des fonctionnalités de sécurité supplémentaires, générez à partir de la source.</span><span class="sxs-lookup"><span data-stu-id="42ca2-198">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="42ca2-199">Télécharger la source et installer les dépendances de build</span><span class="sxs-lookup"><span data-stu-id="42ca2-199">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="42ca2-200">Changer le nom de la réponse Nginx</span><span class="sxs-lookup"><span data-stu-id="42ca2-200">Change the Nginx response name</span></span>

<span data-ttu-id="42ca2-201">Modifiez *src/http/ngx_http_header_filter_module.c* :</span><span class="sxs-lookup"><span data-stu-id="42ca2-201">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="42ca2-202">Configurer les options et générer</span><span class="sxs-lookup"><span data-stu-id="42ca2-202">Configure the options and build</span></span>

<span data-ttu-id="42ca2-203">La bibliothèque PCRE est obligatoire pour les expressions régulières.</span><span class="sxs-lookup"><span data-stu-id="42ca2-203">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="42ca2-204">Les expressions régulières sont utilisées dans la directive d’emplacement pour ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="42ca2-204">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="42ca2-205">http_ssl_module ajoute la prise en charge du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="42ca2-205">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="42ca2-206">Envisagez d’utiliser un pare-feu d’application web comme *ModSecurity* pour renforcer l’application.</span><span class="sxs-lookup"><span data-stu-id="42ca2-206">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="42ca2-207">Configurer le protocole SSL</span><span class="sxs-lookup"><span data-stu-id="42ca2-207">Configure SSL</span></span>

* <span data-ttu-id="42ca2-208">Configurer le serveur pour écouter le trafic HTTPS sur le port `443` en spécifiant un certificat valide émis par une autorité de certificat approuvé (CA).</span><span class="sxs-lookup"><span data-stu-id="42ca2-208">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="42ca2-209">Renforcer la sécurité en utilisant certaines des pratiques mentionnés dans l’exemple suivant */etc/nginx/nginx.conf* fichier.</span><span class="sxs-lookup"><span data-stu-id="42ca2-209">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="42ca2-210">Vous pouvez par exemple choisir un chiffrement plus fort et rediriger tout le trafic sur HTTP vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="42ca2-210">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="42ca2-211">L’ajout d’un en-tête `HTTP Strict-Transport-Security` (HSTS) garantit que toutes les requêtes ultérieures du client s’effectuent uniquement par le biais du protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="42ca2-211">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="42ca2-212">N’ajoutez pas l’en-tête de sécurité de Transport Strict ou avez choisi une `max-age` si SSL est désactivé dans le futur.</span><span class="sxs-lookup"><span data-stu-id="42ca2-212">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="42ca2-213">Ajoutez le fichier de configuration */etc/nginx/proxy.conf* :</span><span class="sxs-lookup"><span data-stu-id="42ca2-213">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="42ca2-214">Modifiez le fichier de configuration */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="42ca2-214">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="42ca2-215">Dans l’exemple, les sections `http` et `server` figurent dans un même fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="42ca2-215">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="42ca2-216">Sécuriser Nginx contre le détournement de clic</span><span class="sxs-lookup"><span data-stu-id="42ca2-216">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="42ca2-217">Le détournement de clic, ou clickjacking, est une technique malveillante visant à recueillir les clics d’un utilisateur infecté.</span><span class="sxs-lookup"><span data-stu-id="42ca2-217">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="42ca2-218">Elle pousse la victime (le visiteur) à cliquer sur un site infecté.</span><span class="sxs-lookup"><span data-stu-id="42ca2-218">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="42ca2-219">Utilisez X-FRAME-OPTIONS pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="42ca2-219">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="42ca2-220">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="42ca2-220">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="42ca2-221">Ajoutez la ligne `add_header X-Frame-Options "SAMEORIGIN";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="42ca2-221">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="42ca2-222">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="42ca2-222">MIME-type sniffing</span></span>

<span data-ttu-id="42ca2-223">Cet en-tête empêche la plupart des navigateurs de détourner le type MIME d’une réponse et de remplacer le type de contenu déclaré, car l’en-tête indique au navigateur qu’il ne doit pas substituer le type de contenu de la réponse.</span><span class="sxs-lookup"><span data-stu-id="42ca2-223">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="42ca2-224">Avec l’option `nosniff`, si le serveur indique que le contenu est « text/html », le navigateur le restitue en tant que « text/html ».</span><span class="sxs-lookup"><span data-stu-id="42ca2-224">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="42ca2-225">Modifiez le fichier *nginx.conf* :</span><span class="sxs-lookup"><span data-stu-id="42ca2-225">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="42ca2-226">Ajoutez la ligne `add_header X-Content-Type-Options "nosniff";` et enregistrez le fichier, puis redémarrez Nginx.</span><span class="sxs-lookup"><span data-stu-id="42ca2-226">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
