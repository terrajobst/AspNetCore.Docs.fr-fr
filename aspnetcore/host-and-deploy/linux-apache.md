---
title: "Héberger ASP.NET Core sur Linux avec Apache"
description: "Découvrez comment configurer Apache comme un serveur de proxy inverse sur CentOS pour rediriger le trafic HTTP pour une application web ASP.NET Core sur Kestrel."
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: b11bc811b6aefce22b60a28afd72c2a2d0b26955
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="a06b8-103">Héberger ASP.NET Core sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="a06b8-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="a06b8-104">Par [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="a06b8-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="a06b8-105">À l’aide de ce guide, découvrez comment configurer [Apache](https://httpd.apache.org/) comme un serveur proxy inverse sur [CentOS 7](https://www.centos.org/) pour rediriger le trafic HTTP pour une application web ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a06b8-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="a06b8-106">Le [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) et créer des modules associés proxy inverse de celui du serveur.</span><span class="sxs-lookup"><span data-stu-id="a06b8-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a06b8-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a06b8-107">Prerequisites</span></span>

1. <span data-ttu-id="a06b8-108">Serveur exécutant CentOS 7 avec un compte d’utilisateur standard avec les privilèges sudo</span><span class="sxs-lookup"><span data-stu-id="a06b8-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="a06b8-109">Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a06b8-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="a06b8-110">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="a06b8-110">Publish the app</span></span>

<span data-ttu-id="a06b8-111">Publier l’application en tant qu’un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) dans la configuration de mise en production pour le runtime CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="a06b8-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="a06b8-112">Copiez le contenu de la *bin/Release/netcoreapp2.0/centos.7-x64/publish* dossier au serveur à l’aide de SCP, FTP ou autre méthode de transfert de fichier.</span><span class="sxs-lookup"><span data-stu-id="a06b8-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="a06b8-113">Dans un scénario de déploiement de production, un flux de travail d’intégration continue effectue le travail de l’application de publication et copie les éléments multimédias sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="a06b8-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="a06b8-114">Configurer un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="a06b8-114">Configure a proxy server</span></span>

