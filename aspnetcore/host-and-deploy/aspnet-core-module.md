---
title: Module ASP.NET Core
author: guardrex
description: Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 4eea360d08c79b889db00132109cf49492f84de6
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837778"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="6387c-103">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6387c-103">ASP.NET Core Module</span></span>

<span data-ttu-id="6387c-104">Par [Nowak](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6387c-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6387c-105">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour effectuer l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="6387c-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="6387c-106">Héberger une application ASP.NET Core à l’intérieur du processus de travail IIS (`w3wp.exe`), appelé [modèle d’hébergement in-process](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6387c-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="6387c-107">Transférer les requêtes web à une application ASP.NET Core du back-end exécutant le [serveur Kestrel](xref:fundamentals/servers/kestrel), appelé [modèle d’hébergement out-of-process](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="6387c-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="6387c-108">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="6387c-108">Supported Windows versions:</span></span>

* <span data-ttu-id="6387c-109">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="6387c-109">Windows 7 or later</span></span>
* <span data-ttu-id="6387c-110">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="6387c-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6387c-111">Lors de l’hébergement in-process, le module utilise une implémentation du serveur in-process pour IIS, nommée serveur HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6387c-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="6387c-112">Lors de l’hébergement out-of-process, le module fonctionne uniquement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6387c-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="6387c-113">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6387c-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="6387c-114">Modèles d'hébergement</span><span class="sxs-lookup"><span data-stu-id="6387c-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="6387c-115">Modèle d’hébergement in-process</span><span class="sxs-lookup"><span data-stu-id="6387c-115">In-process hosting model</span></span>

<span data-ttu-id="6387c-116">Pour configurer l’hébergement in-process dans une application, ajoutez la propriété `<AspNetCoreHostingModel>` au fichier projet de l’application en lui attribuant la valeur `InProcess` (l’hébergement out-of-process est défini sur `OutOfProcess`) :</span><span class="sxs-lookup"><span data-stu-id="6387c-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6387c-117">Le modèle d’hébergement in-process n’est pas pris en charge pour les applications ASP.NET Core qui ciblent le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6387c-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="6387c-118">Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6387c-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="6387c-119">Les caractéristiques suivantes s’appliquent lors de l’hébergement in-process :</span><span class="sxs-lookup"><span data-stu-id="6387c-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="6387c-120">Le serveur HTTP IIS (`IISHttpServer`) est utilisé à la place du serveur [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="6387c-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="6387c-121">L’[attribut requestTimeout](#attributes-of-the-aspnetcore-element) ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="6387c-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="6387c-122">Le partage d’un pool d’applications entre les applications n’est pas pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6387c-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="6387c-123">Utilisez un pool d’applications par application.</span><span class="sxs-lookup"><span data-stu-id="6387c-123">Use one app pool per app.</span></span>

* <span data-ttu-id="6387c-124">Lorsque vous utilisez [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou que vous placez manuellement un [fichier app_offline.htm dans le déploiement](xref:host-and-deploy/iis/index#locked-deployment-files), l’application peut ne pas s’arrêter immédiatement si une connexion est ouverte.</span><span class="sxs-lookup"><span data-stu-id="6387c-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6387c-125">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="6387c-126">L’architecture (nombre de bits) de l’application et le runtime installé (x64 ou x86) doivent correspondre à l’architecture du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="6387c-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="6387c-127">Si vous configurez manuellement l’hôte de l’application avec `WebHostBuilder` (et non [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)), et que l’application est exécutée directement sur le serveur Kestrel (auto-hébergée), appelez `UseKestrel` avant d’appeler `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="6387c-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="6387c-128">Si l’ordre est inversé, l’hôte ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="6387c-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="6387c-129">Les déconnexions du client sont détectées.</span><span class="sxs-lookup"><span data-stu-id="6387c-129">Client disconnects are detected.</span></span> <span data-ttu-id="6387c-130">Le jeton d’annulation [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) est annulé quand le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="6387c-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="6387c-131"><xref:System.IO.Directory.GetCurrentDirectory*> retourne le répertoire de travail du processus démarré par IIS, et non le répertoire de l’application (par exemple, *C:\Windows\System32\inetsrv* pour *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="6387c-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="6387c-132">Pour connaître l’exemple de code qui définit le répertoire actuel de l’application, consultez [CurrentDirectoryHelpers, classe](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="6387c-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="6387c-133">Appelez la méthode `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="6387c-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="6387c-134">Les appels suivants à <xref:System.IO.Directory.GetCurrentDirectory*> fournissent le répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="6387c-135">Modèle d’hébergement out-of-process</span><span class="sxs-lookup"><span data-stu-id="6387c-135">Out-of-process hosting model</span></span>

<span data-ttu-id="6387c-136">Pour configurer une application pour un hébergement out-of-process, utilisez l’une des approches suivantes dans le fichier de projet :</span><span class="sxs-lookup"><span data-stu-id="6387c-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="6387c-137">Ne spécifiez pas la propriété `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="6387c-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="6387c-138">Si la propriété `<AspNetCoreHostingModel>` n’est pas présente dans le fichier, la valeur par défaut est `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="6387c-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="6387c-139">Définissez la valeur de la propriété `<AspNetCoreHostingModel>` sur `OutOfProcess` (l’hébergement in-process est défini avec `InProcess`) :</span><span class="sxs-lookup"><span data-stu-id="6387c-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="6387c-140">Le serveur [Kestrel](xref:fundamentals/servers/kestrel) est utilisé à la place du serveur HTTP IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="6387c-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="6387c-141">Modifications du modèle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="6387c-141">Hosting model changes</span></span>

<span data-ttu-id="6387c-142">Si le paramètre `hostingModel` est modifié dans le fichier *web.config* (comme expliqué dans la section [Configuration avec web.config](#configuration-with-webconfig)), le module recycle le processus de travail pour IIS.</span><span class="sxs-lookup"><span data-stu-id="6387c-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="6387c-143">Pour IIS Express, le module ne recycle pas le processus de travail, mais déclenche plutôt un arrêt approprié du processus IIS Express en cours.</span><span class="sxs-lookup"><span data-stu-id="6387c-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="6387c-144">La requête suivante à l’application génère un nouveau processus IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6387c-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="6387c-145">Nom du processus</span><span class="sxs-lookup"><span data-stu-id="6387c-145">Process name</span></span>

<span data-ttu-id="6387c-146">`Process.GetCurrentProcess().ProcessName` signale `w3wp`/`iisexpress` (in-process) ou `dotnet` (out-of-process).</span><span class="sxs-lookup"><span data-stu-id="6387c-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6387c-147">Le module ASP.NET Core est un module IIS natif qui se connecte dans le pipeline IIS pour transférer les requêtes web aux applications ASP.NET Core du back-end.</span><span class="sxs-lookup"><span data-stu-id="6387c-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="6387c-148">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="6387c-148">Supported Windows versions:</span></span>

* <span data-ttu-id="6387c-149">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="6387c-149">Windows 7 or later</span></span>
* <span data-ttu-id="6387c-150">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="6387c-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6387c-151">Le module fonctionne seulement avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6387c-151">The module only works with Kestrel.</span></span> <span data-ttu-id="6387c-152">Le module n’est pas compatible avec [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="6387c-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="6387c-153">Étant donné que les applications ASP.NET Core s’exécutent dans un processus distinct du processus de travail IIS, le module traite également la gestion des processus.</span><span class="sxs-lookup"><span data-stu-id="6387c-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="6387c-154">Le module démarre le processus de l’application ASP.NET Core quand la première requête arrive et redémarre l’application si elle plante.</span><span class="sxs-lookup"><span data-stu-id="6387c-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="6387c-155">Il s’agit essentiellement du même comportement que celui des applications ASP.NET 4.x qui s’exécutent in-process dans IIS et sont gérées par le [service WAS (Windows Activation Service)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="6387c-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="6387c-156">Le schéma suivant illustre la relation entre IIS, le module ASP.NET Core et une application :</span><span class="sxs-lookup"><span data-stu-id="6387c-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Module ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="6387c-158">Les requêtes arrivent du web au pilote HTTP.sys en mode noyau.</span><span class="sxs-lookup"><span data-stu-id="6387c-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="6387c-159">Le pilote route les requêtes vers IIS sur le port configuré du site web, généralement 80 (HTTP) ou 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6387c-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="6387c-160">Le module transfère les requêtes à Kestrel sur un port aléatoire pour l’application, qui n’est ni le port 80 ni le port 443.</span><span class="sxs-lookup"><span data-stu-id="6387c-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="6387c-161">Le module spécifie le port via une variable d’environnement au démarrage, et le middleware d’intégration IIS configure le serveur pour qu’il écoute sur `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="6387c-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="6387c-162">Des vérifications supplémentaires sont effectuées, et les requêtes qui ne proviennent pas du module sont rejetées.</span><span class="sxs-lookup"><span data-stu-id="6387c-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="6387c-163">Le module ne prend pas en charge le transfert HTTPS : les requêtes sont donc transférées via HTTP, même si IIS les reçoit via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6387c-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="6387c-164">Dès que Kestrel sélectionne la requête dans le module, celle-ci est envoyée (push) dans le pipeline de middlewares d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6387c-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="6387c-165">Le pipeline de middlewares traite la requête et la passe en tant qu’instance de `HttpContext` à la logique de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="6387c-166">Le middleware ajouté par l’intégration d’IIS met à jour le schéma, l’adresse IP distante et la base du chemin pour prendre en compte le transfert de la requête à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6387c-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="6387c-167">La réponse de l’application est ensuite repassée à IIS, qui la renvoie au client HTTP à l’origine de la requête.</span><span class="sxs-lookup"><span data-stu-id="6387c-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="6387c-168">De nombreux modules natifs, comme l’authentification Windows, restent actifs.</span><span class="sxs-lookup"><span data-stu-id="6387c-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="6387c-169">Pour obtenir plus d’informations sur les modules IIS actifs avec le module ASP.NET Core, consultez <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="6387c-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="6387c-170">Le module ASP.NET Core peut également  :</span><span class="sxs-lookup"><span data-stu-id="6387c-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="6387c-171">Définir des variables d’environnement pour le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="6387c-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="6387c-172">Journaliser la sortie de stdout dans un stockage de fichiers pour résoudre les problèmes de démarrage.</span><span class="sxs-lookup"><span data-stu-id="6387c-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="6387c-173">Transférer des jetons d’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="6387c-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="6387c-174">Comment installer et utiliser le module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6387c-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="6387c-175">Pour obtenir des instructions sur l’installation et l’utilisation du module ASP.NET Core, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="6387c-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="6387c-176">Configuration avec web.config</span><span class="sxs-lookup"><span data-stu-id="6387c-176">Configuration with web.config</span></span>

<span data-ttu-id="6387c-177">Le module ASP.NET Core est configuré avec la section `aspNetCore` du nœud `system.webServer` dans le fichier *web.config* du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="6387c-178">Le fichier *web.config* suivant est publié pour un [déploiement dépendant du framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) et configure le module ASP.NET Core pour gérer les demandes du site :</span><span class="sxs-lookup"><span data-stu-id="6387c-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="6387c-179">Le fichier *web.config* suivant est publié pour un [déploiement autonome](/dotnet/articles/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="6387c-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="6387c-180">La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> est définie sur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<emplacement>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications qui se trouvent dans un sous-répertoire de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="6387c-181">Lorsqu’une application est déployée sur [Azure App Service](https://azure.microsoft.com/services/app-service/), le chemin d’accès `stdoutLogFile` est défini sur `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="6387c-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="6387c-182">Le chemin d’accès enregistre les journaux stdout dans le dossier *LogFiles*, un emplacement automatiquement créé par le service.</span><span class="sxs-lookup"><span data-stu-id="6387c-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="6387c-183">Pour plus d’informations sur la configuration d’une sous-application IIS, consultez <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="6387c-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="6387c-184">Attributs de l’élément aspNetCore</span><span class="sxs-lookup"><span data-stu-id="6387c-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="6387c-185">Attribut</span><span class="sxs-lookup"><span data-stu-id="6387c-185">Attribute</span></span> | <span data-ttu-id="6387c-186">Description</span><span class="sxs-lookup"><span data-stu-id="6387c-186">Description</span></span> | <span data-ttu-id="6387c-187">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6387c-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6387c-188">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-188">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-189">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6387c-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6387c-190">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-191">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="6387c-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6387c-192">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-193">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="6387c-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6387c-194">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="6387c-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="6387c-195">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-195">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-196">Spécifie le modèle d’hébergement comme étant in-process (`InProcess`) ou out-of-process (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="6387c-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="6387c-197">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-198">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="6387c-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="6387c-199">&dagger;Pour l’hébergement in-process, la valeur est limitée à `1`.</span><span class="sxs-lookup"><span data-stu-id="6387c-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="6387c-200">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="6387c-200">Default: `1`</span></span><br><span data-ttu-id="6387c-201">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="6387c-201">Min: `1`</span></span><br><span data-ttu-id="6387c-202">Max : `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="6387c-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="6387c-203">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="6387c-203">Required string attribute.</span></span></p><p><span data-ttu-id="6387c-204">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6387c-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6387c-205">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6387c-205">Relative paths are supported.</span></span> <span data-ttu-id="6387c-206">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6387c-207">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-208">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6387c-209">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="6387c-210">Non pris en charge avec hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="6387c-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="6387c-211">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="6387c-211">Default: `10`</span></span><br><span data-ttu-id="6387c-212">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-212">Min: `0`</span></span><br><span data-ttu-id="6387c-213">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="6387c-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6387c-214">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6387c-215">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6387c-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6387c-216">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="6387c-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="6387c-217">Ne s’applique pas à l’hébergement in-process.</span><span class="sxs-lookup"><span data-stu-id="6387c-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="6387c-218">Pour l’hébergement in-process, le module attend que l’application traite la requête.</span><span class="sxs-lookup"><span data-stu-id="6387c-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="6387c-219">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-219">Default: `00:02:00`</span></span><br><span data-ttu-id="6387c-220">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-220">Min: `00:00:00`</span></span><br><span data-ttu-id="6387c-221">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6387c-222">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-223">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="6387c-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6387c-224">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="6387c-224">Default: `10`</span></span><br><span data-ttu-id="6387c-225">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-225">Min: `0`</span></span><br><span data-ttu-id="6387c-226">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="6387c-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6387c-227">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-228">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="6387c-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6387c-229">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="6387c-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6387c-230">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6387c-231">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="6387c-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6387c-232">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="6387c-232">Default: `120`</span></span><br><span data-ttu-id="6387c-233">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-233">Min: `0`</span></span><br><span data-ttu-id="6387c-234">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="6387c-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6387c-235">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-236">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6387c-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6387c-237">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-237">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-238">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="6387c-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6387c-239">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6387c-240">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="6387c-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6387c-241">Tous les dossiers fournis dans le chemin sont créés par le module au moment de la création du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="6387c-241">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="6387c-242">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6387c-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6387c-243">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="6387c-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="6387c-244">Attribut</span><span class="sxs-lookup"><span data-stu-id="6387c-244">Attribute</span></span> | <span data-ttu-id="6387c-245">Description</span><span class="sxs-lookup"><span data-stu-id="6387c-245">Description</span></span> | <span data-ttu-id="6387c-246">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6387c-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6387c-247">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-247">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-248">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6387c-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6387c-249">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-250">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="6387c-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6387c-251">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-252">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="6387c-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6387c-253">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="6387c-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6387c-254">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-255">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="6387c-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="6387c-256">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="6387c-256">Default: `1`</span></span><br><span data-ttu-id="6387c-257">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="6387c-257">Min: `1`</span></span><br><span data-ttu-id="6387c-258">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="6387c-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6387c-259">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="6387c-259">Required string attribute.</span></span></p><p><span data-ttu-id="6387c-260">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6387c-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6387c-261">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6387c-261">Relative paths are supported.</span></span> <span data-ttu-id="6387c-262">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6387c-263">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-264">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6387c-265">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6387c-266">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="6387c-266">Default: `10`</span></span><br><span data-ttu-id="6387c-267">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-267">Min: `0`</span></span><br><span data-ttu-id="6387c-268">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="6387c-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6387c-269">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6387c-270">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6387c-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6387c-271">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.1 ou version ultérieure, la valeur `requestTimeout` est spécifiée en heures, minutes et secondes.</span><span class="sxs-lookup"><span data-stu-id="6387c-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="6387c-272">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-272">Default: `00:02:00`</span></span><br><span data-ttu-id="6387c-273">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-273">Min: `00:00:00`</span></span><br><span data-ttu-id="6387c-274">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6387c-275">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-276">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="6387c-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6387c-277">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="6387c-277">Default: `10`</span></span><br><span data-ttu-id="6387c-278">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-278">Min: `0`</span></span><br><span data-ttu-id="6387c-279">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="6387c-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6387c-280">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-281">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="6387c-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6387c-282">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="6387c-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6387c-283">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="6387c-284">La valeur 0 (zéro) n’est **pas** considérée comme un délai infini.</span><span class="sxs-lookup"><span data-stu-id="6387c-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="6387c-285">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="6387c-285">Default: `120`</span></span><br><span data-ttu-id="6387c-286">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-286">Min: `0`</span></span><br><span data-ttu-id="6387c-287">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="6387c-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6387c-288">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-289">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6387c-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6387c-290">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-290">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-291">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="6387c-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6387c-292">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6387c-293">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="6387c-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6387c-294">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="6387c-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6387c-295">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6387c-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6387c-296">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="6387c-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="6387c-297">Attribut</span><span class="sxs-lookup"><span data-stu-id="6387c-297">Attribute</span></span> | <span data-ttu-id="6387c-298">Description</span><span class="sxs-lookup"><span data-stu-id="6387c-298">Description</span></span> | <span data-ttu-id="6387c-299">Par défaut</span><span class="sxs-lookup"><span data-stu-id="6387c-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="6387c-300">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-300">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-301">Arguments pour l’exécutable spécifié dans **processPath**.</span><span class="sxs-lookup"><span data-stu-id="6387c-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="6387c-302">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-303">Si la valeur est true, la page **502.5 - Échec du processus** est supprimée, et la page de code d’état 502 configurée dans le fichier *web.config* est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="6387c-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="6387c-304">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-305">Si la valeur est true, le jeton est transmis au processus enfant qui écoute sur %ASPNETCORE_PORT% sous la forme d’un en-tête 'MS-ASPNETCORE-WINAUTHTOKEN' par demande.</span><span class="sxs-lookup"><span data-stu-id="6387c-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="6387c-306">Il incombe à ce processus d’appeler CloseHandle sur ce jeton par demande.</span><span class="sxs-lookup"><span data-stu-id="6387c-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="6387c-307">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-308">Spécifie le nombre d’instances du processus indiqué dans le paramètre **processPath** qui peuvent être lancées par application.</span><span class="sxs-lookup"><span data-stu-id="6387c-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="6387c-309">Par défaut : `1`</span><span class="sxs-lookup"><span data-stu-id="6387c-309">Default: `1`</span></span><br><span data-ttu-id="6387c-310">Min : `1`</span><span class="sxs-lookup"><span data-stu-id="6387c-310">Min: `1`</span></span><br><span data-ttu-id="6387c-311">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="6387c-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="6387c-312">Attribut de chaîne requis.</span><span class="sxs-lookup"><span data-stu-id="6387c-312">Required string attribute.</span></span></p><p><span data-ttu-id="6387c-313">Chemin d’accès au fichier exécutable lançant un processus d’écoute des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="6387c-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="6387c-314">Les chemins d’accès relatifs sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6387c-314">Relative paths are supported.</span></span> <span data-ttu-id="6387c-315">Si le chemin d’accès commence par `.`, il est considéré comme étant relatif par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="6387c-316">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-317">Indique le nombre de fois où le processus spécifié dans **processPath** est autorisé à se bloquer par minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="6387c-318">Si cette limite est dépassée, le module arrête le lancement du processus pour le reste de la minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="6387c-319">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="6387c-319">Default: `10`</span></span><br><span data-ttu-id="6387c-320">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-320">Min: `0`</span></span><br><span data-ttu-id="6387c-321">Max : `100`</span><span class="sxs-lookup"><span data-stu-id="6387c-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="6387c-322">Attribut timespan facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="6387c-323">Spécifie la durée pendant laquelle le module ASP.NET Core attend une réponse de la part du processus à l’écoute sur %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="6387c-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="6387c-324">Dans les versions du module ASP.NET Core fournies avec ASP.NET Core 2.0 ou version antérieure, la durée `requestTimeout` doit être spécifiée en minutes entières, sinon la valeur par défaut est de 2 minutes.</span><span class="sxs-lookup"><span data-stu-id="6387c-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="6387c-325">Par défaut : `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-325">Default: `00:02:00`</span></span><br><span data-ttu-id="6387c-326">Min : `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-326">Min: `00:00:00`</span></span><br><span data-ttu-id="6387c-327">Max : `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="6387c-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="6387c-328">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-329">Durée en secondes pendant laquelle le module attend que l’exécutable s’arrête normalement lorsque le fichier *app_offline.htm* est détecté.</span><span class="sxs-lookup"><span data-stu-id="6387c-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="6387c-330">Par défaut : `10`</span><span class="sxs-lookup"><span data-stu-id="6387c-330">Default: `10`</span></span><br><span data-ttu-id="6387c-331">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-331">Min: `0`</span></span><br><span data-ttu-id="6387c-332">Max : `600`</span><span class="sxs-lookup"><span data-stu-id="6387c-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="6387c-333">Attribut entier facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="6387c-334">Durée en secondes pendant laquelle le module attend que le fichier exécutable démarre un processus à l’écoute sur le port.</span><span class="sxs-lookup"><span data-stu-id="6387c-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="6387c-335">Si cette limite de temps est dépassée, le module met fin au processus.</span><span class="sxs-lookup"><span data-stu-id="6387c-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="6387c-336">Le module tente de relancer le processus lorsqu’il reçoit une nouvelle requête, puis continue d’essayer de redémarrer le processus pour les requêtes entrantes suivantes, sauf si l’application ne démarre pas **rapidFailsPerMinute** un certain nombre de fois au cours de la dernière minute.</span><span class="sxs-lookup"><span data-stu-id="6387c-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="6387c-337">Par défaut : `120`</span><span class="sxs-lookup"><span data-stu-id="6387c-337">Default: `120`</span></span><br><span data-ttu-id="6387c-338">Min : `0`</span><span class="sxs-lookup"><span data-stu-id="6387c-338">Min: `0`</span></span><br><span data-ttu-id="6387c-339">Max : `3600`</span><span class="sxs-lookup"><span data-stu-id="6387c-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="6387c-340">Attribut booléen facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="6387c-341">Si la valeur est true, les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont redirigés vers le fichier spécifié dans **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6387c-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="6387c-342">Attribut de chaîne facultatif.</span><span class="sxs-lookup"><span data-stu-id="6387c-342">Optional string attribute.</span></span></p><p><span data-ttu-id="6387c-343">Spécifie le chemin d’accès relatif ou absolu pour lequel les attributs **stdout** et **stderr** du processus spécifié dans **processPath** sont consignés.</span><span class="sxs-lookup"><span data-stu-id="6387c-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="6387c-344">Les chemins d’accès relatifs sont relatifs par rapport à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="6387c-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="6387c-345">Tout chemin d’accès commençant par `.` est relatif par rapport à la racine du site et tous les autres chemins d’accès sont traités comme des chemins d’accès absolus.</span><span class="sxs-lookup"><span data-stu-id="6387c-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="6387c-346">Tous les dossiers spécifiés dans le chemin d’accès doivent exister pour permettre au module de créer le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="6387c-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="6387c-347">Si vous utilisez des délimiteurs de trait de soulignement, un horodatage, un ID de processus et une extension de fichier (*.log*) sont ajoutés au dernier segment du chemin d'accès **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="6387c-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="6387c-348">Si `.\logs\stdout` est fourni en tant que valeur, un exemple de journal stdout est enregistré en tant que *stdout_20180205194132_1934.log* dans le dossier *journaux* avec un enregistrement effectué le 05/02/2018 à 19:41:32 et un ID de processus de 1934.</span><span class="sxs-lookup"><span data-stu-id="6387c-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="6387c-349">Définition des variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="6387c-349">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6387c-350">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6387c-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6387c-351">Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6387c-351">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="6387c-352">Les variables d’environnement définies dans cette section prévalent sur les variables d’environnement système.</span><span class="sxs-lookup"><span data-stu-id="6387c-352">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6387c-353">Des variables d’environnement peuvent être spécifiées pour le processus dans l’attribut `processPath`.</span><span class="sxs-lookup"><span data-stu-id="6387c-353">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="6387c-354">Spécifiez une variable d’environnement à l’aide de l’élément enfant `<environmentVariable>` d’un élément de collection `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="6387c-354">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="6387c-355">Les variables d’environnement définies dans cette section sont en conflit avec les variables d’environnement système qui ont le même nom.</span><span class="sxs-lookup"><span data-stu-id="6387c-355">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="6387c-356">Si une variable d’environnement est définie à la fois dans le fichier *web.config* et au niveau du système dans Windows, la valeur provenant du fichier *web.config* est ajoutée à la valeur de la variable d’environnement système (par exemple, `ASPNETCORE_ENVIRONMENT: Development;Development`), ce qui empêche le démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-356">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="6387c-357">L’exemple suivant définit deux variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="6387c-357">The following example sets two environment variables.</span></span> <span data-ttu-id="6387c-358">`ASPNETCORE_ENVIRONMENT` définit l’environnement de l’application sur `Development`.</span><span class="sxs-lookup"><span data-stu-id="6387c-358">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="6387c-359">Un développeur peut définir temporairement cette valeur dans le fichier *web.config* afin de forcer le chargement de la [Page d’exception de développeur](xref:fundamentals/error-handling) lors du débogage d’une exception d’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-359">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="6387c-360">`CONFIG_DIR` est un exemple de variable d’environnement définie par l’utilisateur, dans laquelle le développeur a écrit du code qui lit la valeur de démarrage afin de former un chemin d’accès pour le chargement du fichier de configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-360">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="6387c-361">Une alternative à la définition directe de l’environnement dans le fichier *web.config* consiste à inclure la propriété `<EnvironmentName>` dans le profil de publication (*.pubxml*) ou le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="6387c-361">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="6387c-362">Cette approche définit l’environnement dans *web.config* lorsque le projet est publié :</span><span class="sxs-lookup"><span data-stu-id="6387c-362">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="6387c-363">Affectez uniquement `Development` à la variable d’environnement `ASPNETCORE_ENVIRONMENT` sur les serveurs de test et de préproduction non accessibles aux réseaux non approuvés, par exemple Internet.</span><span class="sxs-lookup"><span data-stu-id="6387c-363">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="6387c-364">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="6387c-364">app_offline.htm</span></span>

<span data-ttu-id="6387c-365">Si un fichier portant le nom *app_offline.htm* est détecté dans le répertoire racine d’une application, le module ASP.NET Core tente d’arrêter normalement l’application, puis interrompt le traitement des requêtes entrantes.</span><span class="sxs-lookup"><span data-stu-id="6387c-365">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="6387c-366">Si l’application est toujours active après le nombre de secondes défini dans `shutdownTimeLimit`, le module ASP.NET Core met fin au processus en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="6387c-366">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="6387c-367">Tant que le fichier *app_offline.htm* est présent, le module ASP.NET Core répond aux requêtes en renvoyant le contenu du fichier *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="6387c-367">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="6387c-368">Lorsque le fichier *app_offline.htm* est supprimé, la requête suivante démarre l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-368">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6387c-369">Lorsque vous utilisez le modèle d’hébergement out-of-process, l’application peut ne pas s’arrêter immédiatement s’il existe une connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="6387c-369">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="6387c-370">Par exemple, une connexion websocket peut retarder l’arrêt de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-370">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="6387c-371">Page d’erreur de démarrage</span><span class="sxs-lookup"><span data-stu-id="6387c-371">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6387c-372">Les hébergements in-process et out-of-process produisent des pages d’erreurs personnalisées quand ils ne parviennent pas à démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-372">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="6387c-373">Si le module ASP.NET Core ne trouve pas le gestionnaire de requêtes in-process ou out-of-process, une page de code d’état *500.0 - Échec de chargement du gestionnaire in-process/out-of-process* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6387c-373">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="6387c-374">Pour l’hébergement in-process, si le module ASP.NET Core ne peut pas démarrer l’application, une page de code d’état *500.30 - Échec de démarrage* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6387c-374">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="6387c-375">Pour l’hébergement out-of-process, si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6387c-375">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="6387c-376">Pour supprimer cette page et revenir à la page IIS de codes d’état 5xx par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6387c-376">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6387c-377">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6387c-377">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6387c-378">Si le module ASP.NET Core ne peut pas lancer le processus backend ou que ce dernier démarre mais ne parvient pas à écouter sur le port configuré, une page de code d’état *502.5 - Échec du processus* s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6387c-378">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="6387c-379">Pour supprimer cette page et revenir à la page IIS de code d’état 502 par défaut, utilisez l’attribut `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="6387c-379">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="6387c-380">Pour plus d’informations sur la configuration des messages d’erreur personnalisés, consultez [Erreurs HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="6387c-380">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Page de code d’état 502.5 Échec du processus](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="6387c-382">Création et redirection de journal</span><span class="sxs-lookup"><span data-stu-id="6387c-382">Log creation and redirection</span></span>

<span data-ttu-id="6387c-383">Le module ASP.NET Core redirige la sortie de console stdout et stderr vers le disque si les attributs `stdoutLogEnabled` et `stdoutLogFile` de l’élément `aspNetCore` sont définis.</span><span class="sxs-lookup"><span data-stu-id="6387c-383">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="6387c-384">Tous les dossiers du chemin `stdoutLogFile` sont créés par le module au moment de la création du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="6387c-384">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="6387c-385">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits (utilisez `IIS AppPool\<app_pool_name>` pour fournir les autorisations d’écriture).</span><span class="sxs-lookup"><span data-stu-id="6387c-385">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="6387c-386">Aucune rotation n’est appliquée aux journaux, sauf en cas de recyclage/redémarrage du processus.</span><span class="sxs-lookup"><span data-stu-id="6387c-386">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="6387c-387">Il incombe à l’hébergeur de limiter l’espace disque utilisé par les journaux.</span><span class="sxs-lookup"><span data-stu-id="6387c-387">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="6387c-388">L’utilisation du journal stdout est uniquement recommandée pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-388">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="6387c-389">N’utilisez pas le journal stdout à des fins de journalisation d’application générale.</span><span class="sxs-lookup"><span data-stu-id="6387c-389">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="6387c-390">Pour journaliser la routine d’une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et appliquez une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="6387c-390">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="6387c-391">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="6387c-391">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="6387c-392">Un horodatage et une extension de fichier sont ajoutés automatiquement quand le fichier journal est créé.</span><span class="sxs-lookup"><span data-stu-id="6387c-392">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="6387c-393">Le nom du fichier journal est créé en ajoutant l’horodatage, un ID de processus et une extension de fichier (*.log*) au dernier segment du chemin d'accès `stdoutLogFile` (généralement *stdout*), séparés par des traits de soulignement.</span><span class="sxs-lookup"><span data-stu-id="6387c-393">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="6387c-394">Si le chemin d'accès `stdoutLogFile` se termine par *stdout*, un journal pour une application avec un PID de 1934 créé le 5/2/2018 à 19:42:32 affiche le nom de fichier *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="6387c-394">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6387c-395">Si `stdoutLogEnabled` a la valeur false, les erreurs qui se produisent au moment du démarrage de l’application sont capturées et émises dans le journal des événements (30 Ko maximum).</span><span class="sxs-lookup"><span data-stu-id="6387c-395">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="6387c-396">Après le démarrage, tous les journaux supplémentaires sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="6387c-396">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="6387c-397">L’exemple d’élément `aspNetCore` suivant configure la journalisation stdout pour une application hébergée dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6387c-397">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="6387c-398">Un chemin d’accès local ou un chemin de partage réseau peut être utilisé pour une journalisation locale.</span><span class="sxs-lookup"><span data-stu-id="6387c-398">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="6387c-399">Vérifiez que l’identité de l’utilisateur AppPool à l’autorisation d’écrire dans le chemin d’accès fourni.</span><span class="sxs-lookup"><span data-stu-id="6387c-399">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="6387c-400">Journaux de diagnostic améliorés</span><span class="sxs-lookup"><span data-stu-id="6387c-400">Enhanced diagnostic logs</span></span>

<span data-ttu-id="6387c-401">Le module ASP.NET Core est configurable pour proposer des journaux de diagnostic améliorés.</span><span class="sxs-lookup"><span data-stu-id="6387c-401">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="6387c-402">Ajoutez l’élément `<handlerSettings>` à l’élément `<aspNetCore>` dans *web.config*. L’affectation de la valeur `TRACE` à `debugLevel` expose une fidélité plus élevée des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="6387c-402">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="6387c-403">Les valeurs du niveau de débogage (`debugLevel`) peuvent inclure à la fois le niveau et l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="6387c-403">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="6387c-404">Niveaux (dans l’ordre du moins au plus détaillé) :</span><span class="sxs-lookup"><span data-stu-id="6387c-404">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="6387c-405">ERROR</span><span class="sxs-lookup"><span data-stu-id="6387c-405">ERROR</span></span>
* <span data-ttu-id="6387c-406">WARNING</span><span class="sxs-lookup"><span data-stu-id="6387c-406">WARNING</span></span>
* <span data-ttu-id="6387c-407">INFO</span><span class="sxs-lookup"><span data-stu-id="6387c-407">INFO</span></span>
* <span data-ttu-id="6387c-408">TRACE</span><span class="sxs-lookup"><span data-stu-id="6387c-408">TRACE</span></span>

<span data-ttu-id="6387c-409">Emplacements (plusieurs emplacements sont autorisés) :</span><span class="sxs-lookup"><span data-stu-id="6387c-409">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="6387c-410">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="6387c-410">CONSOLE</span></span>
* <span data-ttu-id="6387c-411">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="6387c-411">EVENTLOG</span></span>
* <span data-ttu-id="6387c-412">FICHIER</span><span class="sxs-lookup"><span data-stu-id="6387c-412">FILE</span></span>

<span data-ttu-id="6387c-413">Les paramètres de gestionnaire peuvent également être fournis par le biais de variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="6387c-413">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="6387c-414">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Chemin du fichier journal de débogage.</span><span class="sxs-lookup"><span data-stu-id="6387c-414">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="6387c-415">(Par défaut : *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="6387c-415">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="6387c-416">`ASPNETCORE_MODULE_DEBUG` &ndash; Paramètre du niveau de débogage.</span><span class="sxs-lookup"><span data-stu-id="6387c-416">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="6387c-417">Ne laissez **pas** la journalisation du débogage activée dans le déploiement plus longtemps que nécessaire pour résoudre un problème.</span><span class="sxs-lookup"><span data-stu-id="6387c-417">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="6387c-418">La taille du journal n’est pas limitée.</span><span class="sxs-lookup"><span data-stu-id="6387c-418">The size of the log isn't limited.</span></span> <span data-ttu-id="6387c-419">Si vous laissez la journalisation du débogage activée, vous risquez d’épuiser l’espace disque disponible et de bloquer le serveur ou le service d’application.</span><span class="sxs-lookup"><span data-stu-id="6387c-419">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="6387c-420">Consultez [Configuration avec web.config](#configuration-with-webconfig) pour obtenir un exemple d’élément `aspNetCore` dans le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="6387c-420">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="6387c-421">La configuration du proxy utilise le protocole HTTP et un jeton d’appariement</span><span class="sxs-lookup"><span data-stu-id="6387c-421">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6387c-422">*S’applique uniquement à l’hébergement out-of-process.*</span><span class="sxs-lookup"><span data-stu-id="6387c-422">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="6387c-423">Le proxy créé entre le module ASP.NET Core et Kestrel utilise le protocole HTTP.</span><span class="sxs-lookup"><span data-stu-id="6387c-423">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="6387c-424">Utiliser HTTP est une optimisation des performances où le trafic entre le module et Kestrel a lieu sur une adresse de bouclage hors de l’interface réseau.</span><span class="sxs-lookup"><span data-stu-id="6387c-424">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="6387c-425">Il n’existe aucun risque d’écoute clandestine du trafic entre le module et Kestrel à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6387c-425">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="6387c-426">Un jeton d’appariement est utilisé pour garantir que les demandes reçues par Kestrel ont été traitées par IIS et ne proviennent pas d’une autre source.</span><span class="sxs-lookup"><span data-stu-id="6387c-426">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="6387c-427">Le jeton d’appariement est créé et défini dans une variable d’environnement (`ASPNETCORE_TOKEN`) par le module.</span><span class="sxs-lookup"><span data-stu-id="6387c-427">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="6387c-428">Le jeton d’appariement est également défini dans un en-tête (`MS-ASPNETCORE-TOKEN`) sur toutes les demandes traitées.</span><span class="sxs-lookup"><span data-stu-id="6387c-428">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="6387c-429">L'intergiciel IIS vérifie chaque demande qu’il reçoit pour confirmer que la valeur d’en-tête du jeton d'appariement correspond à la valeur de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="6387c-429">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="6387c-430">Si les valeurs de jeton ne correspondent pas, la demande est connectée et rejetée.</span><span class="sxs-lookup"><span data-stu-id="6387c-430">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="6387c-431">La variable d’environnement du jeton d'appariement et le trafic entre le module et Kestrel ne sont pas accessibles à partir d’un emplacement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6387c-431">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="6387c-432">Sans connaître la valeur du jeton d'appariement, une personne malveillante ne peut pas soumettre des demandes qui contournent la vérification de l’intergiciel IIS.</span><span class="sxs-lookup"><span data-stu-id="6387c-432">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="6387c-433">Module ASP.NET Core avec une configuration partagée IIS</span><span class="sxs-lookup"><span data-stu-id="6387c-433">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="6387c-434">Le programme d’installation du module ASP.NET Core s’exécute avec les privilèges du compte **système**.</span><span class="sxs-lookup"><span data-stu-id="6387c-434">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="6387c-435">Comme le compte système local n’a pas l’autorisation de modifier le chemin d’accès de partage utilisé par la configuration partagée IIS, le programme d’installation rencontre une erreur d’accès refusé lorsque vous tentez de configurer les paramètres du module dans *applicationHost.config* sur le partage.</span><span class="sxs-lookup"><span data-stu-id="6387c-435">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="6387c-436">Lorsque vous utilisez une configuration partagée IIS, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6387c-436">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="6387c-437">Désactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="6387c-437">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="6387c-438">Exécutez le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="6387c-438">Run the installer.</span></span>
1. <span data-ttu-id="6387c-439">Exportez le fichier *applicationHost.config* mis à jour vers le partage.</span><span class="sxs-lookup"><span data-stu-id="6387c-439">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="6387c-440">Réactivez la configuration partagée IIS.</span><span class="sxs-lookup"><span data-stu-id="6387c-440">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="6387c-441">Version du module et journaux du programme d’installation du bundle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="6387c-441">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="6387c-442">Pour déterminer la version du module ASP.NET Core installé :</span><span class="sxs-lookup"><span data-stu-id="6387c-442">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="6387c-443">Sur le système hôte, accédez à *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="6387c-443">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="6387c-444">Recherchez le fichier *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="6387c-444">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="6387c-445">Cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="6387c-445">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="6387c-446">Sélectionnez l'onglet **Détails**. Le **version du fichier** et la **version du produit** représentent la version installée du module.</span><span class="sxs-lookup"><span data-stu-id="6387c-446">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="6387c-447">Les journaux du programme d’installation du bundle d’hébergement du module se trouvent dans *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Le fichier est nommé *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="6387c-447">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="6387c-448">Emplacements des fichiers du module, du schéma et de configuration</span><span class="sxs-lookup"><span data-stu-id="6387c-448">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="6387c-449">Module</span><span class="sxs-lookup"><span data-stu-id="6387c-449">Module</span></span>

<span data-ttu-id="6387c-450">**IIS (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="6387c-450">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="6387c-451">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-451">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="6387c-452">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-452">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6387c-453">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-453">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="6387c-454">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-454">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="6387c-455">**IIS Express (x86/amd64) :**</span><span class="sxs-lookup"><span data-stu-id="6387c-455">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="6387c-456">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-456">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="6387c-457">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-457">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6387c-458">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-458">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="6387c-459">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="6387c-459">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="6387c-460">Schéma</span><span class="sxs-lookup"><span data-stu-id="6387c-460">Schema</span></span>

<span data-ttu-id="6387c-461">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6387c-461">**IIS**</span></span>

   * <span data-ttu-id="6387c-462">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6387c-462">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6387c-463">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6387c-463">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="6387c-464">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6387c-464">**IIS Express**</span></span>

   * <span data-ttu-id="6387c-465">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="6387c-465">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="6387c-466">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="6387c-466">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="6387c-467">Configuration</span><span class="sxs-lookup"><span data-stu-id="6387c-467">Configuration</span></span>

<span data-ttu-id="6387c-468">**IIS**</span><span class="sxs-lookup"><span data-stu-id="6387c-468">**IIS**</span></span>

   * <span data-ttu-id="6387c-469">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6387c-469">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="6387c-470">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="6387c-470">**IIS Express**</span></span>

   * <span data-ttu-id="6387c-471">Visual Studio : {RACINE DE L’APPLICATION}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="6387c-471">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="6387c-472">*iisexpress.exe* CLI : %PROFILUTILISATEUR%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="6387c-472">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="6387c-473">Vous trouverez les fichiers en recherchant *aspnetcore* dans le fichier *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="6387c-473">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6387c-474">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6387c-474">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="6387c-475">Dépôt GitHub du module ASP.NET Core (source de référence)</span><span class="sxs-lookup"><span data-stu-id="6387c-475">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
