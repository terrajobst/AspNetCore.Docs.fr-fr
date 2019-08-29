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

Dans *Startup.cs* :

* gRPC est activé avec la `AddGrpc` méthode.
* Chaque service gRPC est ajouté au pipeline de routage via `MapGrpcService` la méthode.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

ASP.NET Core intergiciels et les fonctionnalités partagent le pipeline de routage, une application peut donc être configurée pour servir des gestionnaires de demandes supplémentaires. Les gestionnaires de demandes supplémentaires, tels que les contrôleurs MVC, fonctionnent en parallèle avec les services gRPC configurés.

### <a name="configure-kestrel"></a>Configurer Kestrel

Points de terminaison Kestrel gRPC:

* Nécessite HTTP/2.
* Doit être sécurisé avec HTTPs.

#### <a name="http2"></a>HTTP/2

Kestrel [prend en charge http/2](xref:fundamentals/servers/kestrel#http2-support) sur la plupart des systèmes d’exploitation modernes. Les points de terminaison Kestrel sont configurés pour prendre en charge les connexions HTTP/1.1 et HTTP/2 par défaut.

> [!NOTE]
> macOS ne prend pas en charge ASP.NET Core gRPC avec [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246). Une configuration supplémentaire est nécessaire pour exécuter correctement les services gRPC sur MacOS. Pour plus d’informations, consultez [Impossible de démarrer l’application ASP.NET Core gRPC sur MacOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).

#### <a name="https"></a>HTTPS

Les points de terminaison Kestrel utilisés pour gRPC doivent être sécurisés avec HTTPs. En cours de développement, un point de terminaison HTTPS `https://localhost:5001` est automatiquement créé lorsque le certificat de développement ASP.net Core est présent. Aucune configuration n’est requise.

En production, HTTPS doit être explicitement configuré. Dans l’exemple *appSettings. JSON* suivant, un point de terminaison http/2 sécurisé avec HTTPS est fourni:

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

Vous pouvez également configurer Kestrel endspoints dans *Program.cs*:

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

Pour plus d’informations sur l’activation de HTTP/2 et HTTPs avec Kestrel, consultez [configuration du point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

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

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
