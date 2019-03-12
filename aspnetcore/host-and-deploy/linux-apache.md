---
title: Héberger ASP.NET Core sur Linux avec Apache
description: Découvrez comment configurer Apache comme serveur proxy inverse sur CentOS pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 02/27/2019
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 69026997b2c269a4fb56ed2a79fa42ae218368e1
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345935"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="2c907-103">Héberger ASP.NET Core sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="2c907-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="2c907-104">Par [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2c907-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="2c907-105">À l’aide de ce guide, découvrez comment configurer [Apache](https://httpd.apache.org/) comme serveur proxy inverse sur [CentOS 7](https://www.centos.org/) pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur un serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2c907-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="2c907-106">[L’extension mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) et les modules associés créent le proxy inverse du serveur.</span><span class="sxs-lookup"><span data-stu-id="2c907-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c907-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="2c907-107">Prerequisites</span></span>

* <span data-ttu-id="2c907-108">Serveur exécutant CentOS 7 avec un compte d’utilisateur standard disposant du privilège sudo.</span><span class="sxs-lookup"><span data-stu-id="2c907-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="2c907-109">Installez le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c907-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="2c907-110">Visitez la [page All Downloads de .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="2c907-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="2c907-111">Dans la liste **Runtime**, sélectionnez le runtime le plus récent qui n’est pas une préversion.</span><span class="sxs-lookup"><span data-stu-id="2c907-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="2c907-112">Sélectionnez et suivez les instructions pour CentOS/Oracle.</span><span class="sxs-lookup"><span data-stu-id="2c907-112">Select and follow the instructions for CentOS/Oracle.</span></span>
* <span data-ttu-id="2c907-113">Une application ASP.NET Core existante.</span><span class="sxs-lookup"><span data-stu-id="2c907-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="2c907-114">Publier et copier sur l’application</span><span class="sxs-lookup"><span data-stu-id="2c907-114">Publish and copy over the app</span></span>

<span data-ttu-id="2c907-115">Configurez l’application pour un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="2c907-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="2c907-116">Exécutez [dotnet publish](/dotnet/core/tools/dotnet-publish) à partir de l’environnement de développement pour empaqueter une application dans un répertoire (par exemple, *bin/Release/&lt;moniker_framework_target&gt;/publish*) exécutable sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="2c907-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="2c907-117">L’application peut également être publiée en tant que [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) si vous préférez ne pas gérer le runtime .NET Core sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c907-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="2c907-118">Copiez l’application ASP.NET Core sur le serveur à l’aide d’un outil qui s’intègre au flux de travail de l’organisation (par exemple, SCP ou SFTP).</span><span class="sxs-lookup"><span data-stu-id="2c907-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="2c907-119">Il est courant de placer les applications web sous le répertoire *var* (par exemple, *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="2c907-119">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="2c907-120">Dans un scénario de déploiement en production, un workflow d’intégration continue effectue le travail de publication de l’application et de copie des composants sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c907-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="2c907-121">Configurer un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="2c907-121">Configure a proxy server</span></span>

<span data-ttu-id="2c907-122">Un proxy inverse est une configuration courante pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="2c907-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="2c907-123">Le proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2c907-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="2c907-124">Un serveur proxy est un serveur qui transfère les requêtes des clients à un autre serveur, au lieu de traiter celles-ci lui-même.</span><span class="sxs-lookup"><span data-stu-id="2c907-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="2c907-125">Un proxy inverse transfère à une destination fixe, en général pour le compte de clients arbitraires.</span><span class="sxs-lookup"><span data-stu-id="2c907-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="2c907-126">Dans ce guide, Apache est configuré comme proxy inverse s’exécutant sur le même serveur où Kestrel traite l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c907-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="2c907-127">Les requêtes étant transférées par le proxy inverse, utilisez le [middleware (intergiciel) des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) à partir du package [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="2c907-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="2c907-128">Le middleware met à jour le `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="2c907-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="2c907-129">Tout composant qui dépend du schéma, tel que l’authentification, la génération de lien, les redirections et la géolocalisation, doit être placé après l’appel du middleware des en-têtes transférés.</span><span class="sxs-lookup"><span data-stu-id="2c907-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="2c907-130">En règle générale, le middleware des en-têtes transférés doit être exécuté avant les autres middlewares, à l’exception des middlewares de gestion des erreurs et de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="2c907-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="2c907-131">Cet ordre permet au middleware qui repose sur les informations des en-têtes transférés d’utiliser les valeurs d’en-tête pour le traitement.</span><span class="sxs-lookup"><span data-stu-id="2c907-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2c907-132">Appelez la méthode <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> dans `Startup.Configure` avant d’appeler <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> ou un intergiciel de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="2c907-132">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="2c907-133">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="2c907-133">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2c907-134">Appelez la méthode <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> dans `Startup.Configure` avant d’appeler <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> et <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> ou un intergiciel de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="2c907-134">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*> and <xref:Microsoft.AspNetCore.Builder.FacebookAppBuilderExtensions.UseFacebookAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="2c907-135">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="2c907-135">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

::: moniker-end

<span data-ttu-id="2c907-136">Si aucune option <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> n’est spécifiée au middleware, les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="2c907-136">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="2c907-137">Les serveurs proxy exécutés sur les adresses de bouclage (127.0.0.0/8, [ :: 1]), notamment l’adresse localhost standard (127.0.0.1), sont approuvés par défaut.</span><span class="sxs-lookup"><span data-stu-id="2c907-137">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="2c907-138">Si d’autres proxys ou réseaux approuvés au sein de l’organisation gèrent les requêtes entre Internet et le serveur web, ajoutez-les à la liste des <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> ou des <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> avec <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="2c907-138">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="2c907-139">L’exemple suivant ajoute un serveur proxy approuvé avec l’adresse IP 10.0.0.100 au middleware des en-têtes transférés `KnownProxies` dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="2c907-139">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="2c907-140">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="2c907-140">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="2c907-141">Installer Apache</span><span class="sxs-lookup"><span data-stu-id="2c907-141">Install Apache</span></span>

<span data-ttu-id="2c907-142">Mettez à jour les packages CentOS vers leur dernière version stable :</span><span class="sxs-lookup"><span data-stu-id="2c907-142">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="2c907-143">Installez le serveur web Apache sur CentOS avec une seule commande `yum` :</span><span class="sxs-lookup"><span data-stu-id="2c907-143">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="2c907-144">Exemple de sortie après exécution de la commande :</span><span class="sxs-lookup"><span data-stu-id="2c907-144">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="2c907-145">Dans cet exemple, la sortie correspond à httpd.86_64, car la version de CentOS 7 est en 64 bits.</span><span class="sxs-lookup"><span data-stu-id="2c907-145">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="2c907-146">Pour vérifier où Apache est installé, exécutez `whereis httpd` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="2c907-146">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="2c907-147">Configurer Apache</span><span class="sxs-lookup"><span data-stu-id="2c907-147">Configure Apache</span></span>

<span data-ttu-id="2c907-148">Les fichiers de configuration pour Apache se trouvent dans le répertoire `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="2c907-148">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="2c907-149">Les fichiers avec l’extension *.conf* sont traités par ordre alphabétique en plus des fichiers de configuration des modules dans `/etc/httpd/conf.modules.d/`, qui contient tous les fichiers de configuration nécessaires au chargement des modules.</span><span class="sxs-lookup"><span data-stu-id="2c907-149">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="2c907-150">Créez un fichier de configuration nommé *helloapp.conf*, pour l’application :</span><span class="sxs-lookup"><span data-stu-id="2c907-150">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="2c907-151">Le bloc `VirtualHost` peut apparaître plusieurs fois, dans un ou plusieurs fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="2c907-151">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="2c907-152">Dans le fichier de configuration précédent, Apache accepte le trafic public sur le port 80.</span><span class="sxs-lookup"><span data-stu-id="2c907-152">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="2c907-153">Le domaine `www.example.com` est pris en charge, et l’alias `*.example.com` aboutit au même site web.</span><span class="sxs-lookup"><span data-stu-id="2c907-153">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="2c907-154">Pour plus d’informations, consultez [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Prise en charge des hôtes virtuels par nom).</span><span class="sxs-lookup"><span data-stu-id="2c907-154">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="2c907-155">Les requêtes sont transmises par proxy à la racine vers le port 5000 du serveur sur 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="2c907-155">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="2c907-156">Pour la communication bidirectionnelle, `ProxyPass` et `ProxyPassReverse` sont requis.</span><span class="sxs-lookup"><span data-stu-id="2c907-156">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="2c907-157">Pour changer le port/l’adresse IP Kestrel, consultez [Kestrel : configuration du point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="2c907-157">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="2c907-158">La spécification d’une [directive ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) incorrecte dans le bloc **VirtualHost** expose votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="2c907-158">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="2c907-159">Une liaison générique de sous-domaine (par exemple, `*.example.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="2c907-159">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="2c907-160">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="2c907-160">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="2c907-161">Vous pouvez configurer la journalisation par `VirtualHost` à l’aide des directives `ErrorLog` et `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="2c907-161">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="2c907-162">`ErrorLog` est l’emplacement où le serveur journalise les erreurs, tandis que `CustomLog` définit le nom de fichier et le format du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="2c907-162">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="2c907-163">Dans ce cas, c’est l’endroit où les informations des requêtes sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="2c907-163">In this case, this is where request information is logged.</span></span> <span data-ttu-id="2c907-164">Il y a une ligne pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="2c907-164">There's one line for each request.</span></span>

<span data-ttu-id="2c907-165">Enregistrez le fichier et testez la configuration.</span><span class="sxs-lookup"><span data-stu-id="2c907-165">Save the file and test the configuration.</span></span> <span data-ttu-id="2c907-166">Si tout réussit, la réponse doit être `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="2c907-166">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2c907-167">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="2c907-167">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="2c907-168">Surveiller l’application</span><span class="sxs-lookup"><span data-stu-id="2c907-168">Monitor the app</span></span>

<span data-ttu-id="2c907-169">Apache est désormais configuré pour transférer les requêtes faites à `http://localhost:80` vers l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="2c907-169">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="2c907-170">Cependant, Apache n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2c907-170">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="2c907-171">Utilisez *systemd* pour créer un fichier de service afin de démarrer et surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="2c907-171">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2c907-172">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="2c907-172">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="2c907-173">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="2c907-173">Create the service file</span></span>

<span data-ttu-id="2c907-174">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="2c907-174">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="2c907-175">Exemple de fichier de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="2c907-175">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="2c907-176">Si l’utilisateur *apache* n’est pas utilisé par la configuration, vous devez d’abord créer l’utilisateur et l’affecter en tant que propriétaire des fichiers.</span><span class="sxs-lookup"><span data-stu-id="2c907-176">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="2c907-177">Utilisez `TimeoutStopSec` pour configurer la durée d’attente de l’arrêt de l’application après la réception du signal d’interruption initial.</span><span class="sxs-lookup"><span data-stu-id="2c907-177">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="2c907-178">Si l’application ne s’arrête pas pendant cette période, le signal SIGKILL est émis pour mettre fin à l’application.</span><span class="sxs-lookup"><span data-stu-id="2c907-178">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="2c907-179">Indiquez la valeur en secondes sans unité (par exemple, `150`), une valeur d’intervalle de temps (par exemple, `2min 30s`) ou `infinity` pour désactiver le délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="2c907-179">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="2c907-180">`TimeoutStopSec` prend la valeur par défaut de `DefaultTimeoutStopSec` dans le fichier de configuration du gestionnaire (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*,  *user.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="2c907-180">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="2c907-181">Le délai d’expiration par défaut pour la plupart des distributions est de 90 secondes.</span><span class="sxs-lookup"><span data-stu-id="2c907-181">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="2c907-182">Certaines valeurs (par exemple, les chaînes de connexion SQL) doivent être placées dans une séquence d’échappement afin que les fournisseurs de configuration puissent lire les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="2c907-182">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="2c907-183">Utilisez la commande suivante pour générer une valeur correctement placée dans une séquence d’échappement en vue de son utilisation dans le fichier de configuration :</span><span class="sxs-lookup"><span data-stu-id="2c907-183">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="2c907-184">Enregistrez le fichier et activez le service :</span><span class="sxs-lookup"><span data-stu-id="2c907-184">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="2c907-185">Démarrez le service et vérifiez qu’il s’exécute :</span><span class="sxs-lookup"><span data-stu-id="2c907-185">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="2c907-186">Le proxy inverse étant configuré et Kestrel étant géré via *systemd*, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur la machine locale à l’adresse `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="2c907-186">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2c907-187">Comme l’indiquent les en-têtes de réponse, l’en-tête **Server** montre que l’application ASP.NET Core est traitée par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="2c907-187">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="2c907-188">Afficher les journaux</span><span class="sxs-lookup"><span data-stu-id="2c907-188">View logs</span></span>

<span data-ttu-id="2c907-189">Puisque l’application web utilisant Kestrel est gérée à l’aide de *systemd*, les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="2c907-189">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2c907-190">Cependant, ce journal comprend les entrées de tous les services et processus gérés par *systemd*.</span><span class="sxs-lookup"><span data-stu-id="2c907-190">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="2c907-191">Pour afficher les éléments propres à `kestrel-helloapp.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2c907-191">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="2c907-192">Pour le filtrage du temps, spécifiez les options appropriées dans la commande.</span><span class="sxs-lookup"><span data-stu-id="2c907-192">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="2c907-193">Par exemple, utilisez `--since today` pour filtrer en fonction du jour actuel ou `--until 1 hour ago` pour voir les entrées de l’heure précédente.</span><span class="sxs-lookup"><span data-stu-id="2c907-193">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="2c907-194">Pour plus d’informations, consultez la [page man pour journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="2c907-194">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="2c907-195">Protection des données</span><span class="sxs-lookup"><span data-stu-id="2c907-195">Data protection</span></span>

<span data-ttu-id="2c907-196">La [pile de protection des données ASP.NET Core](xref:security/data-protection/introduction) est utilisée par plusieurs [intergiciels (middleware)](xref:fundamentals/middleware/index) ASP.NET Core, notamment le middleware d’authentification (par exemple, le middleware cookie) et les protections de la falsification de requête intersites (CSRF, Cross Site Request Forgery).</span><span class="sxs-lookup"><span data-stu-id="2c907-196">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="2c907-197">Même si les API de protection des données ne sont pas appelées par le code de l’utilisateur, la protection des données doit être configurée pour créer un [magasin de clés](xref:security/data-protection/implementation/key-management) de chiffrement persistantes.</span><span class="sxs-lookup"><span data-stu-id="2c907-197">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="2c907-198">Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="2c907-198">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="2c907-199">Si le Key Ring est stocké en mémoire, au redémarrage de l’application :</span><span class="sxs-lookup"><span data-stu-id="2c907-199">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="2c907-200">tous les jetons d’authentification basés sur des cookies sont invalidés</span><span class="sxs-lookup"><span data-stu-id="2c907-200">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="2c907-201">Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande.</span><span class="sxs-lookup"><span data-stu-id="2c907-201">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="2c907-202">toutes les données protégées par le Key Ring ne peuvent plus être déchiffrées.</span><span class="sxs-lookup"><span data-stu-id="2c907-202">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="2c907-203">Ceci peut inclure des [jetons CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) et des [cookies TempData ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="2c907-203">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="2c907-204">Pour configurer la protection des données de façon à conserver et chiffrer le porte-clés (Key Ring), consultez :</span><span class="sxs-lookup"><span data-stu-id="2c907-204">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="2c907-205">Sécuriser l’application</span><span class="sxs-lookup"><span data-stu-id="2c907-205">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="2c907-206">Configurer le pare-feu</span><span class="sxs-lookup"><span data-stu-id="2c907-206">Configure firewall</span></span>

<span data-ttu-id="2c907-207">*Firewalld* est un démon dynamique pour gérer le pare-feu avec prise en charge des zones réseau.</span><span class="sxs-lookup"><span data-stu-id="2c907-207">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="2c907-208">Les ports et le filtrage des paquets peuvent toujours être gérés par iptables.</span><span class="sxs-lookup"><span data-stu-id="2c907-208">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="2c907-209">*Firewalld* doit être installé par défaut.</span><span class="sxs-lookup"><span data-stu-id="2c907-209">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="2c907-210">Vous pouvez utiliser `yum` pour installer le package ou vérifier qu’il est installé.</span><span class="sxs-lookup"><span data-stu-id="2c907-210">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="2c907-211">Utilisez `firewalld` pour ouvrir seulement les ports nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="2c907-211">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="2c907-212">Dans ce cas, les ports 80 et 443 sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="2c907-212">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="2c907-213">Les commandes suivantes définissent l’ouverture des ports 80 et 443 de façon permanente :</span><span class="sxs-lookup"><span data-stu-id="2c907-213">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="2c907-214">Rechargez les paramètres du pare-feu.</span><span class="sxs-lookup"><span data-stu-id="2c907-214">Reload the firewall settings.</span></span> <span data-ttu-id="2c907-215">Vérifiez les services et les ports disponibles dans la zone par défaut.</span><span class="sxs-lookup"><span data-stu-id="2c907-215">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="2c907-216">Vous voyez les options disponibles en examinant le résultat de `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="2c907-216">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="https-configuration"></a><span data-ttu-id="2c907-217">Configuration HTTPS</span><span class="sxs-lookup"><span data-stu-id="2c907-217">HTTPS configuration</span></span>

<span data-ttu-id="2c907-218">Pour configurer Apache pour HTTPS, vous utilisez le module *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="2c907-218">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="2c907-219">Le module *mod_ssl* a été installé parallèlement au module *httpd*.</span><span class="sxs-lookup"><span data-stu-id="2c907-219">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="2c907-220">S’il n’a pas été installé, utilisez `yum` pour l’ajouter à la configuration.</span><span class="sxs-lookup"><span data-stu-id="2c907-220">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="2c907-221">Pour mettre en œuvre HTTPS, installez le module `mod_rewrite` afin de permettre la réécriture d’URL :</span><span class="sxs-lookup"><span data-stu-id="2c907-221">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="2c907-222">Modifiez le fichier *helloapp.conf* pour permettre la réécriture d’URL et sécuriser les communications sur le port 443 :</span><span class="sxs-lookup"><span data-stu-id="2c907-222">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="2c907-223">Cet exemple utilise un certificat généré localement.</span><span class="sxs-lookup"><span data-stu-id="2c907-223">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="2c907-224">**SSLCertificateFile** doit être le fichier de certificat principal pour le nom de domaine.</span><span class="sxs-lookup"><span data-stu-id="2c907-224">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="2c907-225">**SSLCertificateKeyFile** doit être le fichier de clé généré quand la demande de signature de certificat est créée.</span><span class="sxs-lookup"><span data-stu-id="2c907-225">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="2c907-226">**SSLCertificateChainFile** doit être le fichier de certificat intermédiaire (le cas échéant) qui a été fourni par l’autorité de certification.</span><span class="sxs-lookup"><span data-stu-id="2c907-226">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="2c907-227">Enregistrez le fichier et testez la configuration :</span><span class="sxs-lookup"><span data-stu-id="2c907-227">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2c907-228">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="2c907-228">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="2c907-229">Autres suggestions pour Apache</span><span class="sxs-lookup"><span data-stu-id="2c907-229">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="2c907-230">En-têtes facultatifs</span><span class="sxs-lookup"><span data-stu-id="2c907-230">Additional headers</span></span>

<span data-ttu-id="2c907-231">Afin d’assurer une protection contre les attaques malveillantes, vous devez modifier ou ajouter quelques en-têtes.</span><span class="sxs-lookup"><span data-stu-id="2c907-231">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="2c907-232">Vérifiez que le module `mod_headers` est installé :</span><span class="sxs-lookup"><span data-stu-id="2c907-232">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="2c907-233">Sécuriser Apache contre les attaques par détournement de clic</span><span class="sxs-lookup"><span data-stu-id="2c907-233">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="2c907-234">Le [détournement de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), également appelé *UI redress attack*, est une attaque malveillante qui amène le visiteur d’un site web à cliquer sur un lien ou un bouton sur une page différente de celle qu’il est en train de visiter.</span><span class="sxs-lookup"><span data-stu-id="2c907-234">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="2c907-235">Utilisez `X-FRAME-OPTIONS` pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="2c907-235">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="2c907-236">Pour atténuer les attaques par détournement de clic :</span><span class="sxs-lookup"><span data-stu-id="2c907-236">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="2c907-237">Modifiez le fichier *httpd.conf* :</span><span class="sxs-lookup"><span data-stu-id="2c907-237">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="2c907-238">Ajoutez la ligne `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="2c907-238">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="2c907-239">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="2c907-239">Save the file.</span></span>
1. <span data-ttu-id="2c907-240">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="2c907-240">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2c907-241">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="2c907-241">MIME-type sniffing</span></span>

<span data-ttu-id="2c907-242">L’en-tête `X-Content-Type-Options` empêche Internet Explorer d’effectuer une *détection de données MIME* (détermination du `Content-Type` d’un fichier à partir de son contenu).</span><span class="sxs-lookup"><span data-stu-id="2c907-242">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="2c907-243">Si le serveur définit l’en-tête `Content-Type` sur `text/html` et que l’option `nosniff` est définie, Internet Explorer restitue le contenu en tant que `text/html`, quel que soit le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="2c907-243">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="2c907-244">Modifiez le fichier *httpd.conf* :</span><span class="sxs-lookup"><span data-stu-id="2c907-244">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2c907-245">Ajoutez la ligne `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="2c907-245">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="2c907-246">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="2c907-246">Save the file.</span></span> <span data-ttu-id="2c907-247">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="2c907-247">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="2c907-248">Équilibrage de charge</span><span class="sxs-lookup"><span data-stu-id="2c907-248">Load Balancing</span></span>

<span data-ttu-id="2c907-249">Cet exemple montre comment installer et configurer Apache sur CentOS 7 et Kestrel sur la même machine de l’instance.</span><span class="sxs-lookup"><span data-stu-id="2c907-249">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="2c907-250">Afin de multiplier les instances en cas de défaillance, l’utilisation de *mod_proxy_balancer* et la modification du **VirtualHost** permettent de gérer plusieurs instances des applications web derrière le serveur proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="2c907-250">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="2c907-251">Dans le fichier de configuration illustré ci-dessous, une instance supplémentaire de `helloapp` est configurée pour s’exécuter sur le port 5001.</span><span class="sxs-lookup"><span data-stu-id="2c907-251">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="2c907-252">La section *Proxy* est définie avec une configuration d’équilibreur qui comprend deux membres pour équilibrer la charge selon la méthode *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="2c907-252">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="2c907-253">Limites du débit</span><span class="sxs-lookup"><span data-stu-id="2c907-253">Rate Limits</span></span>

<span data-ttu-id="2c907-254">Vous pouvez utiliser *mod_ratelimit*, qui est inclus dans le module *httpd*, pour limiter la bande passante des clients :</span><span class="sxs-lookup"><span data-stu-id="2c907-254">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="2c907-255">L’exemple de fichier limite la bande passante à 600 Ko/s sous l’emplacement racine :</span><span class="sxs-lookup"><span data-stu-id="2c907-255">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="2c907-256">Longs champs d'en-tête de demande</span><span class="sxs-lookup"><span data-stu-id="2c907-256">Long request header fields</span></span>

<span data-ttu-id="2c907-257">Si l’application requiert des champs d’en-tête de demande dépassant la limite autorisée par défaut du serveur proxy (généralement 8190 octets), réglez la valeur de la directive [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize).</span><span class="sxs-lookup"><span data-stu-id="2c907-257">If the app requires request header fields longer than permitted by the proxy server's default setting (typically 8,190 bytes), adjust the value of the [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive.</span></span> <span data-ttu-id="2c907-258">La valeur à appliquer dépend du scénario.</span><span class="sxs-lookup"><span data-stu-id="2c907-258">The value to apply is scenario-dependent.</span></span> <span data-ttu-id="2c907-259">Pour plus d'informations, voir la documentation du serveur.</span><span class="sxs-lookup"><span data-stu-id="2c907-259">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="2c907-260">N’augmentez pas la valeur par défaut de `LimitRequestFieldSize` à moins que ce ne soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="2c907-260">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="2c907-261">Son augmentation augmente le risque de dépassement de mémoire tampon (dépassement de capacité) et d’attaques par déni de service (DoS) par des utilisateurs malveillants.</span><span class="sxs-lookup"><span data-stu-id="2c907-261">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c907-262">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2c907-262">Additional resources</span></span>

* [<span data-ttu-id="2c907-263">Prérequis pour .NET Core sur Linux</span><span class="sxs-lookup"><span data-stu-id="2c907-263">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
