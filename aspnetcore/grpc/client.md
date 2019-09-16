---
title: Appeler gRPC services avec le client .NET
author: jamesnk
description: Découvrez comment appeler des services gRPC avec le client .NET gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 56f79b303a8d53699e8eb6156d328c0da1259416
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011138"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="09ed2-103">Appeler gRPC services avec le client .NET</span><span class="sxs-lookup"><span data-stu-id="09ed2-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="09ed2-104">Une bibliothèque cliente .NET gRPC est disponible dans le package NuGet [gRPC .net. client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="09ed2-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="09ed2-105">Ce document explique comment effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="09ed2-105">This document explains how to:</span></span>

* <span data-ttu-id="09ed2-106">Configurez un client gRPC pour appeler les services gRPC.</span><span class="sxs-lookup"><span data-stu-id="09ed2-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="09ed2-107">Effectuez des appels gRPC à des méthodes unaire, de streaming de serveur, de diffusion de client et de diffusion bidirectionnelle.</span><span class="sxs-lookup"><span data-stu-id="09ed2-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="09ed2-108">Configurer le client gRPC</span><span class="sxs-lookup"><span data-stu-id="09ed2-108">Configure gRPC client</span></span>

<span data-ttu-id="09ed2-109">les clients gRPC sont des types de client concrets qui sont [générés à partir de  *\*fichiers. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="09ed2-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="09ed2-110">Le client gRPC concret possède des méthodes qui traduisent le service gRPC dans le  *\*fichier. proto* .</span><span class="sxs-lookup"><span data-stu-id="09ed2-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="09ed2-111">Un client gRPC est créé à partir d’un canal.</span><span class="sxs-lookup"><span data-stu-id="09ed2-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="09ed2-112">Commencez par utiliser `GrpcChannel.ForAddress` pour créer un canal, puis utilisez le canal pour créer un client gRPC :</span><span class="sxs-lookup"><span data-stu-id="09ed2-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="09ed2-113">Un canal représente une connexion à long terme à un service gRPC.</span><span class="sxs-lookup"><span data-stu-id="09ed2-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="09ed2-114">Lorsqu’un canal est créé, il est configuré avec les options relatives à l’appel d’un service.</span><span class="sxs-lookup"><span data-stu-id="09ed2-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="09ed2-115">Par exemple, le `HttpClient` utilisé pour effectuer des appels, la taille maximale des messages d’envoi et de réception, et la `GrpcChannelOptions` journalisation peuvent `GrpcChannel.ForAddress`être spécifiés sur et utilisés avec.</span><span class="sxs-lookup"><span data-stu-id="09ed2-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="09ed2-116">Pour obtenir la liste complète des options, consultez [options de configuration du client](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="09ed2-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="09ed2-117">La création d’un canal peut être une opération coûteuse et la réutilisation d’un canal pour les appels gRPC offre des avantages en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="09ed2-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="09ed2-118">Plusieurs clients gRPC concrets peuvent être créés à partir d’un canal, y compris différents types de clients.</span><span class="sxs-lookup"><span data-stu-id="09ed2-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="09ed2-119">Les types de clients gRPC concrets sont des objets légers et peuvent être créés quand cela est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="09ed2-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="09ed2-120">`GrpcChannel.ForAddress`n’est pas la seule option pour créer un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="09ed2-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="09ed2-121">Si vous appelez gRPC services à partir d’une application ASP.NET Core, envisagez l’intégration de la [fabrique de clients gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="09ed2-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="09ed2-122">l’intégration de `HttpClientFactory` gRPC avec offre une alternative centralisée à la création de clients gRPC.</span><span class="sxs-lookup"><span data-stu-id="09ed2-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="09ed2-123">Une configuration supplémentaire est requise pour [appeler des services gRPC non sécurisés avec le client .net](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="09ed2-123">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="09ed2-124">Effectuer des appels gRPC</span><span class="sxs-lookup"><span data-stu-id="09ed2-124">Make gRPC calls</span></span>

<span data-ttu-id="09ed2-125">Un appel gRPC est initié par l’appel d’une méthode sur le client.</span><span class="sxs-lookup"><span data-stu-id="09ed2-125">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="09ed2-126">Le client gRPC gère la sérialisation des messages et traite l’appel gRPC au service approprié.</span><span class="sxs-lookup"><span data-stu-id="09ed2-126">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="09ed2-127">gRPC a différents types de méthodes.</span><span class="sxs-lookup"><span data-stu-id="09ed2-127">gRPC has different types of methods.</span></span> <span data-ttu-id="09ed2-128">La façon dont vous utilisez le client pour effectuer un appel gRPC dépend du type de méthode que vous appelez.</span><span class="sxs-lookup"><span data-stu-id="09ed2-128">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="09ed2-129">Les types de méthode gRPC sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="09ed2-129">The gRPC method types are:</span></span>

* <span data-ttu-id="09ed2-130">Unaire</span><span class="sxs-lookup"><span data-stu-id="09ed2-130">Unary</span></span>
* <span data-ttu-id="09ed2-131">Streaming de serveur</span><span class="sxs-lookup"><span data-stu-id="09ed2-131">Server streaming</span></span>
* <span data-ttu-id="09ed2-132">Diffusion en continu du client</span><span class="sxs-lookup"><span data-stu-id="09ed2-132">Client streaming</span></span>
* <span data-ttu-id="09ed2-133">Streaming bidirectionnel</span><span class="sxs-lookup"><span data-stu-id="09ed2-133">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="09ed2-134">Appel unaire</span><span class="sxs-lookup"><span data-stu-id="09ed2-134">Unary call</span></span>

<span data-ttu-id="09ed2-135">Un appel unaire commence par le client envoyant un message de demande.</span><span class="sxs-lookup"><span data-stu-id="09ed2-135">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="09ed2-136">Un message de réponse est retourné lorsque le service se termine.</span><span class="sxs-lookup"><span data-stu-id="09ed2-136">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="09ed2-137">Chaque méthode de  *\** service unaire du fichier. proto se traduira par deux méthodes .net sur le type de client gRPC concret pour l’appel de la méthode : une méthode asynchrone et une méthode de blocage.</span><span class="sxs-lookup"><span data-stu-id="09ed2-137">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="09ed2-138">Par exemple, `GreeterClient` il existe deux façons d’appeler `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="09ed2-138">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="09ed2-139">`GreeterClient.SayHelloAsync`-appelle `Greeter.SayHello` le service de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="09ed2-139">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="09ed2-140">Peut être attendu.</span><span class="sxs-lookup"><span data-stu-id="09ed2-140">Can be awaited.</span></span>
* <span data-ttu-id="09ed2-141">`GreeterClient.SayHello`-appelle `Greeter.SayHello` le service et bloque jusqu’à la fin.</span><span class="sxs-lookup"><span data-stu-id="09ed2-141">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="09ed2-142">N’utilisez pas dans le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="09ed2-142">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="09ed2-143">Appel de streaming de serveur</span><span class="sxs-lookup"><span data-stu-id="09ed2-143">Server streaming call</span></span>

<span data-ttu-id="09ed2-144">Un appel de streaming de serveur commence par le client qui envoie un message de demande.</span><span class="sxs-lookup"><span data-stu-id="09ed2-144">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="09ed2-145">`ResponseStream.MoveNext()`lit les messages diffusés en continu à partir du service.</span><span class="sxs-lookup"><span data-stu-id="09ed2-145">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="09ed2-146">L’appel de streaming de serveur est `ResponseStream.MoveNext()` terminé `false`lorsque retourne.</span><span class="sxs-lookup"><span data-stu-id="09ed2-146">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="09ed2-147">Si vous utilisez C# 8 ou une version ultérieure, `await foreach` la syntaxe peut être utilisée pour lire les messages.</span><span class="sxs-lookup"><span data-stu-id="09ed2-147">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="09ed2-148">La `IAsyncStreamReader<T>.ReadAllAsync()` méthode d’extension lit tous les messages du flux de réponse :</span><span class="sxs-lookup"><span data-stu-id="09ed2-148">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="09ed2-149">Appel de diffusion en continu du client</span><span class="sxs-lookup"><span data-stu-id="09ed2-149">Client streaming call</span></span>

<span data-ttu-id="09ed2-150">Un appel de diffusion en continu du client démarre *sans* que le client envoie un message.</span><span class="sxs-lookup"><span data-stu-id="09ed2-150">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="09ed2-151">Le client peut choisir d’envoyer des messages avec `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="09ed2-151">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="09ed2-152">Une fois que le client a terminé `RequestStream.CompleteAsync` l’envoi des messages, doit être appelé pour notifier le service.</span><span class="sxs-lookup"><span data-stu-id="09ed2-152">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="09ed2-153">L’appel est terminé lorsque le service renvoie un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="09ed2-153">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="09ed2-154">Appel de streaming bidirectionnel</span><span class="sxs-lookup"><span data-stu-id="09ed2-154">Bi-directional streaming call</span></span>

<span data-ttu-id="09ed2-155">Un appel de streaming bidirectionnel démarre *sans* que le client envoie un message.</span><span class="sxs-lookup"><span data-stu-id="09ed2-155">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="09ed2-156">Le client peut choisir d’envoyer des messages `RequestStream.WriteAsync`avec.</span><span class="sxs-lookup"><span data-stu-id="09ed2-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="09ed2-157">Les messages transmis en continu à partir du service `ResponseStream.MoveNext()` sont `ResponseStream.ReadAllAsync()`accessibles avec ou.</span><span class="sxs-lookup"><span data-stu-id="09ed2-157">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="09ed2-158">L’appel de streaming bidirectionnel est terminé lorsque le n' `ResponseStream` a plus de messages.</span><span class="sxs-lookup"><span data-stu-id="09ed2-158">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="09ed2-159">Pendant un appel de streaming bidirectionnel, le client et le service peuvent envoyer des messages entre eux à tout moment.</span><span class="sxs-lookup"><span data-stu-id="09ed2-159">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="09ed2-160">La meilleure logique cliente pour interagir avec un appel bidirectionnel varie en fonction de la logique du service.</span><span class="sxs-lookup"><span data-stu-id="09ed2-160">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09ed2-161">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="09ed2-161">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
