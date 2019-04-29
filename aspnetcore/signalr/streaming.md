---
title: Utiliser la diffusion en continu dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment diffuser des données entre le client et le serveur.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: d185056d3bdda089eaa46ae9b8e13ab7a4354f93
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165074"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="43144-103">Utiliser la diffusion en continu dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="43144-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="43144-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="43144-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43144-105">ASP.NET Core SignalR prend en charge la diffusion en continu du client au serveur et du serveur au client.</span><span class="sxs-lookup"><span data-stu-id="43144-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="43144-106">Cela est utile pour les scénarios où les fragments des données arrivent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="43144-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="43144-107">Lors de la diffusion en continu, chaque fragment est envoyé au client ou serveur dès qu’il est disponible, au lieu d’attendre à toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="43144-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43144-108">ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="43144-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="43144-109">Cela est utile pour les scénarios où les fragments des données arrivent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="43144-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="43144-110">Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="43144-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="43144-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43144-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="43144-112">Configurer un concentrateur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43144-113">Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu quand elle retourne un <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, ou `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="43144-113">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="43144-114">Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un <xref:System.Threading.Channels.ChannelReader%601> ou un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="43144-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="43144-115">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43144-116">Méthodes de concentrateur de diffusion en continu peut retourner `IAsyncEnumerable<T>` outre `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="43144-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="43144-117">La façon la plus simple pour retourner `IAsyncEnumerable<T>` consiste à rendre la méthode de concentrateur une méthode d’itérateur async, comme le montre l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="43144-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="43144-118">Méthodes d’itérateur Hub async peuvent accepter un `CancellationToken` paramètre qui est déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="43144-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="43144-119">Les méthodes Async itérateur éviter les problèmes courants avec des canaux, comme le retour ne pas le `ChannelReader` suffisamment tôt ou de la sortie de la méthode sans terminer la <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="43144-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="43144-120">L’exemple suivant montre les principes de base de données au client à l’aide de canaux de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="43144-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="43144-121">Chaque fois qu’un objet est écrit dans le <xref:System.Threading.Channels.ChannelWriter%601>, l’objet est immédiatement envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="43144-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="43144-122">À la fin, le `ChannelWriter` est terminé pour indiquer au client que le flux est fermé.</span><span class="sxs-lookup"><span data-stu-id="43144-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="43144-123">Écrivez dans le `ChannelWriter<T>` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible.</span><span class="sxs-lookup"><span data-stu-id="43144-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="43144-124">Autres appels de concentrateur sont bloqués jusqu'à ce qu’un `ChannelReader` est retourné.</span><span class="sxs-lookup"><span data-stu-id="43144-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="43144-125">Encapsuler la logique dans un `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="43144-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="43144-126">Terminer la `Channel` dans le `catch` et en dehors du `catch` s’assurer que le hub d’appel de méthode est s’est déroulée correctement.</span><span class="sxs-lookup"><span data-stu-id="43144-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="43144-127">Méthodes de concentrateur de diffusion en continu de client-serveur peuvent accepter un `CancellationToken` paramètre qui est déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="43144-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="43144-128">Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.</span><span class="sxs-lookup"><span data-stu-id="43144-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="43144-129">Client-serveur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-129">Client-to-server streaming</span></span>

<span data-ttu-id="43144-130">Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu client-serveur lorsqu’il accepte un ou plusieurs <xref:System.Threading.Channels.ChannelReader`1>s.</span><span class="sxs-lookup"><span data-stu-id="43144-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more <xref:System.Threading.Channels.ChannelReader`1>s.</span></span> <span data-ttu-id="43144-131">L’exemple suivant montre les principes fondamentaux de la lecture des données de diffusion en continu envoyées depuis le client.</span><span class="sxs-lookup"><span data-stu-id="43144-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="43144-132">Chaque fois que le client écrit dans le <xref:System.Threading.Channels.ChannelWriter`1>, les données sont écrites dans le `ChannelReader` sur le serveur qui lit à partir de la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="43144-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter`1>, the data is written into the `ChannelReader` on the server that the hub method is reading from.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="43144-133">Client .NET</span><span class="sxs-lookup"><span data-stu-id="43144-133">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="43144-134">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-134">Server-to-client streaming</span></span>

<span data-ttu-id="43144-135">Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu du client-serveur.</span><span class="sxs-lookup"><span data-stu-id="43144-135">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="43144-136">Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="43144-136">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="43144-137">Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="43144-137">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="43144-138">Un `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.</span><span class="sxs-lookup"><span data-stu-id="43144-138">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="43144-139">Client-serveur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-139">Client-to-server streaming</span></span>

