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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="881e5-103">Utiliser la diffusion en continu dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="881e5-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="881e5-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="881e5-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="881e5-105">ASP.NET Core SignalR prend en charge la diffusion en continu du client au serveur et du serveur au client.</span><span class="sxs-lookup"><span data-stu-id="881e5-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="881e5-106">Cela est utile pour les scénarios où les fragments des données arrivent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="881e5-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="881e5-107">Lors de la diffusion en continu, chaque fragment est envoyé au client ou serveur dès qu’il est disponible, au lieu d’attendre à toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="881e5-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="881e5-108">ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="881e5-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="881e5-109">Cela est utile pour les scénarios où les fragments des données arrivent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="881e5-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="881e5-110">Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="881e5-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="881e5-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="881e5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="881e5-112">Configurer un concentrateur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="881e5-113">Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu quand elle retourne <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, ou `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="881e5-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="881e5-114">Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un <xref:System.Threading.Channels.ChannelReader%601> ou un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="881e5-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="881e5-115">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="881e5-116">Méthodes de concentrateur de diffusion en continu peut retourner `IAsyncEnumerable<T>` outre `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="881e5-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="881e5-117">La façon la plus simple pour retourner `IAsyncEnumerable<T>` consiste à rendre la méthode de concentrateur une méthode d’itérateur async, comme le montre l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="881e5-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="881e5-118">Méthodes d’itérateur Hub async peuvent accepter un `CancellationToken` paramètre qui est déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="881e5-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="881e5-119">Les méthodes Async itérateur éviter les problèmes courants avec des canaux, comme le retour ne pas le `ChannelReader` suffisamment tôt ou de la sortie de la méthode sans terminer la <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="881e5-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="881e5-120">L’exemple suivant montre les principes de base de données au client à l’aide de canaux de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="881e5-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="881e5-121">Chaque fois qu’un objet est écrit dans le <xref:System.Threading.Channels.ChannelWriter%601>, l’objet est immédiatement envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="881e5-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="881e5-122">À la fin, le `ChannelWriter` est terminé pour indiquer au client que le flux est fermé.</span><span class="sxs-lookup"><span data-stu-id="881e5-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="881e5-123">Écrivez dans le `ChannelWriter<T>` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible.</span><span class="sxs-lookup"><span data-stu-id="881e5-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="881e5-124">Autres appels de concentrateur sont bloqués jusqu'à ce qu’un `ChannelReader` est retourné.</span><span class="sxs-lookup"><span data-stu-id="881e5-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="881e5-125">Encapsuler la logique dans un `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="881e5-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="881e5-126">Terminer la `Channel` dans le `catch` et en dehors du `catch` s’assurer que le hub d’appel de méthode est s’est déroulée correctement.</span><span class="sxs-lookup"><span data-stu-id="881e5-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="881e5-127">Méthodes de concentrateur de diffusion en continu de client-serveur peuvent accepter un `CancellationToken` paramètre qui est déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="881e5-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="881e5-128">Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.</span><span class="sxs-lookup"><span data-stu-id="881e5-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="881e5-129">Client-serveur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-129">Client-to-server streaming</span></span>

<span data-ttu-id="881e5-130">Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu client-serveur lorsqu’il accepte un ou plusieurs objets de type <xref:System.Threading.Channels.ChannelReader%601> ou <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="881e5-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="881e5-131">L’exemple suivant montre les principes fondamentaux de la lecture des données de diffusion en continu envoyées depuis le client.</span><span class="sxs-lookup"><span data-stu-id="881e5-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="881e5-132">Chaque fois que le client écrit dans le <xref:System.Threading.Channels.ChannelWriter%601>, les données sont écrites dans le `ChannelReader` sur le serveur lit à partir de laquelle la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="881e5-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="881e5-133">Un <xref:System.Collections.Generic.IAsyncEnumerable%601> suit de la version de la méthode.</span><span class="sxs-lookup"><span data-stu-id="881e5-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

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

## <a name="net-client"></a><span data-ttu-id="881e5-134">Client .NET</span><span class="sxs-lookup"><span data-stu-id="881e5-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="881e5-135">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="881e5-136">Le `StreamAsync` et `StreamAsChannelAsync` méthodes sur `HubConnection` sont utilisés pour appeler les méthodes de diffusion en continu de client-serveur.</span><span class="sxs-lookup"><span data-stu-id="881e5-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="881e5-137">Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsync` ou `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="881e5-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="881e5-138">Le paramètre générique sur `StreamAsync<T>` et `StreamAsChannelAsync<T>` Spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="881e5-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="881e5-139">Un objet de type `IAsyncEnumerable<T>` ou `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.</span><span class="sxs-lookup"><span data-stu-id="881e5-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="881e5-140">Un `StreamAsync` exemple retourne `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="881e5-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

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

<span data-ttu-id="881e5-141">Correspondante `StreamAsChannelAsync` exemple retourne `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="881e5-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

<span data-ttu-id="881e5-142">Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu du client-serveur.</span><span class="sxs-lookup"><span data-stu-id="881e5-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="881e5-143">Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="881e5-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="881e5-144">Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="881e5-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="881e5-145">Un `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.</span><span class="sxs-lookup"><span data-stu-id="881e5-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="881e5-146">Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu du client-serveur.</span><span class="sxs-lookup"><span data-stu-id="881e5-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="881e5-147">Passez le nom de méthode de hub et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="881e5-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="881e5-148">Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="881e5-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="881e5-149">Un `ChannelReader<T>` est retourné à partir de l’appel de flux et représente le flux sur le client.</span><span class="sxs-lookup"><span data-stu-id="881e5-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="881e5-150">Client-serveur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-150">Client-to-server streaming</span></span>

