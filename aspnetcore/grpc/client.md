---
title: Appeler gRPC services avec le client .NET
author: jamesnk
description: Découvrez comment appeler des services gRPC avec le client .NET gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 1e7887388a752fb35d00e65db210c3924c6ab192
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829099"
---
# <a name="call-grpc-services-with-the-net-client"></a>Appeler gRPC services avec le client .NET

Une bibliothèque cliente .NET gRPC est disponible dans le package NuGet [gRPC .net. client](https://www.nuget.org/packages/Grpc.Net.Client) . Ce document explique comment effectuer les opérations suivantes :

* Configurez un client gRPC pour appeler les services gRPC.
* Effectuez des appels gRPC à des méthodes unaire, de streaming de serveur, de diffusion de client et de diffusion bidirectionnelle.

## <a name="configure-grpc-client"></a>Configurer le client gRPC

les clients gRPC sont des types de client concrets qui sont [générés à partir des fichiers *\*. proto* ](xref:grpc/basics#generated-c-assets). Le client gRPC concret possède des méthodes qui traduisent le service gRPC dans le fichier *\*. proto* .

Un client gRPC est créé à partir d’un canal. Commencez par utiliser `GrpcChannel.ForAddress` pour créer un canal, puis utilisez le canal pour créer un client gRPC :

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Un canal représente une connexion à long terme à un service gRPC. Lorsqu’un canal est créé, il est configuré avec les options relatives à l’appel d’un service. Par exemple, le `HttpClient` utilisé pour effectuer des appels, la taille maximale des messages d’envoi et de réception, et la journalisation peuvent être spécifiés sur `GrpcChannelOptions` et utilisés avec `GrpcChannel.ForAddress`. Pour obtenir la liste complète des options, consultez [options de configuration du client](xref:grpc/configuration#configure-client-options).

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

Performances et utilisation des canaux et des clients :

* La création d’un canal peut être une opération coûteuse. La réutilisation d’un canal pour les appels gRPC offre des avantages en matière de performances.
* les clients gRPC sont créés avec des canaux. les clients gRPC sont des objets légers et n’ont pas besoin d’être mis en cache ou réutilisés.
* Plusieurs clients gRPC peuvent être créés à partir d’un canal, y compris différents types de clients.
* Un canal et des clients créés à partir du canal peuvent être utilisés en toute sécurité par plusieurs threads.
* Les clients créés à partir du canal peuvent effectuer plusieurs appels simultanés.

`GrpcChannel.ForAddress` n’est pas la seule option pour créer un client gRPC. Si vous appelez gRPC services à partir d’une application ASP.NET Core, envisagez l’intégration de la [fabrique de clients gRPC](xref:grpc/clientfactory). l’intégration de gRPC à `HttpClientFactory` offre une alternative centralisée à la création de clients gRPC.

> [!NOTE]
> Une configuration supplémentaire est requise pour [appeler des services gRPC non sécurisés avec le client .net](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).

## <a name="make-grpc-calls"></a>Effectuer des appels gRPC

Un appel gRPC est initié par l’appel d’une méthode sur le client. Le client gRPC gère la sérialisation des messages et traite l’appel gRPC au service approprié.

gRPC a différents types de méthodes. La façon dont vous utilisez le client pour effectuer un appel gRPC dépend du type de méthode que vous appelez. Les types de méthode gRPC sont les suivants :

* Unaire
* Streaming de serveur
* Diffusion en continu du client
* Streaming bidirectionnel

### <a name="unary-call"></a>Appel unaire

Un appel unaire commence par le client envoyant un message de demande. Un message de réponse est retourné lorsque le service se termine.

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

Chaque méthode de service unaire du fichier *\*. proto* génère deux méthodes .net sur le type de client gRPC concret pour l’appel de la méthode : une méthode asynchrone et une méthode de blocage. Par exemple, sur `GreeterClient` il existe deux façons d’appeler `SayHello`:

* `GreeterClient.SayHelloAsync` : appelle `Greeter.SayHello` service de façon asynchrone. Peut être attendu.
* `GreeterClient.SayHello` : appelle `Greeter.SayHello` service et se bloque jusqu’à la fin. N’utilisez pas dans le code asynchrone.

### <a name="server-streaming-call"></a>Appel de streaming de serveur

Un appel de streaming de serveur commence par le client qui envoie un message de demande. `ResponseStream.MoveNext()` lit les messages diffusés en continu à partir du service. L’appel de streaming de serveur est terminé lorsque `ResponseStream.MoveNext()` retourne `false`.

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

Si vous utilisez C# 8 ou une version ultérieure, la syntaxe de `await foreach` peut être utilisée pour lire les messages. La méthode d’extension `IAsyncStreamReader<T>.ReadAllAsync()` lit tous les messages du flux de réponse :

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

### <a name="client-streaming-call"></a>Appel de diffusion en continu du client

Un appel de diffusion en continu du client démarre *sans* que le client envoie un message. Le client peut choisir d’envoyer des messages avec `RequestStream.WriteAsync`. Une fois que le client a terminé l’envoi des messages, `RequestStream.CompleteAsync` doit être appelé pour notifier le service. L’appel est terminé lorsque le service renvoie un message de réponse.

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

### <a name="bi-directional-streaming-call"></a>Appel de streaming bidirectionnel

Un appel de streaming bidirectionnel démarre *sans* que le client envoie un message. Le client peut choisir d’envoyer des messages avec `RequestStream.WriteAsync`. Les messages transmis en continu à partir du service sont accessibles avec `ResponseStream.MoveNext()` ou `ResponseStream.ReadAllAsync()`. L’appel de streaming bidirectionnel est terminé lorsque le `ResponseStream` n’a plus de messages.

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

Pendant un appel de streaming bidirectionnel, le client et le service peuvent envoyer des messages entre eux à tout moment. La meilleure logique cliente pour interagir avec un appel bidirectionnel varie en fonction de la logique du service.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
