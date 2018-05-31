---
title: Héberger ASP.NET Core sur Linux avec Apache
description: Découvrez comment configurer Apache comme serveur proxy inverse sur CentOS pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962815"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="1e091-103">Héberger ASP.NET Core sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="1e091-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="1e091-104">Par [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="1e091-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="1e091-105">À l’aide de ce guide, découvrez comment configurer [Apache](https://httpd.apache.org/) comme serveur proxy inverse sur [CentOS 7](https://www.centos.org/) pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1e091-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="1e091-106">[L’extension mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) et les modules associés créent le proxy inverse du serveur.</span><span class="sxs-lookup"><span data-stu-id="1e091-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e091-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="1e091-107">Prerequisites</span></span>

1. <span data-ttu-id="1e091-108">Serveur exécutant CentOS 7 avec un compte d’utilisateur standard disposant du privilège sudo</span><span class="sxs-lookup"><span data-stu-id="1e091-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="1e091-109">Application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e091-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="1e091-110">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="1e091-110">Publish the app</span></span>

<span data-ttu-id="1e091-111">Publiez l’application en tant que [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) dans une configuration Release pour le runtime CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="1e091-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="1e091-112">Copiez le contenu du dossier *bin/Release/netcoreapp2.0/centos.7-x64/publish* sur le serveur à l’aide de SCP, FTP ou d’une autre méthode de transfert de fichier.</span><span class="sxs-lookup"><span data-stu-id="1e091-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="1e091-113">Dans un scénario de déploiement en production, un workflow d’intégration continue effectue le travail de publication de l’application et de copie des composants sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1e091-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="1e091-114">Configurer un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="1e091-114">Configure a proxy server</span></span>

<span data-ttu-id="1e091-115">Un proxy inverse est une configuration courante pour traiter les applications web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="1e091-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="1e091-116">Le proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e091-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="1e091-117">Un serveur proxy est un serveur qui transfère les requêtes des clients à un autre serveur, au lieu de traiter celles-ci lui-même.</span><span class="sxs-lookup"><span data-stu-id="1e091-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="1e091-118">Un proxy inverse transfère à une destination fixe, en général pour le compte de clients arbitraires.</span><span class="sxs-lookup"><span data-stu-id="1e091-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="1e091-119">Dans ce guide, Apache est configuré comme proxy inverse s’exécutant sur le même serveur où Kestrel traite l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e091-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="1e091-120">Les requêtes étant transférées par le proxy inverse, utilisez le middleware (intergiciel) des en-têtes transférés à partir du package [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="1e091-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="1e091-121">Le middleware met à jour le `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="1e091-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="1e091-122">Quand vous utilisez n’importe quel type de middleware d’authentification, le middleware des en-têtes transférés doit s’exécuter en premier.</span><span class="sxs-lookup"><span data-stu-id="1e091-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="1e091-123">Cet ordre permet d’être sûr que le middleware d’authentification peut consommer les valeurs d’en-tête et générer l’URI de redirection correct.</span><span class="sxs-lookup"><span data-stu-id="1e091-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1e091-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1e091-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1e091-125">Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou un middleware de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="1e091-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="1e091-126">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="1e091-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1e091-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1e091-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1e091-128">Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) et [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou un middleware de schéma d’authentification similaire.</span><span class="sxs-lookup"><span data-stu-id="1e091-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="1e091-129">Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :</span><span class="sxs-lookup"><span data-stu-id="1e091-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="1e091-130">Si aucune option [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) n’est spécifiée au middleware, les en-têtes par défaut à transférer sont `None`.</span><span class="sxs-lookup"><span data-stu-id="1e091-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="1e091-131">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge.</span><span class="sxs-lookup"><span data-stu-id="1e091-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="1e091-132">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="1e091-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="1e091-133">Installer Apache</span><span class="sxs-lookup"><span data-stu-id="1e091-133">Install Apache</span></span>

<span data-ttu-id="1e091-134">Mettez à jour les packages CentOS vers leur dernière version stable :</span><span class="sxs-lookup"><span data-stu-id="1e091-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="1e091-135">Installez le serveur web Apache sur CentOS avec une seule commande `yum` :</span><span class="sxs-lookup"><span data-stu-id="1e091-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="1e091-136">Exemple de sortie après exécution de la commande :</span><span class="sxs-lookup"><span data-stu-id="1e091-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="1e091-137">Dans cet exemple, la sortie correspond à httpd.86_64, car la version de CentOS 7 est en 64 bits.</span><span class="sxs-lookup"><span data-stu-id="1e091-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="1e091-138">Pour vérifier où Apache est installé, exécutez `whereis httpd` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="1e091-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="1e091-139">Configurer Apache pour le proxy inverse</span><span class="sxs-lookup"><span data-stu-id="1e091-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="1e091-140">Les fichiers de configuration pour Apache se trouvent dans le répertoire `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="1e091-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="1e091-141">Les fichiers avec l’extension *.conf* sont traités par ordre alphabétique en plus des fichiers de configuration des modules dans `/etc/httpd/conf.modules.d/`, qui contient tous les fichiers de configuration nécessaires au chargement des modules.</span><span class="sxs-lookup"><span data-stu-id="1e091-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="1e091-142">Créez un fichier de configuration nommé *hellomvc.conf*, pour l’application :</span><span class="sxs-lookup"><span data-stu-id="1e091-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="1e091-143">Le bloc `VirtualHost` peut apparaître plusieurs fois, dans un ou plusieurs fichiers sur un serveur.</span><span class="sxs-lookup"><span data-stu-id="1e091-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="1e091-144">Dans le fichier de configuration précédent, Apache accepte le trafic public sur le port 80.</span><span class="sxs-lookup"><span data-stu-id="1e091-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="1e091-145">Le domaine `www.example.com` est pris en charge, et l’alias `*.example.com` aboutit au même site web.</span><span class="sxs-lookup"><span data-stu-id="1e091-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="1e091-146">Pour plus d’informations, consultez [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Prise en charge des hôtes virtuels par nom).</span><span class="sxs-lookup"><span data-stu-id="1e091-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="1e091-147">Les requêtes sont transmises par proxy à la racine vers le port 5000 du serveur sur 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="1e091-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="1e091-148">Pour la communication bidirectionnelle, `ProxyPass` et `ProxyPassReverse` sont requis.</span><span class="sxs-lookup"><span data-stu-id="1e091-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="1e091-149">La spécification d’une [directive ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) incorrecte dans le bloc **VirtualHost** expose votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="1e091-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="1e091-150">Une liaison générique de sous-domaine (par exemple, `*.example.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="1e091-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1e091-151">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="1e091-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="1e091-152">Vous pouvez configurer la journalisation par `VirtualHost` à l’aide des directives `ErrorLog` et `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="1e091-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="1e091-153">`ErrorLog` est l’emplacement où le serveur journalise les erreurs, tandis que `CustomLog` définit le nom de fichier et le format du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="1e091-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="1e091-154">Dans ce cas, c’est l’endroit où les informations des requêtes sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="1e091-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="1e091-155">Il y a une ligne pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="1e091-155">There's one line for each request.</span></span>

<span data-ttu-id="1e091-156">Enregistrez le fichier et testez la configuration.</span><span class="sxs-lookup"><span data-stu-id="1e091-156">Save the file and test the configuration.</span></span> <span data-ttu-id="1e091-157">Si tout réussit, la réponse doit être `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="1e091-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="1e091-158">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="1e091-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="1e091-159">Surveillance de l’application</span><span class="sxs-lookup"><span data-stu-id="1e091-159">Monitoring the app</span></span>

<span data-ttu-id="1e091-160">Apache est désormais configuré pour transférer les requêtes faites à `http://localhost:80` vers l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="1e091-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="1e091-161">Cependant, Apache n’est pas configuré pour gérer le processus Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1e091-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="1e091-162">Utilisez *systemd* pour créer un fichier de service afin de démarrer et surveiller l’application web sous-jacente.</span><span class="sxs-lookup"><span data-stu-id="1e091-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="1e091-163">*systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="1e091-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="1e091-164">Créer le fichier de service</span><span class="sxs-lookup"><span data-stu-id="1e091-164">Create the service file</span></span>

<span data-ttu-id="1e091-165">Créez le fichier de définition de service :</span><span class="sxs-lookup"><span data-stu-id="1e091-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="1e091-166">Exemple de fichier de service pour l’application :</span><span class="sxs-lookup"><span data-stu-id="1e091-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="1e091-167">**Utilisateur** &mdash; Si l’utilisateur *apache* n’est pas utilisé par la configuration, vous devez d’abord créer l’utilisateur et l’affecter en tant que propriétaire des fichiers.</span><span class="sxs-lookup"><span data-stu-id="1e091-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="1e091-168">Certaines valeurs (par exemple, les chaînes de connexion SQL) doivent être placées dans une séquence d’échappement afin que les fournisseurs de configuration puissent lire les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="1e091-168">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="1e091-169">Utilisez la commande suivante pour générer une valeur correctement placée dans une séquence d’échappement en vue de son utilisation dans le fichier de configuration :</span><span class="sxs-lookup"><span data-stu-id="1e091-169">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="1e091-170">Enregistrez le fichier et activez le service :</span><span class="sxs-lookup"><span data-stu-id="1e091-170">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="1e091-171">Démarrez le service et vérifiez qu’il s’exécute :</span><span class="sxs-lookup"><span data-stu-id="1e091-171">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="1e091-172">Le proxy inverse étant configuré et Kestrel étant géré via *systemd*, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur la machine locale à l’adresse `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1e091-172">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="1e091-173">Comme l’indiquent les en-têtes de réponse, l’en-tête **Server** montre que l’application ASP.NET Core est traitée par Kestrel :</span><span class="sxs-lookup"><span data-stu-id="1e091-173">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="1e091-174">Affichage des journaux</span><span class="sxs-lookup"><span data-stu-id="1e091-174">Viewing logs</span></span>

<span data-ttu-id="1e091-175">Puisque l’application web utilisant Kestrel est gérée à l’aide de *systemd*, les processus et les événements sont enregistrés dans un journal centralisé.</span><span class="sxs-lookup"><span data-stu-id="1e091-175">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="1e091-176">Cependant, ce journal comprend les entrées de tous les services et processus gérés par *systemd*.</span><span class="sxs-lookup"><span data-stu-id="1e091-176">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="1e091-177">Pour afficher les éléments propres à `kestrel-hellomvc.service`, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1e091-177">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="1e091-178">Pour le filtrage du temps, spécifiez les options appropriées dans la commande.</span><span class="sxs-lookup"><span data-stu-id="1e091-178">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="1e091-179">Par exemple, utilisez `--since today` pour filtrer en fonction du jour actuel ou `--until 1 hour ago` pour voir les entrées de l’heure précédente.</span><span class="sxs-lookup"><span data-stu-id="1e091-179">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="1e091-180">Pour plus d’informations, consultez la [page man pour journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="1e091-180">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="1e091-181">Sécurisation de l’application</span><span class="sxs-lookup"><span data-stu-id="1e091-181">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="1e091-182">Configurer le pare-feu</span><span class="sxs-lookup"><span data-stu-id="1e091-182">Configure firewall</span></span>

<span data-ttu-id="1e091-183">*Firewalld* est un démon dynamique pour gérer le pare-feu avec prise en charge des zones réseau.</span><span class="sxs-lookup"><span data-stu-id="1e091-183">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="1e091-184">Les ports et le filtrage des paquets peuvent toujours être gérés par iptables.</span><span class="sxs-lookup"><span data-stu-id="1e091-184">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="1e091-185">*Firewalld* doit être installé par défaut.</span><span class="sxs-lookup"><span data-stu-id="1e091-185">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="1e091-186">Vous pouvez utiliser `yum` pour installer le package ou vérifier qu’il est installé.</span><span class="sxs-lookup"><span data-stu-id="1e091-186">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="1e091-187">Utilisez `firewalld` pour ouvrir seulement les ports nécessaires à l’application.</span><span class="sxs-lookup"><span data-stu-id="1e091-187">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="1e091-188">Dans ce cas, les ports 80 et 443 sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="1e091-188">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="1e091-189">Les commandes suivantes définissent l’ouverture des ports 80 et 443 de façon permanente :</span><span class="sxs-lookup"><span data-stu-id="1e091-189">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="1e091-190">Rechargez les paramètres du pare-feu.</span><span class="sxs-lookup"><span data-stu-id="1e091-190">Reload the firewall settings.</span></span> <span data-ttu-id="1e091-191">Vérifiez les services et les ports disponibles dans la zone par défaut.</span><span class="sxs-lookup"><span data-stu-id="1e091-191">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="1e091-192">Vous voyez les options disponibles en examinant le résultat de `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="1e091-192">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="1e091-193">Configuration de SSL</span><span class="sxs-lookup"><span data-stu-id="1e091-193">SSL configuration</span></span>

<span data-ttu-id="1e091-194">Pour configurer Apache pour SSL, vous utilisez le module *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="1e091-194">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="1e091-195">Le module *mod_ssl* a été installé parallèlement au module *httpd*.</span><span class="sxs-lookup"><span data-stu-id="1e091-195">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="1e091-196">S’il n’a pas été installé, utilisez `yum` pour l’ajouter à la configuration.</span><span class="sxs-lookup"><span data-stu-id="1e091-196">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="1e091-197">Pour mettre en œuvre SSL, installez le module `mod_rewrite` afin de permettre la réécriture d’URL :</span><span class="sxs-lookup"><span data-stu-id="1e091-197">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="1e091-198">Modifiez le fichier *hellomvc.conf* pour permettre la réécriture d’URL et sécuriser les communications sur le port 443 :</span><span class="sxs-lookup"><span data-stu-id="1e091-198">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="1e091-199">Cet exemple utilise un certificat généré localement.</span><span class="sxs-lookup"><span data-stu-id="1e091-199">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="1e091-200">**SSLCertificateFile** doit être le fichier de certificat principal pour le nom de domaine.</span><span class="sxs-lookup"><span data-stu-id="1e091-200">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="1e091-201">**SSLCertificateKeyFile** doit être le fichier de clé généré quand la demande de signature de certificat est créée.</span><span class="sxs-lookup"><span data-stu-id="1e091-201">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="1e091-202">**SSLCertificateChainFile** doit être le fichier de certificat intermédiaire (le cas échéant) qui a été fourni par l’autorité de certification.</span><span class="sxs-lookup"><span data-stu-id="1e091-202">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="1e091-203">Enregistrez le fichier et testez la configuration :</span><span class="sxs-lookup"><span data-stu-id="1e091-203">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="1e091-204">Redémarrez Apache :</span><span class="sxs-lookup"><span data-stu-id="1e091-204">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="1e091-205">Autres suggestions pour Apache</span><span class="sxs-lookup"><span data-stu-id="1e091-205">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="1e091-206">En-têtes facultatifs</span><span class="sxs-lookup"><span data-stu-id="1e091-206">Additional headers</span></span>

<span data-ttu-id="1e091-207">Afin d’assurer une protection contre les attaques malveillantes, vous devez modifier ou ajouter quelques en-têtes.</span><span class="sxs-lookup"><span data-stu-id="1e091-207">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="1e091-208">Vérifiez que le module `mod_headers` est installé :</span><span class="sxs-lookup"><span data-stu-id="1e091-208">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="1e091-209">Sécuriser Apache contre les attaques par détournement de clic</span><span class="sxs-lookup"><span data-stu-id="1e091-209">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="1e091-210">Le [détournement de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), également appelé *UI redress attack*, est une attaque malveillante qui amène le visiteur d’un site web à cliquer sur un lien ou un bouton sur une page différente de celle qu’il est en train de visiter.</span><span class="sxs-lookup"><span data-stu-id="1e091-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="1e091-211">Utilisez `X-FRAME-OPTIONS` pour sécuriser le site.</span><span class="sxs-lookup"><span data-stu-id="1e091-211">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="1e091-212">Modifiez le fichier *httpd.conf* :</span><span class="sxs-lookup"><span data-stu-id="1e091-212">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="1e091-213">Ajoutez la ligne `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="1e091-213">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="1e091-214">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="1e091-214">Save the file.</span></span> <span data-ttu-id="1e091-215">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="1e091-215">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="1e091-216">Détection de type MIME</span><span class="sxs-lookup"><span data-stu-id="1e091-216">MIME-type sniffing</span></span>

<span data-ttu-id="1e091-217">L’en-tête `X-Content-Type-Options` empêche Internet Explorer d’effectuer une *détection de données MIME* (détermination du `Content-Type` d’un fichier à partir de son contenu).</span><span class="sxs-lookup"><span data-stu-id="1e091-217">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="1e091-218">Si le serveur définit l’en-tête `Content-Type` sur `text/html` et que l’option `nosniff` est définie, Internet Explorer restitue le contenu en tant que `text/html`, quel que soit le contenu du fichier.</span><span class="sxs-lookup"><span data-stu-id="1e091-218">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="1e091-219">Modifiez le fichier *httpd.conf* :</span><span class="sxs-lookup"><span data-stu-id="1e091-219">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="1e091-220">Ajoutez la ligne `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="1e091-220">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="1e091-221">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="1e091-221">Save the file.</span></span> <span data-ttu-id="1e091-222">Redémarrez Apache.</span><span class="sxs-lookup"><span data-stu-id="1e091-222">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="1e091-223">Équilibrage de charge</span><span class="sxs-lookup"><span data-stu-id="1e091-223">Load Balancing</span></span> 

<span data-ttu-id="1e091-224">Cet exemple montre comment installer et configurer Apache sur CentOS 7 et Kestrel sur la même machine de l’instance.</span><span class="sxs-lookup"><span data-stu-id="1e091-224">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="1e091-225">Afin de multiplier les instances en cas de défaillance, l’utilisation de *mod_proxy_balancer* et la modification du **VirtualHost** permettent de gérer plusieurs instances des applications web derrière le serveur proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="1e091-225">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="1e091-226">Dans le fichier de configuration illustré ci-dessous, une instance supplémentaire de l’application `hellomvc` est configurée pour s’exécuter sur le port 5001.</span><span class="sxs-lookup"><span data-stu-id="1e091-226">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="1e091-227">La section *Proxy* est définie avec une configuration d’équilibreur qui comprend deux membres pour équilibrer la charge selon la méthode *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="1e091-227">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="1e091-228">Limites du débit</span><span class="sxs-lookup"><span data-stu-id="1e091-228">Rate Limits</span></span>
<span data-ttu-id="1e091-229">Vous pouvez utiliser *mod_ratelimit*, qui est inclus dans le module *httpd*, pour limiter la bande passante des clients :</span><span class="sxs-lookup"><span data-stu-id="1e091-229">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="1e091-230">L’exemple de fichier limite la bande passante à 600 Ko/s sous l’emplacement racine :</span><span class="sxs-lookup"><span data-stu-id="1e091-230">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
