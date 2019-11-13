---
title: SignalR de ASP.NET Core de l’hôte dans les services d’arrière-plan
author: bradygaster
description: Découvrez comment envoyer des messages à des clients SignalR à partir de classes BackgroundService .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 000732115153eeafed3948c2a07acf77ffc34218
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964041"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>SignalR de ASP.NET Core de l’hôte dans les services d’arrière-plan

Par [Brady Gaster](https://twitter.com/bradygaster)

Cet article fournit des conseils pour :

* Hébergement SignalR hubs à l’aide d’un processus de travail en arrière-plan hébergé avec ASP.NET Core.
* Envoi de messages à des clients connectés à partir d’un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).net core.

[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(procédure de téléchargement)](xref:index#how-to-download-a-sample)

## <a name="wire-up-opno-locsignalr-during-startup"></a>Relier SignalR lors du démarrage

::: moniker range=">= aspnetcore-3.0"

L’hébergement de ASP.NET Core SignalR hubs dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un concentrateur dans une application Web ASP.NET Core. Dans la méthode `Startup.ConfigureServices`, l’appel de `services.AddSignalR` ajoute les services requis à la couche d’injection de dépendances ASP.NET Core pour prendre en charge SignalR. Dans `Startup.Configure`, la méthode `MapHub` est appelée dans le rappel `UseEndpoints` pour relier le ou les points de terminaison de concentrateur dans le pipeline de demande ASP.NET Core.

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

L’hébergement de ASP.NET Core SignalR hubs dans le contexte d’un processus de travail en arrière-plan est identique à l’hébergement d’un concentrateur dans une application Web ASP.NET Core. Dans la méthode `Startup.ConfigureServices`, l’appel de `services.AddSignalR` ajoute les services requis à la couche d’injection de dépendances ASP.NET Core pour prendre en charge SignalR. Dans `Startup.Configure`, la méthode `UseSignalR` est appelée pour connecter le ou les points de terminaison de concentrateur dans le pipeline de requête ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

Dans l’exemple précédent, la classe `ClockHub` implémente la classe `Hub<T>` pour créer un concentrateur fortement typé. La `ClockHub` a été configurée dans la classe `Startup` pour répondre aux demandes au niveau du point de terminaison `/hubs/clock`.

Pour plus d’informations sur les hubs fortement typés, consultez [utiliser des hubs dans SignalR pour les ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Cette fonctionnalité n’est pas limitée à la classe [Hub\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Toute classe qui hérite de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), telle que [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), fonctionne également.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

L’interface utilisée par la `ClockHub` fortement typée est l’interface `IClock`.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>Appeler un hub SignalR à partir d’un service en arrière-plan

Au démarrage, la classe `Worker`, un `BackgroundService`, est câblée à l’aide de `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Étant donné que SignalR est également câblée au cours de la phase de `Startup`, dans laquelle chaque concentrateur est attaché à un point de terminaison individuel dans le pipeline de requête HTTP d’ASP.NET Core, chaque concentrateur est représenté par une `IHubContext<T>` sur le serveur. À l’aide des fonctionnalités DI de ASP.NET Core, d’autres classes instanciées par la couche d’hébergement, comme les classes `BackgroundService`, les classes de contrôleur MVC ou les modèles de page Razor, peuvent recevoir des références aux hubs côté serveur en acceptant des instances de `IHubContext<ClockHub, IClock>` pendant la construction.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Comme la méthode `ExecuteAsync` est appelée de manière itérative dans le service d’arrière-plan, la date et l’heure actuelles du serveur sont envoyées aux clients connectés à l’aide de l' `ClockHub`.

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>Réagir aux événements de SignalR avec les services d’arrière-plan

À l’instar d’une application à page unique utilisant le client JavaScript pour SignalR ou une application de bureau .NET peut utiliser à l’aide de l' <xref:signalr/dotnet-client>, une implémentation `BackgroundService` ou `IHostedService` peut également être utilisée pour se connecter aux hubs SignalR et répondre aux événements.

La classe `ClockHubClient` implémente à la fois l’interface `IClock` et l’interface `IHostedService`. De cette façon, il peut être câblé pendant `Startup` pour s’exécuter en continu et répondre aux événements de concentrateur à partir du serveur.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Pendant l’initialisation, le `ClockHubClient` crée une instance d’une `HubConnection` et associe la méthode `IClock.ShowTime` en tant que gestionnaire de l’événement `ShowTime` du concentrateur.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

Dans l’implémentation de `IHostedService.StartAsync`, le `HubConnection` est démarré de manière asynchrone.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Pendant la méthode `IHostedService.StopAsync`, la `HubConnection` est supprimée de façon asynchrone.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Bien démarrer](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publier sur Azure](xref:signalr/publish-to-azure-web-app)
* [Hubs fortement typés](xref:signalr/hubs#strongly-typed-hubs)
