---
title: Utiliser la diffusion en continu dans ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 6d5f707bd2a37e1999c6e87e3cfc369aa0301207
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708437"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Utiliser la diffusion en continu dans ASP.NET Core SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur. Cela est utile pour les scénarios où les fragments de données arriveront au fil du temps. Lorsqu’une valeur de retour est diffusée sur le client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente que toutes les données soient disponibles.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurer le hub

Une méthode de hub devient automatiquement une méthode de hub de diffusion en continu quand elle retourne un `ChannelReader<T>` ou un `Task<ChannelReader<T>>`. Voici un exemple qui montre les principes de diffusion en continu vers le client. Chaque fois qu’un objet est écrit dans le `ChannelReader` cet objet est immédiatement envoyé au client. À la fin, le `ChannelReader` est terminé pour indiquer au client que le flux est fermé.

> [!NOTE]
> Écrivez dans le `ChannelReader` sur un thread d’arrière-plan, puis revenez sur le `ChannelReader` dès que possible. Les autres appels du hub seront bloqués jusqu'à ce qu'un `ChannelReader` soit retourné.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> Dans ASP.NET Core 2.2 ou version ultérieure, la diffusion en continu des méthodes de concentrateur peut accepter un `CancellationToken` paramètre qui sera déclenchée quand le client annule son abonnement à partir du flux. Utiliser ce jeton pour arrêter l’opération de serveur et de libérer les ressources si le client se déconnecte avant la fin du flux.

::: moniker-end

## <a name="net-client"></a>Client .NET

La méthode `StreamAsChannelAsync` sur `HubConnection` est utilisée pour appeler une méthode de diffusion en continu. Passez le nom de la méthode hub et les arguments définis dans la méthode de hub pour `StreamAsChannelAsync`. Le paramètre générique sur `StreamAsChannelAsync<T>` spécifie le type d’objets retournés par la méthode de diffusion en continu. Un `ChannelReader<T>` est retourné à partir de l’appel de flux de données et représente le flux sur le client. Pour lire les données, il est courant d’utiliser une boucle sur `WaitToReadAsync` et d'appeler `TryRead` lorsque les données sont disponibles. La boucle s’arrête lorsque le flux a été fermé par le serveur ou lorsque le jeton d’annulation passé à `StreamAsChannelAsync` est annulé.

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

## <a name="related-resources"></a>Ressources connexes

* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