<span data-ttu-id="881e5-151">Il existe deux façons d’appeler une méthode de concentrateur client-serveur diffusion en continu à partir du client .NET.</span><span class="sxs-lookup"><span data-stu-id="881e5-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="881e5-152">Vous pouvez soit passer un `IAsyncEnumerable<T>` ou un `ChannelReader` en tant qu’argument à `SendAsync`, `InvokeAsync`, ou `StreamAsChannelAsync`, selon la méthode de concentrateur appelée.</span><span class="sxs-lookup"><span data-stu-id="881e5-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="881e5-153">Chaque fois que les données sont écrites dans le `IAsyncEnumerable` ou `ChannelWriter` de l’objet, la méthode de concentrateur sur le serveur reçoit un nouvel élément avec les données du client.</span><span class="sxs-lookup"><span data-stu-id="881e5-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="881e5-154">Si vous utilisez un `IAsyncEnumerable` de l’objet, le flux de données se termine après la méthode retournant les éléments de flux se ferme.</span><span class="sxs-lookup"><span data-stu-id="881e5-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

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

<span data-ttu-id="881e5-155">Ou si vous utilisez un `ChannelWriter`, vous terminez le canal avec `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="881e5-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="881e5-156">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="881e5-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="881e5-157">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-157">Server-to-client streaming</span></span>

<span data-ttu-id="881e5-158">Les clients JavaScript appellent des méthodes de diffusion en continu de client-serveur sur les hubs avec `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="881e5-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="881e5-159">La méthode `stream` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="881e5-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="881e5-160">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="881e5-160">The name of the hub method.</span></span> <span data-ttu-id="881e5-161">Dans l’exemple suivant, le nom de méthode de hub est `Counter`.</span><span class="sxs-lookup"><span data-stu-id="881e5-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="881e5-162">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="881e5-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="881e5-163">Dans l’exemple suivant, les arguments sont un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="881e5-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="881e5-164">`connection.stream` Retourne un `IStreamResult`, qui contient un `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="881e5-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="881e5-165">Passer un `IStreamSubscriber` à `subscribe` et définissez le `next`, `error`, et `complete` rappels pour recevoir des notifications à partir de la `stream` invocation.</span><span class="sxs-lookup"><span data-stu-id="881e5-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="881e5-166">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="881e5-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="881e5-167">Appel de cette méthode provoque l’annulation de la `CancellationToken` paramètre de la méthode de concentrateur, si vous avez fourni un.</span><span class="sxs-lookup"><span data-stu-id="881e5-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="881e5-168">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="881e5-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="881e5-169">Client-serveur de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-169">Client-to-server streaming</span></span>

<span data-ttu-id="881e5-170">Les clients JavaScript appellent des méthodes de diffusion en continu de client-serveur sur les hubs en passant un `Subject` en tant qu’argument à `send`, `invoke`, ou `stream`, selon la méthode de concentrateur appelée.</span><span class="sxs-lookup"><span data-stu-id="881e5-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="881e5-171">Le `Subject` est une classe qui se présente comme un `Subject`.</span><span class="sxs-lookup"><span data-stu-id="881e5-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="881e5-172">Par exemple dans RxJS, vous pouvez utiliser la [sujet](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) classe à partir de cette bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="881e5-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="881e5-173">L’appel `subject.next(item)` avec un élément écrit l’élément dans le flux, et la méthode de concentrateur reçoit l’élément sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="881e5-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="881e5-174">Pour terminer le flux de données, appelez `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="881e5-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="881e5-175">Client Java</span><span class="sxs-lookup"><span data-stu-id="881e5-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="881e5-176">Serveur-client de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="881e5-176">Server-to-client streaming</span></span>

<span data-ttu-id="881e5-177">Le client SignalR Java utilise le `stream` méthode à appeler les méthodes de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="881e5-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="881e5-178">`stream` accepte trois arguments ou plus :</span><span class="sxs-lookup"><span data-stu-id="881e5-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="881e5-179">Le type attendu des éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="881e5-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="881e5-180">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="881e5-180">The name of the hub method.</span></span>
* <span data-ttu-id="881e5-181">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="881e5-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="881e5-182">Le `stream` méthode sur `HubConnection` retourne un Observable du type d’élément de flux de données.</span><span class="sxs-lookup"><span data-stu-id="881e5-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="881e5-183">Le type Observable `subscribe` méthode est where `onNext`, `onError` et `onCompleted` gestionnaires sont définis.</span><span class="sxs-lookup"><span data-stu-id="881e5-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="881e5-184">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="881e5-184">Additional resources</span></span>

* [<span data-ttu-id="881e5-185">Hubs</span><span class="sxs-lookup"><span data-stu-id="881e5-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="881e5-186">Client .NET</span><span class="sxs-lookup"><span data-stu-id="881e5-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="881e5-187">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="881e5-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="881e5-188">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="881e5-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
