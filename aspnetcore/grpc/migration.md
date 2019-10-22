---
title: Migration des services gRPC à partir de C-Core vers ASP.NET Core
author: juntaoluo
description: Découvrez comment déplacer une application gRPC basée sur un noyau C existante pour qu’elle s’exécute sur ASP.NET Core pile.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 596eca0f510387a18472eb353672980e0a8e0d24
ms.sourcegitcommit: eb4fcdeb2f9e8413117624de42841a4997d1d82d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697998"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migration des services gRPC à partir de C-Core vers ASP.NET Core

Par [John Luo](https://github.com/juntaoluo)

En raison de l’implémentation de la pile sous-jacente, certaines fonctionnalités ne fonctionnent pas de la même façon entre les applications [gRPC basées sur le langage C](https://grpc.io/blog/grpc-stacks) et les applications basées sur les ASP.net core. Ce document met en évidence les principales différences en matière de migration entre les deux piles.

## <a name="grpc-service-implementation-lifetime"></a>durée de vie de l’implémentation du service gRPC

Dans la pile ASP.NET Core, les services gRPC, par défaut, sont créés avec une [durée de vie limitée](xref:fundamentals/dependency-injection#service-lifetimes). En revanche, gRPC C-Core est lié par défaut à un service avec une [durée de vie Singleton](xref:fundamentals/dependency-injection#service-lifetimes).

Une durée de vie limitée permet à l’implémentation de service de résoudre d’autres services avec des durées de vie délimitées. Par exemple, une durée de vie délimitée peut également résoudre `DbContext` à partir du conteneur DI par le biais de l’injection de constructeur. Utilisation de la durée de vie limitée :

* Une nouvelle instance de l’implémentation de service est construite pour chaque requête.
* Il n’est pas possible de partager l’état entre les demandes via des membres d’instance sur le type d’implémentation.
* L’objectif est de stocker les États partagés dans un service Singleton dans le conteneur DI. Les États partagés stockés sont résolus dans le constructeur de l’implémentation du service gRPC.

Pour plus d’informations sur les durées de vie des services, consultez <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Ajouter un service Singleton

Pour faciliter la transition d’une implémentation gRPC C-Core à ASP.NET Core, il est possible de modifier la durée de vie du service de l’implémentation de service de l’étendue à Singleton. Cela implique l’ajout d’une instance de l’implémentation de service au conteneur DI :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Toutefois, une implémentation de service avec une durée de vie Singleton n’est plus en mesure de résoudre les services délimités via l’injection de constructeur.

## <a name="configure-grpc-services-options"></a>Configurer les options des services gRPC

Dans les applications basées sur C-Core, les paramètres tels que `grpc.max_receive_message_length` et `grpc.max_send_message_length` sont configurés avec `ChannelOption` lors de [la construction de l’instance de serveur](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

Dans ASP.NET Core, gRPC fournit la configuration via le type de `GrpcServiceOptions`. Par exemple, la taille maximale des messages entrants peut être configurée par le biais de `AddGrpc`. L’exemple suivant modifie la `MaxReceiveMessageSize` par défaut de 4 Mo à 16 Mo :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

Pour plus d’informations sur la configuration, consultez <xref:grpc/configuration>.

## <a name="logging"></a>Journalisation

Les applications basées sur le langage C s’appuient sur la `GrpcEnvironment` pour [configurer l’enregistreur](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) d’événements à des fins de débogage. La pile ASP.NET Core fournit cette fonctionnalité par le biais de l' [API de journalisation](xref:fundamentals/logging/index). Par exemple, un enregistreur d’événements peut être ajouté au service gRPC via l’injection de constructeur :

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Les applications basées sur le langage C configurent le protocole HTTPs via la [propriété Server. ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Un concept similaire est utilisé pour configurer des serveurs dans ASP.NET Core. Par exemple, Kestrel utilise la [configuration de point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration) pour cette fonctionnalité.

## <a name="interceptors-and-middleware"></a>Intercepteurs et intergiciel

ASP.NET Core [intergiciel](xref:fundamentals/middleware/index) offre des fonctionnalités similaires par rapport aux intercepteurs des applications gRPC basées sur le langage C. Les intercepteurs et les intercepteurs sont conceptuels de la même façon que les deux sont utilisés pour construire un pipeline qui gère une demande gRPC. Ils autorisent tous deux le travail à effectuer avant ou après le composant suivant dans le pipeline. Toutefois, ASP.NET Core intergiciel fonctionne sur les messages HTTP/2 sous-jacents, tandis que les intercepteurs opèrent sur la couche d’abstraction gRPC à l’aide de [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
