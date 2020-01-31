---
title: gRPC dans les applications de navigateur
author: jamesnk
description: Découvrez comment configurer les services gRPC sur ASP.NET Core à appeler à partir d’applications de navigateur à l’aide de gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/24/2020
uid: grpc/browser
ms.openlocfilehash: 6359c3b76b3cb1ba2b6d9f9a989f64cbf4c4379d
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830659"
---
# <a name="grpc-in-browser-apps"></a>gRPC dans les applications de navigateur

Par [James Newton-King](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **gRPC-la prise en charge de Web dans .NET est expérimentale**
>
> gRPC-Web pour .NET est un projet expérimental, et non un produit validé. Nous voulons :
>
> * Testez que notre approche de l’implémentation de gRPC-Web fonctionne.
> * Recevez des commentaires sur si cette approche est utile pour les développeurs .NET par rapport à la méthode traditionnelle de configuration de gRPC-Web via un proxy.
>
> N’hésitez pas à nous faire part de vos commentaires au [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) pour nous assurer que nous créons des éléments dont les développeurs aiment et sont productifs.

Il n’est pas possible d’appeler un service gRPC HTTP/2 à partir d’une application basée sur un navigateur. [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) est un protocole qui permet aux applications JavaScript et éblouissantes de navigateur d’appeler des services gRPC. Cet article explique comment utiliser gRPC-Web dans .NET Core.

## <a name="configure-grpc-web-in-aspnet-core"></a>Configurer gRPC-Web dans ASP.NET Core

les services gRPC hébergés dans ASP.NET Core peuvent être configurés pour prendre en charge gRPC-Web parallèlement à HTTP/2 gRPC. gRPC-Web ne nécessite aucune modification des services. La seule modification concerne la configuration du démarrage.

Pour activer gRPC-Web avec un service ASP.NET Core gRPC :

* Ajoutez une référence au package [GRPC. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .
* Configurez l’application pour utiliser gRPC-Web en ajoutant `AddGrpcWeb` et `UseGrpcWeb` à *Startup.cs*:

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=3,10,14)]

Le code précédent :

* Ajoute l’intergiciel gRPC-Web, `UseGrpcWeb`, après le routage et avant les points de terminaison.
* Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge gRPC-Web avec `EnableGrpcWeb`. 

Vous pouvez également configurer tous les services pour prendre en charge gRPC-Web en ajoutant des `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` à ConfigureServices.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=5,12,16)]

Une configuration supplémentaire peut être nécessaire pour appeler gRPC-Web à partir du navigateur, par exemple la configuration de ASP.NET Core pour prendre en charge CORS. Pour plus d’informations, consultez [prise en charge de cors](xref:security/cors).

## <a name="call-grpc-web-from-the-browser"></a>Appeler gRPC-Web à partir du navigateur

Les applications de navigateur peuvent utiliser gRPC-Web pour appeler des services gRPC. Il existe certaines exigences et limitations lors de l’appel de services gRPC avec gRPC-Web à partir du navigateur :

* Le serveur doit avoir été configuré pour prendre en charge gRPC-Web.
* Les appels de diffusion en continu et bidirectionnelle du client ne sont pas pris en charge. La diffusion en continu du serveur est prise en charge.
* L’appel de services gRPC sur un autre domaine requiert la configuration de [cors](xref:security/cors) sur le serveur.

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-client Web

Il existe un client JavaScript gRPC-Web. Pour obtenir des instructions sur l’utilisation de gRPC-Web à partir de JavaScript, consultez [écrire du code JavaScript client avec gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>Configurer gRPC-Web avec le client .NET gRPC

Le client .NET gRPC peut être configuré pour effectuer des appels gRPC-Web. Cela est utile pour les applications de [Webassembly éblouissantes](xref:blazor/index#blazor-webassembly) , qui sont hébergées dans le navigateur et qui ont les mêmes limitations http du code JavaScript. L’appel de gRPC-Web avec un client .NET est [identique à http/2 gRPC](xref:grpc/client). La seule modification est la manière dont le canal est créé.

Pour utiliser gRPC-Web :

* Ajoutez une référence au package [GRPC .net. client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .
* Configurez le canal pour utiliser le `GrpcWebHandler`:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

Le code précédent :

* Configure un canal pour utiliser gRPC-Web.
* Crée un client et effectue un appel à l’aide du canal.

La `GrpcWebHandler` possède les options de configuration suivantes au moment de sa création :

* **InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sous-jacent qui effectue l’appel http, par exemple, `HttpClientHandler`.
* **Mode**: `GrpcWebMode` Enum. `GrpcWebMode.GrpcWebText` configure le contenu pour qu’il soit encodé en base64, ce qui est nécessaire pour prendre en charge les appels de diffusion en continu du serveur.
* **HttpVersion**: `Version`de protocole http. gRPC-Web ne nécessite pas de protocole spécifique et n’en spécifie pas pour effectuer une demande, sauf si elle est configurée.

## <a name="additional-resources"></a>Ressources supplémentaires

* [gRPC pour le projet GitHub des clients Web](https://github.com/grpc/grpc-web)
* <xref:security/cors>
