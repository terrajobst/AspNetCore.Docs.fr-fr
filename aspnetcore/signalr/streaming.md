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
# <a name="use-streaming-in-aspnet-core-signalr"></a>Utiliser la diffusion en continu dans ASP.NET Core SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur. Cela est utile pour les scénarios où les fragments de données arriveront au fil du temps. Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurer le hub

::: moniker range=">= aspnetcore-3.0"

Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu quand elle retourne un `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, ou `Task<IAsyncEnumerable<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un `ChannelReader<T>` ou un `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Dans ASP.NET Core 3.0 ou version ultérieure, la diffusion en continu des méthodes de concentrateur peut retourner `IAsyncEnumerable<T>` outre `ChannelReader<T>`. La façon la plus simple pour retourner `IAsyncEnumerable<T>` consiste à rendre la méthode de concentrateur une méthode d’itérateur async, comme le montre l’exemple suivant. Méthodes d’itérateur Hub async peuvent accepter un `CancellationToken` paramètre qui sera déclenchée quand le client annule son abonnement à partir du flux. Les méthodes Async itérateur facilement évitent des problèmes courants avec les canaux comme le retour ne pas le `ChannelReader` suffisamment tôt ou de la sortie de la méthode sans terminer la `ChannelWriter`.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

L’exemple suivant montre les principes de base de données au client à l’aide de canaux de diffusion en continu. Chaque fois qu’un objet est écrit dans le `ChannelWriter` cet objet est immédiatement envoyé au client. À la fin, le `ChannelWriter` est terminé pour indiquer au client que le flux est fermé.

> [!NOTE]
> * Écrivez dans le `ChannelWriter` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible. Les autres appels du hub seront bloqués jusqu'à ce qu'un `ChannelReader` soit retourné.
> * Encapsuler votre logique dans un `try ... catch` et terminez le `Channel` dans la capture et à l’extérieur catch s’assurer que le hub d’appel de méthode est terminée correctement.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

Dans ASP.NET Core 2.2 ou version ultérieure, la diffusion en continu des méthodes de concentrateur peut accepter un `CancellationToken` paramètre qui sera déclenchée quand le client annule son abonnement à partir du flux. Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.

::: moniker-end

## <a name="net-client"></a>Client .NET

La méthode `StreamAsChannelAsync` sur `HubConnection` est utilisée pour appeler une méthode de diffusion en continu. Passez le nom de la méthode hub et les arguments définis dans la méthode de hub pour `StreamAsChannelAsync`. Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu. Un `ChannelReader<T>` est retourné à partir de l’appel de flux de données et représente le flux sur le client. Pour lire les données, il est courant d’utiliser une boucle sur `WaitToReadAsync` et d'appeler `TryRead` lorsque les données sont disponibles. La boucle s’arrête lorsque le flux a été fermé par le serveur ou lorsque le jeton d’annulation passé à `StreamAsChannelAsync` est annulé.

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

## <a name="javascript-client"></a>Client JavaScript

Les clients JavaScript appellent des méthodes de diffusion en continu sur les hubs à l’aide de `connection.stream`. La méthode `stream` accepte deux arguments :

* Le nom de la méthode de hub. Dans l’exemple suivant, le nom de méthode de hub est `Counter`.
* Les arguments définis dans la méthode de hub. Dans l’exemple suivant, les arguments sont : un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.

`connection.stream` retourne un `IStreamResult` qui contient une méthode `subscribe`. Passez un `IStreamSubscriber` à `subscribe` et définissez les callbacks `next`, `error`, et `complete` pour obtenir des notifications à partir de l'invocation de `stream`.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Pour terminer le flux à partir du client, appelez le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode). Appeler cette méthode provoque la `CancellationToken` paramètre de la méthode de concentrateur (si vous avez fourni une) doit être annulée.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a>Client Java
Le client SignalR Java utilise le `stream` méthode à appeler les méthodes de diffusion en continu. Elle accepte trois arguments ou plus :

* Le type des éléments de flux de données attendu 
* Le nom de la méthode de hub.
* Les arguments définis dans la méthode de hub. 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
Le `stream` méthode sur `HubConnection` retourne un Observable du type d’élément de flux de données. Le type Observable `subscribe` méthode est l’emplacement où vous définissez votre `onNext`, `onError` et `onCompleted` gestionnaires.

::: moniker-end

## <a name="related-resources"></a>Ressources connexes

* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
