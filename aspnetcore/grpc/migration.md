---
title: Migration des services de gRPC à partir de C vers ASP.NET Core
author: juntaoluo
description: Découvrez comment déplacer une application de gRPC en C-core existante s’exécutant sur la pile ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 4d489b5aecf2e15fbbe3ac472b991a4365cd47c1
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672617"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migration des services de gRPC à partir de C vers ASP.NET Core

Par [John Luo](https://github.com/juntaoluo)

En raison de l’implémentation de la pile sous-jacente, toutes les fonctionnalités ne fonctionnent de la même façon entre [gRPC par cœur-C](https://grpc.io/blog/grpc-stacks) applications et les applications ASP.NET Core. Ce document met en évidence les principales différences pour la migration entre les deux piles.

## <a name="grpc-service-implementation-lifetime"></a>durée de vie de gRPC service implémentation

Dans la pile ASP.NET Core, services gRPC, par défaut, sont créés avec un [durée de vie délimitée](xref:fundamentals/dependency-injection#service-lifetimes). En revanche, gRPC C-core par défaut liée à un service avec un [durée de vie singleton](xref:fundamentals/dependency-injection#service-lifetimes).

Une durée de vie délimitée permet à l’implémentation de service résoudre les autres services avec des durées de vie délimitées. Par exemple, une durée de vie délimitée peut également résoudre `DBContext` à partir du conteneur d’injection de dépendances via l’injection de constructeur. À l’aide de la durée de vie délimitée :

* Une nouvelle instance de l’implémentation du service est construite pour chaque requête.
* Il n’est pas possible de partager l’état entre les demandes via les membres d’instance sur le type d’implémentation.
* L’attente consiste à stocker des états partagés dans un service singleton dans le conteneur d’injection de dépendances. Les états partagés stockées sont résolus dans le constructeur de l’implémentation du service gRPC.

Pour plus d’informations sur les durées de vie de service, consultez <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Ajouter un service singleton

Pour faciliter la transition à partir d’une implémentation de C-core gRPC vers ASP.NET Core, il est possible de modifier la durée de vie du service de l’implémentation du service en limitées à un singleton. Cela implique l’ajout d’une instance de l’implémentation du service dans le conteneur d’injection de dépendance :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Toutefois, une implémentation de service avec une durée de vie singleton n’est plus en mesure de résoudre des services délimités par le biais d’injection de constructeur.

## <a name="configure-grpc-services-options"></a>Configurer les options de services gRPC

Dans C-core-applications, paramètres tels que `grpc.max_receive_message_length` et `grpc.max_send_message_length` sont configurés avec `ChannelOption` lorsque [construction de l’instance de serveur](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

Dans ASP.NET Core, `GrpcServiceOptions` permet de configurer ces paramètres. Les paramètres peuvent être appliquées globalement à tous les services de gRPC ou à un type d’implémentation de service individuels. Options spécifiées pour les types d’implémentation de service individuels remplacent les paramètres globaux du configuré.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a>Journalisation

C-core-applications s’appuient sur le `GrpcEnvironment` à [configurer l’enregistreur d’événements](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) à des fins de débogage. La pile ASP.NET Core fournit cette fonctionnalité via le [API de journalisation](xref:fundamentals/logging/index). Par exemple, un enregistreur d’événements peut être ajoutée au service gRPC via l’injection de constructeur :

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

C-core-applications configurer HTTPS via le [Server.Ports propriété](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Un concept similaire est utilisé pour configurer des serveurs dans ASP.NET Core. Par exemple, Kestrel utilise [configuration de point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration) pour cette fonctionnalité.

## <a name="interceptors-and-middleware"></a>Intergiciel (middleware) et des intercepteurs

ASP.NET Core [intergiciel (middleware)](xref:fundamentals/middleware/index) offre des fonctionnalités similaires par rapport aux intercepteurs dans les applications gRPC par cœur-C. Intergiciel (middleware) et les intercepteurs sont conceptuellement les mêmes que les deux sont utilisés pour construire un pipeline qui gère une demande de gRPC. Les deux permettent de travail à exécuter avant ou après le composant suivant dans le pipeline. Toutefois, intergiciel (middleware) ASP.NET Core fonctionne sur les messages HTTP/2 sous-jacents, tandis qu’intercepteurs fonctionnent sur la couche d’abstraction à l’aide de gRPC le [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
