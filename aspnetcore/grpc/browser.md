---
title: Utiliser gRPC dans les applications de navigateur
author: jamesnk
description: Découvrez comment configurer les services gRPC sur ASP.NET Core à appeler à partir d’applications de navigateur à l’aide de gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/16/2020
uid: grpc/browser
ms.openlocfilehash: 3beeffc26ffd3c2dc85bfc22a46d97d5fd78d3d0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664198"
---
# <a name="use-grpc-in-browser-apps"></a>Utiliser gRPC dans les applications de navigateur

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

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

Le code précédent :

* Ajoute l’intergiciel gRPC-Web, `UseGrpcWeb`, après le routage et avant les points de terminaison.
* Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge gRPC-Web avec `EnableGrpcWeb`. 

Vous pouvez également configurer tous les services pour prendre en charge gRPC-Web en ajoutant des `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` à ConfigureServices.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

### <a name="grpc-web-and-cors"></a>gRPC-Web et CORS

La sécurité du navigateur empêche une page Web d’effectuer des demandes vers un autre domaine que celui qui a servi la page Web. Cette restriction s’applique à la création d’appels gRPC-Web avec des applications de navigateur. Par exemple, une application de navigateur servie par `https://www.contoso.com` est bloquée lors de l’appel de gRPC-services Web hébergés sur `https://services.contoso.com`. Le partage des ressources Cross-Origin (CORS) peut être utilisé pour assouplir cette restriction.

Pour permettre à votre application de navigateur d’effectuer des appels gRPC-Web Cross-Origin, configurez [cors dans ASP.net Core](xref:security/cors). Utilisez la prise en charge de CORS intégrée et exposez des en-têtes spécifiques à gRPC avec <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>.

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

Le code précédent :

* Appelle `AddCors` pour ajouter des services CORS et configure une stratégie CORS qui expose des en-têtes spécifiques à gRPC.
* Appelle `UseCors` pour ajouter l’intergiciel (middleware) CORS après le routage et avant les points de terminaison.
* Spécifie que la méthode `endpoints.MapGrpcService<GreeterService>()` prend en charge CORS avec `RequiresCors`.

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
* Assurez-vous que la référence au package [GRPC .net. client](https://www.nuget.org/packages/Grpc.Net.Client) est 2.27.0 ou supérieure.
* Configurez le canal pour utiliser le `GrpcWebHandler`:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

Le code précédent :

* Configure un canal pour utiliser gRPC-Web.
* Crée un client et effectue un appel à l’aide du canal.

La `GrpcWebHandler` possède les options de configuration suivantes au moment de sa création :

* **InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sous-jacente qui effectue la requête http gRPC, par exemple, `HttpClientHandler`.
* **Mode**: type d’énumération qui spécifie si la demande de demande HTTP gRPC `Content-Type` est `application/grpc-web` ou `application/grpc-web-text`.
    * `GrpcWebMode.GrpcWeb` configure le contenu à envoyer sans encodage. Valeur par défaut.
    * `GrpcWebMode.GrpcWebText` configure le contenu pour qu’il soit encodé en base64. Requis pour les appels de diffusion en continu du serveur dans les navigateurs.
* **HttpVersion**: le protocole http `Version` utilisé pour définir [HttpRequestMessage. version](xref:System.Net.Http.HttpRequestMessage.Version) sur la requête http gRPC sous-jacente. gRPC-Web n’a pas besoin d’une version spécifique et ne remplace pas la valeur par défaut, sauf indication contraire.

> [!IMPORTANT]
> Les clients gRPC générés ont des méthodes synchrones et asynchrones pour appeler des méthodes unaires. Par exemple, `SayHello` est Sync et `SayHelloAsync` est Async. L’appel d’une méthode Sync dans une application de webassembly éblouissante entraîne le blocage de l’application. Les méthodes Async doivent toujours être utilisées dans le webassembly éblouissant.

## <a name="additional-resources"></a>Ressources supplémentaires

* [gRPC pour le projet GitHub des clients Web](https://github.com/grpc/grpc-web)
* <xref:security/cors>
