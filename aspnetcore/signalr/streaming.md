---
title: Utiliser la diffusion en continu dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment diffuser des données entre le client et le serveur.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: a75156f398e113393ddb891d16eec3f09de80c09
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750186"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Utiliser la diffusion en continu dans ASP.NET Core SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR prend en charge la diffusion en continu du client au serveur et du serveur au client. Cela est utile pour les scénarios où les fragments des données arrivent au fil du temps. Lors de la diffusion en continu, chaque fragment est envoyé au client ou serveur dès qu’il est disponible, au lieu d’attendre à toutes les données soient disponibles.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur. Cela est utile pour les scénarios où les fragments des données arrivent au fil du temps. Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.

::: moniker-end

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Configurer un concentrateur de diffusion en continu

::: moniker range=">= aspnetcore-3.0"

Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu quand elle retourne <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, ou `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un <xref:System.Threading.Channels.ChannelReader%601> ou un `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Serveur-client de diffusion en continu

::: moniker range=">= aspnetcore-3.0"

Méthodes de concentrateur de diffusion en continu peut retourner `IAsyncEnumerable<T>` outre `ChannelReader<T>`. La façon la plus simple pour retourner `IAsyncEnumerable<T>` consiste à rendre la méthode de concentrateur une méthode d’itérateur async, comme le montre l’exemple suivant. Méthodes d’itérateur Hub async peuvent accepter un `CancellationToken` paramètre qui est déclenchée quand le client annule son abonnement à partir du flux. Les méthodes Async itérateur éviter les problèmes courants avec des canaux, comme le retour ne pas le `ChannelReader` suffisamment tôt ou de la sortie de la méthode sans terminer la <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

L’exemple suivant montre les principes de base de données au client à l’aide de canaux de diffusion en continu. Chaque fois qu’un objet est écrit dans le <xref:System.Threading.Channels.ChannelWriter%601>, l’objet est immédiatement envoyée au client. À la fin, le `ChannelWriter` est terminé pour indiquer au client que le flux est fermé.

> [!NOTE]
> Écrivez dans le `ChannelWriter<T>` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible. Autres appels de concentrateur sont bloqués jusqu'à ce qu’un `ChannelReader` est retourné.
>
> Encapsuler la logique dans un `try ... catch`. Terminer la `Channel` dans le `catch` et en dehors du `catch` s’assurer que le hub d’appel de méthode est s’est déroulée correctement.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Méthodes de concentrateur de diffusion en continu de client-serveur peuvent accepter un `CancellationToken` paramètre qui est déclenchée quand le client annule son abonnement à partir du flux. Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Client-serveur de diffusion en continu

Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu client-serveur lorsqu’il accepte un ou plusieurs objets de type <xref:System.Threading.Channels.ChannelReader%601> ou <xref:System.Collections.Generic.IAsyncEnumerable%601>. L’exemple suivant montre les principes fondamentaux de la lecture des données de diffusion en continu envoyées depuis le client. Chaque fois que le client écrit dans le <xref:System.Threading.Channels.ChannelWriter%601>, les données sont écrites dans le `ChannelReader` sur le serveur lit à partir de laquelle la méthode de concentrateur.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Un <xref:System.Collections.Generic.IAsyncEnumerable%601> suit de la version de la méthode.

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<Stream> stream) 
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a>Client .NET

### <a name="server-to-client-streaming"></a>Serveur-client de diffusion en continu


::: moniker range=">= aspnetcore-3.0"

Le `StreamAsync` et `StreamAsChannelAsync` méthodes sur `HubConnection` sont utilisés pour appeler les méthodes de diffusion en continu de client-serveur. Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsync` ou `StreamAsChannelAsync`. Le paramètre générique sur `StreamAsync<T>` et `StreamAsChannelAsync<T>` Spécifie le type d’objets retournés par la méthode de diffusion en continu. Un objet de type `IAsyncEnumerable<T>` ou `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.

Un `StreamAsync` exemple retourne `IAsyncEnumerable<int>`:

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

Correspondante `StreamAsChannelAsync` exemple retourne `ChannelReader<int>`:

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu du client-serveur. Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`. Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu. Un `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu du client-serveur. Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`. Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu. Un `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Client-serveur de diffusion en continu

Il existe deux façons d’appeler une méthode de concentrateur client-serveur diffusion en continu à partir du client .NET. Vous pouvez soit passer un `IAsyncEnumerable<T>` ou un `ChannelReader` en tant qu’argument à `SendAsync`, `InvokeAsync`, ou `StreamAsChannelAsync`, selon la méthode de concentrateur appelée.

Chaque fois que les données sont écrites dans le `IAsyncEnumerable` ou `ChannelWriter` de l’objet, la méthode de concentrateur sur le serveur reçoit un nouvel élément avec les données du client.

Si vous utilisez un `IAsyncEnumerable` de l’objet, le flux de données se termine après la méthode retournant les éléments de flux se ferme.

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

Ou si vous utilisez un `ChannelWriter`, vous terminez le canal avec `channel.Writer.Complete()`:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Client JavaScript

### <a name="server-to-client-streaming"></a>Serveur-client de diffusion en continu

Les clients JavaScript appellent des méthodes de diffusion en continu de client-serveur sur les hubs avec `connection.stream`. La méthode `stream` accepte deux arguments :

* Le nom de la méthode de hub. Dans l’exemple suivant, le nom de méthode de hub est `Counter`.
* Les arguments définis dans la méthode de hub. Dans l’exemple suivant, les arguments sont un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.

`connection.stream` Retourne un `IStreamResult`, qui contient un `subscribe` (méthode). Passer un `IStreamSubscriber` à `subscribe` et définissez le `next`, `error`, et `complete` rappels pour recevoir des notifications à partir de la `stream` invocation.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode). Appel de cette méthode provoque l’annulation de la `CancellationToken` paramètre de la méthode de concentrateur, si vous avez fourni un.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Client-serveur de diffusion en continu

Les clients JavaScript appellent des méthodes de diffusion en continu de client-serveur sur les hubs en passant un `Subject` en tant qu’argument à `send`, `invoke`, ou `stream`, selon la méthode de concentrateur appelée. Le `Subject` est une classe qui se présente comme un `Subject`. Par exemple dans RxJS, vous pouvez utiliser la [sujet](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) classe à partir de cette bibliothèque.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

L’appel `subject.next(item)` avec un élément écrit l’élément dans le flux, et la méthode de concentrateur reçoit l’élément sur le serveur.

Pour terminer le flux de données, appelez `subject.complete()`.

## <a name="java-client"></a>Client Java

### <a name="server-to-client-streaming"></a>Serveur-client de diffusion en continu

Le client SignalR Java utilise le `stream` méthode à appeler les méthodes de diffusion en continu. `stream` accepte trois arguments ou plus :

* Le type attendu des éléments de flux de données.
* Le nom de la méthode de hub.
* Les arguments définis dans la méthode de hub.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

Le `stream` méthode sur `HubConnection` retourne un Observable du type d’élément de flux de données. Le type Observable `subscribe` méthode est where `onNext`, `onError` et `onCompleted` gestionnaires sont définis.

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
