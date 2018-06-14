---
title: Utilisez la diffusion en continu dans ASP.NET Core SignalR
author: rachelappel
description: ''
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/streaming
ms.openlocfilehash: e8b32d5f25ae3bf8555fd2375c0fbb99a6f8bd53
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358423"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Utilisez la diffusion en continu dans ASP.NET Core SignalR

Par [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR prend en charge la diffusion en continu des valeurs de retour des méthodes de serveur. Cela est utile pour les scénarios où des fragments de données entrent au fil du temps. Lorsqu’une valeur de retour est transmise au client, chaque fragment est envoyé au client dès qu’il est disponible, et non en attente pour toutes les données soient disponibles.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurer le concentrateur

Une méthode de concentrateur devient automatiquement une méthode de concentrateur de diffusion en continu lorsqu’elle retourne un `ChannelReader<T>` ou `Task<ChannelReader<T>>`. Voici un exemple qui illustre les principes de base de données au client de diffusion en continu. Chaque fois qu’un objet est écrit dans le `ChannelReader` de cet objet est immédiatement envoyé au client. À la fin, le `ChannelReader` soit terminée pour indiquer au client le flux est fermé.

> [!NOTE]
> Écrire dans le `ChannelReader` sur un thread d’arrière-plan et de retourner le `ChannelReader` dès que possible. Autres appels de concentrateur seront bloquées jusqu'à ce qu’une `ChannelReader` est retourné.

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a>Client .NET

Le `StreamAsChannelAsync` méthode sur `HubConnection` est utilisé pour appeler une méthode de diffusion en continu. Passez le nom de méthode de concentrateur et les arguments définis dans la méthode de concentrateur pour `StreamAsChannelAsync`. Le paramètre générique sur `StreamAsChannelAsync<T>` Spécifie le type d’objets retournés par la méthode de diffusion en continu. A `ChannelReader<T>` est retourné à partir de l’appel de flux de données et représente le flux de données sur le client. Pour lire les données, il est courant d’une boucle sur `WaitToReadAsync` et appelez `TryRead` lorsque les données sont disponibles. La boucle s’arrête lorsque le flux de données a été fermée par le serveur, ou le jeton d’annulation passé à `StreamAsChannelAsync` est annulée.

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

## <a name="javascript-client"></a>Client JavaScript

Les clients JavaScript appellent des méthodes de diffusion en continu sur les concentrateurs à l’aide de `connection.stream`. Le `stream` méthode accepte deux arguments :

* Le nom de la méthode de concentrateur. Dans l’exemple suivant, le nom de la méthode de concentrateur est `Counter`.
* Arguments définis dans la méthode de concentrateur. Dans l’exemple suivant, les arguments sont : un nombre pour le nombre d’éléments de flux de données à recevoir et le délai entre les éléments de flux de données.

`connection.stream` Retourne un `IStreamResult` qui contient un `subscribe` (méthode). Passer un `IStreamSubscriber` à `subscribe` et définir le `next`, `error`, et `complete` rappels pour obtenir des notifications à partir de la `stream` invocation.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

À la fin du flux à partir de l’appel du client le `dispose` méthode sur le `ISubscription` qui est retourné à partir de la `subscribe` (méthode).

## <a name="related-resources"></a>Ressources connexes

* [Hubs](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Client JavaScript](xref:signalr/javascript-client)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)