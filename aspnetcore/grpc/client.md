---
title: Appeler gRPC services avec le client .NET
author: jamesnk
description: Découvrez comment appeler des services gRPC avec le client .NET gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667173"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="cd715-103">Appeler gRPC services avec le client .NET</span><span class="sxs-lookup"><span data-stu-id="cd715-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="cd715-104">Une bibliothèque cliente .NET gRPC est disponible dans le package NuGet [gRPC .net. client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="cd715-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="cd715-105">Ce document explique comment effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="cd715-105">This document explains how to:</span></span>

* <span data-ttu-id="cd715-106">Configurez un client gRPC pour appeler les services gRPC.</span><span class="sxs-lookup"><span data-stu-id="cd715-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="cd715-107">Effectuez des appels gRPC à des méthodes unaire, de streaming de serveur, de diffusion de client et de diffusion bidirectionnelle.</span><span class="sxs-lookup"><span data-stu-id="cd715-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="cd715-108">Configurer le client gRPC</span><span class="sxs-lookup"><span data-stu-id="cd715-108">Configure gRPC client</span></span>

<span data-ttu-id="cd715-109">les clients gRPC sont des types de client concrets qui sont [générés à partir des fichiers *\*. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="cd715-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="cd715-110">Le client gRPC concret possède des méthodes qui traduisent le service gRPC dans le fichier *\*. proto* .</span><span class="sxs-lookup"><span data-stu-id="cd715-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="cd715-111">Un client gRPC est créé à partir d’un canal.</span><span class="sxs-lookup"><span data-stu-id="cd715-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="cd715-112">Commencez par utiliser `GrpcChannel.ForAddress` pour créer un canal, puis utilisez le canal pour créer un client gRPC :</span><span class="sxs-lookup"><span data-stu-id="cd715-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="cd715-113">Un canal représente une connexion à long terme à un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="cd715-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="cd715-114">Lorsqu’un canal est créé, il est configuré avec les options relatives à l’appel d’un service.</span><span class="sxs-lookup"><span data-stu-id="cd715-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="cd715-115">Par exemple, le `HttpClient` utilisé pour effectuer des appels, la taille maximale des messages d’envoi et de réception, et la journalisation peuvent être spécifiés sur `GrpcChannelOptions` et utilisés avec `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="cd715-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="cd715-116">Pour obtenir la liste complète des options, consultez [options de configuration du client](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="cd715-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="cd715-117">Performances et utilisation des canaux et des clients :</span><span class="sxs-lookup"><span data-stu-id="cd715-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="cd715-118">La création d’un canal peut être une opération coûteuse.</span><span class="sxs-lookup"><span data-stu-id="cd715-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="cd715-119">La réutilisation d’un canal pour les appels gRPC offre des avantages en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="cd715-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="cd715-120">les clients gRPC sont créés avec des canaux.</span><span class="sxs-lookup"><span data-stu-id="cd715-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="cd715-121">les clients gRPC sont des objets légers et n’ont pas besoin d’être mis en cache ou réutilisés.</span><span class="sxs-lookup"><span data-stu-id="cd715-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="cd715-122">Plusieurs clients gRPC peuvent être créés à partir d’un canal, y compris différents types de clients.</span><span class="sxs-lookup"><span data-stu-id="cd715-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="cd715-123">Un canal et des clients créés à partir du canal peuvent être utilisés en toute sécurité par plusieurs threads.</span><span class="sxs-lookup"><span data-stu-id="cd715-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="cd715-124">Les clients créés à partir du canal peuvent effectuer plusieurs appels simultanés.</span><span class="sxs-lookup"><span data-stu-id="cd715-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="cd715-125">`GrpcChannel.ForAddress` n’est pas la seule option pour créer un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="cd715-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="cd715-126">Si vous appelez gRPC services à partir d’une application ASP.NET Core, envisagez l’intégration de la [fabrique de clients gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="cd715-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="cd715-127">l’intégration de gRPC à `HttpClientFactory` offre une alternative centralisée à la création de clients gRPC.</span><span class="sxs-lookup"><span data-stu-id="cd715-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="cd715-128">Une configuration supplémentaire est requise pour [appeler des services gRPC non sécurisés avec le client .net](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="cd715-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="cd715-129">L’appel de gRPC sur HTTP/2 avec `Grpc.Net.Client` n’est actuellement pas pris en charge sur Xamarin.</span><span class="sxs-lookup"><span data-stu-id="cd715-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="cd715-130">Nous nous efforçons d’améliorer la prise en charge de HTTP/2 dans une prochaine version de Xamarin.</span><span class="sxs-lookup"><span data-stu-id="cd715-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="cd715-131">[GRPC. Core](https://www.nuget.org/packages/Grpc.Core) et [GRPC-Web](xref:grpc/browser) sont des alternatives viables qui fonctionnent aujourd’hui.</span><span class="sxs-lookup"><span data-stu-id="cd715-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="cd715-132">Effectuer des appels gRPC</span><span class="sxs-lookup"><span data-stu-id="cd715-132">Make gRPC calls</span></span>

<span data-ttu-id="cd715-133">Un appel gRPC est initié par l’appel d’une méthode sur le client.</span><span class="sxs-lookup"><span data-stu-id="cd715-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="cd715-134">Le client gRPC gère la sérialisation des messages et traite l’appel gRPC au service approprié.</span><span class="sxs-lookup"><span data-stu-id="cd715-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="cd715-135">gRPC a différents types de méthodes.</span><span class="sxs-lookup"><span data-stu-id="cd715-135">gRPC has different types of methods.</span></span> <span data-ttu-id="cd715-136">La façon dont vous utilisez le client pour effectuer un appel gRPC dépend du type de méthode que vous appelez.</span><span class="sxs-lookup"><span data-stu-id="cd715-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="cd715-137">Les types de méthode gRPC sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="cd715-137">The gRPC method types are:</span></span>

* <span data-ttu-id="cd715-138">Unaire</span><span class="sxs-lookup"><span data-stu-id="cd715-138">Unary</span></span>
* <span data-ttu-id="cd715-139">Streaming de serveur</span><span class="sxs-lookup"><span data-stu-id="cd715-139">Server streaming</span></span>
* <span data-ttu-id="cd715-140">Diffusion en continu du client</span><span class="sxs-lookup"><span data-stu-id="cd715-140">Client streaming</span></span>
* <span data-ttu-id="cd715-141">Streaming bidirectionnel</span><span class="sxs-lookup"><span data-stu-id="cd715-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="cd715-142">Appel unaire</span><span class="sxs-lookup"><span data-stu-id="cd715-142">Unary call</span></span>

<span data-ttu-id="cd715-143">Un appel unaire commence par le client envoyant un message de demande.</span><span class="sxs-lookup"><span data-stu-id="cd715-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="cd715-144">Un message de réponse est retourné lorsque le service se termine.</span><span class="sxs-lookup"><span data-stu-id="cd715-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="cd715-145">Chaque méthode de service unaire du fichier *\*. proto* génère deux méthodes .net sur le type de client gRPC concret pour l’appel de la méthode : une méthode asynchrone et une méthode de blocage.</span><span class="sxs-lookup"><span data-stu-id="cd715-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="cd715-146">Par exemple, sur `GreeterClient` il existe deux façons d’appeler `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="cd715-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="cd715-147">`GreeterClient.SayHelloAsync` : appelle `Greeter.SayHello` service de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cd715-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="cd715-148">Peut être attendu.</span><span class="sxs-lookup"><span data-stu-id="cd715-148">Can be awaited.</span></span>
* <span data-ttu-id="cd715-149">`GreeterClient.SayHello` : appelle `Greeter.SayHello` service et se bloque jusqu’à la fin.</span><span class="sxs-lookup"><span data-stu-id="cd715-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="cd715-150">N’utilisez pas dans le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cd715-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="cd715-151">Appel de streaming de serveur</span><span class="sxs-lookup"><span data-stu-id="cd715-151">Server streaming call</span></span>

<span data-ttu-id="cd715-152">Un appel de streaming de serveur commence par le client qui envoie un message de demande.</span><span class="sxs-lookup"><span data-stu-id="cd715-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="cd715-153">`ResponseStream.MoveNext()` lit les messages diffusés en continu à partir du service.</span><span class="sxs-lookup"><span data-stu-id="cd715-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="cd715-154">L’appel de streaming de serveur est terminé lorsque `ResponseStream.MoveNext()` retourne `false`.</span><span class="sxs-lookup"><span data-stu-id="cd715-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

<span data-ttu-id="cd715-155">Si vous utilisez C# 8 ou une version ultérieure, la syntaxe de `await foreach` peut être utilisée pour lire les messages.</span><span class="sxs-lookup"><span data-stu-id="cd715-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="cd715-156">La méthode d’extension `IAsyncStreamReader<T>.ReadAllAsync()` lit tous les messages du flux de réponse :</span><span class="sxs-lookup"><span data-stu-id="cd715-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="cd715-157">Appel de diffusion en continu du client</span><span class="sxs-lookup"><span data-stu-id="cd715-157">Client streaming call</span></span>

<span data-ttu-id="cd715-158">Un appel de diffusion en continu du client démarre *sans* que le client envoie un message.</span><span class="sxs-lookup"><span data-stu-id="cd715-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="cd715-159">Le client peut choisir d’envoyer des messages avec `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="cd715-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="cd715-160">Une fois que le client a terminé l’envoi des messages, `RequestStream.CompleteAsync` doit être appelé pour notifier le service.</span><span class="sxs-lookup"><span data-stu-id="cd715-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="cd715-161">L’appel est terminé lorsque le service renvoie un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="cd715-161">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="cd715-162">Appel de streaming bidirectionnel</span><span class="sxs-lookup"><span data-stu-id="cd715-162">Bi-directional streaming call</span></span>

<span data-ttu-id="cd715-163">Un appel de streaming bidirectionnel démarre *sans* que le client envoie un message.</span><span class="sxs-lookup"><span data-stu-id="cd715-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="cd715-164">Le client peut choisir d’envoyer des messages avec `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="cd715-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="cd715-165">Les messages transmis en continu à partir du service sont accessibles avec `ResponseStream.MoveNext()` ou `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="cd715-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="cd715-166">L’appel de streaming bidirectionnel est terminé lorsque le `ResponseStream` n’a plus de messages.</span><span class="sxs-lookup"><span data-stu-id="cd715-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

<span data-ttu-id="cd715-167">Pendant un appel de streaming bidirectionnel, le client et le service peuvent envoyer des messages entre eux à tout moment.</span><span class="sxs-lookup"><span data-stu-id="cd715-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="cd715-168">La meilleure logique cliente pour interagir avec un appel bidirectionnel varie en fonction de la logique du service.</span><span class="sxs-lookup"><span data-stu-id="cd715-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd715-169">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cd715-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
