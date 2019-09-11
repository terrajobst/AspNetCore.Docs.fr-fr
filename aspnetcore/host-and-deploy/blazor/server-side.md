---
title: Héberger et déployer ASP.NET Core Blazor côté serveur
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor côté serveur avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: fc47dfa1344b74ec7110211e3698217e246ab86d
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878486"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="7c881-103">Héberger et déployer Blazor côté serveur</span><span class="sxs-lookup"><span data-stu-id="7c881-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="7c881-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7c881-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="7c881-105">Valeurs de configuration de l’hôte</span><span class="sxs-lookup"><span data-stu-id="7c881-105">Host configuration values</span></span>

<span data-ttu-id="7c881-106">Les applications côté serveur qui utilisent le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side) peuvent accepter des [valeurs de configuration d’hôte générique](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="7c881-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="7c881-107">Déploiement</span><span class="sxs-lookup"><span data-stu-id="7c881-107">Deployment</span></span>

<span data-ttu-id="7c881-108">Avec le [modèle d’hébergement côté serveur](xref:blazor/hosting-models#server-side), Blazor est exécuté sur le serveur à partir d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c881-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="7c881-109">Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="7c881-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="7c881-110">Un serveur web capable d’héberger une application ASP.NET Core est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7c881-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="7c881-111">Visual Studio inclut le modèle de projet **Application serveur Blazor** (modèle `blazorserverside` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="7c881-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="7c881-112">Extensibilité</span><span class="sxs-lookup"><span data-stu-id="7c881-112">Scalability</span></span>

<span data-ttu-id="7c881-113">Planifiez un déploiement pour tirer le meilleur parti de l’infrastructure disponible pour une application serveur éblouissante.</span><span class="sxs-lookup"><span data-stu-id="7c881-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="7c881-114">Consultez les ressources suivantes pour résoudre l’évolutivité de l’application serveur éblouissante :</span><span class="sxs-lookup"><span data-stu-id="7c881-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="7c881-115">Notions de base des applications serveur éblouissantes</span><span class="sxs-lookup"><span data-stu-id="7c881-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#server-side)
* <xref:security/blazor/server-side>

### <a name="deployment-server"></a><span data-ttu-id="7c881-116">Serveur de déploiement</span><span class="sxs-lookup"><span data-stu-id="7c881-116">Deployment server</span></span>

<span data-ttu-id="7c881-117">Lorsque vous envisagez l’évolutivité d’un serveur unique (montée en puissance), la mémoire disponible pour une application est probablement la première ressource que l’application épuisera en fonction des demandes des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="7c881-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="7c881-118">La mémoire disponible sur le serveur affecte les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="7c881-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="7c881-119">Nombre de circuits actifs qu’un serveur peut prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="7c881-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="7c881-120">Latence de l’interface utilisateur sur le client.</span><span class="sxs-lookup"><span data-stu-id="7c881-120">UI latency on the client.</span></span>

<span data-ttu-id="7c881-121">Pour obtenir des conseils sur la création d’applications serveur éblouissantes sécurisées et évolutives, consultez <xref:security/blazor/server-side>.</span><span class="sxs-lookup"><span data-stu-id="7c881-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server-side>.</span></span>

<span data-ttu-id="7c881-122">Chaque circuit utilise environ 250 Ko de mémoire pour une application de type *Hello World*minimale.</span><span class="sxs-lookup"><span data-stu-id="7c881-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="7c881-123">La taille d’un circuit dépend du code de l’application et des exigences de maintenance d’état associées à chaque composant.</span><span class="sxs-lookup"><span data-stu-id="7c881-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="7c881-124">Nous vous recommandons de mesurer les demandes de ressources pendant le développement de votre application et de votre infrastructure, mais la ligne de base suivante peut être un point de départ pour la planification de votre cible de déploiement : Si vous vous attendez à ce que votre application prenne en charge 5 000 utilisateurs simultanés, envisagez de budgétiser au moins 1,3 Go de mémoire serveur vers l’application (ou ~ 273 Ko par utilisateur).</span><span class="sxs-lookup"><span data-stu-id="7c881-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="7c881-125">Configuration SignalR</span><span class="sxs-lookup"><span data-stu-id="7c881-125">SignalR configuration</span></span>

<span data-ttu-id="7c881-126">Les applications serveur éblouissantes utilisent ASP.NET Core Signalr pour communiquer avec le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7c881-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="7c881-127">[Les conditions d’hébergement et de mise à l’échelle de signalr](xref:signalr/publish-to-azure-web-app) s’appliquent aux applications de serveur éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="7c881-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="7c881-128">Éblouissant fonctionne mieux lorsque vous utilisez WebSockets comme transport Signalr en raison d’une latence, d’une fiabilité et d’une [sécurité](xref:signalr/security)moindres.</span><span class="sxs-lookup"><span data-stu-id="7c881-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="7c881-129">L’interrogation longue est utilisée par Signalr lorsque WebSocket n’est pas disponible ou lorsque l’application est configurée de manière explicite pour utiliser une interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="7c881-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="7c881-130">Lors du déploiement sur Azure App Service, configurez l’application pour qu’elle utilise WebSockets dans les paramètres Portail Azure pour le service.</span><span class="sxs-lookup"><span data-stu-id="7c881-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="7c881-131">Pour plus d’informations sur la configuration de l’application pour Azure App Service, consultez les [instructions de publication signalr](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="7c881-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="7c881-132">Nous vous recommandons d’utiliser le [service Azure signalr](/azure/azure-signalr) pour les applications serveur éblouissantes.</span><span class="sxs-lookup"><span data-stu-id="7c881-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="7c881-133">Le service permet de mettre à l’échelle une application de serveur éblouissant sur un grand nombre de connexions Signalr simultanées.</span><span class="sxs-lookup"><span data-stu-id="7c881-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="7c881-134">En outre, la portée mondiale et les centres de données haute performance du service Signalr contribuent de manière significative à réduire la latence en raison de la géographie.</span><span class="sxs-lookup"><span data-stu-id="7c881-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="7c881-135">Mesurer la latence du réseau</span><span class="sxs-lookup"><span data-stu-id="7c881-135">Measure network latency</span></span>

<span data-ttu-id="7c881-136">L' [interopérabilité js](xref:blazor/javascript-interop) peut être utilisée pour mesurer la latence du réseau, comme le montre l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7c881-136">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```cshtml
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

<span data-ttu-id="7c881-137">Pour une expérience d’interface utilisateur raisonnable, nous recommandons une latence d’interface utilisateur soutenue de 250 ms ou moins.</span><span class="sxs-lookup"><span data-stu-id="7c881-137">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
