---
title: Hôte ASP.NET Core SignalR dans les services d’arrière-plan
author: bradygaster
description: Découvrez comment envoyer des messages aux clients de SignalR à partir de classes de .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739668"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="a3487-103">Hôte ASP.NET Core SignalR dans les services d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="a3487-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="a3487-104">Par [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="a3487-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="a3487-105">Cet article fournit des conseils pour :</span><span class="sxs-lookup"><span data-stu-id="a3487-105">This article provides guidance for:</span></span>

* <span data-ttu-id="a3487-106">Les concentrateurs SignalR à l’aide d’un processus de travail en arrière-plan hébergé avec ASP.NET Core d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a3487-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="a3487-107">Envoi de messages à des clients à partir d’au sein d’un cœur de .NET connectés [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="a3487-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="a3487-108">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a3487-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="a3487-109">Raccordées SignalR lors du démarrage</span><span class="sxs-lookup"><span data-stu-id="a3487-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="a3487-110">Hébergement ASP.NET Core SignalR Hubs dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un concentrateur dans une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3487-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="a3487-111">Dans le `Startup.ConfigureServices` (méthode), en appelant `services.AddSignalR` ajoute les services nécessaires à la couche de l’Injection de dépendance ASP.NET Core (DI) pour prendre en charge SignalR.</span><span class="sxs-lookup"><span data-stu-id="a3487-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="a3487-112">Dans `Startup.Configure`, le `UseSignalR` méthode est appelée pour associer les points de terminaison de Hub dans le pipeline de demande ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3487-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="a3487-113">Dans l’exemple précédent, le `ClockHub` la classe implémente la `Hub<T>` classe pour créer un Hub fortement typé.</span><span class="sxs-lookup"><span data-stu-id="a3487-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="a3487-114">Le `ClockHub` a été configuré dans le `Startup` classe pour répondre aux requêtes au point de terminaison `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="a3487-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="a3487-115">Pour plus d’informations sur les Hubs fortement typées, consultez [utiliser des hubs dans SignalR pour ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="a3487-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="a3487-116">Cette fonctionnalité n’est pas limitée à la [Hub\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) classe.</span><span class="sxs-lookup"><span data-stu-id="a3487-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="a3487-117">Toute classe qui hérite de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), tel que [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), fonctionne également.</span><span class="sxs-lookup"><span data-stu-id="a3487-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="a3487-118">L’interface utilisée par fortement typée `ClockHub` est le `IClock` interface.</span><span class="sxs-lookup"><span data-stu-id="a3487-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="a3487-119">Appeler un concentrateur SignalR à partir d’un service en arrière-plan</span><span class="sxs-lookup"><span data-stu-id="a3487-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="a3487-120">Lors du démarrage, le `Worker` (classe), un `BackgroundService`, est associé à l’aide de `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="a3487-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="a3487-121">Étant donné que SignalR est également associé au cours de la `Startup` phase, dans lequel chaque Hub est attaché à un point de terminaison individuel dans le pipeline de demande HTTP d’ASP.NET Core, chaque concentrateur est représenté par un `IHubContext<T>` sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="a3487-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="a3487-122">À l’aide d’ASP.NET Core l’injection de dépendances des fonctionnalités, autres classes instanciés par la couche d’hébergement, par exemple `BackgroundService` classes, des classes de contrôleur MVC ou des modèles de page Razor, peuvent obtenir des références aux concentrateurs côté serveur en acceptant les instances de `IHubContext<ClockHub, IClock>` pendant la construction.</span><span class="sxs-lookup"><span data-stu-id="a3487-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="a3487-123">Comme le `ExecuteAsync` méthode est appelée de manière itérative dans le service d’arrière-plan, date et l’heure du serveur sont envoyés aux clients connectés à l’aide de la `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="a3487-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="a3487-124">Réagir aux événements de SignalR avec les services d’arrière-plan</span><span class="sxs-lookup"><span data-stu-id="a3487-124">React to SignalR events with background services</span></span>

<span data-ttu-id="a3487-125">Comme une application à Page unique à l’aide le client JavaScript pour faire de SignalR ou une application de bureau .NET à l’aide du <xref:signalr/dotnet-client>, un `BackgroundService` ou `IHostedService` implémentation peut également être utilisée pour vous connecter à des concentrateurs SignalR et répondre aux événements.</span><span class="sxs-lookup"><span data-stu-id="a3487-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="a3487-126">Le `ClockHubClient` classe implémente à la fois le `IClock` interface et le `IHostedService` interface.</span><span class="sxs-lookup"><span data-stu-id="a3487-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="a3487-127">Ainsi, il peut être câblée pendant `Startup` pour s’exécuter en continu et répondre aux événements du Hub à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="a3487-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="a3487-128">Pendant l’initialisation, le `ClockHubClient` crée une instance d’un `HubConnection` et relie la `IClock.ShowTime` méthode comme un gestionnaire pour le Hub `ShowTime` événement.</span><span class="sxs-lookup"><span data-stu-id="a3487-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="a3487-129">Dans le `IHostedService.StartAsync` implémentation, le `HubConnection` est démarrée de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3487-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="a3487-130">Lors de la `IHostedService.StopAsync` (méthode), le `HubConnection` est supprimé de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a3487-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="a3487-131">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a3487-131">Additional resources</span></span>

* [<span data-ttu-id="a3487-132">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="a3487-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="a3487-133">Hubs</span><span class="sxs-lookup"><span data-stu-id="a3487-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a3487-134">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="a3487-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="a3487-135">Hubs fortement typés</span><span class="sxs-lookup"><span data-stu-id="a3487-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
