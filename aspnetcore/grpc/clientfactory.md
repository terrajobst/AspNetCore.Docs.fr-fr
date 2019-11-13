---
title: intégration de la fabrique de clients gRPC dans .NET Core
author: jamesnk
description: Découvrez comment créer des clients gRPC à l’aide de la fabrique de clients.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963680"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="9e6f3-103">intégration de la fabrique de clients gRPC dans .NET Core</span><span class="sxs-lookup"><span data-stu-id="9e6f3-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="9e6f3-104">l’intégration de gRPC à `HttpClientFactory` offre un moyen centralisé de créer des clients gRPC.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="9e6f3-105">Il peut être utilisé comme alternative à la [configuration d’instances clientes gRPC autonomes](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="9e6f3-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="9e6f3-106">L’intégration des usines est disponible dans le package NuGet [GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="9e6f3-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="9e6f3-107">La fabrique offre les avantages suivants :</span><span class="sxs-lookup"><span data-stu-id="9e6f3-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="9e6f3-108">Fournit un emplacement central pour la configuration des instances de client gRPC logiques</span><span class="sxs-lookup"><span data-stu-id="9e6f3-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="9e6f3-109">Gère la durée de vie du `HttpClientMessageHandler` sous-jacent</span><span class="sxs-lookup"><span data-stu-id="9e6f3-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="9e6f3-110">Propagation automatique de l’échéance et de l’annulation dans un service ASP.NET Core gRPC</span><span class="sxs-lookup"><span data-stu-id="9e6f3-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="9e6f3-111">Inscrire les clients gRPC</span><span class="sxs-lookup"><span data-stu-id="9e6f3-111">Register gRPC clients</span></span>

<span data-ttu-id="9e6f3-112">Pour inscrire un client gRPC, vous pouvez utiliser la méthode d’extension de `AddGrpcClient` générique dans `Startup.ConfigureServices`, en spécifiant la classe de client et l’adresse de service gRPC typés :</span><span class="sxs-lookup"><span data-stu-id="9e6f3-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

<span data-ttu-id="9e6f3-113">Le type de client gRPC est inscrit comme temporaire avec l’injection de dépendances (DI).</span><span class="sxs-lookup"><span data-stu-id="9e6f3-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="9e6f3-114">Le client peut maintenant être injecté et consommé directement dans les types créés par DI.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="9e6f3-115">ASP.NET Core les contrôleurs MVC, SignalR hubs et gRPC services sont des emplacements où les clients gRPC peuvent être automatiquement injectés :</span><span class="sxs-lookup"><span data-stu-id="9e6f3-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a><span data-ttu-id="9e6f3-116">Configurer HttpClient</span><span class="sxs-lookup"><span data-stu-id="9e6f3-116">Configure HttpClient</span></span>

<span data-ttu-id="9e6f3-117">`HttpClientFactory` crée le `HttpClient` utilisé par le client gRPC.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="9e6f3-118">Les méthodes de `HttpClientFactory` standard peuvent être utilisées pour ajouter un intergiciel (middleware) de demandes sortantes ou pour configurer les `HttpClientHandler` sous-jacentes de l' `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="9e6f3-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

<span data-ttu-id="9e6f3-119">Pour plus d’informations, consultez [effectuer des requêtes http à l’aide de IHttpClientFactory](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="9e6f3-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="9e6f3-120">Configurer le canal et les intercepteurs</span><span class="sxs-lookup"><span data-stu-id="9e6f3-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="9e6f3-121">les méthodes spécifiques à gRPC sont disponibles pour :</span><span class="sxs-lookup"><span data-stu-id="9e6f3-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="9e6f3-122">Configurez le canal sous-jacent d’un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="9e6f3-123">Ajoutez `Interceptor` instances que le client utilisera pour effectuer des appels gRPC.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="9e6f3-124">Propagation de l’échéance et de l’annulation</span><span class="sxs-lookup"><span data-stu-id="9e6f3-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="9e6f3-125">les clients gRPC créés par la fabrique dans un service gRPC peuvent être configurés avec `EnableCallContextPropagation()` pour propager automatiquement l’échéance et le jeton d’annulation aux appels enfants.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="9e6f3-126">La méthode d’extension `EnableCallContextPropagation()` est disponible dans le package NuGet [GRPC. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="9e6f3-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="9e6f3-127">La propagation du contexte d’appel fonctionne en lisant l’échéance et le jeton d’annulation dans le contexte de requête gRPC actuel et en les propageant automatiquement aux appels sortants effectués par le client gRPC.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="9e6f3-128">La propagation du contexte d’appel est un excellent moyen de s’assurer que les scénarios de gRPC complexes et imbriqués propagent toujours l’échéance et l’annulation.</span><span class="sxs-lookup"><span data-stu-id="9e6f3-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="9e6f3-129">Pour plus d’informations sur les échéances et l’annulation RPC, consultez [cycle de vie RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span><span class="sxs-lookup"><span data-stu-id="9e6f3-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e6f3-130">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9e6f3-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
