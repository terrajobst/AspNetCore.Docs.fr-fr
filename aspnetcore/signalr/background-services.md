---
title: Hôte ASP.NET Core Signalr dans les services d’arrière-plan
author: bradygaster
description: Découvrez comment envoyer des messages aux clients Signalr à partir de classes BackgroundService .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773925"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Hôte ASP.NET Core Signalr dans les services d’arrière-plan

Par [Brady Gaster](https://twitter.com/bradygaster)

Cet article fournit des conseils pour :

* Hébergement de hubs Signalr à l’aide d’un processus de travail en arrière-plan hébergé avec ASP.NET Core.
* Envoi de messages à des clients connectés à partir d’un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).net core.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Signaler le Signalr au démarrage

::: moniker range=">= aspnetcore-3.0"

L’hébergement ASP.NET Core hubs Signalr dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un Hub dans une application Web ASP.NET Core. Dans la `Startup.ConfigureServices` méthode, l' `services.AddSignalR` appel de ajoute les services requis à la couche d’injection de dépendances ASP.net Core (di) pour prendre en charge signalr. Dans `Startup.Configure`, la `MapHub` méthode est appelée dans le `UseEndpoints` rappel pour relier le ou les points de terminaison de concentrateur dans le pipeline de requête ASP.net core.

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

L’hébergement ASP.NET Core hubs Signalr dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un Hub dans une application Web ASP.NET Core. Dans la `Startup.ConfigureServices` méthode, l' `services.AddSignalR` appel de ajoute les services requis à la couche d’injection de dépendances ASP.net Core (di) pour prendre en charge signalr. Dans `Startup.Configure`, la `UseSignalR` méthode est appelée pour relier le ou les points de terminaison de concentrateur dans le pipeline de requête ASP.net core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

Dans l’exemple précédent, la `ClockHub` classe implémente la `Hub<T>` classe pour créer un concentrateur fortement typé. A été configuré dans la `Startup` classe pour répondre aux demandes au niveau du point `/hubs/clock`de terminaison. `ClockHub`

Pour plus d’informations sur les hubs fortement typés, consultez [utiliser des hubs dans signalr pour ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Cette fonctionnalité n’est pas limitée à la classe [\<Hub t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Toute classe qui hérite de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), telle que [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), fonctionne également.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

L’interface utilisée par le fortement typé `ClockHub` est l' `IClock` interface.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Appeler un concentrateur Signalr à partir d’un service d’arrière-plan

Au démarrage, la `Worker` classe, a `BackgroundService`, est connectée à `AddHostedService`l’aide de.

```csharp
services.AddHostedService<Worker>();
```

Étant donné que signalr est également relié au `Startup` cours de la phase, dans lequel chaque concentrateur est attaché à un point de terminaison individuel dans le pipeline de requête HTTP `IHubContext<T>` de ASP.net Core, chaque concentrateur est représenté par un sur le serveur. À l’aide des fonctionnalités di de ASP.net Core, les autres classes instanciées par la `BackgroundService` couche d’hébergement, comme les classes, les classes de contrôleur MVC ou les modèles de page Razor, peuvent recevoir des `IHubContext<ClockHub, IClock>` références aux hubs côté serveur en acceptant des instances de lors de la construction.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Étant donné `ExecuteAsync` que la méthode est appelée de manière itérative dans le service d’arrière-plan, la date et l’heure actuelles du serveur sont `ClockHub`envoyées aux clients connectés à l’aide de.

## <a name="react-to-signalr-events-with-background-services"></a>Réagir aux événements Signalr avec les services d’arrière-plan

À l’instar d’une application à page unique utilisant le client JavaScript pour signalr ou une application de bureau .NET <xref:signalr/dotnet-client> `BackgroundService` , vous pouvez `IHostedService` utiliser le à l’aide de l’implémentation, ou pour vous connecter aux hubs signalr et répondre aux événements.

La `ClockHubClient` classe implémente à la `IClock` fois l’interface `IHostedService` et l’interface. De cette façon, il peut être connecté `Startup` pendant l’exécution continue et répondre aux événements de concentrateur à partir du serveur.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

`ClockHubClient` Pendant l’initialisation, crée une instance `HubConnection` de et relie la `IClock.ShowTime` méthode en tant que gestionnaire de l’événement du `ShowTime` concentrateur.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

Dans l' `IHostedService.StartAsync` implémentation, le `HubConnection` est démarré de manière asynchrone.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Pendant la `IHostedService.StopAsync` méthode, le `HubConnection` est supprimé de manière asynchrone.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Hubs fortement typés](xref:signalr/hubs#strongly-typed-hubs)
