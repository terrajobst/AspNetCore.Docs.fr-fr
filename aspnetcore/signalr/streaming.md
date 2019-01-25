---
title: Utiliser la diffusion en continu dans ASP.NET Core SignalR
author: bradygaster
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: ade2d6fb6e799d53ff3aaa69c641d0088acdee95
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837401"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="b41ca-102">Utiliser la diffusion en continu dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b41ca-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="b41ca-103">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="b41ca-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="b41ca-104">ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="b41ca-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="b41ca-105">Cela est utile pour les scénarios où les fragments de données arriveront au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="b41ca-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="b41ca-106">Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="b41ca-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="b41ca-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b41ca-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="b41ca-108">Configurer le hub</span><span class="sxs-lookup"><span data-stu-id="b41ca-108">Set up the hub</span></span>

<span data-ttu-id="b41ca-109">Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un `ChannelReader<T>` ou un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="b41ca-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="b41ca-110">Voici un exemple qui montre les principes de diffusion en continu vers le client.</span><span class="sxs-lookup"><span data-stu-id="b41ca-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="b41ca-111">Chaque fois qu’un objet est écrit dans le `ChannelReader` cet objet est immédiatement envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="b41ca-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="b41ca-112">À la fin, le `ChannelReader` est terminé pour indiquer au client que le flux est fermé.</span><span class="sxs-lookup"><span data-stu-id="b41ca-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="b41ca-113">Écrivez dans le `ChannelReader` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible.</span><span class="sxs-lookup"><span data-stu-id="b41ca-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="b41ca-114">Les autres appels du hub seront bloqués jusqu'à ce qu'un `ChannelReader` soit retourné.</span><span class="sxs-lookup"><span data-stu-id="b41ca-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="b41ca-115">Encapsuler votre logique dans un `try ... catch` et terminez le `Channel` dans la capture et à l’extérieur catch s’assurer que le hub d’appel de méthode est terminée correctement.</span><span class="sxs-lookup"><span data-stu-id="b41ca-115">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="b41ca-116">Dans ASP.NET Core 2.2 ou version ultérieure, la diffusion en continu des méthodes de concentrateur peut accepter un `CancellationToken` paramètre qui sera déclenchée quand le client annule son abonnement à partir du flux.</span><span class="sxs-lookup"><span data-stu-id="b41ca-116">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="b41ca-117">Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.</span><span class="sxs-lookup"><span data-stu-id="b41ca-117">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="b41ca-118">Client .NET</span><span class="sxs-lookup"><span data-stu-id="b41ca-118">.NET client</span></span>

<span data-ttu-id="b41ca-119">La méthode `StreamAsChannelAsync` sur `HubConnection` est utilisée pour appeler une méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="b41ca-119">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="b41ca-120">Passez le nom de la méthode hub et les arguments définis dans la méthode de hub pour `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="b41ca-120">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="b41ca-121">Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="b41ca-121">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="b41ca-122">Un `ChannelReader<T>` est retourné à partir de l’appel de flux de données et représente le flux sur le client.</span><span class="sxs-lookup"><span data-stu-id="b41ca-122">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="b41ca-123">Pour lire les données, il est courant d’utiliser une boucle sur `WaitToReadAsync` et d'appeler `TryRead` lorsque les données sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="b41ca-123">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="b41ca-124">La boucle s’arrête lorsque le flux a été fermé par le serveur ou lorsque le jeton d’annulation passé à `StreamAsChannelAsync` est annulé.</span><span class="sxs-lookup"><span data-stu-id="b41ca-124">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
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

## <a name="javascript-client"></a><span data-ttu-id="b41ca-125">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b41ca-125">JavaScript client</span></span>

<span data-ttu-id="b41ca-126">Les clients JavaScript appellent des méthodes de diffusion en continu sur les hubs à l’aide de `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="b41ca-126">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="b41ca-127">La méthode `stream` accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="b41ca-127">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="b41ca-128">Le nom de la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="b41ca-128">The name of the hub method.</span></span> <span data-ttu-id="b41ca-129">Dans l’exemple suivant, le nom de méthode de hub est `Counter`.</span><span class="sxs-lookup"><span data-stu-id="b41ca-129">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="b41ca-130">Les arguments définis dans la méthode de hub.</span><span class="sxs-lookup"><span data-stu-id="b41ca-130">Arguments defined in the hub method.</span></span> <span data-ttu-id="b41ca-131">Dans l’exemple suivant, les arguments sont : un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="b41ca-131">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="b41ca-132">`connection.stream` retourne un `IStreamResult` qui contient une méthode `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="b41ca-132">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="b41ca-133">Passez un `IStreamSubscriber` à `subscribe` et définissez les callbacks `next`, `error`, et `complete` pour obtenir des notifications à partir de l'invocation de `stream`.</span><span class="sxs-lookup"><span data-stu-id="b41ca-133">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="b41ca-134">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b41ca-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b41ca-135">Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b41ca-135">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="b41ca-136">Appeler cette méthode provoque la `CancellationToken` paramètre de la méthode de concentrateur (si vous avez fourni une) doit être annulée.</span><span class="sxs-lookup"><span data-stu-id="b41ca-136">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="b41ca-137">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="b41ca-137">Related resources</span></span>

* [<span data-ttu-id="b41ca-138">Hubs</span><span class="sxs-lookup"><span data-stu-id="b41ca-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b41ca-139">Client .NET</span><span class="sxs-lookup"><span data-stu-id="b41ca-139">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b41ca-140">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b41ca-140">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b41ca-141">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="b41ca-141">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
