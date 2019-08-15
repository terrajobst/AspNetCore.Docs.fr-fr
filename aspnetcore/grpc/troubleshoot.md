---
title: Résoudre les problèmes de gRPC sur .NET Core
author: jamesnk
description: Résoudre les erreurs lors de l’utilisation de gRPC sur .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/12/2019
uid: grpc/troubleshoot
ms.openlocfilehash: ad74bfa57d2dde316734d55d86075f463e78ee56
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029032"
---
# <a name="troubleshoot-grpc-on-net-core"></a>Résoudre les problèmes de gRPC sur .NET Core

Par [James Newton-King](https://twitter.com/jamesnk)

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>Incompatibilité entre la configuration SSL/TLS du client et du service

Le modèle gRPC et les exemples utilisent le protocole [TLS (Transport Layer Security)](https://tools.ietf.org/html/rfc5246) pour sécuriser les services gRPC par défaut. les clients gRPC doivent utiliser une connexion sécurisée pour appeler correctement les services gRPC sécurisés.

Vous pouvez vérifier que ASP.NET Core le service gRPC utilise TLS dans les journaux écrits au démarrage de l’application. Le service est à l’écoute sur un point de terminaison HTTPs:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

Le client .net Core doit utiliser `https` dans l’adresse du serveur pour effectuer des appels avec une connexion sécurisée:

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

Toutes les implémentations du client gRPC prennent en charge TLS. les clients gRPC d’autres langages requièrent généralement le `SslCredentials`canal configuré avec. `SslCredentials`Spécifie le certificat que le client utilisera, et il doit être utilisé à la place d’informations d’identification non sécurisées. Pour obtenir des exemples de configuration des différentes implémentations du client gRPC pour utiliser TLS, consultez [authentification gRPC](https://www.grpc.io/docs/guides/auth/).

## <a name="call-insecure-grpc-services-with-net-core-client"></a>Appeler des services gRPC non sécurisés avec un client .NET Core

Une configuration supplémentaire est requise pour appeler des services gRPC non sécurisés avec le client .NET Core. Le client gRPC doit définir le `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` commutateur sur `true` et l' `http` utiliser dans l’adresse du serveur:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>Impossible de démarrer ASP.NET Core application gRPC sur macOS

Kestrel ne prend pas en charge HTTP/2 avec TLS sur macOS et les anciennes versions de Windows telles que Windows 7. Le modèle et les exemples de ASP.NET Core gRPC utilisent TLS par défaut. Le message d’erreur suivant s’affiche lorsque vous essayez de démarrer le serveur gRPC:

> Impossible de lier à https://localhost:5001 sur l’interface de bouclage IPv4: «HTTP/2 sur TLS n’est pas pris en charge sur macOS en raison de la prise en charge ALPN manquante.».

Pour contourner ce problème, configurez Kestrel et le client gRPC pour utiliser HTTP/2 *sans* TLS. Vous ne devez effectuer cette opération qu’au cours du développement. Si vous n’utilisez pas TLS, les messages gRPC sont envoyés sans chiffrement.

Kestrel doit configurer un point de terminaison HTTP/2 sans TLS dans *Program.cs*:

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

Le client gRPC doit également être configuré de façon à ne pas utiliser TLS. Pour plus d’informations, consultez [appeler des services gRPC non sécurisés avec un client .net Core](#call-insecure-grpc-services-with-net-core-client).

> [!WARNING]
> HTTP/2 sans TLS doit être utilisé uniquement pendant le développement de l’application. Les applications de production doivent toujours utiliser la sécurité de transport. Pour plus d’informations, consultez Considérations sur la [sécurité dans gRPC pour ASP.net Core](xref:grpc/security#transport-security).

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>les C# ressources gRPC ne sont pas générées par du code à partir de  *\*fichiers. proto*

la génération de code gRPC des clients concrets et des classes de base de service requiert que les fichiers et les outils protobuf soient référencés à partir d’un projet. Vous devez inclure les éléments suivants:

* fichiers *. proto* que vous souhaitez utiliser dans le `<Protobuf>` groupe d’éléments. Les [fichiers *. proto* ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) importés doivent être référencés par le projet.
* Référence de package au package d’outils gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).

Pour plus d’informations sur la C# génération de ressources <xref:grpc/basics>gRPC, consultez.

Par défaut, une `<Protobuf>` référence génère un client concret et une classe de base de service. L’attribut de l' `GrpcServices` élément de référence peut être utilisé C# pour limiter la génération d’éléments multimédias. Les `GrpcServices` options valides sont les suivantes:

* `Both`(valeur par défaut absente)
* `Server`
* `Client`
* `None`

Une application Web ASP.NET Core hébergeant gRPC services a uniquement besoin de la classe de base de service générée:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

Une application cliente gRPC qui effectue des appels gRPC a uniquement besoin du client concret généré:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```
