---
title: Informations de référence sur la configuration du Module ASP.NET Core
author: guardrex
description: Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 5a3fd9c3453c07ee550c7de0333c9a49d5d5d1af
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450656"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="77abb-103">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77abb-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="77abb-104">Par [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) et [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="77abb-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="77abb-105">Ce document fournit des instructions permettant de configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77abb-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="77abb-106">Pour obtenir une présentation du module ASP.NET Core et des instructions d’installation, consultez la [vue d’ensemble du module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="77abb-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="77abb-107">Modèle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="77abb-107">Hosting model</span></span>

<span data-ttu-id="77abb-108">Pour les applications exécutées sur .NET Core 2.2 ou version ultérieure, le module prend en charge un modèle d’hébergement in-process pour améliorer les performances par rapport à l’hébergement de proxy inverse (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="77abb-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="77abb-109">Pour plus d'informations, consultez <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="77abb-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="77abb-110">L’hébergement in-process est au choix pour les applications existantes, mais les modèles [dotnet new](/dotnet/core/tools/dotnet-new) sont définis par défaut sur le modèle d’hébergement in-process pour tous les scénarios IIS et IIS Express.</span><span class="sxs-lookup"><span data-stu-id="77abb-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="77abb-111">Pour configurer l’hébergement in-process dans une application, ajoutez la propriété `<AspNetCoreHostingModel>` au fichier projet de l’application (par exemple, *MyApp.csproj*) en lui affectant la valeur `inprocess` (l’hébergement out-of-process est défini avec `outofprocess`) :</span><span class="sxs-lookup"><span data-stu-id="77abb-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="77abb-112">Les caractéristiques suivantes s’appliquent lors de l’hébergement in-process :</span><span class="sxs-lookup"><span data-stu-id="77abb-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="77abb-113">Le [serveur Kestrel](xref:fundamentals/servers/kestrel) n’est pas utilisé.</span><span class="sxs-lookup"><span data-stu-id="77abb-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="77abb-114">Implémentation <xref:Microsoft.AspNetCore.Hosting.Server.IServer> personnalisée, `IISHttpServer` joue le rôle de serveur de l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="77abb-115">L’[attribut requestTimeout](#attributes-of-the-aspnetcore-element) ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="77abb-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="77abb-116">Le partage d’un pool d’applications entre les applications n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="77abb-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="77abb-117">Utilisez un pool d’applications par application.</span><span class="sxs-lookup"><span data-stu-id="77abb-117">Use one app pool per app.</span></span>

* <span data-ttu-id="77abb-118">Lorsque vous utilisez [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou que vous placez manuellement un [fichier app_offline.htm dans le déploiement](xref:host-and-deploy/iis/index#locked-deployment-files), l’application peut ne pas s’arrêter immédiatement si une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="77abb-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="77abb-119">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="77abb-120">L’architecture (nombre de bits) de l’application et le runtime installé (x64 ou x86) doivent correspondre à l’architecture du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="77abb-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="77abb-121">Si vous configurez manuellement l’hôte de l’application avec `WebHostBuilder` (et non [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)), et que l’application est exécutée directement sur le serveur Kestrel (auto-hébergée), appelez `UseKestrel` avant d’appeler `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="77abb-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="77abb-122">Si l’ordre est inversé, l’hôte ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="77abb-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="77abb-123">Les déconnexions du client sont détectées.</span><span class="sxs-lookup"><span data-stu-id="77abb-123">Client disconnects are detected.</span></span> <span data-ttu-id="77abb-124">Le jeton d’annulation [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) est annulé quand le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="77abb-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="77abb-125">`Directory.GetCurrentDirectory()` retourne le répertoire de travail du processus démarré par IIS, et non le répertoire de l’application (par exemple, *C:\Windows\System32\inetsrv* pour *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="77abb-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="77abb-126">Modifications du modèle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="77abb-126">Hosting model changes</span></span>

<span data-ttu-id="77abb-127">Si le paramètre `hostingModel` est modifié dans le fichier *web.config* (comme expliqué dans la section [Configuration avec web.config](#configuration-with-webconfig)), le module recycle le processus de travail pour IIS.</span><span class="sxs-lookup"><span data-stu-id="77abb-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="77abb-128">Pour IIS Express, le module ne recycle pas le processus de travail, mais déclenche plutôt un arrêt approprié du processus IIS Express en cours.</span><span class="sxs-lookup"><span data-stu-id="77abb-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="77abb-129">La requête suivante à l’application génère un nouveau processus IIS Express.</span><span class="sxs-lookup"><span data-stu-id="77abb-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="77abb-130">Nom du processus</span><span class="sxs-lookup"><span data-stu-id="77abb-130">Process name</span></span>

<span data-ttu-id="77abb-131">`Process.GetCurrentProcess().ProcessName` signale `w3wp`/`iisexpress` (in-process) ou `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="77abb-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="77abb-132">Configuration avec web.config</span><span class="sxs-lookup"><span data-stu-id="77abb-132">Configuration with web.config</span></span>

<span data-ttu-id="77abb-133">Le module ASP.NET Core est configuré avec la section `aspNetCore` du nœud `system.webServer` dans le fichier *web.config* du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="77abb-134">Le fichier *web.config* suivant est publié pour un [déploiement dépendant du framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le module ASP.NET Core pour gérer les demandes du site :</span><span class="sxs-lookup"><span data-stu-id="77abb-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="77abb-135">Le fichier *web.config* suivant est publié pour un [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="77abb-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="77abb-136">La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> est définie sur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<emplacement>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications qui se trouvent dans un sous-répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-136">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="77abb-137">Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le chemin d’accès `stdoutLogFile` est défini sur `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="77abb-137">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="77abb-138">Le chemin d’accès enregistre les journaux stdout dans le dossier *LogFiles*, un emplacement automatiquement créé par le service.</span><span class="sxs-lookup"><span data-stu-id="77abb-138">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="77abb-139">Pour plus d’informations sur la configuration d’une sous-application IIS, consultez <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="77abb-139">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="77abb-140">Attributs de l’élément aspNetCore</span><span class="sxs-lookup"><span data-stu-id="77abb-140">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="77abb-141">Attribut</span><span class="sxs-lookup"><span data-stu-id="77abb-141">Attribute</span></span> | <span data-ttu-id="77abb-142">Description</span><span class="sxs-lookup"><span data-stu-id="77abb-142">Description</span></span> | <span data-ttu-id="77abb-143">Par défaut</span><span class="sxs-lookup"><span data-stu-id="77abb-143">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="77abb-144">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-144">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-145">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="77abb-145">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="77abb-146">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-147">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="77abb-147">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="77abb-148">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-148">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-149">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="77abb-149">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="77abb-150">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="77abb-150">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="77abb-151">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-151">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-152">Spécifie le modèle d’hébergement comme étant in-process (`inprocess`) ou out-of-process (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="77abb-152">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="77abb-153">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-153">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-154">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="77abb-154">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="77abb-155">&dagger;Pour l’hébergement in-process, la valeur est limitée à `1`.</span><span class="sxs-lookup"><span data-stu-id="77abb-155">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="77abb-156">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="77abb-156">Default: `1`</span></span><br><span data-ttu-id="77abb-157">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="77abb-157">Min: `1`</span></span><br><span data-ttu-id="77abb-158">Max : `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="77abb-158">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="77abb-159">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="77abb-159">Required string attribute.</span></span></p><p><span data-ttu-id="77abb-160">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="77abb-160">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="77abb-161">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="77abb-161">Relative paths are supported.</span></span> <span data-ttu-id="77abb-162">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-162">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="77abb-163">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-163">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-164">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-164">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="77abb-165">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-165">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="77abb-166">Non pris en charge avec hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="77abb-166">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="77abb-167">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="77abb-167">Default: `10`</span></span><br><span data-ttu-id="77abb-168">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-168">Min: `0`</span></span><br><span data-ttu-id="77abb-169">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="77abb-169">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="77abb-170">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-170">Optional timespan attribute.</span></span></p><p><span data-ttu-id="77abb-171">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="77abb-171">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="77abb-172">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="77abb-172">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="77abb-173">Ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="77abb-173">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="77abb-174">Pour l’hébergement in-process, le module attend que l’application traite la requête.</span><span class="sxs-lookup"><span data-stu-id="77abb-174">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="77abb-175">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-175">Default: `00:02:00`</span></span><br><span data-ttu-id="77abb-176">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-176">Min: `00:00:00`</span></span><br><span data-ttu-id="77abb-177">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-177">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="77abb-178">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-178">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-179">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="77abb-179">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="77abb-180">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="77abb-180">Default: `10`</span></span><br><span data-ttu-id="77abb-181">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-181">Min: `0`</span></span><br><span data-ttu-id="77abb-182">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="77abb-182">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="77abb-183">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-183">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-184">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="77abb-184">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="77abb-185">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="77abb-185">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="77abb-186">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-186">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="77abb-187">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="77abb-187">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="77abb-188">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="77abb-188">Default: `120`</span></span><br><span data-ttu-id="77abb-189">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-189">Min: `0`</span></span><br><span data-ttu-id="77abb-190">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="77abb-190">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="77abb-191">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-191">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-192">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="77abb-192">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="77abb-193">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-193">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-194">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="77abb-194">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="77abb-195">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-195">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="77abb-196">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="77abb-196">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="77abb-197">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="77abb-197">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="77abb-198">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="77abb-198">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="77abb-199">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="77abb-199">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="77abb-200">Attribut</span><span class="sxs-lookup"><span data-stu-id="77abb-200">Attribute</span></span> | <span data-ttu-id="77abb-201">Description</span><span class="sxs-lookup"><span data-stu-id="77abb-201">Description</span></span> | <span data-ttu-id="77abb-202">Par défaut</span><span class="sxs-lookup"><span data-stu-id="77abb-202">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="77abb-203">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-203">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-204">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="77abb-204">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="77abb-205">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-206">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="77abb-206">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="77abb-207">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-207">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-208">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="77abb-208">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="77abb-209">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="77abb-209">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="77abb-210">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-211">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="77abb-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="77abb-212">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="77abb-212">Default: `1`</span></span><br><span data-ttu-id="77abb-213">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="77abb-213">Min: `1`</span></span><br><span data-ttu-id="77abb-214">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="77abb-214">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="77abb-215">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="77abb-215">Required string attribute.</span></span></p><p><span data-ttu-id="77abb-216">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="77abb-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="77abb-217">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="77abb-217">Relative paths are supported.</span></span> <span data-ttu-id="77abb-218">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="77abb-219">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-220">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="77abb-221">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="77abb-222">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="77abb-222">Default: `10`</span></span><br><span data-ttu-id="77abb-223">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-223">Min: `0`</span></span><br><span data-ttu-id="77abb-224">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="77abb-224">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="77abb-225">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-225">Optional timespan attribute.</span></span></p><p><span data-ttu-id="77abb-226">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="77abb-226">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="77abb-227">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="77abb-227">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="77abb-228">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-228">Default: `00:02:00`</span></span><br><span data-ttu-id="77abb-229">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-229">Min: `00:00:00`</span></span><br><span data-ttu-id="77abb-230">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-230">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="77abb-231">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-231">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-232">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="77abb-232">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="77abb-233">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="77abb-233">Default: `10`</span></span><br><span data-ttu-id="77abb-234">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-234">Min: `0`</span></span><br><span data-ttu-id="77abb-235">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="77abb-235">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="77abb-236">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-236">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-237">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="77abb-237">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="77abb-238">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="77abb-238">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="77abb-239">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-239">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="77abb-240">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="77abb-240">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="77abb-241">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="77abb-241">Default: `120`</span></span><br><span data-ttu-id="77abb-242">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-242">Min: `0`</span></span><br><span data-ttu-id="77abb-243">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="77abb-243">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="77abb-244">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-244">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-245">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="77abb-245">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="77abb-246">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-246">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-247">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="77abb-247">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="77abb-248">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-248">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="77abb-249">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="77abb-249">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="77abb-250">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="77abb-250">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="77abb-251">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="77abb-251">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="77abb-252">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="77abb-252">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="77abb-253">Attribut</span><span class="sxs-lookup"><span data-stu-id="77abb-253">Attribute</span></span> | <span data-ttu-id="77abb-254">Description</span><span class="sxs-lookup"><span data-stu-id="77abb-254">Description</span></span> | <span data-ttu-id="77abb-255">Par défaut</span><span class="sxs-lookup"><span data-stu-id="77abb-255">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="77abb-256">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-256">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-257">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="77abb-257">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="77abb-258">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-259">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="77abb-259">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="77abb-260">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-260">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-261">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="77abb-261">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="77abb-262">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="77abb-262">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="77abb-263">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-264">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="77abb-264">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="77abb-265">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="77abb-265">Default: `1`</span></span><br><span data-ttu-id="77abb-266">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="77abb-266">Min: `1`</span></span><br><span data-ttu-id="77abb-267">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="77abb-267">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="77abb-268">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="77abb-268">Required string attribute.</span></span></p><p><span data-ttu-id="77abb-269">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="77abb-269">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="77abb-270">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="77abb-270">Relative paths are supported.</span></span> <span data-ttu-id="77abb-271">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-271">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="77abb-272">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-272">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-273">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-273">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="77abb-274">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-274">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="77abb-275">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="77abb-275">Default: `10`</span></span><br><span data-ttu-id="77abb-276">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-276">Min: `0`</span></span><br><span data-ttu-id="77abb-277">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="77abb-277">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="77abb-278">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-278">Optional timespan attribute.</span></span></p><p><span data-ttu-id="77abb-279">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="77abb-279">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="77abb-280">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.0 ou version antérieure, la durée `requestTimeout` doit être spécifiée en minutes entières, sinon la valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="77abb-280">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="77abb-281">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-281">Default: `00:02:00`</span></span><br><span data-ttu-id="77abb-282">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-282">Min: `00:00:00`</span></span><br><span data-ttu-id="77abb-283">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="77abb-283">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="77abb-284">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-284">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-285">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="77abb-285">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="77abb-286">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="77abb-286">Default: `10`</span></span><br><span data-ttu-id="77abb-287">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-287">Min: `0`</span></span><br><span data-ttu-id="77abb-288">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="77abb-288">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="77abb-289">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="77abb-290">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="77abb-290">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="77abb-291">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="77abb-291">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="77abb-292">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="77abb-292">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="77abb-293">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="77abb-293">Default: `120`</span></span><br><span data-ttu-id="77abb-294">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="77abb-294">Min: `0`</span></span><br><span data-ttu-id="77abb-295">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="77abb-295">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="77abb-296">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-296">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="77abb-297">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="77abb-297">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="77abb-298">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="77abb-298">Optional string attribute.</span></span></p><p><span data-ttu-id="77abb-299">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="77abb-299">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="77abb-300">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="77abb-300">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="77abb-301">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="77abb-301">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="77abb-302">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="77abb-302">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="77abb-303">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="77abb-303">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="77abb-304">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="77abb-304">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="77abb-305">Définition des variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="77abb-305">Setting environment variables</span></span>

<span data-ttu-id="77abb-306">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="77abb-306">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="77abb-307">Spécifiez une variable d’environnement à l’aide de l’élément enfant `environmentVariable` d’un élément de collection `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="77abb-307">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="77abb-308">Les variables d’environnement définies dans cette section prévalent sur les variables d’environnement système.</span><span class="sxs-lookup"><span data-stu-id="77abb-308">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="77abb-309">L’exemple suivant définit deux variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="77abb-309">The following example sets two environment variables.</span></span> <span data-ttu-id="77abb-310">`ASPNETCORE_ENVIRONMENT` définit l’environnement de l’application sur `Development`.</span><span class="sxs-lookup"><span data-stu-id="77abb-310">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="77abb-311">Un développeur peut définir temporairement cette valeur dans le fichier *web.config* afin de forcer le chargement de la [Page d’exception de développeur](xref:fundamentals/error-handling) lors du débogage d’une exception d’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-311">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="77abb-312">`CONFIG_DIR` est un exemple de variable d’environnement définie par l’utilisateur, dans laquelle le développeur a écrit du code qui lit la valeur de démarrage afin de former un chemin d’accès pour le chargement du fichier de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-312">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="77abb-313">Affectez uniquement `Development` à la variable d’environnement `ASPNETCORE_ENVIRONMENT` sur les serveurs de test et de préproduction non accessibles aux réseaux non approuvés, par exemple Internet.</span><span class="sxs-lookup"><span data-stu-id="77abb-313">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="77abb-314">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="77abb-314">app_offline.htm</span></span>

<span data-ttu-id="77abb-315">Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le module ASP.NET Core tente d’arrêter normalement l’application, puis interrompt le traitement des requêtes entrantes.</span><span class="sxs-lookup"><span data-stu-id="77abb-315">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="77abb-316">Si l’application est toujours active après le nombre de secondes défini dans `shutdownTimeLimit`, le module ASP.NET Core met fin au processus en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="77abb-316">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="77abb-317">Tant que le fichier *app_offline.htm* est présent, le module ASP.NET Core répond aux requêtes en renvoyant le contenu du fichier *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="77abb-317">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="77abb-318">Lorsque le fichier *app_offline.htm* est supprimé, la requête suivante démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-318">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="77abb-319">Lorsque vous utilisez le modèle d’hébergement out-of-process, l’application peut ne pas s’arrêter immédiatement s’il existe une connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="77abb-319">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="77abb-320">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-320">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="77abb-321">Page d’erreur de démarrage</span><span class="sxs-lookup"><span data-stu-id="77abb-321">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="77abb-322">Les hébergements in-process et out-of-process produisent des pages d’erreurs personnalisées quand ils ne parviennent pas à démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-322">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="77abb-323">Si le module ASP.NET Core ne trouve pas le gestionnaire de requêtes in-process ou out-of-process, une page de code d’état *500.0 - Échec de chargement du gestionnaire in-process/out-of-process* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77abb-323">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="77abb-324">Pour l’hébergement in-process, si le module ASP.NET Core ne peut pas démarrer l’application, une page de code d’état *500.30 - Échec de démarrage* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77abb-324">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="77abb-325">Pour l’hébergement out-of-process, si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77abb-325">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="77abb-326">Pour supprimer cette page et revenir à la page IIS de codes d’état 5xx par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="77abb-326">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="77abb-327">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="77abb-327">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="77abb-328">Si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="77abb-328">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="77abb-329">Pour supprimer cette page et revenir à la page IIS de code d’état 502 par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="77abb-329">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="77abb-330">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="77abb-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Page de code d’état 502.5 Échec du processus](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="77abb-332">Création et redirection de journal</span><span class="sxs-lookup"><span data-stu-id="77abb-332">Log creation and redirection</span></span>

<span data-ttu-id="77abb-333">Le module ASP.NET Core redirige la sortie de console stdout et stderr vers le disque si les attributs `stdoutLogEnabled` et `stdoutLogFile` de l’élément `aspNetCore` sont définis.</span><span class="sxs-lookup"><span data-stu-id="77abb-333">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="77abb-334">Tous les dossiers spécifiés dans le chemin d’accès `stdoutLogFile` doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="77abb-334">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="77abb-335">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).</span><span class="sxs-lookup"><span data-stu-id="77abb-335">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="77abb-336">Aucune rotation n’est appliquée aux journaux, sauf en cas de recyclage/redémarrage du processus.</span><span class="sxs-lookup"><span data-stu-id="77abb-336">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="77abb-337">Il incombe à l’hébergeur de limiter l’espace disque utilisé par les journaux.</span><span class="sxs-lookup"><span data-stu-id="77abb-337">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="77abb-338">L’utilisation du journal stdout est uniquement recommandée pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-338">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="77abb-339">N’utilisez pas le journal stdout à des fins de journalisation d’application générale.</span><span class="sxs-lookup"><span data-stu-id="77abb-339">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="77abb-340">Pour journaliser la routine d’une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et appliquez une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="77abb-340">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="77abb-341">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="77abb-341">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="77abb-342">Un horodatage et une extension de fichier sont ajoutés automatiquement quand le fichier journal est créé.</span><span class="sxs-lookup"><span data-stu-id="77abb-342">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="77abb-343">Le nom du fichier journal est créé en ajoutant l’horodatage, un ID de processus et une extension de fichier (*.log*) au dernier segment du chemin d'accès `stdoutLogFile` (généralement *stdout*), séparés par des traits de soulignement.</span><span class="sxs-lookup"><span data-stu-id="77abb-343">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="77abb-344">Si le chemin d'accès `stdoutLogFile` se termine par *stdout*, un journal pour une application avec un PID de 1934 créé le 5/2/2018 à 19:42:32 affiche le nom de fichier *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="77abb-344">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="77abb-345">Si `stdoutLogEnabled` a la valeur false, les erreurs qui se produisent au moment du démarrage de l’application sont capturées et émises dans le journal des événements (30 Ko maximum).</span><span class="sxs-lookup"><span data-stu-id="77abb-345">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="77abb-346">Après le démarrage, tous les journaux supplémentaires sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="77abb-346">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="77abb-347">L’exemple d’élément `aspNetCore` suivant configure la journalisation stdout pour une application hébergée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="77abb-347">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="77abb-348">Un chemin d’accès local ou un chemin de partage réseau peut être utilisé pour une journalisation locale.</span><span class="sxs-lookup"><span data-stu-id="77abb-348">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="77abb-349">Vérifiez que l’identité de l’utilisateur AppPool à l’autorisation d’écrire dans le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="77abb-349">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="77abb-350">Journaux de diagnostic améliorés</span><span class="sxs-lookup"><span data-stu-id="77abb-350">Enhanced diagnostic logs</span></span>

<span data-ttu-id="77abb-351">Le module ASP.NET Core fourni est configurable pour proposer des journaux de diagnostic améliorés.</span><span class="sxs-lookup"><span data-stu-id="77abb-351">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="77abb-352">Ajoutez l’élément `<handlerSettings>` à l’élément `<aspNetCore>` dans *web.config*. L’affectation de la valeur `TRACE` à `debugLevel` expose une fidélité plus élevée des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="77abb-352">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="77abb-353">Les valeurs du niveau de débogage (`debugLevel`) peuvent inclure à la fois le niveau et l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="77abb-353">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="77abb-354">Niveaux (dans l’ordre du moins au plus détaillé) :</span><span class="sxs-lookup"><span data-stu-id="77abb-354">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="77abb-355">ERROR</span><span class="sxs-lookup"><span data-stu-id="77abb-355">ERROR</span></span>
* <span data-ttu-id="77abb-356">WARNING</span><span class="sxs-lookup"><span data-stu-id="77abb-356">WARNING</span></span>
* <span data-ttu-id="77abb-357">INFO</span><span class="sxs-lookup"><span data-stu-id="77abb-357">INFO</span></span>
* <span data-ttu-id="77abb-358">TRACE</span><span class="sxs-lookup"><span data-stu-id="77abb-358">TRACE</span></span>

<span data-ttu-id="77abb-359">Emplacements (plusieurs emplacements sont autorisés) :</span><span class="sxs-lookup"><span data-stu-id="77abb-359">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="77abb-360">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="77abb-360">CONSOLE</span></span>
* <span data-ttu-id="77abb-361">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="77abb-361">EVENTLOG</span></span>
* <span data-ttu-id="77abb-362">FICHIER</span><span class="sxs-lookup"><span data-stu-id="77abb-362">FILE</span></span>

<span data-ttu-id="77abb-363">Les paramètres de gestionnaire peuvent également être fournis par le biais de variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="77abb-363">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="77abb-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Chemin du fichier journal de débogage.</span><span class="sxs-lookup"><span data-stu-id="77abb-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="77abb-365">(Par défaut : *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="77abb-365">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="77abb-366">`ASPNETCORE_MODULE_DEBUG` &ndash; Paramètre du niveau de débogage.</span><span class="sxs-lookup"><span data-stu-id="77abb-366">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="77abb-367">Ne laissez **pas** la journalisation du débogage activée dans le déploiement plus longtemps que nécessaire pour résoudre un problème.</span><span class="sxs-lookup"><span data-stu-id="77abb-367">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="77abb-368">La taille du journal n’est pas limitée.</span><span class="sxs-lookup"><span data-stu-id="77abb-368">The size of the log isn't limited.</span></span> <span data-ttu-id="77abb-369">Si vous laissez la journalisation du débogage activée, vous risquez d’épuiser l’espace disque disponible et de bloquer le serveur ou le service d’application.</span><span class="sxs-lookup"><span data-stu-id="77abb-369">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="77abb-370">Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple d’élément `aspNetCore` dans le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="77abb-370">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="77abb-371">La configuration du proxy utilise le protocole HTTP et un jeton d’appariement</span><span class="sxs-lookup"><span data-stu-id="77abb-371">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="77abb-372">*S’applique uniquement à l’hébergement out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="77abb-372">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="77abb-373">Le proxy créé entre le module ASP.NET Core et Kestrel utilise le protocole HTTP.</span><span class="sxs-lookup"><span data-stu-id="77abb-373">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="77abb-374">Utiliser HTTP est une optimisation des performances où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau.</span><span class="sxs-lookup"><span data-stu-id="77abb-374">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="77abb-375">Il n’existe aucun risque d’écoute clandestine du trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="77abb-375">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="77abb-376">Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel ont été traitées par IIS et ne proviennent pas d’une autre source.</span><span class="sxs-lookup"><span data-stu-id="77abb-376">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="77abb-377">Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module.</span><span class="sxs-lookup"><span data-stu-id="77abb-377">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="77abb-378">Le jeton d’appariement est également défini dans un en-tête (`MSAspNetCoreToken`) sur toutes les demandes traitées.</span><span class="sxs-lookup"><span data-stu-id="77abb-378">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="77abb-379">L'intergiciel IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d'appariement correspond à la valeur de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="77abb-379">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="77abb-380">Si les valeurs de jeton ne correspondent pas, la demande est connectée et rejetée.</span><span class="sxs-lookup"><span data-stu-id="77abb-380">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="77abb-381">La variable d’environnement du jeton d'appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="77abb-381">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="77abb-382">Sans connaître la valeur du jeton d'appariement, une personne malveillante ne peut pas soumettre des demandes qui contournent la vérification de l’intergiciel IIS.</span><span class="sxs-lookup"><span data-stu-id="77abb-382">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="77abb-383">Module ASP.NET Core avec une configuration partagée IIS</span><span class="sxs-lookup"><span data-stu-id="77abb-383">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="77abb-384">Le programme d’installation du module ASP.NET Core s’exécute avec les privilèges du compte **système**.</span><span class="sxs-lookup"><span data-stu-id="77abb-384">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="77abb-385">Comme le compte système local n’a pas l’autorisation de modifier le chemin d’accès de partage utilisé par la configuration partagée IIS, le programme d’installation rencontre une erreur d’accès refusé lorsque vous tentez de configurer les paramètres du module dans *applicationHost.config* sur le partage.</span><span class="sxs-lookup"><span data-stu-id="77abb-385">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="77abb-386">Lorsque vous utilisez une configuration partagée IIS, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="77abb-386">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="77abb-387">Désactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="77abb-387">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="77abb-388">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="77abb-388">Run the installer.</span></span>
1. <span data-ttu-id="77abb-389">Exportez le fichier *applicationHost.config* mis à jour vers le partage.</span><span class="sxs-lookup"><span data-stu-id="77abb-389">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="77abb-390">Réactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="77abb-390">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="77abb-391">Version du module et journaux du programme d’installation du bundle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="77abb-391">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="77abb-392">Pour déterminer la version du module ASP.NET Core installé :</span><span class="sxs-lookup"><span data-stu-id="77abb-392">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="77abb-393">Sur le système hôte, accédez à *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="77abb-393">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="77abb-394">Recherchez le fichier *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="77abb-394">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="77abb-395">Cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="77abb-395">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="77abb-396">Sélectionnez l'onglet **Détails**. Le **version du fichier** et la **version du produit** représentent la version installée du module.</span><span class="sxs-lookup"><span data-stu-id="77abb-396">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="77abb-397">Les journaux du programme d’installation du bundle d’hébergement du module se trouvent dans *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="77abb-397">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="77abb-398">Emplacements des fichiers du module, du schéma et de configuration</span><span class="sxs-lookup"><span data-stu-id="77abb-398">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="77abb-399">Module</span><span class="sxs-lookup"><span data-stu-id="77abb-399">Module</span></span>

<span data-ttu-id="77abb-400">**IIS (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="77abb-400">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="77abb-401">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-401">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="77abb-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="77abb-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="77abb-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="77abb-405">**IIS Express (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="77abb-405">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="77abb-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="77abb-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="77abb-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="77abb-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="77abb-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="77abb-410">Schéma</span><span class="sxs-lookup"><span data-stu-id="77abb-410">Schema</span></span>

<span data-ttu-id="77abb-411">**IIS**</span><span class="sxs-lookup"><span data-stu-id="77abb-411">**IIS**</span></span>

   * <span data-ttu-id="77abb-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="77abb-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="77abb-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="77abb-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="77abb-414">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="77abb-414">**IIS Express**</span></span>

   * <span data-ttu-id="77abb-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="77abb-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="77abb-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="77abb-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="77abb-417">Configuration</span><span class="sxs-lookup"><span data-stu-id="77abb-417">Configuration</span></span>

<span data-ttu-id="77abb-418">**IIS**</span><span class="sxs-lookup"><span data-stu-id="77abb-418">**IIS**</span></span>

   * <span data-ttu-id="77abb-419">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="77abb-419">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="77abb-420">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="77abb-420">**IIS Express**</span></span>

   * <span data-ttu-id="77abb-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="77abb-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="77abb-422">Vous trouverez les fichiers en recherchant *aspnetcore* dans le fichier *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="77abb-422">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
