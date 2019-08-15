---
title: Services gRPC avec ASP.NET Core
author: juntaoluo
description: Découvrez les concepts de base liés à l’écriture de services gRPC avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 38111c152c581c50767f9cd4e5fa257bd3fd804e
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022311"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="b2adf-103">Services gRPC avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2adf-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="b2adf-104">Ce document montre comment prendre en main les services gRPC à l’aide de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b2adf-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2adf-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b2adf-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2adf-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2adf-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b2adf-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b2adf-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b2adf-108">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b2adf-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="b2adf-109">Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2adf-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="b2adf-110">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b2adf-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b2adf-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2adf-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2adf-112">Pour obtenir des instructions détaillées sur la création d’un projet gRPC, consultez [prise en main des services gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="b2adf-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b2adf-113">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b2adf-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b2adf-114">Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b2adf-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="b2adf-115">Ajouter des services gRPC à une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2adf-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="b2adf-116">gRPC requiert le package [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="b2adf-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="b2adf-117">Configurer gRPC</span><span class="sxs-lookup"><span data-stu-id="b2adf-117">Configure gRPC</span></span>

<span data-ttu-id="b2adf-118">gRPC est activé avec la `AddGrpc` méthode:</span><span class="sxs-lookup"><span data-stu-id="b2adf-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="b2adf-119">Chaque service gRPC est ajouté au pipeline de routage via `MapGrpcService` la méthode:</span><span class="sxs-lookup"><span data-stu-id="b2adf-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="b2adf-120">ASP.NET Core intergiciels et les fonctionnalités partagent le pipeline de routage, une application peut donc être configurée pour servir des gestionnaires de demandes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="b2adf-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="b2adf-121">Les gestionnaires de demandes supplémentaires, tels que les contrôleurs MVC, fonctionnent en parallèle avec les services gRPC configurés.</span><span class="sxs-lookup"><span data-stu-id="b2adf-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="b2adf-122">Intégration avec les API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2adf-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="b2adf-123">les services gRPC ont un accès complet aux fonctionnalités de ASP.NET Core telles que l' [injection](xref:fundamentals/dependency-injection) de dépendances (di) et la [journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="b2adf-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="b2adf-124">Par exemple, l’implémentation de service peut résoudre un service d’enregistreur d’événements à partir du conteneur DI via le constructeur:</span><span class="sxs-lookup"><span data-stu-id="b2adf-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="b2adf-125">Par défaut, l’implémentation du service gRPC peut résoudre d’autres services DI avec n’importe quelle durée de vie (Singleton, étendue ou transitoire).</span><span class="sxs-lookup"><span data-stu-id="b2adf-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="b2adf-126">Résoudre HttpContext dans les méthodes gRPC</span><span class="sxs-lookup"><span data-stu-id="b2adf-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="b2adf-127">L’API gRPC permet d’accéder à certaines données de message HTTP/2, telles que la méthode, l’hôte, l’en-tête et les codes de fin.</span><span class="sxs-lookup"><span data-stu-id="b2adf-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="b2adf-128">L’accès s’effectue `ServerCallContext` par le biais de l’argument passé à chaque méthode gRPC:</span><span class="sxs-lookup"><span data-stu-id="b2adf-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="b2adf-129">`ServerCallContext`ne fournit pas un accès complet `HttpContext` à dans toutes les API ASP.net.</span><span class="sxs-lookup"><span data-stu-id="b2adf-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="b2adf-130">La `GetHttpContext` méthode d’extension fournit un accès complet `HttpContext` au représentant le message http/2 sous-jacent dans les API ASP.net:</span><span class="sxs-lookup"><span data-stu-id="b2adf-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="b2adf-131">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b2adf-131">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
