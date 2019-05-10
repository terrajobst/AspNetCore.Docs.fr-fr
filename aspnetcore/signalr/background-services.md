---
title: Hôte ASP.NET Core SignalR dans les services d’arrière-plan
author: bradygaster
description: Découvrez comment envoyer des messages aux clients de SignalR à partir de classes de .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: dcd62f0c7056a3f987291b6c8bb8b87f94160865
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087770"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Hôte ASP.NET Core SignalR dans les services d’arrière-plan

Par [Brady Gaster](https://twitter.com/bradygaster)

Cet article fournit des conseils pour :

* Les concentrateurs SignalR à l’aide d’un processus de travail en arrière-plan hébergé avec ASP.NET Core d’hébergement.
* Envoi de messages à des clients à partir d’au sein d’un cœur de .NET connectés [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(comment télécharger)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Raccordées SignalR lors du démarrage

Hébergement ASP.NET Core SignalR Hubs dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un concentrateur dans une application web ASP.NET Core. Dans le `Startup.ConfigureServices` (méthode), en appelant `services.AddSignalR` ajoute les services nécessaires à la couche de l’Injection de dépendance ASP.NET Core (DI) pour prendre en charge SignalR. Dans `Startup.Configure`, le `UseSignalR` méthode est appelée pour associer les points de terminaison de Hub dans le pipeline de demande ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

Dans l’exemple précédent, le `ClockHub` la classe implémente la `Hub<T>` classe pour créer un Hub fortement typé. Le `ClockHub` a été configuré dans le `Startup` classe pour répondre aux requêtes au point de terminaison `/hubs/clock`.

Pour plus d’informations sur les Hubs fortement typées, consultez [utiliser des hubs dans SignalR pour ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Cette fonctionnalité n’est pas limitée à la [Hub\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) classe. Toute classe qui hérite de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), tel que [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), fonctionne également.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

L’interface utilisée par fortement typée `ClockHub` est le `IClock` interface.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Appeler un concentrateur SignalR à partir d’un service en arrière-plan

Lors du démarrage, le `Worker` (classe), un `BackgroundService`, est associé à l’aide de `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Étant donné que SignalR est également associé au cours de la `Startup` phase, dans lequel chaque Hub est attaché à un point de terminaison individuel dans le pipeline de demande HTTP d’ASP.NET Core, chaque concentrateur est représenté par un `IHubContext<T>` sur le serveur. À l’aide d’ASP.NET Core l’injection de dépendances des fonctionnalités, autres classes instanciés par la couche d’hébergement, par exemple `BackgroundService` classes, des classes de contrôleur MVC ou des modèles de page Razor, peuvent obtenir des références aux concentrateurs côté serveur en acceptant les instances de `IHubContext<ClockHub, IClock>` pendant la construction.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Comme le `ExecuteAsync` méthode est appelée de manière itérative dans le service d’arrière-plan, date et l’heure du serveur sont envoyés aux clients connectés à l’aide de la `ClockHub`.

## <a name="react-to-signalr-events-with-background-services"></a>Réagir aux événements de SignalR avec les services d’arrière-plan

Comme une application à Page unique à l’aide le client JavaScript pour faire de SignalR ou une application de bureau .NET à l’aide du <xref:signalr/dotnet-client>, un `BackgroundService` ou `IHostedService` implémentation peut également être utilisée pour vous connecter à des concentrateurs SignalR et répondre aux événements.

Le `ClockHubClient` classe implémente à la fois le `IClock` interface et le `IHostedService` interface. Ainsi, il peut être câblée pendant `Startup` pour s’exécuter en continu et répondre aux événements du Hub à partir du serveur. 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Pendant l’initialisation, le `ClockHubClient` crée une instance d’un `HubConnection` et relie la `IClock.ShowTime` méthode comme un gestionnaire pour le Hub `ShowTime` événement.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

Dans le `IHostedService.StartAsync` implémentation, le `HubConnection` est démarrée de façon asynchrone.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Lors de la `IHostedService.StopAsync` (méthode), le `HubConnection` est supprimé de façon asynchrone.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Hubs fortement typés](xref:signalr/hubs#strongly-typed-hubs)
