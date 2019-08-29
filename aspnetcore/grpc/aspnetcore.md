---
title: Services gRPC avec ASP.NET Core
author: juntaoluo
description: Découvrez les concepts de base liés à l’écriture de services gRPC avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/28/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 128f5b36eac9112460c33693db5537134a077476
ms.sourcegitcommit: 23f79bd71d49c4efddb56377c1f553cc993d781b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70130705"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="eedaa-103">Services gRPC avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eedaa-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="eedaa-104">Ce document montre comment prendre en main les services gRPC à l’aide de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eedaa-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eedaa-105">Prérequis</span><span class="sxs-lookup"><span data-stu-id="eedaa-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eedaa-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eedaa-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eedaa-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eedaa-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eedaa-108">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="eedaa-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="eedaa-109">Bien démarrer avec le service gRPC dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eedaa-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="eedaa-110">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="eedaa-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eedaa-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eedaa-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eedaa-112">Pour obtenir des instructions détaillées sur la création d’un projet gRPC, consultez [prise en main des services gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="eedaa-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="eedaa-113">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="eedaa-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="eedaa-114">Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="eedaa-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="eedaa-115">Ajouter des services gRPC à une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eedaa-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="eedaa-116">gRPC requiert le package [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="eedaa-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="eedaa-117">Configurer gRPC</span><span class="sxs-lookup"><span data-stu-id="eedaa-117">Configure gRPC</span></span>

<span data-ttu-id="eedaa-118">Dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="eedaa-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="eedaa-119">gRPC est activé avec la `AddGrpc` méthode.</span><span class="sxs-lookup"><span data-stu-id="eedaa-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="eedaa-120">Chaque service gRPC est ajouté au pipeline de routage via `MapGrpcService` la méthode.</span><span class="sxs-lookup"><span data-stu-id="eedaa-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="eedaa-121">ASP.NET Core intergiciels et les fonctionnalités partagent le pipeline de routage, une application peut donc être configurée pour servir des gestionnaires de demandes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="eedaa-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="eedaa-122">Les gestionnaires de demandes supplémentaires, tels que les contrôleurs MVC, fonctionnent en parallèle avec les services gRPC configurés.</span><span class="sxs-lookup"><span data-stu-id="eedaa-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="eedaa-123">Configurer Kestrel</span><span class="sxs-lookup"><span data-stu-id="eedaa-123">Configure Kestrel</span></span>

<span data-ttu-id="eedaa-124">Points de terminaison Kestrel gRPC:</span><span class="sxs-lookup"><span data-stu-id="eedaa-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="eedaa-125">Nécessite HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="eedaa-125">Require HTTP/2.</span></span>
* <span data-ttu-id="eedaa-126">Doit être sécurisé avec HTTPs.</span><span class="sxs-lookup"><span data-stu-id="eedaa-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="eedaa-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="eedaa-127">HTTP/2</span></span>

<span data-ttu-id="eedaa-128">Kestrel [prend en charge http/2](xref:fundamentals/servers/kestrel#http2-support) sur la plupart des systèmes d’exploitation modernes.</span><span class="sxs-lookup"><span data-stu-id="eedaa-128">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="eedaa-129">Les points de terminaison Kestrel sont configurés pour prendre en charge les connexions HTTP/1.1 et HTTP/2 par défaut.</span><span class="sxs-lookup"><span data-stu-id="eedaa-129">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

> [!NOTE]
> <span data-ttu-id="eedaa-130">macOS ne prend pas en charge ASP.NET Core gRPC avec [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="eedaa-130">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="eedaa-131">Une configuration supplémentaire est nécessaire pour exécuter correctement les services gRPC sur MacOS.</span><span class="sxs-lookup"><span data-stu-id="eedaa-131">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="eedaa-132">Pour plus d’informations, consultez [Impossible de démarrer l’application ASP.NET Core gRPC sur MacOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="eedaa-132">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

#### <a name="https"></a><span data-ttu-id="eedaa-133">HTTPS</span><span class="sxs-lookup"><span data-stu-id="eedaa-133">HTTPS</span></span>

<span data-ttu-id="eedaa-134">Les points de terminaison Kestrel utilisés pour gRPC doivent être sécurisés avec HTTPs.</span><span class="sxs-lookup"><span data-stu-id="eedaa-134">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="eedaa-135">En cours de développement, un point de terminaison HTTPS `https://localhost:5001` est automatiquement créé lorsque le certificat de développement ASP.net Core est présent.</span><span class="sxs-lookup"><span data-stu-id="eedaa-135">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="eedaa-136">Aucune configuration n’est requise.</span><span class="sxs-lookup"><span data-stu-id="eedaa-136">No configuration is required.</span></span>

<span data-ttu-id="eedaa-137">En production, HTTPS doit être explicitement configuré.</span><span class="sxs-lookup"><span data-stu-id="eedaa-137">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="eedaa-138">Dans l’exemple *appSettings. JSON* suivant, un point de terminaison http/2 sécurisé avec HTTPS est fourni:</span><span class="sxs-lookup"><span data-stu-id="eedaa-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="eedaa-139">Vous pouvez également configurer Kestrel endspoints dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="eedaa-139">Alternatively, Kestrel endspoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="eedaa-140">Pour plus d’informations sur l’activation de HTTP/2 et HTTPs avec Kestrel, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="eedaa-140">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="eedaa-141">Intégration avec les API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eedaa-141">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="eedaa-142">les services gRPC ont un accès complet aux fonctionnalités de ASP.NET Core telles que l' [injection](xref:fundamentals/dependency-injection) de dépendances (di) et la [journalisation](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="eedaa-142">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="eedaa-143">Par exemple, l’implémentation de service peut résoudre un service d’enregistreur d’événements à partir du conteneur DI via le constructeur:</span><span class="sxs-lookup"><span data-stu-id="eedaa-143">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="eedaa-144">Par défaut, l’implémentation du service gRPC peut résoudre d’autres services DI avec n’importe quelle durée de vie (Singleton, étendue ou transitoire).</span><span class="sxs-lookup"><span data-stu-id="eedaa-144">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="eedaa-145">Résoudre HttpContext dans les méthodes gRPC</span><span class="sxs-lookup"><span data-stu-id="eedaa-145">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="eedaa-146">L’API gRPC permet d’accéder à certaines données de message HTTP/2, telles que la méthode, l’hôte, l’en-tête et les codes de fin.</span><span class="sxs-lookup"><span data-stu-id="eedaa-146">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="eedaa-147">L’accès s’effectue `ServerCallContext` par le biais de l’argument passé à chaque méthode gRPC:</span><span class="sxs-lookup"><span data-stu-id="eedaa-147">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="eedaa-148">`ServerCallContext`ne fournit pas un accès complet `HttpContext` à dans toutes les API ASP.net.</span><span class="sxs-lookup"><span data-stu-id="eedaa-148">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="eedaa-149">La `GetHttpContext` méthode d’extension fournit un accès complet `HttpContext` au représentant le message http/2 sous-jacent dans les API ASP.net:</span><span class="sxs-lookup"><span data-stu-id="eedaa-149">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="eedaa-150">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="eedaa-150">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
