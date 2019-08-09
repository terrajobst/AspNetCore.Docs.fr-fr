---
title: Services gRPC avec ASP.NET Core
author: juntaoluo
description: Découvrez les concepts de base liés à l’écriture de services gRPC avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862938"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="d8d60-103">Services gRPC avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8d60-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="d8d60-104">Ce document montre comment prendre en main les services gRPC à l’aide de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8d60-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8d60-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d8d60-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8d60-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8d60-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d8d60-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8d60-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d8d60-108">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d8d60-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="d8d60-109">Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8d60-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="d8d60-110">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d8d60-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8d60-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8d60-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d8d60-112">Pour obtenir des instructions détaillées sur la création d’un projet gRPC, consultez [prise en main des services gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="d8d60-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d8d60-113">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d8d60-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="d8d60-114">Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="d8d60-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="d8d60-115">Ajouter des services gRPC à une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8d60-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="d8d60-116">gRPC requiert le package [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="d8d60-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="d8d60-117">Configurer gRPC</span><span class="sxs-lookup"><span data-stu-id="d8d60-117">Configure gRPC</span></span>

<span data-ttu-id="d8d60-118">gRPC est activé avec la `AddGrpc` méthode:</span><span class="sxs-lookup"><span data-stu-id="d8d60-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="d8d60-119">Chaque service gRPC est ajouté au pipeline de routage via `MapGrpcService` la méthode:</span><span class="sxs-lookup"><span data-stu-id="d8d60-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="d8d60-120">ASP.NET Core intergiciels et les fonctionnalités partagent le pipeline de routage, une application peut donc être configurée pour servir des gestionnaires de demandes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="d8d60-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="d8d60-121">Les gestionnaires de demandes supplémentaires, tels que les contrôleurs MVC, fonctionnent en parallèle avec les services gRPC configurés.</span><span class="sxs-lookup"><span data-stu-id="d8d60-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="d8d60-122">Intégration avec les API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8d60-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="d8d60-123">les services gRPC ont un accès complet aux fonctionnalités de ASP.NET Core telles que l' [injection](xref:fundamentals/dependency-injection) de dépendances (di) et la [journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="d8d60-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="d8d60-124">Par exemple, l’implémentation de service peut résoudre un service d’enregistreur d’événements à partir du conteneur DI via le constructeur:</span><span class="sxs-lookup"><span data-stu-id="d8d60-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="d8d60-125">Par défaut, l’implémentation du service gRPC peut résoudre d’autres services DI avec n’importe quelle durée de vie (Singleton, étendue ou transitoire).</span><span class="sxs-lookup"><span data-stu-id="d8d60-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="d8d60-126">Résoudre HttpContext dans les méthodes gRPC</span><span class="sxs-lookup"><span data-stu-id="d8d60-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="d8d60-127">L’API gRPC permet d’accéder à certaines données de message HTTP/2, telles que la méthode, l’hôte, l’en-tête et les codes de fin.</span><span class="sxs-lookup"><span data-stu-id="d8d60-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="d8d60-128">L’accès s’effectue `ServerCallContext` par le biais de l’argument passé à chaque méthode gRPC:</span><span class="sxs-lookup"><span data-stu-id="d8d60-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="d8d60-129">`ServerCallContext`ne fournit pas un accès complet `HttpContext` à dans toutes les API ASP.net.</span><span class="sxs-lookup"><span data-stu-id="d8d60-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="d8d60-130">La `GetHttpContext` méthode d’extension fournit un accès complet `HttpContext` au représentant le message http/2 sous-jacent dans les API ASP.net:</span><span class="sxs-lookup"><span data-stu-id="d8d60-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a><span data-ttu-id="d8d60-131">gRPC et ASP.NET Core sur macOS</span><span class="sxs-lookup"><span data-stu-id="d8d60-131">gRPC and ASP.NET Core on macOS</span></span>

<span data-ttu-id="d8d60-132">Kestrel ne prend pas en charge HTTP/2 avec [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246) sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="d8d60-132">Kestrel doesn't support HTTP/2 with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) on macOS.</span></span> <span data-ttu-id="d8d60-133">Le modèle et les exemples de ASP.NET Core gRPC utilisent TLS par défaut.</span><span class="sxs-lookup"><span data-stu-id="d8d60-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="d8d60-134">Le message d’erreur suivant s’affiche lorsque vous essayez de démarrer le serveur gRPC:</span><span class="sxs-lookup"><span data-stu-id="d8d60-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="d8d60-135">Impossible de lier à https://localhost:5001 sur l’interface de bouclage IPv4: «HTTP/2 sur TLS n’est pas pris en charge sur macOS en raison de la prise en charge ALPN manquante.».</span><span class="sxs-lookup"><span data-stu-id="d8d60-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="d8d60-136">Pour contourner ce problème, configurez Kestrel et le client gRPC pour utiliser HTTP/2 **sans** TLS.</span><span class="sxs-lookup"><span data-stu-id="d8d60-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 **without** TLS.</span></span> <span data-ttu-id="d8d60-137">Vous ne devez effectuer cette opération qu’au cours du développement.</span><span class="sxs-lookup"><span data-stu-id="d8d60-137">You should only do this during development.</span></span> <span data-ttu-id="d8d60-138">Si vous n’utilisez pas TLS, les messages gRPC sont envoyés sans chiffrement.</span><span class="sxs-lookup"><span data-stu-id="d8d60-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="d8d60-139">Kestrel doit configurer un point de terminaison HTTP/2 sans `Program.cs`TLS dans:</span><span class="sxs-lookup"><span data-stu-id="d8d60-139">Kestrel must configure a HTTP/2 endpoint without TLS in `Program.cs`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="d8d60-140">Le client gRPC doit définir le `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` commutateur sur `true` et l' `http` utiliser dans l’adresse du serveur:</span><span class="sxs-lookup"><span data-stu-id="d8d60-140">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> <span data-ttu-id="d8d60-141">HTTP/2 sans TLS doit être utilisé uniquement pendant le développement de l’application.</span><span class="sxs-lookup"><span data-stu-id="d8d60-141">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="d8d60-142">Les applications de production doivent toujours utiliser la sécurité de transport.</span><span class="sxs-lookup"><span data-stu-id="d8d60-142">Production applications should always use transport security.</span></span> <span data-ttu-id="d8d60-143">Pour plus d’informations, consultez Considérations sur la [sécurité dans gRPC pour ASP.net Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="d8d60-143">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d8d60-144">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d8d60-144">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
