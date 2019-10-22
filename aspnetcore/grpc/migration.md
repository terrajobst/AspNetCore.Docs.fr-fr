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
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="253ae-103">Migration des services gRPC à partir de C-Core vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="253ae-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="253ae-104">Par [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="253ae-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="253ae-105">En raison de l’implémentation de la pile sous-jacente, certaines fonctionnalités ne fonctionnent pas de la même façon entre les applications [gRPC basées sur le langage C](https://grpc.io/blog/grpc-stacks) et les applications basées sur les ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="253ae-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="253ae-106">Ce document met en évidence les principales différences en matière de migration entre les deux piles.</span><span class="sxs-lookup"><span data-stu-id="253ae-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="253ae-107">durée de vie de l’implémentation du service gRPC</span><span class="sxs-lookup"><span data-stu-id="253ae-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="253ae-108">Dans la pile ASP.NET Core, les services gRPC, par défaut, sont créés avec une [durée de vie limitée](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="253ae-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="253ae-109">En revanche, gRPC C-Core est lié par défaut à un service avec une [durée de vie Singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="253ae-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="253ae-110">Une durée de vie limitée permet à l’implémentation de service de résoudre d’autres services avec des durées de vie délimitées.</span><span class="sxs-lookup"><span data-stu-id="253ae-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="253ae-111">Par exemple, une durée de vie délimitée peut également résoudre `DbContext` à partir du conteneur DI par le biais de l’injection de constructeur.</span><span class="sxs-lookup"><span data-stu-id="253ae-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="253ae-112">Utilisation de la durée de vie limitée :</span><span class="sxs-lookup"><span data-stu-id="253ae-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="253ae-113">Une nouvelle instance de l’implémentation de service est construite pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="253ae-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="253ae-114">Il n’est pas possible de partager l’état entre les demandes via des membres d’instance sur le type d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="253ae-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="253ae-115">L’objectif est de stocker les États partagés dans un service Singleton dans le conteneur DI.</span><span class="sxs-lookup"><span data-stu-id="253ae-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="253ae-116">Les États partagés stockés sont résolus dans le constructeur de l’implémentation du service gRPC.</span><span class="sxs-lookup"><span data-stu-id="253ae-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="253ae-117">Pour plus d’informations sur les durées de vie des services, consultez <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="253ae-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="253ae-118">Ajouter un service Singleton</span><span class="sxs-lookup"><span data-stu-id="253ae-118">Add a singleton service</span></span>

<span data-ttu-id="253ae-119">Pour faciliter la transition d’une implémentation gRPC C-Core à ASP.NET Core, il est possible de modifier la durée de vie du service de l’implémentation de service de l’étendue à Singleton.</span><span class="sxs-lookup"><span data-stu-id="253ae-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="253ae-120">Cela implique l’ajout d’une instance de l’implémentation de service au conteneur DI :</span><span class="sxs-lookup"><span data-stu-id="253ae-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="253ae-121">Toutefois, une implémentation de service avec une durée de vie Singleton n’est plus en mesure de résoudre les services délimités via l’injection de constructeur.</span><span class="sxs-lookup"><span data-stu-id="253ae-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="253ae-122">Configurer les options des services gRPC</span><span class="sxs-lookup"><span data-stu-id="253ae-122">Configure gRPC services options</span></span>

<span data-ttu-id="253ae-123">Dans les applications basées sur C-Core, les paramètres tels que `grpc.max_receive_message_length` et `grpc.max_send_message_length` sont configurés avec `ChannelOption` lors de [la construction de l’instance de serveur](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="253ae-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="253ae-124">Dans ASP.NET Core, gRPC fournit la configuration via le type de `GrpcServiceOptions`.</span><span class="sxs-lookup"><span data-stu-id="253ae-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="253ae-125">Par exemple, la taille maximale des messages entrants peut être configurée par le biais de `AddGrpc`.</span><span class="sxs-lookup"><span data-stu-id="253ae-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="253ae-126">L’exemple suivant modifie la `MaxReceiveMessageSize` par défaut de 4 Mo à 16 Mo :</span><span class="sxs-lookup"><span data-stu-id="253ae-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="253ae-127">Pour plus d’informations sur la configuration, consultez <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="253ae-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="253ae-128">Journalisation</span><span class="sxs-lookup"><span data-stu-id="253ae-128">Logging</span></span>

<span data-ttu-id="253ae-129">Les applications basées sur le langage C s’appuient sur la `GrpcEnvironment` pour [configurer l’enregistreur](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) d’événements à des fins de débogage.</span><span class="sxs-lookup"><span data-stu-id="253ae-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="253ae-130">La pile ASP.NET Core fournit cette fonctionnalité par le biais de l' [API de journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="253ae-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="253ae-131">Par exemple, un enregistreur d’événements peut être ajouté au service gRPC via l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="253ae-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="253ae-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="253ae-132">HTTPS</span></span>

<span data-ttu-id="253ae-133">Les applications basées sur le langage C configurent le protocole HTTPs via la [propriété Server. ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="253ae-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="253ae-134">Un concept similaire est utilisé pour configurer des serveurs dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="253ae-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="253ae-135">Par exemple, Kestrel utilise la [configuration de point de terminaison](xref:fundamentals/servers/kestrel#endpoint-configuration) pour cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="253ae-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="253ae-136">Intercepteurs et intergiciel</span><span class="sxs-lookup"><span data-stu-id="253ae-136">Interceptors and Middleware</span></span>

<span data-ttu-id="253ae-137">ASP.NET Core [intergiciel](xref:fundamentals/middleware/index) offre des fonctionnalités similaires par rapport aux intercepteurs des applications gRPC basées sur le langage C.</span><span class="sxs-lookup"><span data-stu-id="253ae-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="253ae-138">Les intercepteurs et les intercepteurs sont conceptuels de la même façon que les deux sont utilisés pour construire un pipeline qui gère une demande gRPC.</span><span class="sxs-lookup"><span data-stu-id="253ae-138">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="253ae-139">Ils autorisent tous deux le travail à effectuer avant ou après le composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="253ae-139">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="253ae-140">Toutefois, ASP.NET Core intergiciel fonctionne sur les messages HTTP/2 sous-jacents, tandis que les intercepteurs opèrent sur la couche d’abstraction gRPC à l’aide de [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="253ae-140">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="253ae-141">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="253ae-141">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
