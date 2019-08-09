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
# <a name="grpc-services-with-aspnet-core"></a>Services gRPC avec ASP.NET Core

Ce document montre comment prendre en main les services gRPC à l’aide de ASP.NET Core.

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Bien démarrer avec le service gRPC dans ASP.NET Core

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Pour obtenir des instructions détaillées sur la création d’un projet gRPC, consultez [prise en main des services gRPC](xref:tutorials/grpc/grpc-start) .

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Ajouter des services gRPC à une application ASP.NET Core

gRPC requiert le package [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .

### <a name="configure-grpc"></a>Configurer gRPC

gRPC est activé avec la `AddGrpc` méthode:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Chaque service gRPC est ajouté au pipeline de routage via `MapGrpcService` la méthode:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core intergiciels et les fonctionnalités partagent le pipeline de routage, une application peut donc être configurée pour servir des gestionnaires de demandes supplémentaires. Les gestionnaires de demandes supplémentaires, tels que les contrôleurs MVC, fonctionnent en parallèle avec les services gRPC configurés.

## <a name="integration-with-aspnet-core-apis"></a>Intégration avec les API ASP.NET Core

les services gRPC ont un accès complet aux fonctionnalités de ASP.NET Core telles que l' [injection](xref:fundamentals/dependency-injection) de dépendances (di) et la [journalisation](xref:fundamentals/logging/index). Par exemple, l’implémentation de service peut résoudre un service d’enregistreur d’événements à partir du conteneur DI via le constructeur:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Par défaut, l’implémentation du service gRPC peut résoudre d’autres services DI avec n’importe quelle durée de vie (Singleton, étendue ou transitoire).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Résoudre HttpContext dans les méthodes gRPC

L’API gRPC permet d’accéder à certaines données de message HTTP/2, telles que la méthode, l’hôte, l’en-tête et les codes de fin. L’accès s’effectue `ServerCallContext` par le biais de l’argument passé à chaque méthode gRPC:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`ne fournit pas un accès complet `HttpContext` à dans toutes les API ASP.net. La `GetHttpContext` méthode d’extension fournit un accès complet `HttpContext` au représentant le message http/2 sous-jacent dans les API ASP.net:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a>gRPC et ASP.NET Core sur macOS

Kestrel ne prend pas en charge HTTP/2 avec [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246) sur MacOS. Le modèle et les exemples de ASP.NET Core gRPC utilisent TLS par défaut. Le message d’erreur suivant s’affiche lorsque vous essayez de démarrer le serveur gRPC:

> Impossible de lier à https://localhost:5001 sur l’interface de bouclage IPv4: «HTTP/2 sur TLS n’est pas pris en charge sur macOS en raison de la prise en charge ALPN manquante.».

Pour contourner ce problème, configurez Kestrel et le client gRPC pour utiliser HTTP/2 **sans** TLS. Vous ne devez effectuer cette opération qu’au cours du développement. Si vous n’utilisez pas TLS, les messages gRPC sont envoyés sans chiffrement.

Kestrel doit configurer un point de terminaison HTTP/2 sans `Program.cs`TLS dans:

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

Le client gRPC doit définir le `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` commutateur sur `true` et l' `http` utiliser dans l’adresse du serveur:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> HTTP/2 sans TLS doit être utilisé uniquement pendant le développement de l’application. Les applications de production doivent toujours utiliser la sécurité de transport. Pour plus d’informations, consultez Considérations sur la [sécurité dans gRPC pour ASP.net Core](xref:grpc/security#transport-security).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
