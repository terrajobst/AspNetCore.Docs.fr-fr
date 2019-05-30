---
title: Services gRPC avec ASP.NET Core
author: juntaoluo
description: Découvrez les concepts de base lors de l’écriture des services de gRPC avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 190004de8b70a463f9f58a25164d5a86ecc266d6
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376353"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="c3788-103">Services gRPC avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3788-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="c3788-104">Ce document montre comment bien démarrer avec les services de gRPC à l’aide d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3788-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="c3788-105">Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3788-105">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="c3788-106">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c3788-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c3788-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c3788-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c3788-108">Consultez [prise en main des services de gRPC](xref:tutorials/grpc/grpc-start) pour obtenir des instructions détaillées sur la façon de créer un projet de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c3788-108">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c3788-109">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c3788-109">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c3788-110">Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="c3788-110">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="c3788-111">Ajouter des services de gRPC à une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3788-111">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="c3788-112">gRPC requiert les packages suivants :</span><span class="sxs-lookup"><span data-stu-id="c3788-112">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="c3788-113">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="c3788-113">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="c3788-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) pour protobuf message API.</span><span class="sxs-lookup"><span data-stu-id="c3788-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="c3788-115">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="c3788-115">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="c3788-116">Configurer gRPC</span><span class="sxs-lookup"><span data-stu-id="c3788-116">Configure gRPC</span></span>

<span data-ttu-id="c3788-117">gRPC est activé avec le `AddGrpc` méthode :</span><span class="sxs-lookup"><span data-stu-id="c3788-117">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="c3788-118">Chaque service gRPC est ajouté au pipeline routage via le `MapGrpcService` méthode :</span><span class="sxs-lookup"><span data-stu-id="c3788-118">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

<span data-ttu-id="c3788-119">Fonctionnalités et des intergiciels d’ASP.NET Core partagent le pipeline de routage, par conséquent, une application peut être configurée pour servir des gestionnaires de demandes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="c3788-119">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="c3788-120">Les gestionnaires de demande supplémentaires, tels que les contrôleurs MVC, travaillent en parallèle avec les services configurés gRPC.</span><span class="sxs-lookup"><span data-stu-id="c3788-120">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="c3788-121">Intégration avec les API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3788-121">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="c3788-122">gRPC services ont un accès complet aux fonctionnalités ASP.NET Core comme [l’Injection de dépendances](xref:fundamentals/dependency-injection) (DI) et [journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c3788-122">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="c3788-123">Par exemple, l’implémentation du service peut résoudre un service d’enregistreur d’événements à partir du conteneur d’injection de dépendances via le constructeur :</span><span class="sxs-lookup"><span data-stu-id="c3788-123">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="c3788-124">Par défaut, l’implémentation du service gRPC peut résoudre d’autres services de l’injection de dépendances avec toute durée de vie (Singleton, délimitées ou temporaires).</span><span class="sxs-lookup"><span data-stu-id="c3788-124">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="c3788-125">Résoudre HttpContext dans les méthodes de gRPC</span><span class="sxs-lookup"><span data-stu-id="c3788-125">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="c3788-126">L’API gRPC fournit l’accès à certaines données de message HTTP/2, telles que la méthode, hôte, en-tête et codes de fin.</span><span class="sxs-lookup"><span data-stu-id="c3788-126">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="c3788-127">Accès s’effectue via le `ServerCallContext` argument passé à chaque méthode gRPC :</span><span class="sxs-lookup"><span data-stu-id="c3788-127">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="c3788-128">`ServerCallContext` ne fournit pas un accès complet à `HttpContext` dans toutes les APIs ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3788-128">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="c3788-129">Le `GetHttpContext` méthode d’extension fournit un accès complet à la `HttpContext` représentant le message HTTP/2 sous-jacent dans ASP.NET APIs :</span><span class="sxs-lookup"><span data-stu-id="c3788-129">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a><span data-ttu-id="c3788-130">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c3788-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
