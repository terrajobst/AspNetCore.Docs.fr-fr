---
title: Journalisation et diagnostics dans gRPC sur .NET
author: jamesnk
description: Découvrez Comment collecter des diagnostics à partir de votre application gRPC sur .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250736"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Journalisation et diagnostics dans gRPC sur .NET

Par [James Newton-King](https://twitter.com/jamesnk)

Cet article fournit des conseils pour la collecte des diagnostics à partir de votre application gRPC afin de vous aider à résoudre les problèmes.

## <a name="grpc-services-logging"></a>journalisation des services gRPC

> [!WARNING]
> Les journaux côté serveur peuvent contenir des informations sensibles de votre application. **Ne jamais** poster des journaux bruts à partir d’applications de production vers des forums publics tels que github.

Étant donné que les services gRPC sont hébergés sur ASP.NET Core, ils utilisent le système de journalisation ASP.NET Core. Dans la configuration par défaut, gRPC enregistre très peu d’informations, mais cela peut être configuré. Pour plus d’informations sur la configuration de la journalisation des ASP.NET Core, consultez la documentation sur [ASP.net Core Logging](xref:fundamentals/logging/index#configuration) .

gRPC ajoute des journaux sous `Grpc` la catégorie. Pour activer les journaux détaillés à partir de `Grpc` gRPC, configurez les préfixes sur le `Debug` niveau dans votre fichier *appSettings. JSON* en ajoutant les éléments `Logging`suivants à la `LogLevel` sous-section dans :

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

Vous pouvez également configurer cette configuration dans Startup.cs `ConfigureLogging`avec :

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

Si vous n’utilisez pas la configuration basée sur JSON, définissez la valeur de configuration suivante dans votre système de configuration :

* `Logging:LogLevel:Grpc` = `Debug`

Consultez la documentation de votre système de configuration pour déterminer comment spécifier les valeurs de configuration imbriquées. Par exemple, lors de l’utilisation de variables `_` d’environnement, deux caractères sont `:` utilisés à la place `Logging__LogLevel__Grpc`de (par exemple,).

Nous vous recommandons d' `Debug` utiliser le niveau lorsque vous collectez des diagnostics plus détaillés pour votre application. Le `Trace` niveau produit des diagnostics de bas niveau et est rarement nécessaire pour diagnostiquer les problèmes dans votre application.

### <a name="sample-logging-output"></a>Exemple de sortie de la journalisation

Voici un exemple de sortie de console au `Debug` niveau d’un service gRPC :

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a>Accéder aux journaux côté serveur

La façon dont vous accédez aux journaux côté serveur dépend de l’environnement dans lequel vous exécutez.

### <a name="as-a-console-app"></a>En tant qu’application console

Si vous exécutez dans une application console, l’enregistreur d’événements de [console](xref:fundamentals/logging/index#console-provider) doit être activé par défaut. les journaux gRPC s’affichent dans la console.

### <a name="other-environments"></a>Autres environnements

Si l’application est déployée dans un autre environnement (par exemple, docker, Kubernetes ou service Windows), <xref:fundamentals/logging/index> consultez pour plus d’informations sur la configuration des fournisseurs de journalisation appropriés pour l’environnement.

## <a name="grpc-client-logging"></a>journalisation du client gRPC

> [!WARNING]
> Les journaux côté client peuvent contenir des informations sensibles de votre application. **Ne jamais** poster des journaux bruts à partir d’applications de production vers des forums publics tels que github.

Pour récupérer des journaux à partir du client .net, vous pouvez `GrpcChannelOptions.LoggerFactory` définir la propriété lorsque le canal du client est créé. Si vous appelez un service gRPC à partir d’une application ASP.NET Core, la fabrique d’enregistreur peut être résolue à partir de l’injection de dépendances (DI) :

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

Une autre façon d’activer la journalisation du client consiste à utiliser la [fabrique de clients gRPC](xref:grpc/clientfactory) pour créer le client. Un client gRPC inscrit auprès de la fabrique de clients et résolu à partir de DI utilisera automatiquement la journalisation configurée de l’application.

Si votre application n’utilise pas di, vous pouvez créer une `ILoggerFactory` nouvelle instance avec [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Pour accéder à cette méthode, ajoutez le package [Microsoft. extensions. logginging](https://www.nuget.org/packages/microsoft.extensions.logging/) à votre application.

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>Exemple de sortie de la journalisation

Voici un exemple de sortie de console au `Debug` niveau d’un client gRPC :

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