<span data-ttu-id="a06b8-115">Un proxy inverse est une installation commune pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="a06b8-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="a06b8-116">Le proxy inverse met fin à la requête HTTP et le transmet à l’application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a06b8-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="a06b8-117">Un proxy est un serveur qui transfère les demandes des clients vers un autre serveur au lieu de répondre aux demandes lui-même.</span><span class="sxs-lookup"><span data-stu-id="a06b8-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="a06b8-118">Un proxy inverse transfère à une destination fixe, en général pour le compte de clients arbitraires.</span><span class="sxs-lookup"><span data-stu-id="a06b8-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="a06b8-119">Dans ce guide, Apache est configuré en tant que le proxy inverse en cours d’exécution sur le même serveur que Kestrel sert à l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a06b8-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="a06b8-120">Étant donné que les demandes sont transmises par le proxy inverse, utilisez l’intergiciel en-têtes transférés à partir de la [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span><span class="sxs-lookup"><span data-stu-id="a06b8-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="a06b8-121">Les mises à jour de l’intergiciel (middleware) le `Request.Scheme`, à l’aide du `X-Forwarded-Proto` en-tête, afin qu’URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="a06b8-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="a06b8-122">Lorsque vous utilisez n’importe quel type d’intergiciel (middleware) d’authentification, les en-têtes transféré intergiciel (middleware) doit exécuter en premier.</span><span class="sxs-lookup"><span data-stu-id="a06b8-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="a06b8-123">Cette commande garantit que l’intergiciel (middleware) d’authentification permettre consommer les valeurs d’en-tête et générer l’URI de redirection correcte.</span><span class="sxs-lookup"><span data-stu-id="a06b8-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a06b8-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a06b8-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a06b8-125">Appeler le [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) méthode dans `Startup.Configure` avant d’appeler [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou intergiciel (middleware) du schéma d’authentification similaire :</span><span class="sxs-lookup"><span data-stu-id="a06b8-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a06b8-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a06b8-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a06b8-127">Appeler le [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) méthode dans `Startup.Configure` avant d’appeler [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) et [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou le schéma d’authentification similaires intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="a06b8-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="a06b8-128">Si aucun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sont spécifiés à l’intergiciel (middleware), les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="a06b8-129">Installer Apache</span><span class="sxs-lookup"><span data-stu-id="a06b8-129">Install Apache</span></span>

<span data-ttu-id="a06b8-130">Mettre à jour les packages CentOS vers leur dernière version stable :</span><span class="sxs-lookup"><span data-stu-id="a06b8-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="a06b8-131">Installer le serveur web Apache sur CentOS avec une seule `yum` commande :</span><span class="sxs-lookup"><span data-stu-id="a06b8-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="a06b8-132">Exemple de sortie après l’exécution de la commande :</span><span class="sxs-lookup"><span data-stu-id="a06b8-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="a06b8-133">Dans cet exemple, la sortie reflète httpd.86_64 depuis la version CentOS 7 est de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="a06b8-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="a06b8-134">Pour vérifier où Apache est installé, exécutez `whereis httpd` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="a06b8-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="a06b8-135">Configurer Apache pour le proxy inverse</span><span class="sxs-lookup"><span data-stu-id="a06b8-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="a06b8-136">Les fichiers de configuration pour Apache se trouvent dans le répertoire `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="a06b8-137">Tout fichier portant le *.conf* extension est traitée dans l’ordre alphabétique en plus des fichiers de configuration de module dans `/etc/httpd/conf.modules.d/`, qui contient toutes les configurations de fichiers nécessaire au chargement des modules.</span><span class="sxs-lookup"><span data-stu-id="a06b8-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="a06b8-138">Créer un fichier de configuration pour l’application nommée `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="a06b8-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="a06b8-139">Le **VirtualHost** nœud peut apparaître plusieurs fois dans un ou plusieurs fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="a06b8-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="a06b8-140">**VirtualHost** est définie pour écouter sur n’importe quelle adresse IP à l’aide du port 80.</span><span class="sxs-lookup"><span data-stu-id="a06b8-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="a06b8-141">Les deux lignes sont définies pour les demandes à la racine à 127.0.0.1 dans le serveur proxy sur le port 5000.</span><span class="sxs-lookup"><span data-stu-id="a06b8-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="a06b8-142">Pour la communication bidirectionnelle, *ProxyPass* et *ProxyPassReverse* sont requis.</span><span class="sxs-lookup"><span data-stu-id="a06b8-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="a06b8-143">La journalisation peut être configurée par **VirtualHost** à l’aide de **ErrorLog** et **CustomLog** directives.</span><span class="sxs-lookup"><span data-stu-id="a06b8-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="a06b8-144">**Journal des erreurs** est l’emplacement où le serveur enregistre les erreurs, et **CustomLog** définit le nom de fichier et le format du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="a06b8-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="a06b8-145">Il s’agit dans ce cas, où les informations de demande sont consignées.</span><span class="sxs-lookup"><span data-stu-id="a06b8-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="a06b8-146">Il existe une ligne pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="a06b8-146">There's one line for each request.</span></span>

<span data-ttu-id="a06b8-147">Enregistrez le fichier et la configuration de test.</span><span class="sxs-lookup"><span data-stu-id="a06b8-147">Save the file and test the configuration.</span></span> <span data-ttu-id="a06b8-148">Si tout réussit, la réponse doit être `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="a06b8-149">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="a06b8-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="a06b8-150">Surveillance de l’application</span><span class="sxs-lookup"><span data-stu-id="a06b8-150">Monitoring the app</span></span>

<span data-ttu-id="a06b8-151">Apache est désormais configuré pour transférer les demandes faites à `http://localhost:80` à l’application ASP.NET Core s’exécutant sur Kestrel à `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="a06b8-152">Toutefois, Apache n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a06b8-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="a06b8-153">Utilisez *systemd* et créer un fichier de service pour démarrer et surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="a06b8-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="a06b8-154">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="a06b8-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="a06b8-155">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="a06b8-155">Create the service file</span></span>

<span data-ttu-id="a06b8-156">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="a06b8-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="a06b8-157">Un exemple de fichier service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="a06b8-157">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="a06b8-158">**Utilisateur** &mdash; si l’utilisateur *apache* n’est pas utilisé par la configuration, l’utilisateur doit être créé en premier et étant donné la propriété appropriée pour les fichiers.</span><span class="sxs-lookup"><span data-stu-id="a06b8-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="a06b8-159">Enregistrez le fichier et activer le service :</span><span class="sxs-lookup"><span data-stu-id="a06b8-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="a06b8-160">Démarrez le service et vérifiez qu’il s’exécute :</span><span class="sxs-lookup"><span data-stu-id="a06b8-160">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="a06b8-161">Avec la configuration de proxy inverse et Kestrel gérés via *systemd*, l’application web est entièrement configurée et qu’il est accessible à partir d’un navigateur sur l’ordinateur local à `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="a06b8-162">Inspecter les en-têtes de réponse, le **Server** en-tête indique que l’application ASP.NET Core est pris en charge par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="a06b8-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="a06b8-163">Affichage des journaux</span><span class="sxs-lookup"><span data-stu-id="a06b8-163">Viewing logs</span></span>

<span data-ttu-id="a06b8-164">Depuis l’application web à l’aide de Kestrel est géré à l’aide de *systemd*, processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="a06b8-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="a06b8-165">Toutefois, ce journal inclut des entrées pour tous les services et les processus gérés par *systemd*.</span><span class="sxs-lookup"><span data-stu-id="a06b8-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="a06b8-166">Pour afficher les éléments propres à `kestrel-hellomvc.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a06b8-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="a06b8-167">Pour le filtrage du temps, spécifiez les options d’exécution avec la commande.</span><span class="sxs-lookup"><span data-stu-id="a06b8-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="a06b8-168">Par exemple, utilisez `--since today` pour filtrer le jour actuel ou `--until 1 hour ago` pour afficher les entrées de l’heure précédente.</span><span class="sxs-lookup"><span data-stu-id="a06b8-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="a06b8-169">Pour plus d’informations, consultez la [page manuel journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="a06b8-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="a06b8-170">Sécurisation de l’application</span><span class="sxs-lookup"><span data-stu-id="a06b8-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="a06b8-171">Configurer le pare-feu</span><span class="sxs-lookup"><span data-stu-id="a06b8-171">Configure firewall</span></span>

<span data-ttu-id="a06b8-172">*Firewalld* est un démon dynamique pour gérer le pare-feu avec prise en charge pour les zones du réseau.</span><span class="sxs-lookup"><span data-stu-id="a06b8-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="a06b8-173">Ports et le filtrage des paquets peuvent toujours être gérés par iptables.</span><span class="sxs-lookup"><span data-stu-id="a06b8-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="a06b8-174">*Firewalld* doit être installé par défaut.</span><span class="sxs-lookup"><span data-stu-id="a06b8-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="a06b8-175">`yum` peut être utilisé pour installer le package ou de vérifier qu’il est installé.</span><span class="sxs-lookup"><span data-stu-id="a06b8-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="a06b8-176">Utilisez `firewalld` à ouvrir uniquement les ports nécessaires pour l’application.</span><span class="sxs-lookup"><span data-stu-id="a06b8-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="a06b8-177">Dans ce cas, les ports 80 et 443 sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="a06b8-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="a06b8-178">Les commandes suivantes à définir les ports 80 et 443 pour ouvrir de manière permanente :</span><span class="sxs-lookup"><span data-stu-id="a06b8-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="a06b8-179">Recharger les paramètres du pare-feu.</span><span class="sxs-lookup"><span data-stu-id="a06b8-179">Reload the firewall settings.</span></span> <span data-ttu-id="a06b8-180">Vérifiez les services disponibles et les ports de la zone par défaut.</span><span class="sxs-lookup"><span data-stu-id="a06b8-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="a06b8-181">Options sont disponibles en inspectant `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="a06b8-182">Configuration de SSL</span><span class="sxs-lookup"><span data-stu-id="a06b8-182">SSL configuration</span></span>

<span data-ttu-id="a06b8-183">Pour configurer Apache pour SSL, le *mod_ssl* module est utilisé.</span><span class="sxs-lookup"><span data-stu-id="a06b8-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="a06b8-184">Lorsque le *httpd* module a été installé, le *mod_ssl* module a également été installé.</span><span class="sxs-lookup"><span data-stu-id="a06b8-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="a06b8-185">S’il n’a pas été installé, utilisez `yum` pour l’ajouter à la configuration.</span><span class="sxs-lookup"><span data-stu-id="a06b8-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="a06b8-186">Pour mettre en œuvre SSL, vous devez installer le `mod_rewrite` module pour permettre la réécriture d’URL :</span><span class="sxs-lookup"><span data-stu-id="a06b8-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="a06b8-187">Modifier la *hellomvc.conf* fichier pour permettre la réécriture d’URL et de sécuriser les communications sur le port 443 :</span><span class="sxs-lookup"><span data-stu-id="a06b8-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="a06b8-188">Cet exemple utilise un certificat généré localement.</span><span class="sxs-lookup"><span data-stu-id="a06b8-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="a06b8-189">**SSLCertificateFile** doit être le fichier de certificat principal pour le nom de domaine.</span><span class="sxs-lookup"><span data-stu-id="a06b8-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="a06b8-190">**SSLCertificateKeyFile** doit être le fichier de clé généré lors de la création de signature de certificat.</span><span class="sxs-lookup"><span data-stu-id="a06b8-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="a06b8-191">**SSLCertificateChainFile** doit être le fichier de certificat intermédiaire (le cas échéant) qui a été fourni par l’autorité de certification.</span><span class="sxs-lookup"><span data-stu-id="a06b8-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="a06b8-192">Enregistrez le fichier et la configuration de test :</span><span class="sxs-lookup"><span data-stu-id="a06b8-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="a06b8-193">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="a06b8-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="a06b8-194">Autres suggestions pour Apache</span><span class="sxs-lookup"><span data-stu-id="a06b8-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="a06b8-195">En-têtes supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a06b8-195">Additional headers</span></span>

<span data-ttu-id="a06b8-196">Afin de sécuriser contre les attaques, il existe quelques en-têtes qui doivent être modifiés ou ajoutés.</span><span class="sxs-lookup"><span data-stu-id="a06b8-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="a06b8-197">Vérifiez que le `mod_headers` module est installé :</span><span class="sxs-lookup"><span data-stu-id="a06b8-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="a06b8-198">Apache sécurisé contre les attaques de clickjacking</span><span class="sxs-lookup"><span data-stu-id="a06b8-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="a06b8-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), également appelé un *UI recours attaque*, est une attaque malveillante où l’exécute un visiteur de site Web en cliquant sur un lien ou un bouton sur une autre page, qu’ils vous visitez actuellement.</span><span class="sxs-lookup"><span data-stu-id="a06b8-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="a06b8-200">Utilisez `X-FRAME-OPTIONS` pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="a06b8-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="a06b8-201">Modifier la *httpd.conf* fichier :</span><span class="sxs-lookup"><span data-stu-id="a06b8-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="a06b8-202">Ajoutez la ligne `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="a06b8-203">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="a06b8-203">Save the file.</span></span> <span data-ttu-id="a06b8-204">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="a06b8-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="a06b8-205">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="a06b8-205">MIME-type sniffing</span></span>

<span data-ttu-id="a06b8-206">Le `X-Content-Type-Options` en-tête empêche Internet Explorer de *détection MIME sur* (détermination d’un fichier `Content-Type` dans le contenu du fichier).</span><span class="sxs-lookup"><span data-stu-id="a06b8-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="a06b8-207">Si le serveur définit le `Content-Type` en-tête `text/html` avec la `nosniff` option est définie, Internet Explorer restitue le contenu en tant que `text/html` , quelle que soit le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="a06b8-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="a06b8-208">Modifier la *httpd.conf* fichier :</span><span class="sxs-lookup"><span data-stu-id="a06b8-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="a06b8-209">Ajoutez la ligne `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="a06b8-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="a06b8-210">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="a06b8-210">Save the file.</span></span> <span data-ttu-id="a06b8-211">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="a06b8-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="a06b8-212">Équilibrage de charge</span><span class="sxs-lookup"><span data-stu-id="a06b8-212">Load Balancing</span></span> 

<span data-ttu-id="a06b8-213">Cet exemple montre comment installer et configurer Apache sur CentOS 7 et Kestrel sur la même machine de l’instance.</span><span class="sxs-lookup"><span data-stu-id="a06b8-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="a06b8-214">Afin de ne pas disposer d’un seul point de défaillance ; à l’aide de *mod_proxy_balancer* et la modification de la **VirtualHost** permettrait permettant de gérer plusieurs instances des applications web derrière le serveur proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="a06b8-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="a06b8-215">Dans le fichier de configuration illustré ci-dessous, une instance supplémentaire de la `hellomvc` application est configurée pour s’exécuter sur le port 5001.</span><span class="sxs-lookup"><span data-stu-id="a06b8-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="a06b8-216">Le *Proxy* section est définie avec une configuration d’équilibrage avec deux membres pour équilibrer la charge *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="a06b8-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="a06b8-217">Limites du débit</span><span class="sxs-lookup"><span data-stu-id="a06b8-217">Rate Limits</span></span>
<span data-ttu-id="a06b8-218">À l’aide de *mod_ratelimit*, qui est inclus dans le *httpd* module, la bande passante de clients peut être limitée :</span><span class="sxs-lookup"><span data-stu-id="a06b8-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="a06b8-219">L’exemple de fichier limite la bande passante 600 Ko/s sous l’emplacement racine :</span><span class="sxs-lookup"><span data-stu-id="a06b8-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
