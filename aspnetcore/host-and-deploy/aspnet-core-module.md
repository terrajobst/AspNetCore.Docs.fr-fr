---
title: Module ASP.NET Core
author: guardrex
description: Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 4a360023cc7fab2f066d490f7f368fc35815703a
ms.sourcegitcommit: 4b00e77f9984ce76356e829cfe7f75f0f61a7a8f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2019
ms.locfileid: "67500451"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="64f71-103">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64f71-103">ASP.NET Core Module</span></span>

<span data-ttu-id="64f71-104">Par [Nowak](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="64f71-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-105">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour effectuer l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="64f71-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="64f71-106">Héberger une application ASP.NET Core à l’intérieur du processus de travail IIS (`w3wp.exe`), appelé [modèle d’hébergement in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="64f71-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="64f71-107">Transférer les requêtes web à une application ASP.NET Core du back-end exécutant le [serveur Kestrel](xref:fundamentals/servers/kestrel), appelé [modèle d’hébergement out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="64f71-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="64f71-108">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="64f71-108">Supported Windows versions:</span></span>

* <span data-ttu-id="64f71-109">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="64f71-109">Windows 7 or later</span></span>
* <span data-ttu-id="64f71-110">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="64f71-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="64f71-111">Lors de l’hébergement in-process, le module utilise une implémentation du serveur in-process pour IIS, nommée serveur HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="64f71-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="64f71-112">Lors de l’hébergement out-of-process, le module fonctionne uniquement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="64f71-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="64f71-113">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="64f71-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="64f71-114">Modèles d'hébergement</span><span class="sxs-lookup"><span data-stu-id="64f71-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="64f71-115">Modèle d’hébergement in-process</span><span class="sxs-lookup"><span data-stu-id="64f71-115">In-process hosting model</span></span>

<span data-ttu-id="64f71-116">Pour configurer l’hébergement in-process dans une application, ajoutez la propriété `<AspNetCoreHostingModel>` au fichier projet de l’application en lui attribuant la valeur `InProcess` (l’hébergement out-of-process est défini sur `OutOfProcess`) :</span><span class="sxs-lookup"><span data-stu-id="64f71-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="64f71-117">Le modèle d’hébergement in-process n’est pas pris en charge pour les applications ASP.NET Core qui ciblent le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="64f71-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="64f71-118">Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="64f71-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="64f71-119">Les caractéristiques suivantes s’appliquent lors de l’hébergement in-process :</span><span class="sxs-lookup"><span data-stu-id="64f71-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="64f71-120">Le serveur HTTP IIS (`IISHttpServer`) est utilisé à la place du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="64f71-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="64f71-121">Pour in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> pour :</span><span class="sxs-lookup"><span data-stu-id="64f71-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="64f71-122">Enregistrer `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="64f71-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="64f71-123">Configurer le port et le chemin de base sur lesquels le serveur doit écouter lors de l’exécution derrière le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64f71-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="64f71-124">Configurer l’hôte pour capturer des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="64f71-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="64f71-125">L’[attribut requestTimeout](#attributes-of-the-aspnetcore-element) ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="64f71-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="64f71-126">Le partage d’un pool d’applications entre les applications n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="64f71-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="64f71-127">Utilisez un pool d’applications par application.</span><span class="sxs-lookup"><span data-stu-id="64f71-127">Use one app pool per app.</span></span>

* <span data-ttu-id="64f71-128">Lorsque vous utilisez [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou que vous placez manuellement un [fichier app_offline.htm dans le déploiement](xref:host-and-deploy/iis/index#locked-deployment-files), l’application peut ne pas s’arrêter immédiatement si une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="64f71-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="64f71-129">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="64f71-130">L’architecture (nombre de bits) de l’application et le runtime installé (x64 ou x86) doivent correspondre à l’architecture du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="64f71-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="64f71-131">Si vous configurez manuellement l’hôte de l’application avec `WebHostBuilder` (et non [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)), et que l’application est exécutée directement sur le serveur Kestrel (auto-hébergée), appelez `UseKestrel` avant d’appeler `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="64f71-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="64f71-132">Si l’ordre est inversé, l’hôte ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="64f71-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="64f71-133">Les déconnexions du client sont détectées.</span><span class="sxs-lookup"><span data-stu-id="64f71-133">Client disconnects are detected.</span></span> <span data-ttu-id="64f71-134">Le jeton d’annulation [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) est annulé quand le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="64f71-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="64f71-135">Dans ASP.NET Core version 2.2.1 ou antérieure, <xref:System.IO.Directory.GetCurrentDirectory*> retourne le répertoire de travail du processus démarré par IIS, et non le répertoire de l’application (par exemple, *C:\Windows\System32\inetsrv* pour *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="64f71-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="64f71-136">Pour connaître l’exemple de code qui définit le répertoire actuel de l’application, consultez [CurrentDirectoryHelpers, classe](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="64f71-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="64f71-137">Appelez la méthode `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="64f71-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="64f71-138">Les appels suivants à <xref:System.IO.Directory.GetCurrentDirectory*> fournissent le répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="64f71-139">Dans le cas d’un hébergement in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelé en interne pour initialiser un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="64f71-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="64f71-140">Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="64f71-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="64f71-141">Si vous transformez les revendications avec une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, appelez <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> pour ajouter des services d’authentification :</span><span class="sxs-lookup"><span data-stu-id="64f71-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="64f71-142">Modèle d’hébergement out-of-process</span><span class="sxs-lookup"><span data-stu-id="64f71-142">Out-of-process hosting model</span></span>

<span data-ttu-id="64f71-143">Pour configurer une application pour un hébergement out-of-process, utilisez l’une des approches suivantes dans le fichier de projet :</span><span class="sxs-lookup"><span data-stu-id="64f71-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="64f71-144">Ne spécifiez pas la propriété `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="64f71-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="64f71-145">Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="64f71-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="64f71-146">Définissez la valeur de la propriété `<AspNetCoreHostingModel>` sur `OutOfProcess` (l’hébergement in-process est défini avec `InProcess`) :</span><span class="sxs-lookup"><span data-stu-id="64f71-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="64f71-147">Le serveur [Kestrel](xref:fundamentals/servers/kestrel) est utilisé à la place du serveur HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="64f71-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="64f71-148">Pour out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) appelle <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> pour :</span><span class="sxs-lookup"><span data-stu-id="64f71-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="64f71-149">Configurer le port et le chemin de base sur lesquels le serveur doit écouter lors de l’exécution derrière le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64f71-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="64f71-150">Configurer l’hôte pour capturer des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="64f71-150">Configure the host to capture startup errors.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64f71-151">Dans le cas d’un hébergement out-of-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelé en interne pour initialiser un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="64f71-151">When hosting out-of-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="64f71-152">Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut.</span><span class="sxs-lookup"><span data-stu-id="64f71-152">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="64f71-153">Si vous transformez les revendications avec une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, appelez <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> pour ajouter des services d’authentification :</span><span class="sxs-lookup"><span data-stu-id="64f71-153">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
    services.AddAuthentication(IISDefaults.AuthenticationScheme);
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="hosting-model-changes"></a><span data-ttu-id="64f71-154">Modifications du modèle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="64f71-154">Hosting model changes</span></span>

<span data-ttu-id="64f71-155">Si le paramètre `hostingModel` est modifié dans le fichier *web.config* (comme expliqué dans la section [Configuration avec web.config](#configuration-with-webconfig)), le module recycle le processus de travail pour IIS.</span><span class="sxs-lookup"><span data-stu-id="64f71-155">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="64f71-156">Pour IIS Express, le module ne recycle pas le processus de travail, mais déclenche plutôt un arrêt approprié du processus IIS Express en cours.</span><span class="sxs-lookup"><span data-stu-id="64f71-156">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="64f71-157">La requête suivante à l’application génère un nouveau processus IIS Express.</span><span class="sxs-lookup"><span data-stu-id="64f71-157">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="64f71-158">Nom du processus</span><span class="sxs-lookup"><span data-stu-id="64f71-158">Process name</span></span>

<span data-ttu-id="64f71-159">`Process.GetCurrentProcess().ProcessName` signale `w3wp`/`iisexpress` (in-process) ou `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="64f71-159">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="64f71-160">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour transférer les requêtes web aux applications ASP.NET Core du back-end.</span><span class="sxs-lookup"><span data-stu-id="64f71-160">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="64f71-161">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="64f71-161">Supported Windows versions:</span></span>

* <span data-ttu-id="64f71-162">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="64f71-162">Windows 7 or later</span></span>
* <span data-ttu-id="64f71-163">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="64f71-163">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="64f71-164">Le module fonctionne seulement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="64f71-164">The module only works with Kestrel.</span></span> <span data-ttu-id="64f71-165">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="64f71-165">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="64f71-166">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="64f71-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="64f71-167">Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante.</span><span class="sxs-lookup"><span data-stu-id="64f71-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="64f71-168">Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="64f71-168">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="64f71-169">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application :</span><span class="sxs-lookup"><span data-stu-id="64f71-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="64f71-171">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="64f71-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="64f71-172">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="64f71-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="64f71-173">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.</span><span class="sxs-lookup"><span data-stu-id="64f71-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="64f71-174">Le module spécifie le port via une variable d’environnement au démarrage, et le [middleware d’intégration IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="64f71-174">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="64f71-175">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="64f71-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="64f71-176">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="64f71-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="64f71-177">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64f71-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="64f71-178">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="64f71-179">Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="64f71-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="64f71-180">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="64f71-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="64f71-181">De nombreux modules natifs, comme l’authentification Windows, restent actifs.</span><span class="sxs-lookup"><span data-stu-id="64f71-181">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="64f71-182">Pour obtenir plus d’informations sur les modules IIS actifs avec le module ASP.NET Core, consultez <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="64f71-182">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="64f71-183">Le module ASP.NET Core peut également  :</span><span class="sxs-lookup"><span data-stu-id="64f71-183">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="64f71-184">Définir des variables d’environnement pour le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="64f71-184">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="64f71-185">Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.</span><span class="sxs-lookup"><span data-stu-id="64f71-185">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="64f71-186">Transférer des jetons d’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="64f71-186">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="64f71-187">Comment installer et utiliser le module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64f71-187">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="64f71-188">Pour obtenir des instructions sur l’installation du module ASP.NET Core, consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="64f71-188">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="64f71-189">Configuration avec web.config</span><span class="sxs-lookup"><span data-stu-id="64f71-189">Configuration with web.config</span></span>

<span data-ttu-id="64f71-190">Le module ASP.NET Core est configuré avec la section `aspNetCore` du nœud `system.webServer` dans le fichier *web.config* du site.</span><span class="sxs-lookup"><span data-stu-id="64f71-190">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="64f71-191">Le fichier *web.config* suivant est publié pour un [déploiement dépendant du framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le module ASP.NET Core pour gérer les demandes du site :</span><span class="sxs-lookup"><span data-stu-id="64f71-191">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="InProcess" />
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

<span data-ttu-id="64f71-192">Le fichier *web.config* suivant est publié pour un [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="64f71-192">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="64f71-193">La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> est définie sur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<emplacement>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications qui se trouvent dans un sous-répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-193">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="64f71-194">Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le chemin d’accès `stdoutLogFile` est défini sur `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="64f71-194">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="64f71-195">Le chemin d’accès enregistre les journaux stdout dans le dossier *LogFiles*, un emplacement automatiquement créé par le service.</span><span class="sxs-lookup"><span data-stu-id="64f71-195">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="64f71-196">Pour plus d’informations sur la configuration d’une sous-application IIS, consultez <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="64f71-196">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="64f71-197">Attributs de l’élément aspNetCore</span><span class="sxs-lookup"><span data-stu-id="64f71-197">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="64f71-198">Attribut</span><span class="sxs-lookup"><span data-stu-id="64f71-198">Attribute</span></span> | <span data-ttu-id="64f71-199">Description</span><span class="sxs-lookup"><span data-stu-id="64f71-199">Description</span></span> | <span data-ttu-id="64f71-200">Default</span><span class="sxs-lookup"><span data-stu-id="64f71-200">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="64f71-201">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-201">Optional string attribute.</span></span></p><p><span data-ttu-id="64f71-202">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="64f71-202">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="64f71-203">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="64f71-204">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="64f71-204">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="64f71-205">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="64f71-206">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="64f71-206">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="64f71-207">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="64f71-207">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="64f71-208">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-208">Optional string attribute.</span></span></p><p><span data-ttu-id="64f71-209">Spécifie le modèle d’hébergement comme étant in-process (`InProcess`) ou out-of-process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="64f71-209">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="64f71-210">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-211">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="64f71-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="64f71-212">&dagger;Pour l’hébergement in-process, la valeur est limitée à `1`.</span><span class="sxs-lookup"><span data-stu-id="64f71-212">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="64f71-213">Il est déconseillé de définir `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="64f71-213">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="64f71-214">Cet attribut sera supprimé dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="64f71-214">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="64f71-215">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="64f71-215">Default: `1`</span></span><br><span data-ttu-id="64f71-216">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="64f71-216">Min: `1`</span></span><br><span data-ttu-id="64f71-217">Max : `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="64f71-217">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="64f71-218">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="64f71-218">Required string attribute.</span></span></p><p><span data-ttu-id="64f71-219">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64f71-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="64f71-220">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="64f71-220">Relative paths are supported.</span></span> <span data-ttu-id="64f71-221">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="64f71-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="64f71-222">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-223">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="64f71-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="64f71-224">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="64f71-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="64f71-225">Non pris en charge avec hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="64f71-225">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="64f71-226">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="64f71-226">Default: `10`</span></span><br><span data-ttu-id="64f71-227">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="64f71-227">Min: `0`</span></span><br><span data-ttu-id="64f71-228">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="64f71-228">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="64f71-229">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-229">Optional timespan attribute.</span></span></p><p><span data-ttu-id="64f71-230">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="64f71-230">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="64f71-231">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="64f71-231">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="64f71-232">Ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="64f71-232">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="64f71-233">Pour l’hébergement in-process, le module attend que l’application traite la requête.</span><span class="sxs-lookup"><span data-stu-id="64f71-233">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="64f71-234">Les valeurs valides pour les segments de minutes et secondes de la chaîne sont dans la plage de 0 à 59.</span><span class="sxs-lookup"><span data-stu-id="64f71-234">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="64f71-235">L’utilisation de **60** dans la valeur de minutes ou de secondes affiche *500 - Erreur interne du serveur*.</span><span class="sxs-lookup"><span data-stu-id="64f71-235">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="64f71-236">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="64f71-236">Default: `00:02:00`</span></span><br><span data-ttu-id="64f71-237">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="64f71-237">Min: `00:00:00`</span></span><br><span data-ttu-id="64f71-238">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="64f71-238">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="64f71-239">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-240">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="64f71-240">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="64f71-241">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="64f71-241">Default: `10`</span></span><br><span data-ttu-id="64f71-242">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="64f71-242">Min: `0`</span></span><br><span data-ttu-id="64f71-243">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="64f71-243">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="64f71-244">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-244">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-245">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="64f71-245">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="64f71-246">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="64f71-246">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="64f71-247">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="64f71-247">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="64f71-248">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="64f71-248">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="64f71-249">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="64f71-249">Default: `120`</span></span><br><span data-ttu-id="64f71-250">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="64f71-250">Min: `0`</span></span><br><span data-ttu-id="64f71-251">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="64f71-251">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="64f71-252">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-252">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="64f71-253">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="64f71-253">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="64f71-254">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-254">Optional string attribute.</span></span></p><p><span data-ttu-id="64f71-255">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="64f71-255">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="64f71-256">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="64f71-256">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="64f71-257">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="64f71-257">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="64f71-258">Tous les dossiers fournis dans le chemin sont créés par le module au moment de la création du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="64f71-258">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="64f71-259">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier ( *.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="64f71-259">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="64f71-260">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="64f71-260">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="64f71-261">Attribut</span><span class="sxs-lookup"><span data-stu-id="64f71-261">Attribute</span></span> | <span data-ttu-id="64f71-262">Description</span><span class="sxs-lookup"><span data-stu-id="64f71-262">Description</span></span> | <span data-ttu-id="64f71-263">Default</span><span class="sxs-lookup"><span data-stu-id="64f71-263">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="64f71-264">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-264">Optional string attribute.</span></span></p><p><span data-ttu-id="64f71-265">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="64f71-265">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="64f71-266">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-266">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="64f71-267">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="64f71-267">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="64f71-268">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-268">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="64f71-269">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="64f71-269">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="64f71-270">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="64f71-270">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="64f71-271">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-272">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="64f71-272">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="64f71-273">Il est déconseillé de définir `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="64f71-273">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="64f71-274">Cet attribut sera supprimé dans une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="64f71-274">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="64f71-275">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="64f71-275">Default: `1`</span></span><br><span data-ttu-id="64f71-276">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="64f71-276">Min: `1`</span></span><br><span data-ttu-id="64f71-277">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="64f71-277">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="64f71-278">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="64f71-278">Required string attribute.</span></span></p><p><span data-ttu-id="64f71-279">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="64f71-279">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="64f71-280">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="64f71-280">Relative paths are supported.</span></span> <span data-ttu-id="64f71-281">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="64f71-281">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="64f71-282">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-283">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="64f71-283">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="64f71-284">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="64f71-284">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="64f71-285">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="64f71-285">Default: `10`</span></span><br><span data-ttu-id="64f71-286">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="64f71-286">Min: `0`</span></span><br><span data-ttu-id="64f71-287">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="64f71-287">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="64f71-288">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-288">Optional timespan attribute.</span></span></p><p><span data-ttu-id="64f71-289">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="64f71-289">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="64f71-290">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="64f71-290">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="64f71-291">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="64f71-291">Default: `00:02:00`</span></span><br><span data-ttu-id="64f71-292">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="64f71-292">Min: `00:00:00`</span></span><br><span data-ttu-id="64f71-293">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="64f71-293">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="64f71-294">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-295">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="64f71-295">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="64f71-296">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="64f71-296">Default: `10`</span></span><br><span data-ttu-id="64f71-297">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="64f71-297">Min: `0`</span></span><br><span data-ttu-id="64f71-298">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="64f71-298">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="64f71-299">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-299">Optional integer attribute.</span></span></p><p><span data-ttu-id="64f71-300">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="64f71-300">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="64f71-301">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="64f71-301">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="64f71-302">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="64f71-302">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="64f71-303">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="64f71-303">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="64f71-304">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="64f71-304">Default: `120`</span></span><br><span data-ttu-id="64f71-305">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="64f71-305">Min: `0`</span></span><br><span data-ttu-id="64f71-306">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="64f71-306">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="64f71-307">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-307">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="64f71-308">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="64f71-308">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="64f71-309">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="64f71-309">Optional string attribute.</span></span></p><p><span data-ttu-id="64f71-310">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="64f71-310">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="64f71-311">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="64f71-311">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="64f71-312">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="64f71-312">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="64f71-313">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="64f71-313">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="64f71-314">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier ( *.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="64f71-314">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="64f71-315">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="64f71-315">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="64f71-316">Définition des variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="64f71-316">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64f71-317">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="64f71-317">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="64f71-318">Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="64f71-318">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="64f71-319">Les variables d’environnement définies dans cette section prévalent sur les variables d’environnement système.</span><span class="sxs-lookup"><span data-stu-id="64f71-319">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64f71-320">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="64f71-320">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="64f71-321">Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="64f71-321">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="64f71-322">Les variables d’environnement définies dans cette section sont en conflit avec les variables d’environnement système qui ont le même nom.</span><span class="sxs-lookup"><span data-stu-id="64f71-322">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="64f71-323">Si une variable d’environnement est définie à la fois dans le fichier *web.config* et au niveau du système dans Windows, la valeur provenant du fichier *web.config* est ajoutée à la valeur de la variable d’environnement système (par exemple, `ASPNETCORE_ENVIRONMENT: Development;Development`), ce qui empêche le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-323">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="64f71-324">L’exemple suivant définit deux variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="64f71-324">The following example sets two environment variables.</span></span> <span data-ttu-id="64f71-325">`ASPNETCORE_ENVIRONMENT` définit l’environnement de l’application sur `Development`.</span><span class="sxs-lookup"><span data-stu-id="64f71-325">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="64f71-326">Un développeur peut définir temporairement cette valeur dans le fichier *web.config* afin de forcer le chargement de la [Page d’exception de développeur](xref:fundamentals/error-handling) lors du débogage d’une exception d’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-326">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="64f71-327">`CONFIG_DIR` est un exemple de variable d’environnement définie par l’utilisateur, dans laquelle le développeur a écrit du code qui lit la valeur de démarrage afin de former un chemin d’accès pour le chargement du fichier de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-327">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
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

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="64f71-328">Une alternative à la définition directe de l’environnement dans le fichier *web.config* consiste à inclure la propriété `<EnvironmentName>` dans le profil de publication ( *.pubxml*) ou le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="64f71-328">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="64f71-329">Cette approche définit l’environnement dans *web.config* lorsque le projet est publié :</span><span class="sxs-lookup"><span data-stu-id="64f71-329">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="64f71-330">Affectez uniquement `Development` à la variable d’environnement `ASPNETCORE_ENVIRONMENT` sur les serveurs de test et de préproduction non accessibles aux réseaux non approuvés, par exemple Internet.</span><span class="sxs-lookup"><span data-stu-id="64f71-330">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="64f71-331">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="64f71-331">app_offline.htm</span></span>

<span data-ttu-id="64f71-332">Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le module ASP.NET Core tente d’arrêter normalement l’application, puis interrompt le traitement des requêtes entrantes.</span><span class="sxs-lookup"><span data-stu-id="64f71-332">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="64f71-333">Si l’application est toujours active après le nombre de secondes défini dans `shutdownTimeLimit`, le module ASP.NET Core met fin au processus en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="64f71-333">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="64f71-334">Tant que le fichier *app_offline.htm* est présent, le module ASP.NET Core répond aux requêtes en renvoyant le contenu du fichier *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="64f71-334">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="64f71-335">Lorsque le fichier *app_offline.htm* est supprimé, la requête suivante démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-335">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-336">Lorsque vous utilisez le modèle d’hébergement out-of-process, l’application peut ne pas s’arrêter immédiatement s’il existe une connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="64f71-336">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="64f71-337">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-337">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="64f71-338">Page d’erreur de démarrage</span><span class="sxs-lookup"><span data-stu-id="64f71-338">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-339">Les hébergements in-process et out-of-process produisent des pages d’erreurs personnalisées quand ils ne parviennent pas à démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-339">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="64f71-340">Si le module ASP.NET Core ne trouve pas le gestionnaire de requêtes in-process ou out-of-process, une page de code d’état *500.0 - Échec de chargement du gestionnaire in-process/out-of-process* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="64f71-340">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="64f71-341">Pour l’hébergement in-process, si le module ASP.NET Core ne peut pas démarrer l’application, une page de code d’état *500.30 - Échec de démarrage* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="64f71-341">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="64f71-342">Pour l’hébergement out-of-process, si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="64f71-342">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="64f71-343">Pour supprimer cette page et revenir à la page IIS de codes d’état 5xx par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="64f71-343">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="64f71-344">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="64f71-344">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="64f71-345">Si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="64f71-345">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="64f71-346">Pour supprimer cette page et revenir à la page IIS de code d’état 502 par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="64f71-346">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="64f71-347">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="64f71-347">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Page de code d’état 502.5 Échec du processus](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="64f71-349">Création et redirection de journal</span><span class="sxs-lookup"><span data-stu-id="64f71-349">Log creation and redirection</span></span>

<span data-ttu-id="64f71-350">Le module ASP.NET Core redirige la sortie de console stdout et stderr vers le disque si les attributs `stdoutLogEnabled` et `stdoutLogFile` de l’élément `aspNetCore` sont définis.</span><span class="sxs-lookup"><span data-stu-id="64f71-350">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="64f71-351">Tous les dossiers du chemin `stdoutLogFile` sont créés par le module au moment de la création du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="64f71-351">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="64f71-352">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).</span><span class="sxs-lookup"><span data-stu-id="64f71-352">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="64f71-353">Aucune rotation n’est appliquée aux journaux, sauf en cas de recyclage/redémarrage du processus.</span><span class="sxs-lookup"><span data-stu-id="64f71-353">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="64f71-354">Il incombe à l’hébergeur de limiter l’espace disque utilisé par les journaux.</span><span class="sxs-lookup"><span data-stu-id="64f71-354">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="64f71-355">L’utilisation du journal stdout est uniquement recommandée pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-355">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="64f71-356">N’utilisez pas le journal stdout à des fins de journalisation d’application générale.</span><span class="sxs-lookup"><span data-stu-id="64f71-356">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="64f71-357">Pour journaliser la routine d’une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et appliquez une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="64f71-357">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="64f71-358">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="64f71-358">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="64f71-359">Un horodatage et une extension de fichier sont ajoutés automatiquement quand le fichier journal est créé.</span><span class="sxs-lookup"><span data-stu-id="64f71-359">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="64f71-360">Le nom du fichier journal est créé en ajoutant l’horodatage, un ID de processus et une extension de fichier ( *.log*) au dernier segment du chemin d'accès `stdoutLogFile` (généralement *stdout*), séparés par des traits de soulignement.</span><span class="sxs-lookup"><span data-stu-id="64f71-360">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="64f71-361">Si le chemin d'accès `stdoutLogFile` se termine par *stdout*, un journal pour une application avec un PID de 1934 créé le 5/2/2018 à 19:42:32 affiche le nom de fichier *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="64f71-361">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-362">Si `stdoutLogEnabled` a la valeur false, les erreurs qui se produisent au moment du démarrage de l’application sont capturées et émises dans le journal des événements (30 Ko maximum).</span><span class="sxs-lookup"><span data-stu-id="64f71-362">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="64f71-363">Après le démarrage, tous les journaux supplémentaires sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="64f71-363">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="64f71-364">L’exemple d’élément `aspNetCore` suivant configure la journalisation stdout pour une application hébergée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="64f71-364">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="64f71-365">Un chemin d’accès local ou un chemin de partage réseau peut être utilisé pour une journalisation locale.</span><span class="sxs-lookup"><span data-stu-id="64f71-365">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="64f71-366">Vérifiez que l’identité de l’utilisateur AppPool à l’autorisation d’écrire dans le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="64f71-366">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="64f71-367">Journaux de diagnostic améliorés</span><span class="sxs-lookup"><span data-stu-id="64f71-367">Enhanced diagnostic logs</span></span>

<span data-ttu-id="64f71-368">Le module ASP.NET Core est configurable pour proposer des journaux de diagnostic améliorés.</span><span class="sxs-lookup"><span data-stu-id="64f71-368">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="64f71-369">Ajoutez l’élément `<handlerSettings>` à l’élément `<aspNetCore>` dans *web.config*. L’affectation de la valeur `TRACE` à `debugLevel` expose une fidélité plus élevée des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="64f71-369">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64f71-370">Tous les dossiers dans le chemin d’accès (*logs* dans l’exemple précédent) sont créés par le module lorsque le fichier journal est créé.</span><span class="sxs-lookup"><span data-stu-id="64f71-370">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="64f71-371">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).</span><span class="sxs-lookup"><span data-stu-id="64f71-371">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64f71-372">Les dossiers situés dans le chemin fourni pour la valeur `<handlerSetting>` (*logs* dans l’exemple précédent) ne sont pas créés automatiquement par le module. Ils doivent préexister dans le déploiement.</span><span class="sxs-lookup"><span data-stu-id="64f71-372">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="64f71-373">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).</span><span class="sxs-lookup"><span data-stu-id="64f71-373">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-374">Les valeurs du niveau de débogage (`debugLevel`) peuvent inclure à la fois le niveau et l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="64f71-374">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="64f71-375">Niveaux (dans l’ordre du moins au plus détaillé) :</span><span class="sxs-lookup"><span data-stu-id="64f71-375">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="64f71-376">ERROR</span><span class="sxs-lookup"><span data-stu-id="64f71-376">ERROR</span></span>
* <span data-ttu-id="64f71-377">WARNING</span><span class="sxs-lookup"><span data-stu-id="64f71-377">WARNING</span></span>
* <span data-ttu-id="64f71-378">INFO</span><span class="sxs-lookup"><span data-stu-id="64f71-378">INFO</span></span>
* <span data-ttu-id="64f71-379">TRACE</span><span class="sxs-lookup"><span data-stu-id="64f71-379">TRACE</span></span>

<span data-ttu-id="64f71-380">Emplacements (plusieurs emplacements sont autorisés) :</span><span class="sxs-lookup"><span data-stu-id="64f71-380">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="64f71-381">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="64f71-381">CONSOLE</span></span>
* <span data-ttu-id="64f71-382">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="64f71-382">EVENTLOG</span></span>
* <span data-ttu-id="64f71-383">FICHIER</span><span class="sxs-lookup"><span data-stu-id="64f71-383">FILE</span></span>

<span data-ttu-id="64f71-384">Les paramètres de gestionnaire peuvent également être fournis par le biais de variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="64f71-384">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="64f71-385">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Chemin du fichier journal de débogage.</span><span class="sxs-lookup"><span data-stu-id="64f71-385">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="64f71-386">(Par défaut : *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="64f71-386">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="64f71-387">`ASPNETCORE_MODULE_DEBUG` &ndash; Paramètre du niveau de débogage.</span><span class="sxs-lookup"><span data-stu-id="64f71-387">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="64f71-388">Ne laissez **pas** la journalisation du débogage activée dans le déploiement plus longtemps que nécessaire pour résoudre un problème.</span><span class="sxs-lookup"><span data-stu-id="64f71-388">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="64f71-389">La taille du journal n’est pas limitée.</span><span class="sxs-lookup"><span data-stu-id="64f71-389">The size of the log isn't limited.</span></span> <span data-ttu-id="64f71-390">Si vous laissez la journalisation du débogage activée, vous risquez d’épuiser l’espace disque disponible et de bloquer le serveur ou le service d’application.</span><span class="sxs-lookup"><span data-stu-id="64f71-390">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="64f71-391">Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple d’élément `aspNetCore` dans le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="64f71-391">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="modify-the-stack-size"></a><span data-ttu-id="64f71-392">Modifier la taille de pile</span><span class="sxs-lookup"><span data-stu-id="64f71-392">Modify the stack size</span></span>

<span data-ttu-id="64f71-393">Configurez la taille de pile managée à l’aide du paramètre `stackSize` en octets.</span><span class="sxs-lookup"><span data-stu-id="64f71-393">Configure the managed stack size using the `stackSize` setting in bytes.</span></span> <span data-ttu-id="64f71-394">La taille par défaut est `1048576` octets (1 Mo).</span><span class="sxs-lookup"><span data-stu-id="64f71-394">The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="64f71-395">La configuration du proxy utilise le protocole HTTP et un jeton d’appariement</span><span class="sxs-lookup"><span data-stu-id="64f71-395">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-396">*S’applique uniquement à l’hébergement out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="64f71-396">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="64f71-397">Le proxy créé entre le module ASP.NET Core et Kestrel utilise le protocole HTTP.</span><span class="sxs-lookup"><span data-stu-id="64f71-397">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="64f71-398">Utiliser HTTP est une optimisation des performances où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau.</span><span class="sxs-lookup"><span data-stu-id="64f71-398">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="64f71-399">Il n’existe aucun risque d’écoute clandestine du trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="64f71-399">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="64f71-400">Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel ont été traitées par IIS et ne proviennent pas d’une autre source.</span><span class="sxs-lookup"><span data-stu-id="64f71-400">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="64f71-401">Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module.</span><span class="sxs-lookup"><span data-stu-id="64f71-401">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="64f71-402">Le jeton d’appariement est également défini dans un en-tête (`MS-ASPNETCORE-TOKEN`) sur toutes les demandes traitées.</span><span class="sxs-lookup"><span data-stu-id="64f71-402">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="64f71-403">L'intergiciel IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d'appariement correspond à la valeur de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="64f71-403">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="64f71-404">Si les valeurs de jeton ne correspondent pas, la demande est connectée et rejetée.</span><span class="sxs-lookup"><span data-stu-id="64f71-404">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="64f71-405">La variable d’environnement du jeton d'appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="64f71-405">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="64f71-406">Sans connaître la valeur du jeton d'appariement, une personne malveillante ne peut pas soumettre des demandes qui contournent la vérification de l’intergiciel IIS.</span><span class="sxs-lookup"><span data-stu-id="64f71-406">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="64f71-407">Module ASP.NET Core avec une configuration partagée IIS</span><span class="sxs-lookup"><span data-stu-id="64f71-407">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="64f71-408">Le programme d’installation du module ASP.NET Core s’exécute avec les privilèges du compte **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="64f71-408">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="64f71-409">Comme le compte système local n’a pas l’autorisation de modifier le chemin de partage utilisé par la configuration partagée IIS, le programme d’installation génère une erreur d’accès refusé pendant la tentative de configuration des paramètres du module dans le fichier *applicationHost.config* du partage.</span><span class="sxs-lookup"><span data-stu-id="64f71-409">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="64f71-410">Quand une configuration partagée IIS est utilisée sur la même machine que l’installation IIS, il convient d’exécuter le programme d’installation du bundle d’hébergement ASP.NET Core avec le paramètre `OPT_NO_SHARED_CONFIG_CHECK` défini sur `1` :</span><span class="sxs-lookup"><span data-stu-id="64f71-410">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="64f71-411">Quand le chemin de la configuration partagée ne se trouve pas sur la même machine que l’installation IIS, effectuez ces étapes :</span><span class="sxs-lookup"><span data-stu-id="64f71-411">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="64f71-412">Désactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="64f71-412">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="64f71-413">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="64f71-413">Run the installer.</span></span>
1. <span data-ttu-id="64f71-414">Exportez le fichier *applicationHost.config* mis à jour vers le partage.</span><span class="sxs-lookup"><span data-stu-id="64f71-414">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="64f71-415">Réactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="64f71-415">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="64f71-416">Lorsque vous utilisez une configuration partagée IIS, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="64f71-416">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="64f71-417">Désactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="64f71-417">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="64f71-418">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="64f71-418">Run the installer.</span></span>
1. <span data-ttu-id="64f71-419">Exportez le fichier *applicationHost.config* mis à jour vers le partage.</span><span class="sxs-lookup"><span data-stu-id="64f71-419">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="64f71-420">Réactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="64f71-420">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="64f71-421">Version du module et journaux du programme d’installation du bundle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="64f71-421">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="64f71-422">Pour déterminer la version du module ASP.NET Core installé :</span><span class="sxs-lookup"><span data-stu-id="64f71-422">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="64f71-423">Sur le système hôte, accédez à *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="64f71-423">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="64f71-424">Recherchez le fichier *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="64f71-424">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="64f71-425">Cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="64f71-425">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="64f71-426">Sélectionnez l'onglet **Détails**. Le **version du fichier** et la **version du produit** représentent la version installée du module.</span><span class="sxs-lookup"><span data-stu-id="64f71-426">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="64f71-427">Les journaux du programme d’installation du bundle d’hébergement du module se trouvent dans *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="64f71-427">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="64f71-428">Emplacements des fichiers du module, du schéma et de configuration</span><span class="sxs-lookup"><span data-stu-id="64f71-428">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="64f71-429">Module</span><span class="sxs-lookup"><span data-stu-id="64f71-429">Module</span></span>

<span data-ttu-id="64f71-430">**IIS (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="64f71-430">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="64f71-431">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-431">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="64f71-432">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-432">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="64f71-433">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-433">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="64f71-434">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-434">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="64f71-435">**IIS Express (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="64f71-435">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="64f71-436">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-436">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="64f71-437">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-437">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="64f71-438">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-438">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="64f71-439">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="64f71-439">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="64f71-440">Schéma</span><span class="sxs-lookup"><span data-stu-id="64f71-440">Schema</span></span>

<span data-ttu-id="64f71-441">**IIS**</span><span class="sxs-lookup"><span data-stu-id="64f71-441">**IIS**</span></span>

* <span data-ttu-id="64f71-442">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="64f71-442">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="64f71-443">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="64f71-443">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="64f71-444">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="64f71-444">**IIS Express**</span></span>

* <span data-ttu-id="64f71-445">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="64f71-445">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="64f71-446">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="64f71-446">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="64f71-447">Configuration</span><span class="sxs-lookup"><span data-stu-id="64f71-447">Configuration</span></span>

<span data-ttu-id="64f71-448">**IIS**</span><span class="sxs-lookup"><span data-stu-id="64f71-448">**IIS**</span></span>

* <span data-ttu-id="64f71-449">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="64f71-449">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="64f71-450">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="64f71-450">**IIS Express**</span></span>

* <span data-ttu-id="64f71-451">Visual Studio : {RACINE DE L’APPLICATION}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="64f71-451">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="64f71-452">*iisexpress.exe* CLI : %PROFILUTILISATEUR%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="64f71-452">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="64f71-453">Vous trouverez les fichiers en recherchant *aspnetcore* dans le fichier *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="64f71-453">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64f71-454">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="64f71-454">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="64f71-455">Dépôt GitHub du module ASP.NET Core (source de référence)</span><span class="sxs-lookup"><span data-stu-id="64f71-455">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
