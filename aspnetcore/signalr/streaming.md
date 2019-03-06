---
title: Utiliser la diffusion en continu dans ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment retourner des flux de valeurs à partir de méthodes de concentrateur de serveur et de consommer les flux à l’aide les clients .NET et JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: fb7183f7189d62c181f69ffdb170e3da25612919
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345585"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="6bd76-103">Utiliser la diffusion en continu dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6bd76-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="6bd76-104">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="6bd76-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="6bd76-105">ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="6bd76-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="6bd76-106">Cela est utile pour les scénarios où les fragments de données arriveront au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="6bd76-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="6bd76-107">Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="6bd76-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="6bd76-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6bd76-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="6bd76-109">Configurer le hub</span><span class="sxs-lookup"><span data-stu-id="6bd76-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6bd76-110">Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu quand elle retourne un `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, ou `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6bd76-111">Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un `ChannelReader<T>` ou un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6bd76-112">Dans ASP.NET Core 3.0 ou version ultérieure, la diffusion en continu des méthodes de concentrateur peut retourner `IAsyncEnumerable<T>` outre `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="6bd76-113">La façon la plus simple pour retourner `IAsyncEnumerable<T>` consiste à rendre la méthode de concentrateur une méthode d’itérateur async, comme le montre l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="6bd76-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="6bd76-114">Méthodes d’itérateur Hub async peuvent accepter un `CancellationToken` paramètre qui sera déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="6bd76-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="6bd76-115">Les méthodes Async itérateur facilement évitent des problèmes courants avec les canaux comme le retour ne pas le `ChannelReader` suffisamment tôt ou de la sortie de la méthode sans terminer la `ChannelWriter`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="6bd76-116">L’exemple suivant montre les principes de base de données au client à l’aide de canaux de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="6bd76-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="6bd76-117">Chaque fois qu’un objet est écrit dans le `ChannelWriter` cet objet est immédiatement envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="6bd76-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="6bd76-118">À la fin, le `ChannelWriter` est terminé pour indiquer au client que le flux est fermé.</span><span class="sxs-lookup"><span data-stu-id="6bd76-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="6bd76-119">Écrivez dans le `ChannelWriter` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible.</span><span class="sxs-lookup"><span data-stu-id="6bd76-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="6bd76-120">Les autres appels du hub seront bloqués jusqu'à ce qu'un `ChannelReader` soit retourné.</span><span class="sxs-lookup"><span data-stu-id="6bd76-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="6bd76-121">Encapsuler votre logique dans un `try ... catch` et terminez le `Channel` dans la capture et à l’extérieur catch s’assurer que le hub d’appel de méthode est terminée correctement.</span><span class="sxs-lookup"><span data-stu-id="6bd76-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="6bd76-122">Dans ASP.NET Core 2.2 ou version ultérieure, la diffusion en continu des méthodes de concentrateur peut accepter un `CancellationToken` paramètre qui sera déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="6bd76-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="6bd76-123">Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.</span><span class="sxs-lookup"><span data-stu-id="6bd76-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="6bd76-124">Client .NET</span><span class="sxs-lookup"><span data-stu-id="6bd76-124">.NET client</span></span>

<span data-ttu-id="6bd76-125">La méthode `StreamAsChannelAsync` sur `HubConnection` est utilisée pour appeler une méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="6bd76-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="6bd76-126">Passez le nom de la méthode hub et les arguments définis dans la méthode de hub pour `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="6bd76-127">Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="6bd76-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="6bd76-128">Un `ChannelReader<T>` est retourné à partir de l’appel de flux de données et représente le flux sur le client.</span><span class="sxs-lookup"><span data-stu-id="6bd76-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="6bd76-129">Pour lire les données, il est courant d’utiliser une boucle sur `WaitToReadAsync` et d'appeler `TryRead` lorsque les données sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="6bd76-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="6bd76-130">La boucle s’arrête lorsque le flux a été fermé par le serveur ou lorsque le jeton d’annulation passé à `StreamAsChannelAsync` est annulé.</span><span class="sxs-lookup"><span data-stu-id="6bd76-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="6bd76-131">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bd76-131">JavaScript client</span></span>

<span data-ttu-id="6bd76-132">Les clients JavaScript appellent des méthodes de diffusion en continu sur les hubs à l’aide de `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="6bd76-133">La méthode `stream` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="6bd76-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="6bd76-134">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="6bd76-134">The name of the hub method.</span></span> <span data-ttu-id="6bd76-135">Dans l’exemple suivant, le nom de méthode de hub est `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="6bd76-136">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="6bd76-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="6bd76-137">Dans l’exemple suivant, les arguments sont : un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="6bd76-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="6bd76-138">`connection.stream` retourne un `IStreamResult` qui contient une méthode `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="6bd76-139">Passez un `IStreamSubscriber` à `subscribe` et définissez les callbacks `next`, `error`, et `complete` pour obtenir des notifications à partir de l'invocation de `stream`.</span><span class="sxs-lookup"><span data-stu-id="6bd76-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="6bd76-140">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6bd76-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6bd76-141">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6bd76-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="6bd76-142">Appeler cette méthode provoque la `CancellationToken` paramètre de la méthode de concentrateur (si vous avez fourni une) doit être annulée.</span><span class="sxs-lookup"><span data-stu-id="6bd76-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a><span data-ttu-id="6bd76-143">Client Java</span><span class="sxs-lookup"><span data-stu-id="6bd76-143">Java client</span></span>
<span data-ttu-id="6bd76-144">Le client SignalR Java utilise le `stream` méthode à appeler les méthodes de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="6bd76-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="6bd76-145">Elle accepte trois arguments ou plus :</span><span class="sxs-lookup"><span data-stu-id="6bd76-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="6bd76-146">Le type des éléments de flux de données attendu</span><span class="sxs-lookup"><span data-stu-id="6bd76-146">The expected type of the stream items</span></span> 
* <span data-ttu-id="6bd76-147">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="6bd76-147">The name of the hub method.</span></span>
* <span data-ttu-id="6bd76-148">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="6bd76-148">Arguments defined in the hub method.</span></span> 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
<span data-ttu-id="6bd76-149">Le `stream` méthode sur `HubConnection` retourne un Observable du type d’élément de flux de données.</span><span class="sxs-lookup"><span data-stu-id="6bd76-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="6bd76-150">Le type Observable `subscribe` méthode est l’emplacement où vous définissez votre `onNext`, `onError` et `onCompleted` gestionnaires.</span><span class="sxs-lookup"><span data-stu-id="6bd76-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="6bd76-151">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="6bd76-151">Related resources</span></span>

* [<span data-ttu-id="6bd76-152">Hubs</span><span class="sxs-lookup"><span data-stu-id="6bd76-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6bd76-153">Client .NET</span><span class="sxs-lookup"><span data-stu-id="6bd76-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="6bd76-154">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bd76-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="6bd76-155">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="6bd76-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
