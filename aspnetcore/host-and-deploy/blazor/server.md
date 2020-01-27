---
title: Héberger et déployer ASP.NET Core serveur Blazor
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor Server à l’aide de ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/17/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: e8b3a7faaf1dc88059a79abbc7e74657ebb2068c
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726730"
---
# <a name="host-and-deploy-opno-locblazor-server"></a><span data-ttu-id="3da88-103">Héberger et déployer Blazor serveur</span><span class="sxs-lookup"><span data-stu-id="3da88-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="3da88-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="3da88-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="3da88-105">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="3da88-105">Host configuration values</span></span>

<span data-ttu-id="3da88-106">les [applicationsBlazor Server](xref:blazor/hosting-models#blazor-server) peuvent accepter des [valeurs de configuration d’hôte génériques](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="3da88-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="3da88-107">Déploiement</span><span class="sxs-lookup"><span data-stu-id="3da88-107">Deployment</span></span>

<span data-ttu-id="3da88-108">À l’aide du [modèle d’hébergement de serveurBlazor](xref:blazor/hosting-models#blazor-server), Blazor est exécutée sur le serveur à partir d’une application ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="3da88-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="3da88-109">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés via une connexion [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="3da88-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="3da88-110">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="3da88-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="3da88-111">Visual Studio comprend le modèle de projet d' **applicationBlazor Server** (`blazorserverside` modèle lors de l’utilisation de la commande [dotnet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="3da88-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="3da88-112">Extensibilité</span><span class="sxs-lookup"><span data-stu-id="3da88-112">Scalability</span></span>

<span data-ttu-id="3da88-113">Planifiez un déploiement pour tirer le meilleur parti de l’infrastructure disponible pour une application Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3da88-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="3da88-114">Consultez les ressources suivantes pour résoudre Blazor extensibilité des applications serveur :</span><span class="sxs-lookup"><span data-stu-id="3da88-114">See the following resources to address Blazor Server app scalability:</span></span>

* <span data-ttu-id="3da88-115">[Notions de base des applications Blazor Server](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="3da88-115">[Fundamentals of Blazor Server apps](xref:blazor/hosting-models#blazor-server)</span></span>
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="3da88-116">Serveur de déploiement</span><span class="sxs-lookup"><span data-stu-id="3da88-116">Deployment server</span></span>

<span data-ttu-id="3da88-117">Lorsque vous envisagez l’évolutivité d’un serveur unique (montée en puissance), la mémoire disponible pour une application est probablement la première ressource que l’application épuisera en fonction des demandes des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3da88-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="3da88-118">La mémoire disponible sur le serveur affecte les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="3da88-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="3da88-119">Nombre de circuits actifs qu’un serveur peut prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="3da88-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="3da88-120">Latence de l’interface utilisateur sur le client.</span><span class="sxs-lookup"><span data-stu-id="3da88-120">UI latency on the client.</span></span>

<span data-ttu-id="3da88-121">Pour obtenir des conseils sur la création d’applications Blazor Server sécurisées et évolutives, consultez <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="3da88-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="3da88-122">Chaque circuit utilise environ 250 Ko de mémoire pour une application de type *Hello World*minimale.</span><span class="sxs-lookup"><span data-stu-id="3da88-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="3da88-123">La taille d’un circuit dépend du code de l’application et des exigences de maintenance d’état associées à chaque composant.</span><span class="sxs-lookup"><span data-stu-id="3da88-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="3da88-124">Nous vous recommandons de mesurer les demandes de ressources pendant le développement de votre application et de votre infrastructure, mais la ligne de base suivante peut être un point de départ pour la planification de votre cible de déploiement : Si vous vous attendez à ce que votre application prenne en charge 5 000 utilisateurs simultanés, pensez à la budgétisation à moins de 1,3 Go de mémoire serveur pour l’application (ou ~ 273 Ko par utilisateur).</span><span class="sxs-lookup"><span data-stu-id="3da88-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="opno-locsignalr-configuration"></a><span data-ttu-id="3da88-125">configuration de SignalR</span><span class="sxs-lookup"><span data-stu-id="3da88-125">SignalR configuration</span></span>

<span data-ttu-id="3da88-126">les applications Blazor Server utilisent ASP.NET Core SignalR pour communiquer avec le navigateur.</span><span class="sxs-lookup"><span data-stu-id="3da88-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="3da88-127">[les conditions d’hébergement et de mise à l’échelle deSignalR](xref:signalr/publish-to-azure-web-app) s’appliquent aux applications Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="3da88-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

Blazor<span data-ttu-id="3da88-128"> works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="3da88-128"> works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="3da88-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span><span class="sxs-lookup"><span data-stu-id="3da88-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="3da88-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span><span class="sxs-lookup"><span data-stu-id="3da88-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="3da88-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="3da88-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

#### <a name="azure-opno-locsignalr-service"></a><span data-ttu-id="3da88-132">Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="3da88-132">Azure SignalR Service</span></span>

<span data-ttu-id="3da88-133">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span><span class="sxs-lookup"><span data-stu-id="3da88-133">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="3da88-134">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span><span class="sxs-lookup"><span data-stu-id="3da88-134">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="3da88-135">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span><span class="sxs-lookup"><span data-stu-id="3da88-135">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="3da88-136">To configure an app (and optionally provision) the Azure SignalR Service:</span><span class="sxs-lookup"><span data-stu-id="3da88-136">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

1. <span data-ttu-id="3da88-137">Enable the service to support *sticky sessions*, where clients are [redirected back to the same server when prerendering](xref:blazor/hosting-models#reconnection-to-the-same-server).</span><span class="sxs-lookup"><span data-stu-id="3da88-137">Enable the service to support *sticky sessions*, where clients are [redirected back to the same server when prerendering](xref:blazor/hosting-models#reconnection-to-the-same-server).</span></span> <span data-ttu-id="3da88-138">Set the `ServerStickyMode` option or configuration value to `Required`.</span><span class="sxs-lookup"><span data-stu-id="3da88-138">Set the `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="3da88-139">Typically, an app creates the configuration using **one** of the following approaches:</span><span class="sxs-lookup"><span data-stu-id="3da88-139">Typically, an app creates the configuration using **one** of the following approaches:</span></span>

   * <span data-ttu-id="3da88-140">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3da88-140">`Startup.ConfigureServices`:</span></span>
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * <span data-ttu-id="3da88-141">Configuration (use **one** of the following approaches):</span><span class="sxs-lookup"><span data-stu-id="3da88-141">Configuration (use **one** of the following approaches):</span></span>
  
     * <span data-ttu-id="3da88-142">*appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="3da88-142">*appsettings.json*:</span></span>

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * <span data-ttu-id="3da88-143">The app service's **Configuration** > **Application settings** in the Azure portal (**Name**: `Azure:SignalR:ServerStickyMode`, **Value**: `Required`).</span><span class="sxs-lookup"><span data-stu-id="3da88-143">The app service's **Configuration** > **Application settings** in the Azure portal (**Name**: `Azure:SignalR:ServerStickyMode`, **Value**: `Required`).</span></span>

1. <span data-ttu-id="3da88-144">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span><span class="sxs-lookup"><span data-stu-id="3da88-144">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
1. <span data-ttu-id="3da88-145">Add the **Azure SignalR Service** dependency to the profile.</span><span class="sxs-lookup"><span data-stu-id="3da88-145">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="3da88-146">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span><span class="sxs-lookup"><span data-stu-id="3da88-146">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
1. <span data-ttu-id="3da88-147">Publish the app to Azure.</span><span class="sxs-lookup"><span data-stu-id="3da88-147">Publish the app to Azure.</span></span>

#### <a name="iis"></a><span data-ttu-id="3da88-148">IIS</span><span class="sxs-lookup"><span data-stu-id="3da88-148">IIS</span></span>

<span data-ttu-id="3da88-149">When using IIS, sticky sessions are enabled with Application Request Routing.</span><span class="sxs-lookup"><span data-stu-id="3da88-149">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="3da88-150">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="3da88-150">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="kubernetes"></a><span data-ttu-id="3da88-151">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="3da88-151">Kubernetes</span></span>

<span data-ttu-id="3da88-152">Create an ingress definition with the following [Kubernetes annotations for sticky sessions](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span><span class="sxs-lookup"><span data-stu-id="3da88-152">Create an ingress definition with the following [Kubernetes annotations for sticky sessions](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span></span>

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

#### <a name="linux-with-nginx"></a><span data-ttu-id="3da88-153">Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="3da88-153">Linux with Nginx</span></span>

<span data-ttu-id="3da88-154">For SignalR WebSockets to function properly, set the proxy's `Upgrade` and `Connection` headers to the following:</span><span class="sxs-lookup"><span data-stu-id="3da88-154">For SignalR WebSockets to function properly, set the proxy's `Upgrade` and `Connection` headers to the following:</span></span>

```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

<span data-ttu-id="3da88-155">For more information, see [NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/).</span><span class="sxs-lookup"><span data-stu-id="3da88-155">For more information, see [NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/).</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="3da88-156">Measure network latency</span><span class="sxs-lookup"><span data-stu-id="3da88-156">Measure network latency</span></span>

<span data-ttu-id="3da88-157">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span><span class="sxs-lookup"><span data-stu-id="3da88-157">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```razor
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

<span data-ttu-id="3da88-158">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span><span class="sxs-lookup"><span data-stu-id="3da88-158">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
