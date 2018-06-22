---
title: Utilisez la diffusion en continu dans ASP.NET Core SignalR
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: ae0e733dddfb48db07d77ea73f4673cf8f783b88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275848"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="13736-102">Utilisez la diffusion en continu dans ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="13736-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="13736-103">Par [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="13736-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="13736-104">ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="13736-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="13736-105">Cela est utile pour les scénarios où des fragments de données entrent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="13736-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="13736-106">Lorsqu’une valeur de retour est transmise au client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente pour toutes les données soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="13736-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="13736-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="13736-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="13736-108">Configurer le concentrateur</span><span class="sxs-lookup"><span data-stu-id="13736-108">Set up the hub</span></span>

<span data-ttu-id="13736-109">Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu lorsqu’elle retourne un `ChannelReader<T>` ou `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="13736-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="13736-110">Voici un exemple qui illustre les principes de base de données au client de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="13736-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="13736-111">Chaque fois qu’un objet est écrit dans le `ChannelReader` de cet objet est immédiatement envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="13736-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="13736-112">À la fin, le `ChannelReader` soit terminée pour indiquer au client le flux est fermé.</span><span class="sxs-lookup"><span data-stu-id="13736-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="13736-113">Écrire dans le `ChannelReader` sur un thread d’arrière-plan et de retourner le `ChannelReader` dès que possible.</span><span class="sxs-lookup"><span data-stu-id="13736-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="13736-114">Autres appels de concentrateur seront bloquées jusqu'à ce qu’une `ChannelReader` est retourné.</span><span class="sxs-lookup"><span data-stu-id="13736-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="13736-115">Client .NET</span><span class="sxs-lookup"><span data-stu-id="13736-115">.NET client</span></span>

<span data-ttu-id="13736-116">Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="13736-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="13736-117">Passez le nom de méthode de concentrateur et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="13736-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="13736-118">Le paramètre générique sur `StreamAsChannelAsync<T>` Spécifie le type d’objets retournés par la méthode de diffusion en continu.</span><span class="sxs-lookup"><span data-stu-id="13736-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="13736-119">A `ChannelReader<T>` est retourné à partir de l’appel de flux de données et représente le flux de données sur le client.</span><span class="sxs-lookup"><span data-stu-id="13736-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="13736-120">Pour lire les données, il est courant d’une boucle sur `WaitToReadAsync` et appelez `TryRead` lorsque les données sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="13736-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="13736-121">La boucle s’arrête lorsque le flux de données a été fermée par le serveur, ou le jeton d’annulation passé à `StreamAsChannelAsync` est annulée.</span><span class="sxs-lookup"><span data-stu-id="13736-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a><span data-ttu-id="13736-122">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="13736-122">JavaScript client</span></span>

<span data-ttu-id="13736-123">Les clients JavaScript appellent des méthodes de diffusion en continu sur les concentrateurs à l’aide de `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="13736-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="13736-124">Le `stream` méthode accepte deux arguments :</span><span class="sxs-lookup"><span data-stu-id="13736-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="13736-125">Le nom de la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="13736-125">The name of the hub method.</span></span> <span data-ttu-id="13736-126">Dans l’exemple suivant, le nom de la méthode de concentrateur est `Counter`.</span><span class="sxs-lookup"><span data-stu-id="13736-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="13736-127">Arguments définis dans la méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="13736-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="13736-128">Dans l’exemple suivant, les arguments sont : un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="13736-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="13736-129">`connection.stream` Retourne un `IStreamResult` qui contient un `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="13736-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="13736-130">Passer un `IStreamSubscriber` à `subscribe` et définir le `next`, `error`, et `complete` rappels pour obtenir des notifications à partir de la `stream` invocation.</span><span class="sxs-lookup"><span data-stu-id="13736-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="13736-131">À la fin du flux à partir de l’appel du client le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).</span><span class="sxs-lookup"><span data-stu-id="13736-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="13736-132">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="13736-132">Related resources</span></span>

* [<span data-ttu-id="13736-133">Hubs</span><span class="sxs-lookup"><span data-stu-id="13736-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="13736-134">Client .NET</span><span class="sxs-lookup"><span data-stu-id="13736-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="13736-135">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="13736-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="13736-136">Publier sur Azure</span><span class="sxs-lookup"><span data-stu-id="13736-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)