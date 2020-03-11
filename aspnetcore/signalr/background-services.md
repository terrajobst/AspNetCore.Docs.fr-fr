---
title: SignalR de ASP.NET Core de l’hôte dans les services d’arrière-plan
author: bradygaster
description: Découvrez comment envoyer des messages à des clients SignalR à partir de classes BackgroundService .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 86319cc93febab18c29e2fb6366cef0d025943ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658143"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a><span data-ttu-id="df537-103">SignalR de ASP.NET Core de l’hôte dans les services d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="df537-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="df537-104">Par [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="df537-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="df537-105">Cet article fournit des conseils pour :</span><span class="sxs-lookup"><span data-stu-id="df537-105">This article provides guidance for:</span></span>

* <span data-ttu-id="df537-106">Hébergement SignalR hubs à l’aide d’un processus de travail en arrière-plan hébergé avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df537-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="df537-107">Envoi de messages à des clients connectés à partir d’un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).net core.</span><span class="sxs-lookup"><span data-stu-id="df537-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="df537-108">[Afficher ou télécharger l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="df537-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-opno-locsignalr-in-startup"></a><span data-ttu-id="df537-109">Activer SignalR au démarrage</span><span class="sxs-lookup"><span data-stu-id="df537-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df537-110">L’hébergement de ASP.NET Core SignalR hubs dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un concentrateur dans une application Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df537-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="df537-111">Dans la méthode `Startup.ConfigureServices`, l’appel de `services.AddSignalR` ajoute les services requis à la couche d’injection de dépendances ASP.NET Core pour prendre en charge SignalR.</span><span class="sxs-lookup"><span data-stu-id="df537-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="df537-112">Dans `Startup.Configure`, la méthode `MapHub` est appelée dans le rappel `UseEndpoints` pour connecter les points de terminaison de Hub dans le pipeline de demande ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df537-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="df537-113">L’hébergement de ASP.NET Core SignalR hubs dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un concentrateur dans une application Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df537-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="df537-114">Dans la méthode `Startup.ConfigureServices`, l’appel de `services.AddSignalR` ajoute les services requis à la couche d’injection de dépendances ASP.NET Core pour prendre en charge SignalR.</span><span class="sxs-lookup"><span data-stu-id="df537-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="df537-115">Dans `Startup.Configure`, la méthode `UseSignalR` est appelée pour connecter le ou les points de terminaison de concentrateur dans le pipeline de requête ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df537-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="df537-116">Dans l’exemple précédent, la classe `ClockHub` implémente la classe `Hub<T>` pour créer un concentrateur fortement typé.</span><span class="sxs-lookup"><span data-stu-id="df537-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="df537-117">La `ClockHub` a été configurée dans la classe `Startup` pour répondre aux demandes au niveau du point de terminaison `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="df537-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="df537-118">Pour plus d’informations sur les hubs fortement typés, consultez [utiliser des hubs dans SignalR pour les ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="df537-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="df537-119">Cette fonctionnalité n’est pas limitée à la classe [Hub\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="df537-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="df537-120">Toute classe qui hérite de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), telle que [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), fonctionne également.</span><span class="sxs-lookup"><span data-stu-id="df537-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="df537-121">L’interface utilisée par la `ClockHub` fortement typée est l’interface `IClock`.</span><span class="sxs-lookup"><span data-stu-id="df537-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a><span data-ttu-id="df537-122">Appeler un hub SignalR à partir d’un service en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="df537-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="df537-123">Au démarrage, la classe `Worker`, un `BackgroundService`, est activée à l’aide de `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="df537-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="df537-124">Étant donné que SignalR est également activé au cours de la phase de `Startup`, dans laquelle chaque concentrateur est attaché à un point de terminaison individuel dans le pipeline de requête HTTP d’ASP.NET Core, chaque concentrateur est représenté par une `IHubContext<T>` sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="df537-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="df537-125">À l’aide des fonctionnalités DI de ASP.NET Core, d’autres classes instanciées par la couche d’hébergement, comme les classes `BackgroundService`, les classes de contrôleur MVC ou les modèles de page Razor, peuvent recevoir des références aux hubs côté serveur en acceptant des instances de `IHubContext<ClockHub, IClock>` pendant la construction.</span><span class="sxs-lookup"><span data-stu-id="df537-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="df537-126">Comme la méthode `ExecuteAsync` est appelée de manière itérative dans le service d’arrière-plan, la date et l’heure actuelles du serveur sont envoyées aux clients connectés à l’aide de l' `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="df537-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-opno-locsignalr-events-with-background-services"></a><span data-ttu-id="df537-127">Réagir aux événements de SignalR avec les services d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="df537-127">React to SignalR events with background services</span></span>

<span data-ttu-id="df537-128">À l’instar d’une application à page unique utilisant le client JavaScript pour SignalR ou une application de bureau .NET peut utiliser à l’aide de l' <xref:signalr/dotnet-client>, une implémentation `BackgroundService` ou `IHostedService` peut également être utilisée pour se connecter aux hubs SignalR et répondre aux événements.</span><span class="sxs-lookup"><span data-stu-id="df537-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="df537-129">La classe `ClockHubClient` implémente à la fois l’interface `IClock` et l’interface `IHostedService`.</span><span class="sxs-lookup"><span data-stu-id="df537-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="df537-130">De cette façon, il peut être activé pendant `Startup` pour s’exécuter en continu et répondre aux événements de concentrateur à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="df537-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="df537-131">Pendant l’initialisation, le `ClockHubClient` crée une instance d’une `HubConnection` et active la méthode `IClock.ShowTime` comme gestionnaire de l’événement `ShowTime` du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="df537-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="df537-132">Dans l’implémentation de `IHostedService.StartAsync`, le `HubConnection` est démarré de manière asynchrone.</span><span class="sxs-lookup"><span data-stu-id="df537-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="df537-133">Pendant la méthode `IHostedService.StopAsync`, la `HubConnection` est supprimée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="df537-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="df537-134">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="df537-134">Additional resources</span></span>

* [<span data-ttu-id="df537-135">Prise en main</span><span class="sxs-lookup"><span data-stu-id="df537-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="df537-136">Hubs</span><span class="sxs-lookup"><span data-stu-id="df537-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="df537-137">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="df537-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="df537-138">Hubs fortement typés</span><span class="sxs-lookup"><span data-stu-id="df537-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
