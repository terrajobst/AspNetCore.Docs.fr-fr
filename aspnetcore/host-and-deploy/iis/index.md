---
title: Héberger ASP.NET Core sur Windows avec IIS
author: guardrex
description: Découvrez comment héberger des applications ASP.NET Core sur Windows Server Internet Information Services (IIS).
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1b34195dc51ca8dab5e8eda10f05ff6678fbc78c
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570163"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="e53c8-103">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="e53c8-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e53c8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="e53c8-105">Installer le bundle d’hébergement .NET Core</span><span class="sxs-lookup"><span data-stu-id="e53c8-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="e53c8-106">Systèmes d'exploitation pris en charge</span><span class="sxs-lookup"><span data-stu-id="e53c8-106">Supported operating systems</span></span>

<span data-ttu-id="e53c8-107">Les systèmes d’exploitation suivants sont pris en charge :</span><span class="sxs-lookup"><span data-stu-id="e53c8-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="e53c8-108">Windows 7 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-108">Windows 7 or later</span></span>
* <span data-ttu-id="e53c8-109">Windows Server 2008 R2 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="e53c8-110">Le [serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)) ne fonctionne pas dans une configuration de proxy inverse avec IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="e53c8-111">Utilisez le [serveur Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e53c8-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="e53c8-112">Pour plus d’informations sur l’hébergement dans Azure, consultez <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="e53c8-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="e53c8-113">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="e53c8-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="e53c8-114">Activer les composants IISIntegration</span><span class="sxs-lookup"><span data-stu-id="e53c8-114">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e53c8-115">Un fichier *Program.cs* standard appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour commencer à configurer un hôte :</span><span class="sxs-lookup"><span data-stu-id="e53c8-115">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e53c8-116">Un fichier *Program.cs* standard appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour commencer à configurer un hôte :</span><span class="sxs-lookup"><span data-stu-id="e53c8-116">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e53c8-117">**Modèle d’hébergement in-process**</span><span class="sxs-lookup"><span data-stu-id="e53c8-117">**In-process hosting model**</span></span>

<span data-ttu-id="e53c8-118">`CreateDefaultBuilder` appelle la méthode `UseIIS` pour démarrer le [CoreCLR](/dotnet/standard/glossary#coreclr) et héberger l’application à l’intérieur du processus Worker (*w3wp.exe* ou *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="e53c8-118">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="e53c8-119">Les tests de performance indiquent que l’hébergement d’une application.NET Core in-process fournit un débit de requêtes beaucoup plus élevé que l'hébergement de l’application out-of-process et des requêtes proxy vers [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e53c8-119">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="e53c8-120">**Modèle d’hébergement out-of-process**</span><span class="sxs-lookup"><span data-stu-id="e53c8-120">**Out-of-process hosting model**</span></span>

<span data-ttu-id="e53c8-121">Pour l'hébergement out-of-process avec IIS, `CreateDefaultBuilder` configure [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et permet l'intégration IIS en configurant le chemin de base et le port du [module ASP.NET Core ](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-121">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="e53c8-122">Le module ASP.NET Core génère un port dynamique à assigner au processus backend.</span><span class="sxs-lookup"><span data-stu-id="e53c8-122">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="e53c8-123">`CreateDefaultBuilder` appelle la méthode <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>.</span><span class="sxs-lookup"><span data-stu-id="e53c8-123">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="e53c8-124">`UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="e53c8-124">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="e53c8-125">Si le port dynamique est 1234, Kestrel écoute sur `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-125">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="e53c8-126">Cette configuration remplace les autres configurations URL fournies par :</span><span class="sxs-lookup"><span data-stu-id="e53c8-126">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="e53c8-127">API d’écoute Kestrel</span><span class="sxs-lookup"><span data-stu-id="e53c8-127">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="e53c8-128">[Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="e53c8-128">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="e53c8-129">L’utilisation du module évite les appels à `UseUrls` ou à l’API `Listen` de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e53c8-129">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="e53c8-130">Si `UseUrls` ou `Listen` est appelé, Kestrel écoute sur les ports spécifiés uniquement lors de l’exécution de l’application sans IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-130">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="e53c8-131">Pour plus d’informations sur les modèles d’hébergement in-process et out-of-process, consultez [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) et [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-131">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="e53c8-132">`CreateDefaultBuilder` configure [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et active l’intégration d’IIS en configurant le chemin et le port de base pour le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-132">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="e53c8-133">Le module ASP.NET Core génère un port dynamique à assigner au processus backend.</span><span class="sxs-lookup"><span data-stu-id="e53c8-133">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="e53c8-134">`CreateDefaultBuilder` appelle la méthode [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration).</span><span class="sxs-lookup"><span data-stu-id="e53c8-134">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="e53c8-135">`UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="e53c8-135">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="e53c8-136">Si le port dynamique est 1234, Kestrel écoute sur `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-136">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="e53c8-137">Cette configuration remplace les autres configurations URL fournies par :</span><span class="sxs-lookup"><span data-stu-id="e53c8-137">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="e53c8-138">API d’écoute Kestrel</span><span class="sxs-lookup"><span data-stu-id="e53c8-138">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="e53c8-139">[Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="e53c8-139">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="e53c8-140">L’utilisation du module évite les appels à `UseUrls` ou à l’API `Listen` de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e53c8-140">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="e53c8-141">Si `UseUrls` ou `Listen` est appelé, Kestrel écoute sur le port spécifié uniquement lors de l’exécution de l’application sans IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-141">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e53c8-142">`CreateDefaultBuilder` configure [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et active l’intégration d’IIS en configurant le chemin et le port de base pour le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-142">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="e53c8-143">Le module ASP.NET Core génère un port dynamique à assigner au processus backend.</span><span class="sxs-lookup"><span data-stu-id="e53c8-143">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="e53c8-144">`CreateDefaultBuilder` appelle la méthode [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration).</span><span class="sxs-lookup"><span data-stu-id="e53c8-144">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="e53c8-145">`UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="e53c8-145">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="e53c8-146">Si le port dynamique est 1234, Kestrel écoute sur `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-146">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="e53c8-147">Cette configuration remplace les autres configurations URL fournies par :</span><span class="sxs-lookup"><span data-stu-id="e53c8-147">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="e53c8-148">API d’écoute Kestrel</span><span class="sxs-lookup"><span data-stu-id="e53c8-148">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="e53c8-149">[Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="e53c8-149">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="e53c8-150">L’utilisation du module évite les appels à `UseUrls` ou à l’API `Listen` de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e53c8-150">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="e53c8-151">Si `UseUrls` ou `Listen` est appelé, Kestrel écoute sur le port spécifié uniquement lors de l’exécution de l’application sans IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-151">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e53c8-152">Incluez une dépendance envers le package [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) dans les dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-152">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="e53c8-153">Utilisez l’intergiciel (middleware) d’intégration IIS en ajoutant la méthode d’extension [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) à [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) :</span><span class="sxs-lookup"><span data-stu-id="e53c8-153">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="e53c8-154">[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) et [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) sont tous les deux requis.</span><span class="sxs-lookup"><span data-stu-id="e53c8-154">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="e53c8-155">Le code qui appelle `UseIISIntegration` n’affecte pas la portabilité du code.</span><span class="sxs-lookup"><span data-stu-id="e53c8-155">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="e53c8-156">Si l’application n’est pas exécutée derrière IIS (par exemple si l’application est exécutée directement sur Kestrel), `UseIISIntegration` n’effectue pas d’opérations.</span><span class="sxs-lookup"><span data-stu-id="e53c8-156">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="e53c8-157">Le module ASP.NET Core génère un port dynamique à assigner au processus backend.</span><span class="sxs-lookup"><span data-stu-id="e53c8-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="e53c8-158">`UseIISIntegration` configure Kestrel pour écouter sur le port dynamique à l’adresse IP localhost (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="e53c8-158">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="e53c8-159">Si le port dynamique est 1234, Kestrel écoute sur `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-159">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="e53c8-160">Cette configuration remplace les autres configurations URL fournies par :</span><span class="sxs-lookup"><span data-stu-id="e53c8-160">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="e53c8-161">[Configuration](xref:fundamentals/configuration/index) (ou [options --url de ligne de commande](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="e53c8-161">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="e53c8-162">L’utilisation du module rend inutile tout appel à `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-162">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="e53c8-163">Si `UseUrls` est appelé, Kestrel écoute sur le port spécifié uniquement lors de l’exécution de l’application sans IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-163">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="e53c8-164">Si `UseUrls` est appelé dans une application ASP.NET Core 1.0, appelez-le **avant** d’appeler `UseIISIntegration` pour que le port configuré par le module ne soit pas remplacé.</span><span class="sxs-lookup"><span data-stu-id="e53c8-164">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="e53c8-165">Cet ordre d’appel est inutile avec ASP.NET Core 1.1, car le paramètre du module remplace `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-165">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="e53c8-166">Pour plus d’informations sur l’hébergement, consultez [Héberger dans ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="e53c8-166">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="e53c8-167">Options IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-167">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e53c8-168">**Modèle d’hébergement in-process**</span><span class="sxs-lookup"><span data-stu-id="e53c8-168">**In-process hosting model**</span></span>

<span data-ttu-id="e53c8-169">Pour configurer des options IIS Server, ajoutez une configuration de services pour [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) dans [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="e53c8-169">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="e53c8-170">L’exemple suivant désactive AutomaticAuthentication :</span><span class="sxs-lookup"><span data-stu-id="e53c8-170">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="e53c8-171">Option</span><span class="sxs-lookup"><span data-stu-id="e53c8-171">Option</span></span>                         | <span data-ttu-id="e53c8-172">Par défaut</span><span class="sxs-lookup"><span data-stu-id="e53c8-172">Default</span></span> | <span data-ttu-id="e53c8-173">Paramètre</span><span class="sxs-lookup"><span data-stu-id="e53c8-173">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="e53c8-174">Si `true`, IIS Server définit le `HttpContext.User` authentifié par [Authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-174">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="e53c8-175">Si `false`, le serveur fournit uniquement une identité pour `HttpContext.User` et répond aux questions explicitement posées par `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-175">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="e53c8-176">L’authentification Windows doit être activée dans IIS pour que `AutomaticAuthentication` fonctionne.</span><span class="sxs-lookup"><span data-stu-id="e53c8-176">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="e53c8-177">Pour plus d’informations, consultez [Authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-177">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="e53c8-178">Définit le nom d’affichage montré aux utilisateurs sur les pages de connexion.</span><span class="sxs-lookup"><span data-stu-id="e53c8-178">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="e53c8-179">**Modèle d’hébergement out-of-process**</span><span class="sxs-lookup"><span data-stu-id="e53c8-179">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="e53c8-180">Pour configurer des options IIS, ajoutez une configuration de services pour [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) dans [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="e53c8-180">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="e53c8-181">L’exemple suivant empêche l’application d’être renseignée `HttpContext.Connection.ClientCertificate` :</span><span class="sxs-lookup"><span data-stu-id="e53c8-181">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="e53c8-182">Option</span><span class="sxs-lookup"><span data-stu-id="e53c8-182">Option</span></span>                         | <span data-ttu-id="e53c8-183">Par défaut</span><span class="sxs-lookup"><span data-stu-id="e53c8-183">Default</span></span> | <span data-ttu-id="e53c8-184">Paramètre</span><span class="sxs-lookup"><span data-stu-id="e53c8-184">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="e53c8-185">Si `true`, l’Intergiciel (Middleware) d’intégration IIS définit le `HttpContext.User` authentifié par [Authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-185">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="e53c8-186">Si `false`, l’intergiciel (middleware) fournit uniquement une identité pour `HttpContext.User` et répond aux questions explicitement posées par `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-186">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="e53c8-187">L’authentification Windows doit être activée dans IIS pour que `AutomaticAuthentication` fonctionne.</span><span class="sxs-lookup"><span data-stu-id="e53c8-187">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="e53c8-188">Pour plus d'informations, consultez la rubrique [Authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-188">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="e53c8-189">Définit le nom d’affichage montré aux utilisateurs sur les pages de connexion.</span><span class="sxs-lookup"><span data-stu-id="e53c8-189">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="e53c8-190">Si la valeur est `true` et que l’en-tête de requête `MS-ASPNETCORE-CLIENTCERT` est présent, `HttpContext.Connection.ClientCertificate` est renseigné.</span><span class="sxs-lookup"><span data-stu-id="e53c8-190">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e53c8-191">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="e53c8-191">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e53c8-192">L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête.</span><span class="sxs-lookup"><span data-stu-id="e53c8-192">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="e53c8-193">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="e53c8-193">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="e53c8-194">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="e53c8-194">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="e53c8-195">fichier web.config</span><span class="sxs-lookup"><span data-stu-id="e53c8-195">web.config file</span></span>

<span data-ttu-id="e53c8-196">Le fichier *web.config* configure le [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-196">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e53c8-197">La création, la transformation et la publication du fichier *web.config* sont gérées par une cible MSBuild (`_TransformWebConfig`) quand le projet est publié.</span><span class="sxs-lookup"><span data-stu-id="e53c8-197">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="e53c8-198">Cette cible est présente dans les cibles du SDK web (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="e53c8-198">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="e53c8-199">Le SDK est défini en haut du fichier projet :</span><span class="sxs-lookup"><span data-stu-id="e53c8-199">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="e53c8-200">Si le projet ne contient pas de fichier *web.config*, le fichier est créé avec le *processPath* et les *arguments* corrects pour configurer le [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), puis il est déplacé vers la [sortie publiée](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="e53c8-200">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="e53c8-201">Si un fichier *web.config* se trouve dans le projet, il est transformé avec le *processPath* et les *arguments* corrects pour configurer le Module ASP.NET Core, puis il est déplacé vers la sortie publiée.</span><span class="sxs-lookup"><span data-stu-id="e53c8-201">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="e53c8-202">La transformation ne modifie pas les paramètres de configuration IIS dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="e53c8-202">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="e53c8-203">Le fichier *web.config* peut fournir des paramètres de configuration IIS supplémentaires qui contrôlent les modules IIS actifs.</span><span class="sxs-lookup"><span data-stu-id="e53c8-203">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="e53c8-204">Pour plus d’informations sur les modules IIS capables de traiter les requêtes avec des applications ASP.NET Core, consultez la rubrique [Modules IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="e53c8-204">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="e53c8-205">Pour empêcher le Kit de développement logiciel (SDK) Web de transformer le fichier *web.config*, ajoutez la propriété **\<IsTransformWebConfigDisabled>** au fichier projet :</span><span class="sxs-lookup"><span data-stu-id="e53c8-205">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="e53c8-206">Lorsque vous désactivez le Kit de développement logiciel (SDK) Web en transformant le fichier, le *processPath* et les *arguments* doivent être définis manuellement par le développeur.</span><span class="sxs-lookup"><span data-stu-id="e53c8-206">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="e53c8-207">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-207">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="e53c8-208">emplacement du fichier web.config</span><span class="sxs-lookup"><span data-stu-id="e53c8-208">web.config file location</span></span>

<span data-ttu-id="e53c8-209">Pour configurer le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) correctement, le fichier *web.config* doit être présent dans le chemin de racine de contenu (généralement le chemin de base de l’application) de l’application déployée.</span><span class="sxs-lookup"><span data-stu-id="e53c8-209">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="e53c8-210">Il s’agit du même emplacement que le chemin physique du site Web fourni à IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-210">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="e53c8-211">Le fichier *web.config* est nécessaire à la racine de l’application pour permettre la publication de plusieurs applications à l’aide de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="e53c8-211">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="e53c8-212">Il existe des fichiers sensibles sur le chemin physique de l’application, notamment *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (commentaires de documentation XML) et *\<assembly>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="e53c8-212">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="e53c8-213">Lorsque le fichier *web.config* est présent et que le site démarre normalement, IIS ne traite pas ces fichiers sensibles s’ils sont requis.</span><span class="sxs-lookup"><span data-stu-id="e53c8-213">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="e53c8-214">Si le fichier *web.config* est absent, nommé de manière incorrecte ou s’il est incapable de configurer le site pour un démarrage normal, IIS peut traiter des fichiers sensibles publiquement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-214">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="e53c8-215">**Le fichier *web.config* doit être présent dans le déploiement en permanence, correctement nommé et capable de configurer le site pour un démarrage normal. Ne retirez jamais le fichier *web.config* d’un déploiement de production.**</span><span class="sxs-lookup"><span data-stu-id="e53c8-215">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="e53c8-216">Configuration d’IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-216">IIS configuration</span></span>

<span data-ttu-id="e53c8-217">**Systèmes d’exploitation Windows Server**</span><span class="sxs-lookup"><span data-stu-id="e53c8-217">**Windows Server operating systems**</span></span>

<span data-ttu-id="e53c8-218">Activez le rôle serveur **Serveur Web (IIS)** et établissez des services de rôle.</span><span class="sxs-lookup"><span data-stu-id="e53c8-218">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="e53c8-219">Utilisez l’Assistant **Ajouter des rôles et des fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-219">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="e53c8-220">À l’étape **Rôles de serveurs**, cochez la case **Serveur Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-220">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Le rôle Serveur Web IIS est sélectionné à l’étape Sélectionner des rôles de serveurs.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="e53c8-222">Après l’étape **Fonctionnalités**, l’étape **Services de rôle** se charge pour le serveur Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="e53c8-222">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="e53c8-223">Sélectionnez les services de rôle IIS souhaités ou acceptez les services de rôle par défaut fournis.</span><span class="sxs-lookup"><span data-stu-id="e53c8-223">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Les services de rôle par défaut sont sélectionnés à l’étape Sélectionner des services de rôle.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="e53c8-225">**Authentification Windows (facultatif)**</span><span class="sxs-lookup"><span data-stu-id="e53c8-225">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="e53c8-226">Pour activer l’authentification Windows, développez les nœuds suivants : **Serveur Web**  > **Sécurité**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-226">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="e53c8-227">Sélectionnez la fonctionnalité **Authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-227">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="e53c8-228">Pour plus d’informations, consultez [Authentification Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) et [Configurer l’authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-228">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="e53c8-229">**WebSockets (facultatif)**</span><span class="sxs-lookup"><span data-stu-id="e53c8-229">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="e53c8-230">WebSockets est pris en charge avec ASP.NET Core 1.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e53c8-230">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="e53c8-231">Pour activer les WebSockets, développez les nœuds suivants : **Serveur Web** > **Développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-231">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="e53c8-232">Sélectionnez la fonctionnalité **Protocole WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-232">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="e53c8-233">Pour plus d’informations, consultez [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="e53c8-233">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="e53c8-234">Validez l’étape de **Confirmation** pour installer les services et le rôle de serveur web.</span><span class="sxs-lookup"><span data-stu-id="e53c8-234">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="e53c8-235">Un redémarrage du serveur/d’IIS n’est pas nécessaire après l’installation du rôle **Serveur Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-235">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="e53c8-236">**Systèmes d’exploitation Windows Desktop**</span><span class="sxs-lookup"><span data-stu-id="e53c8-236">**Windows desktop operating systems**</span></span>

<span data-ttu-id="e53c8-237">Activez la **Console de gestion IIS** et les **Services World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-237">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="e53c8-238">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="e53c8-238">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="e53c8-239">Ouvrez le nœud **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-239">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="e53c8-240">Ouvrez le nœud **Outils de gestion Web**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-240">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="e53c8-241">Cochez la case **Console de gestion IIS**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-241">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="e53c8-242">Cochez la case **Services World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-242">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="e53c8-243">Acceptez les fonctionnalités par défaut pour **Services World Wide Web** ou personnalisez les fonctionnalités d’IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-243">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="e53c8-244">**Authentification Windows (facultatif)**</span><span class="sxs-lookup"><span data-stu-id="e53c8-244">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="e53c8-245">Pour activer l’authentification Windows, développez les nœuds suivants : **Services World Wide Web** > **Sécurité**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-245">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="e53c8-246">Sélectionnez la fonctionnalité **Authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-246">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="e53c8-247">Pour plus d’informations, consultez [Authentification Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) et [Configurer l’authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-247">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="e53c8-248">**WebSockets (facultatif)**</span><span class="sxs-lookup"><span data-stu-id="e53c8-248">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="e53c8-249">WebSockets est pris en charge avec ASP.NET Core 1.1 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e53c8-249">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="e53c8-250">Pour activer les WebSockets, développez les nœuds suivants : **Services World Wide Web** > **Fonctionnalités de développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-250">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="e53c8-251">Sélectionnez la fonctionnalité **Protocole WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-251">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="e53c8-252">Pour plus d’informations, consultez [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="e53c8-252">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="e53c8-253">Si l’installation d’IIS requiert un redémarrage, redémarrez le système.</span><span class="sxs-lookup"><span data-stu-id="e53c8-253">If the IIS installation requires a restart, restart the system.</span></span>

![Console de gestion IIS et Services World Wide Web sont sélectionnés dans Fonctionnalités de Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="e53c8-255">Installer le bundle d’hébergement .NET Core</span><span class="sxs-lookup"><span data-stu-id="e53c8-255">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="e53c8-256">Installez le *bundle d’hébergement .NET Core* sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="e53c8-256">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="e53c8-257">Le bundle installe le Runtime .NET Core, la bibliothèque .NET Core et le [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-257">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e53c8-258">Le module permet aux applications ASP.NET Core de s’exécuter derrière IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-258">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="e53c8-259">Si le système n’a pas de connexion Internet, obtenez et installez [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) avant d’installer le bundle d’hébergement .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-259">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e53c8-260">Si le bundle d’hébergement est installé avant IIS, l’installation du bundle doit être réparée.</span><span class="sxs-lookup"><span data-stu-id="e53c8-260">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="e53c8-261">Après avoir installé IIS, réexécutez le programme d’installation du bundle d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-261">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="e53c8-262">Téléchargement direct (version actuelle)</span><span class="sxs-lookup"><span data-stu-id="e53c8-262">Direct download (current version)</span></span>

<span data-ttu-id="e53c8-263">Téléchargez le programme d’installation à l’aide du lien suivant :</span><span class="sxs-lookup"><span data-stu-id="e53c8-263">Download the installer using the following link:</span></span>

[<span data-ttu-id="e53c8-264">Programme d’installation du bundle d’hébergement .NET Core actuel (téléchargement direct)</span><span class="sxs-lookup"><span data-stu-id="e53c8-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="e53c8-265">Versions antérieures du programme d’installation</span><span class="sxs-lookup"><span data-stu-id="e53c8-265">Earlier versions of the installer</span></span>

<span data-ttu-id="e53c8-266">Pour obtenir une version antérieure du programme d’installation :</span><span class="sxs-lookup"><span data-stu-id="e53c8-266">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="e53c8-267">Accédez aux [archives des téléchargements .NET](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="e53c8-267">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="e53c8-268">Sous **.NET Core**, sélectionnez la version de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-268">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="e53c8-269">Dans la colonne **Run apps - Runtime**, recherchez la ligne de la version du runtime .NET Core souhaitée.</span><span class="sxs-lookup"><span data-stu-id="e53c8-269">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="e53c8-270">Téléchargez le programme d’installation à l’aide du lien **Runtime & Hosting Bundle**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-270">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="e53c8-271">Certains programmes d’installation contiennent des versions qui sont arrivées à leur fin de vie (EOL) et qui ne sont plus prises en charge par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e53c8-271">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="e53c8-272">Pour plus d’informations, consultez la [politique de support](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="e53c8-272">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="e53c8-273">Installer le bundle d’hébergement</span><span class="sxs-lookup"><span data-stu-id="e53c8-273">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="e53c8-274">Exécutez le programme d’installation sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e53c8-274">Run the installer on the server.</span></span> <span data-ttu-id="e53c8-275">Les commutateurs suivants sont disponibles lorsque vous exécutez le programme d’installation à partir d’une invite de commandes administrateur :</span><span class="sxs-lookup"><span data-stu-id="e53c8-275">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="e53c8-276">`OPT_NO_ANCM=1` &ndash; Sauter l’installation du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-276">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="e53c8-277">`OPT_NO_RUNTIME=1` &ndash;Sauter l'installation du runtime .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-277">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="e53c8-278">`OPT_NO_SHAREDFX=1` &ndash; Sauter l'installation de l’infrastructure partagée ASP.NET (runtime ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="e53c8-278">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="e53c8-279">`OPT_NO_X86=1` &ndash; Ignorer l’installation des runtimes x86.</span><span class="sxs-lookup"><span data-stu-id="e53c8-279">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="e53c8-280">Utilisez ce commutateur lorsque vous savez que vous n’hébergerez pas d’applications 32 bits.</span><span class="sxs-lookup"><span data-stu-id="e53c8-280">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="e53c8-281">Si vous n’excluez pas d’avoir à héberger des applications 32 bits et 64 bits dans le futur, n'utilisez pas ce commutateur et installez les deux runtimes.</span><span class="sxs-lookup"><span data-stu-id="e53c8-281">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="e53c8-282">Redémarrez le système ou exécutez **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="e53c8-282">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="e53c8-283">Le redémarrage d’IIS récupère une modification apportée au CHEMIN D’ACCÈS du système, qui est une variable d’environnement, par le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="e53c8-283">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="e53c8-284">Si le programme d'installation du pack d'hébergement Windows détecte qu’IIS requiert une réinitialisation pour terminer l’installation, le programme d’installation réinitialise IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-284">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="e53c8-285">Si le programme d’installation déclenche une réinitialisation d’IIS, tous les pools d’applications et sites Web IIS sont redémarrés.</span><span class="sxs-lookup"><span data-stu-id="e53c8-285">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="e53c8-286">Pour plus d’informations sur la configuration partagée IIS, consultez [Module ASP.NET Core avec configuration partagée des services Internet (IIS)](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="e53c8-286">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="e53c8-287">Installer Web Deploy lors de la publication avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e53c8-287">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="e53c8-288">Quand vous déployez des applications sur un serveur avec [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), installez la dernière version de Web Deploy sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e53c8-288">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="e53c8-289">Pour installer Web Deploy, utilisez [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenez un programme d’installation directement à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="e53c8-289">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="e53c8-290">La méthode recommandée est d’utiliser WebPI.</span><span class="sxs-lookup"><span data-stu-id="e53c8-290">The preferred method is to use WebPI.</span></span> <span data-ttu-id="e53c8-291">WebPI offre une installation autonome et une configuration pour les fournisseurs d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-291">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="e53c8-292">Créer le site IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-292">Create the IIS site</span></span>

1. <span data-ttu-id="e53c8-293">Sur le système d’hébergement, créez un dossier pour contenir les fichiers et dossiers publiés de l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-293">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="e53c8-294">Une disposition du déploiement de l’application est décrite dans la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="e53c8-294">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="e53c8-295">Dans le nouveau dossier, créez un dossier *journaux* où stocker les journaux stdout du Module ASP.NET Core quand la journalisation stdout est activée.</span><span class="sxs-lookup"><span data-stu-id="e53c8-295">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="e53c8-296">Si l’application est déployée avec un dossier *logs* dans la charge utile, ignorez cette étape.</span><span class="sxs-lookup"><span data-stu-id="e53c8-296">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="e53c8-297">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *journaux* automatiquement lorsque le projet est généré localement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="e53c8-297">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="e53c8-298">Utilisez uniquement le journal stdout pour résoudre les échecs de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-298">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="e53c8-299">N’utilisez jamais la journalisation stdout pour la journalisation de l’application de routine.</span><span class="sxs-lookup"><span data-stu-id="e53c8-299">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="e53c8-300">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="e53c8-300">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="e53c8-301">Le pool d’applications doit avoir un accès en écriture à l’emplacement auquel les journaux sont écrits.</span><span class="sxs-lookup"><span data-stu-id="e53c8-301">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="e53c8-302">Tous les dossiers sur le chemin de l’emplacement des journaux doivent exister.</span><span class="sxs-lookup"><span data-stu-id="e53c8-302">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="e53c8-303">Pour plus d’informations sur le journal stdout, consultez [Création et redirection des journaux](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="e53c8-303">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="e53c8-304">Pour plus d’informations sur la journalisation dans une application ASP.NET Core, consultez la rubrique [Journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e53c8-304">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="e53c8-305">Dans **Gestionnaire IIS**, ouvrez le nœud du serveur dans le panneau **Connexions**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-305">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="e53c8-306">Cliquez avec le bouton de droite sur le dossier **Sites**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-306">Right-click the **Sites** folder.</span></span> <span data-ttu-id="e53c8-307">Sélectionnez **Ajouter un site Web** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="e53c8-307">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="e53c8-308">Spécifiez le **Nom du site** et définissez le **Chemin physique** sur le dossier de déploiement de l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-308">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="e53c8-309">Spécifiez la configuration **Liaison** et créez le site Web en sélectionnant **OK** :</span><span class="sxs-lookup"><span data-stu-id="e53c8-309">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Spécifiez le nom du site, le chemin physique et le nom d’hôte à l’étape Ajouter un site Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="e53c8-311">Les liaisons génériques de niveau supérieur (`http://*:80/` et `http://+:80`) ne doivent **pas** être utilisées.</span><span class="sxs-lookup"><span data-stu-id="e53c8-311">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="e53c8-312">Les liaisons génériques de niveau supérieur peuvent exposer votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="e53c8-312">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="e53c8-313">Cela s’applique aux caractères génériques forts et faibles.</span><span class="sxs-lookup"><span data-stu-id="e53c8-313">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="e53c8-314">Utilisez des noms d’hôte explicites plutôt que des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="e53c8-314">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="e53c8-315">Une liaison générique de sous-domaine (par exemple, `*.mysub.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="e53c8-315">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="e53c8-316">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e53c8-316">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="e53c8-317">Sous le nœud du serveur, sélectionnez **Pools d’applications**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-317">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="e53c8-318">Cliquez avec le bouton de droite sur le pool d’applications du site et sélectionnez **Paramètres de base** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="e53c8-318">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="e53c8-319">Dans la fenêtre **Modifier le pool d’applications**, définissez la **version .NET CLR** sur **Aucun code managé** :</span><span class="sxs-lookup"><span data-stu-id="e53c8-319">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Définissez Aucun code managé pour la version CLR .NET.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="e53c8-321">ASP.NET Core s’exécute dans un processus séparé et gère l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e53c8-321">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="e53c8-322">ASP.NET Core ne repose pas sur le chargement du CLR de bureau.</span><span class="sxs-lookup"><span data-stu-id="e53c8-322">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="e53c8-323">La configuration de la **version CLR .NET** sur **Aucun code managé** est facultative.</span><span class="sxs-lookup"><span data-stu-id="e53c8-323">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="e53c8-324">Vérifiez que l’identité de modèle de processus dispose des autorisations appropriées.</span><span class="sxs-lookup"><span data-stu-id="e53c8-324">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="e53c8-325">Si l’identité par défaut du pool d’applications (**Modèle de processus** > **Identité**) **ApplicationPoolIdentity** est remplacée par une autre identité, vérifiez que la nouvelle identité dispose des autorisations nécessaires pour accéder au dossier de l’application, à la base de données et aux autres ressources nécessaires.</span><span class="sxs-lookup"><span data-stu-id="e53c8-325">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="e53c8-326">Par exemple, le pool d’applications nécessite l’accès en lecture et en écriture aux dossiers dans lesquels l’application lit et écrit des fichiers.</span><span class="sxs-lookup"><span data-stu-id="e53c8-326">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="e53c8-327">**Configuration de l’authentification Windows (facultatif)**</span><span class="sxs-lookup"><span data-stu-id="e53c8-327">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="e53c8-328">Pour plus d'informations, consultez la rubrique [Configurer l’authentification Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="e53c8-328">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="e53c8-329">Déployer l’application</span><span class="sxs-lookup"><span data-stu-id="e53c8-329">Deploy the app</span></span>

<span data-ttu-id="e53c8-330">Déployez l’application vers le dossier créé sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-330">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="e53c8-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) est le mécanisme recommandé pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="e53c8-332">Web Deploy avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e53c8-332">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="e53c8-333">Pour découvrir comment créer un profil de publication pour une utilisation avec Web Deploy, consultez la rubrique [Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="e53c8-333">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="e53c8-334">Si le fournisseur d’hébergement fournit un profil de publication ou prend en charge sa création, téléchargez son profil et importez-le à l’aide de la boîte de dialogue **Publier** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e53c8-334">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Boîte de dialogue Publier](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="e53c8-336">Web Deploy en dehors de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e53c8-336">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="e53c8-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) peut également être utilisé en dehors de Visual Studio à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="e53c8-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="e53c8-338">Pour plus d’informations, consultez [Web Deployment Tool (Outil de déploiement Web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="e53c8-338">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="e53c8-339">Alternatives à Web Deploy</span><span class="sxs-lookup"><span data-stu-id="e53c8-339">Alternatives to Web Deploy</span></span>

<span data-ttu-id="e53c8-340">Utilisez la méthode de votre choix, telle que la copie manuelle, Xcopy, Robocopy ou PowerShell, pour déplacer l’application vers le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-340">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="e53c8-341">Pour plus d’informations sur le déploiement d’ASP.NET Core sur IIS, consultez la section [Déploiement de ressources pour les administrateurs IIS](#deployment-resources-for-iis-administrators).</span><span class="sxs-lookup"><span data-stu-id="e53c8-341">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="e53c8-342">Parcourir le site web</span><span class="sxs-lookup"><span data-stu-id="e53c8-342">Browse the website</span></span>

![Le navigateur Microsoft Edge a chargé la page de démarrage IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="e53c8-344">Fichiers de déploiement verrouillés</span><span class="sxs-lookup"><span data-stu-id="e53c8-344">Locked deployment files</span></span>

<span data-ttu-id="e53c8-345">Les fichiers dans le dossier de déploiement sont verrouillés quand l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e53c8-345">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="e53c8-346">Les fichiers verrouillés ne peuvent pas être remplacés au cours du déploiement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-346">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="e53c8-347">Pour libérer des fichiers verrouillés dans un déploiement, arrêtez le pool d’applications à l’aide **d’une** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e53c8-347">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="e53c8-348">Utilisez Web Deploy et référencez `Microsoft.NET.Sdk.Web` dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="e53c8-348">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="e53c8-349">Un fichier *app_offline.htm* est placé à la racine du répertoire de l’application web.</span><span class="sxs-lookup"><span data-stu-id="e53c8-349">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="e53c8-350">Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="e53c8-350">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="e53c8-351">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="e53c8-351">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="e53c8-352">Arrêtez manuellement le pool d’applications dans le Gestionnaire IIS sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e53c8-352">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="e53c8-353">Utilisez PowerShell pour supprimer *app_offline.html* (nécessite PowerShell 5 ou ultérieur) :</span><span class="sxs-lookup"><span data-stu-id="e53c8-353">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="e53c8-354">Protection des données</span><span class="sxs-lookup"><span data-stu-id="e53c8-354">Data protection</span></span>

<span data-ttu-id="e53c8-355">La [pile de protection des données ASP.NET Core](xref:security/data-protection/introduction) est utilisée par plusieurs [intergiciels (middlewares)](xref:fundamentals/middleware/index) ASP.NET Core, y compris l’intergiciel utilisé dans l’authentification.</span><span class="sxs-lookup"><span data-stu-id="e53c8-355">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="e53c8-356">Même si les API de protection des données ne sont pas appelées par le code de l’utilisateur, la protection des données doit être configurée avec un script de déploiement ou dans un code utilisateur pour créer un [magasin de clés](xref:security/data-protection/implementation/key-management) de chiffrement persistantes.</span><span class="sxs-lookup"><span data-stu-id="e53c8-356">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="e53c8-357">Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-357">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="e53c8-358">Si le Key Ring est stocké en mémoire, au redémarrage de l’application :</span><span class="sxs-lookup"><span data-stu-id="e53c8-358">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="e53c8-359">tous les jetons d’authentification basés sur des cookies sont invalidés</span><span class="sxs-lookup"><span data-stu-id="e53c8-359">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="e53c8-360">Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande.</span><span class="sxs-lookup"><span data-stu-id="e53c8-360">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="e53c8-361">toutes les données protégées par le Key Ring ne peuvent plus être déchiffrées.</span><span class="sxs-lookup"><span data-stu-id="e53c8-361">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="e53c8-362">Ceci peut inclure des [jetons CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) et des [cookies TempData ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="e53c8-362">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="e53c8-363">Pour configurer la protection des données sous IIS afin de rendre persistante le Key Ring, adoptez **l’une** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="e53c8-363">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="e53c8-364">**Créer des clés de Registre de la protection des données**</span><span class="sxs-lookup"><span data-stu-id="e53c8-364">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="e53c8-365">Les clés de la protection des données utilisées par les applications ASP.NET Core sont stockées dans le registre externe aux applications.</span><span class="sxs-lookup"><span data-stu-id="e53c8-365">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="e53c8-366">Afin de conserver les clés pour une application donnée, créez des clés de Registre pour le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="e53c8-366">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="e53c8-367">Pour les installations IIS autonomes hors d’une batterie de serveurs, le [script PowerShell Provision-AutoGenKeys.ps1 de protection des données](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) peut être utilisé pour chaque pool d’applications utilisé avec une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-367">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="e53c8-368">Ce script crée une clé de Registre dans le Registre HKLM, accessible uniquement pour le compte du processus Worker du pool d’applications de l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-368">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="e53c8-369">Les clés sont chiffrées au repos à l’aide de DPAPI avec une clé à l’échelle de la machine.</span><span class="sxs-lookup"><span data-stu-id="e53c8-369">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="e53c8-370">Dans les scénarios de batterie de serveurs Web, vous pouvez configurer une application afin qu’elle utilise un chemin UNC pour stocker son Key Ring de protection des données.</span><span class="sxs-lookup"><span data-stu-id="e53c8-370">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="e53c8-371">Par défaut, les clés de protection des données ne sont pas chiffrées.</span><span class="sxs-lookup"><span data-stu-id="e53c8-371">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="e53c8-372">Vérifiez que les autorisations de fichiers pour le partage réseau sont limitées au compte Windows sous lequel s’exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-372">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="e53c8-373">Un certificat X509 peut être utilisé pour protéger les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="e53c8-373">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="e53c8-374">Mettez en œuvre un mécanisme permettant aux utilisateurs de charger des certificats : placez les certificats dans le magasin de certificats approuvés de l’utilisateur et vérifiez qu’ils sont disponibles sur toutes les machines où s’exécute l’application de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e53c8-374">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="e53c8-375">Pour plus d’informations, consultez [Configurer la protection des données ASP.NET Core](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="e53c8-375">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="e53c8-376">**Configurer le pool d’applications IIS pour charger le profil utilisateur**</span><span class="sxs-lookup"><span data-stu-id="e53c8-376">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="e53c8-377">Ce paramètre se trouve dans la section **Modèle de processus** sous les **Paramètres avancés** du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="e53c8-377">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="e53c8-378">Définissez Charger le profil utilisateur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-378">Set Load User Profile to `True`.</span></span> <span data-ttu-id="e53c8-379">Cela permet de stocker les clés sous le répertoire du profil utilisateur et de les protéger à l’aide de DPAPI avec une clé propre au compte d’utilisateur utilisé par le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="e53c8-379">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="e53c8-380">**Utiliser le système de fichiers comme magasin de Key Ring**</span><span class="sxs-lookup"><span data-stu-id="e53c8-380">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="e53c8-381">Ajustez le code d’application pour [utiliser le système de fichiers comme magasin de porte-clés](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="e53c8-381">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="e53c8-382">Utilisez un certificat X509 pour protéger le Key Ring et vérifiez qu’il s’agit d’un certificat approuvé.</span><span class="sxs-lookup"><span data-stu-id="e53c8-382">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="e53c8-383">S’il s’agit d’un certificat auto-signé, placez-le dans le magasin Racine approuvée.</span><span class="sxs-lookup"><span data-stu-id="e53c8-383">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="e53c8-384">Quand vous utilisez IIS dans une batterie de serveurs web :</span><span class="sxs-lookup"><span data-stu-id="e53c8-384">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="e53c8-385">Utilisez un partage de fichiers accessible par tous les ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="e53c8-385">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="e53c8-386">Déployez un certificat X509 sur chaque ordinateur.</span><span class="sxs-lookup"><span data-stu-id="e53c8-386">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="e53c8-387">Configurez la [protection des données dans le code](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="e53c8-387">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="e53c8-388">**Définir une stratégie au niveau de l’ordinateur pour la protection des données**</span><span class="sxs-lookup"><span data-stu-id="e53c8-388">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="e53c8-389">Le système de protection des données offre une prise en charge limitée de la définition d’une [stratégie au niveau de l’ordinateur](xref:security/data-protection/configuration/machine-wide-policy) par défaut pour toutes les applications qui utilisent les API de protection des données.</span><span class="sxs-lookup"><span data-stu-id="e53c8-389">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="e53c8-390">Pour plus d'informations, consultez <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="e53c8-390">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="e53c8-391">Configuration de la sous-application</span><span class="sxs-lookup"><span data-stu-id="e53c8-391">Sub-application configuration</span></span>

<span data-ttu-id="e53c8-392">Les sous-applications ajoutées sous l’application racine ne doivent pas inclure le module ASP.NET Core en tant que gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="e53c8-392">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="e53c8-393">Si le module est ajouté en guise de gestionnaire dans le fichier *web.config* d’une sous-application, une *Erreur de serveur interne 500.19* faisant référence au fichier config défaillant est reçue lors de la tentative de navigation dans la sous-application.</span><span class="sxs-lookup"><span data-stu-id="e53c8-393">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="e53c8-394">L’exemple suivant présente un fichier *web.config* publié pour une sous-application ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="e53c8-394">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
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
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="e53c8-395">Si vous hébergez une sous-application non-ASP.NET Core sous une application ASP.NET Core, supprimez explicitement le gestionnaire hérité dans le fichier *web.config* de la sous-application :</span><span class="sxs-lookup"><span data-stu-id="e53c8-395">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="e53c8-396">Pour plus d’informations sur la configuration du module ASP.NET Core, consultez la rubrique [Introduction au module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) et les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e53c8-396">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="e53c8-397">Configuration d’IIS avec web.config</span><span class="sxs-lookup"><span data-stu-id="e53c8-397">Configuration of IIS with web.config</span></span>

<span data-ttu-id="e53c8-398">La configuration d’IIS est influencée par la section **\<system.webServer>** de *web.config* pour les fonctionnalités d’IIS qui s’appliquent à une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="e53c8-398">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="e53c8-399">Si IIS est configuré au niveau du serveur pour utiliser la compression dynamique, l’élément **\<urlCompression>** dans le fichier *web.config* de l’application peut le désactiver.</span><span class="sxs-lookup"><span data-stu-id="e53c8-399">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="e53c8-400">Pour plus d’informations, consultez [Informations de référence sur la configuration de \<system.webServer>](/iis/configuration/system.webServer/), [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et [Modules IIS avec ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="e53c8-400">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="e53c8-401">Pour définir des variables d’environnement pour des applications individuelles exécutées dans des pools de données isolés, (prises en charge pour IIS 10.0 ou version ultérieure), consultez la section *AppCmd.exe command* de la rubrique [Variables d’environnement \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dans la documentation de référence IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-401">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="e53c8-402">Sections de configuration de web.config</span><span class="sxs-lookup"><span data-stu-id="e53c8-402">Configuration sections of web.config</span></span>

<span data-ttu-id="e53c8-403">Les sections de configuration des applications ASP.NET 4.x dans *web.config* ne sont pas utilisées par les applications ASP.NET Core pour la configuration :</span><span class="sxs-lookup"><span data-stu-id="e53c8-403">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="e53c8-404">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="e53c8-404">**\<system.web>**</span></span>
* <span data-ttu-id="e53c8-405">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="e53c8-405">**\<appSettings>**</span></span>
* <span data-ttu-id="e53c8-406">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="e53c8-406">**\<connectionStrings>**</span></span>
* <span data-ttu-id="e53c8-407">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="e53c8-407">**\<location>**</span></span>

<span data-ttu-id="e53c8-408">Les applications ASP.NET Core sont configurées à l’aide d’autres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="e53c8-408">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="e53c8-409">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e53c8-409">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="e53c8-410">Pools d'applications</span><span class="sxs-lookup"><span data-stu-id="e53c8-410">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e53c8-411">L’isolation des pools d’applications est déterminée par le modèle d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="e53c8-411">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="e53c8-412">Hébergement in-process &ndash; Les applications doivent s’exécuter dans des pools d’applications distincts.</span><span class="sxs-lookup"><span data-stu-id="e53c8-412">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="e53c8-413">Hébergement out-of-process &ndash; Nous vous recommandons d’isoler les applications les unes des autres en exécutant chaque application dans son propre pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="e53c8-413">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="e53c8-414">La boîte de dialogue **Ajouter un site web** d’IIS applique un seul pool d’applications par application par défaut.</span><span class="sxs-lookup"><span data-stu-id="e53c8-414">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="e53c8-415">Quand un **Nom du site** est fourni, le texte est automatiquement transféré vers la zone de texte **Pool d’applications**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-415">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="e53c8-416">Un nouveau pool d’applications est créé avec le nom du site une fois qu’il est ajouté.</span><span class="sxs-lookup"><span data-stu-id="e53c8-416">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e53c8-417">Quand vous hébergez plusieurs sites Web sur un même serveur, nous vous recommandons d’isoler les applications les unes des autres en exécutant chaque application dans son propre pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="e53c8-417">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="e53c8-418">La boîte de dialogue **Ajouter un site Web** d’IIS applique cette configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="e53c8-418">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="e53c8-419">Quand un **Nom du site** est fourni, le texte est automatiquement transféré vers la zone de texte **Pool d’applications**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-419">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="e53c8-420">Un nouveau pool d’applications est créé avec le nom du site une fois qu’il est ajouté.</span><span class="sxs-lookup"><span data-stu-id="e53c8-420">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="e53c8-421">Identité du pool d’applications</span><span class="sxs-lookup"><span data-stu-id="e53c8-421">Application Pool Identity</span></span>

<span data-ttu-id="e53c8-422">Un compte d’identité du pool d’applications permet à une application de s’exécuter sous un compte unique sans qu’il soit nécessaire de créer et de gérer des domaines ou des comptes locaux.</span><span class="sxs-lookup"><span data-stu-id="e53c8-422">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="e53c8-423">Sur IIS 8.0 ou version ultérieure, le processus Worker d’administration IIS (WAS) crée un compte virtuel avec le nom du nouveau pool d’applications et exécute les processus Worker du pool d’applications sous ce compte par défaut.</span><span class="sxs-lookup"><span data-stu-id="e53c8-423">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="e53c8-424">Dans la console de gestion IIS, sous **Paramètres avancés** pour le pool d’applications, vérifiez que **l’Identité** est configurée pour utiliser **ApplicationPoolIdentity** :</span><span class="sxs-lookup"><span data-stu-id="e53c8-424">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Boîte de dialogue Paramètres avancés du pool applications](index/_static/apppool-identity.png)

<span data-ttu-id="e53c8-426">Le processus de gestion IIS crée un identificateur sécurisé avec le nom du pool d’applications dans le système de sécurité Windows.</span><span class="sxs-lookup"><span data-stu-id="e53c8-426">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="e53c8-427">Les ressources peuvent être sécurisées à l’aide de cette identité.</span><span class="sxs-lookup"><span data-stu-id="e53c8-427">Resources can be secured using this identity.</span></span> <span data-ttu-id="e53c8-428">Toutefois, cette identité n’est pas un compte d’utilisateur réel et n’apparaît pas dans la console de gestion d’utilisateurs Windows.</span><span class="sxs-lookup"><span data-stu-id="e53c8-428">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="e53c8-429">Si le processus Worker IIS a besoin d’un accès élevé à l’application, modifiez la liste de contrôle d’accès (ACL) du répertoire contenant l’application :</span><span class="sxs-lookup"><span data-stu-id="e53c8-429">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="e53c8-430">Ouvrez l’Explorateur Windows et accédez au répertoire.</span><span class="sxs-lookup"><span data-stu-id="e53c8-430">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="e53c8-431">Cliquez avec le bouton droit sur le répertoire et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-431">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="e53c8-432">Sous l’onglet **Sécurité**, sélectionnez le bouton **Modifier**, puis le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-432">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="e53c8-433">Sélectionnez le bouton **Emplacements**, puis vérifiez que le système est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e53c8-433">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="e53c8-434">Entrez **IIS AppPool\\<app_pool_name>** dans la zone **Entrer les noms des objets à sélectionner**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-434">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="e53c8-435">Sélectionnez le bouton **Vérifier les noms**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-435">Select the **Check Names** button.</span></span> <span data-ttu-id="e53c8-436">Pour le *DefaultAppPool*, vérifiez les noms à l’aide de **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-436">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="e53c8-437">Lorsque le bouton **Vérifier les noms** est sélectionné, une valeur de **DefaultAppPool** est indiquée dans la zone des noms d’objets.</span><span class="sxs-lookup"><span data-stu-id="e53c8-437">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="e53c8-438">Il n’est pas possible d’entrer le nom du pool d’applications directement dans la zone des noms d’objets.</span><span class="sxs-lookup"><span data-stu-id="e53c8-438">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="e53c8-439">Utilisez le format **IIS AppPool\\<app_pool_name>** lors de la vérification du nom de l’objet.</span><span class="sxs-lookup"><span data-stu-id="e53c8-439">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Sélectionnez la boîte de dialogue utilisateurs ou groupes pour le dossier d’applications : ajoutez le nom du pool d’applications « DefaultAppPool » à « IIS AppPool\" dans la zone des noms d’objets avant de sélectionner « Vérifier les noms ».](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="e53c8-441">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-441">Select **OK**.</span></span>

   ![Sélectionnez la boîte de dialogue utilisateurs ou groupes pour le dossier d’applications : après avoir sélectionné « Vérifier les noms », le nom d’objet « DefaultAppPool » est indiqué dans la zone des noms d’objets.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="e53c8-443">Les autorisations Lire &amp; exécuter doivent être accordées par défaut.</span><span class="sxs-lookup"><span data-stu-id="e53c8-443">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="e53c8-444">Fournissez des autorisations supplémentaires si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="e53c8-444">Provide additional permissions as needed.</span></span>

<span data-ttu-id="e53c8-445">L’accès peut également être octroyé par le biais d’une invite de commandes à l’aide de l’outil **ICACLS**.</span><span class="sxs-lookup"><span data-stu-id="e53c8-445">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="e53c8-446">À l’aide de *DefaultAppPool* en exemple, la commande suivante est utilisée :</span><span class="sxs-lookup"><span data-stu-id="e53c8-446">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="e53c8-447">Pour plus d’informations, consultez la rubrique [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="e53c8-447">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="e53c8-448">Prise en charge de HTTP/2</span><span class="sxs-lookup"><span data-stu-id="e53c8-448">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e53c8-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge avec ASP.NET Core dans les scénarios de déploiement IIS suivants :</span><span class="sxs-lookup"><span data-stu-id="e53c8-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="e53c8-450">In-process</span><span class="sxs-lookup"><span data-stu-id="e53c8-450">In-process</span></span>
  * <span data-ttu-id="e53c8-451">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-451">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="e53c8-452">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-452">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="e53c8-453">Out-of-process</span><span class="sxs-lookup"><span data-stu-id="e53c8-453">Out-of-process</span></span>
  * <span data-ttu-id="e53c8-454">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-454">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="e53c8-455">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse pour le [serveur Kestrel](xref:fundamentals/servers/kestrel) utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="e53c8-455">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="e53c8-456">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-456">TLS 1.2 or later connection</span></span>

<span data-ttu-id="e53c8-457">Pour un déploiement in-process lorsqu’une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-457">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="e53c8-458">Pour un déploiement in-process lorsqu’une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-458">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="e53c8-459">Pour plus d’informations sur les modèles d’hébergement in-process et out-of-process, consultez la rubrique <xref:fundamentals/servers/aspnet-core-module> et <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="e53c8-459">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e53c8-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) est pris en charge pour les déploiements out-of-process qui répondent aux exigences de base suivantes :</span><span class="sxs-lookup"><span data-stu-id="e53c8-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="e53c8-461">Windows Server 2016/Windows 10 ou version ultérieure ; IIS 10 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-461">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="e53c8-462">Les connexions au serveur périphérique public utilisent HTTP/2, mais la connexion de proxy inverse pour le [serveur Kestrel](xref:fundamentals/servers/kestrel) utilise HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="e53c8-462">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="e53c8-463">Version cible de .Net Framework : non applicable aux déploiements out-of-process, étant donné que la connexion HTTP/2 est gérée entièrement par IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-463">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="e53c8-464">TLS 1.2 ou connexion ultérieure</span><span class="sxs-lookup"><span data-stu-id="e53c8-464">TLS 1.2 or later connection</span></span>

<span data-ttu-id="e53c8-465">Si une connexion HTTP/2 est établie, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) retourne `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="e53c8-465">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="e53c8-466">HTTP/2 est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="e53c8-466">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="e53c8-467">Les connexions reviennent à HTTP/1.1 si une connexion HTTP/2 n’est pas établie.</span><span class="sxs-lookup"><span data-stu-id="e53c8-467">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="e53c8-468">Pour plus d’informations sur la configuration de HTTP/2 avec les déploiements IIS, consultez [HTTP/2 sur IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="e53c8-468">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="e53c8-469">Déploiement de ressources pour les administrateurs IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-469">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="e53c8-470">Pour en savoir plus sur IIS, consultez la documentation IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-470">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="e53c8-471">Documentation IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-471">IIS documentation</span></span>](/iis)

<span data-ttu-id="e53c8-472">Découvrez les modèles de déploiement d’application .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-472">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="e53c8-473">Déploiement d’applications .NET Core</span><span class="sxs-lookup"><span data-stu-id="e53c8-473">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="e53c8-474">Découvrez comment le module ASP.NET Core permet au serveur web Kestrel d’utiliser IIS ou IIS Express en tant que serveur proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="e53c8-474">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="e53c8-475">Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e53c8-475">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="e53c8-476">Découvrez comment configurer le module ASP.NET Core pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-476">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="e53c8-477">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e53c8-477">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="e53c8-478">Découvrez la structure de répertoires des applications ASP.NET Core publiées.</span><span class="sxs-lookup"><span data-stu-id="e53c8-478">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="e53c8-479">Structure de répertoires</span><span class="sxs-lookup"><span data-stu-id="e53c8-479">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="e53c8-480">Découvrez les modules IIS actifs et inactifs pour les applications ASP.NET Core et comment gérer les modules IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-480">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="e53c8-481">Modules IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-481">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="e53c8-482">Découvrez comment diagnostiquer les problèmes liés aux déploiements IIS d’applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e53c8-482">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="e53c8-483">Résoudre les problèmes</span><span class="sxs-lookup"><span data-stu-id="e53c8-483">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="e53c8-484">Repérer les erreurs courantes liées à l’hébergement d’applications ASP.NET Core sur IIS.</span><span class="sxs-lookup"><span data-stu-id="e53c8-484">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="e53c8-485">Informations de référence sur les erreurs courantes pour Azure App Service et IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-485">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="e53c8-486">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e53c8-486">Additional resources</span></span>

* [<span data-ttu-id="e53c8-487">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e53c8-487">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="e53c8-488">Site officiel de Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-488">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="e53c8-489">Bibliothèque de contenu technique Windows Server</span><span class="sxs-lookup"><span data-stu-id="e53c8-489">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="e53c8-490">HTTP/2 sur IIS</span><span class="sxs-lookup"><span data-stu-id="e53c8-490">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
