---
title: Services gRPC avec ASP.NET Core
author: juntaoluo
description: Découvrez les concepts de base lors de l’écriture des services de gRPC avec ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: ca06478e6168c59d9abf43d99213fa8a7091e178
ms.sourcegitcommit: 756114cab5e24e99ec23de62b0c1c16b15197ac2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67169531"
---
# <a name="grpc-services-with-aspnet-core"></a>Services gRPC avec ASP.NET Core

Ce document montre comment bien démarrer avec les services de gRPC à l’aide d’ASP.NET Core.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Bien démarrer avec le service gRPC dans ASP.NET Core

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Consultez [prise en main des services de gRPC](xref:tutorials/grpc/grpc-start) pour obtenir des instructions détaillées sur la façon de créer un projet de gRPC.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio pour Mac](#tab/visual-studio-code+visual-studio-mac)

Exécutez `dotnet new grpc -o GrpcGreeter` à partir de la ligne de commande.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Ajouter des services de gRPC à une application ASP.NET Core

gRPC requiert les packages suivants :

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) pour protobuf message API.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>Configurer gRPC

gRPC est activé avec le `AddGrpc` méthode :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Chaque service gRPC est ajouté au pipeline routage via le `MapGrpcService` méthode :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

Fonctionnalités et des intergiciels d’ASP.NET Core partagent le pipeline de routage, par conséquent, une application peut être configurée pour servir des gestionnaires de demandes supplémentaires. Les gestionnaires de demande supplémentaires, tels que les contrôleurs MVC, travaillent en parallèle avec les services configurés gRPC.

## <a name="integration-with-aspnet-core-apis"></a>Intégration avec les API ASP.NET Core

gRPC services ont un accès complet aux fonctionnalités ASP.NET Core comme [l’Injection de dépendances](xref:fundamentals/dependency-injection) (DI) et [journalisation](xref:fundamentals/logging/index). Par exemple, l’implémentation du service peut résoudre un service d’enregistreur d’événements à partir du conteneur d’injection de dépendances via le constructeur :

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Par défaut, l’implémentation du service gRPC peut résoudre d’autres services de l’injection de dépendances avec toute durée de vie (Singleton, délimitées ou temporaires).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Résoudre HttpContext dans les méthodes de gRPC

L’API gRPC fournit l’accès à certaines données de message HTTP/2, telles que la méthode, hôte, en-tête et codes de fin. Accès s’effectue via le `ServerCallContext` argument passé à chaque méthode gRPC :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` ne fournit pas un accès complet à `HttpContext` dans toutes les APIs ASP.NET. Le `GetHttpContext` méthode d’extension fournit un accès complet à la `HttpContext` représentant le message HTTP/2 sous-jacent dans ASP.NET APIs :

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