<span data-ttu-id="43144-140">Pour appeler une méthode de concentrateur client-serveur diffusion en continu à partir du client .NET, créez un `Channel` et passer le `ChannelReader` en tant qu’argument à `SendAsync`, `InvokeAsync`, ou `StreamAsChannelAsync`, selon la méthode de concentrateur appelée.</span><span class="sxs-lookup"><span data-stu-id="43144-140">To invoke a client-to-server streaming hub method from the .NET client, create a `Channel` and pass the `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="43144-141">Chaque fois que les données sont écrites dans le `ChannelWriter`, la méthode de concentrateur sur le serveur reçoit un nouvel élément avec les données du client.</span><span class="sxs-lookup"><span data-stu-id="43144-141">Whenever data is written to the `ChannelWriter`, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="43144-142">Pour terminer le flux de données, terminez le canal avec `channel.Writer.Complete()`.</span><span class="sxs-lookup"><span data-stu-id="43144-142">To end the stream, complete the channel with `channel.Writer.Complete()`.</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="43144-143">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="43144-143">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="43144-144">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-144">Server-to-client streaming</span></span>

<span data-ttu-id="43144-145">Les clients JavaScript appellent des méthodes de diffusion en continu de client-serveur sur les hubs avec `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="43144-145">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="43144-146">La méthode `stream` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="43144-146">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="43144-147">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="43144-147">The name of the hub method.</span></span> <span data-ttu-id="43144-148">Dans l’exemple suivant, le nom de méthode de hub est `Counter`.</span><span class="sxs-lookup"><span data-stu-id="43144-148">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="43144-149">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="43144-149">Arguments defined in the hub method.</span></span> <span data-ttu-id="43144-150">Dans l’exemple suivant, les arguments sont un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="43144-150">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="43144-151">`connection.stream` Retourne un `IStreamResult`, qui contient un `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="43144-151">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="43144-152">Passer un `IStreamSubscriber` à `subscribe` et définissez le `next`, `error`, et `complete` rappels pour recevoir des notifications à partir de la `stream` invocation.</span><span class="sxs-lookup"><span data-stu-id="43144-152">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="43144-153">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="43144-153">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="43144-154">Appel de cette méthode provoque l’annulation de la `CancellationToken` paramètre de la méthode de concentrateur, si vous avez fourni un.</span><span class="sxs-lookup"><span data-stu-id="43144-154">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="43144-155">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="43144-155">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="43144-156">Client-serveur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-156">Client-to-server streaming</span></span>

<span data-ttu-id="43144-157">Les clients JavaScript appellent des méthodes de diffusion en continu de client-serveur sur les hubs en passant un `Subject` en tant qu’argument à `send`, `invoke`, ou `stream`, selon la méthode de concentrateur appelée.</span><span class="sxs-lookup"><span data-stu-id="43144-157">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="43144-158">Le `Subject` est une classe qui se présente comme un `Subject`.</span><span class="sxs-lookup"><span data-stu-id="43144-158">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="43144-159">Par exemple dans RxJS, vous pouvez utiliser la [sujet](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) classe à partir de cette bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="43144-159">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="43144-160">L’appel `subject.next(item)` avec un élément écrit l’élément dans le flux, et la méthode de concentrateur reçoit l’élément sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="43144-160">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="43144-161">Pour terminer le flux de données, appelez `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="43144-161">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="43144-162">Client Java</span><span class="sxs-lookup"><span data-stu-id="43144-162">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="43144-163">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="43144-163">Server-to-client streaming</span></span>

<span data-ttu-id="43144-164">Le client SignalR Java utilise le `stream` méthode à appeler les méthodes de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="43144-164">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="43144-165">`stream` accepte trois arguments ou plus :</span><span class="sxs-lookup"><span data-stu-id="43144-165">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="43144-166">Le type attendu des éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="43144-166">The expected type of the stream items.</span></span>
* <span data-ttu-id="43144-167">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="43144-167">The name of the hub method.</span></span>
* <span data-ttu-id="43144-168">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="43144-168">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="43144-169">Le `stream` méthode sur `HubConnection` retourne un Observable du type d’élément de flux de données.</span><span class="sxs-lookup"><span data-stu-id="43144-169">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="43144-170">Le type Observable `subscribe` méthode est where `onNext`, `onError` et `onCompleted` gestionnaires sont définis.</span><span class="sxs-lookup"><span data-stu-id="43144-170">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="43144-171">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="43144-171">Additional resources</span></span>

* [<span data-ttu-id="43144-172">Hubs</span><span class="sxs-lookup"><span data-stu-id="43144-172">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="43144-173">Client .NET</span><span class="sxs-lookup"><span data-stu-id="43144-173">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="43144-174">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="43144-174">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="43144-175">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="43144-175">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
