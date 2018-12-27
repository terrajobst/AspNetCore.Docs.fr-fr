---
title: Différences entre SignalR et ASP.NET Core SignalR
author: tdykstra
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: fb10d6e62ff28128e6e9e5dcef55e44f25de8ad0
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425118"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Différences entre ASP.NET SignalR et ASP.NET Core SignalR

ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR. Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.

## <a name="how-to-identify-the-signalr-version"></a>Comment identifier la version de SignalR

|                      | SignalR ASP.NET | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Package NuGet serveur | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Packages NuGet clients | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Client npm Package | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Client Java | [Référentiel GitHub](https://github.com/SignalR/java-client) (déconseillée)  | Package Maven [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Type d’application serveur | ASP.NET (System.Web) ou l’auto-hébergement OWIN | ASP.NET Core |
| Plateformes serveur prises en charge | .NET framework 4.5 ou version ultérieure | .NET Framework 4.6.1 ou ultérieur<br>.NET core 2.1 ou version ultérieure |

## <a name="feature-differences"></a>Différences de fonctionnalités

### <a name="automatic-reconnects"></a>Reconnexions automatiques

Reconnexions automatique ne sont pas pris en charge dans ASP.NET Core SignalR. Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’ils souhaitent se reconnecter. Dans ASP.NET SignalR, SignalR tente de se reconnecter au serveur si la connexion est abandonnée. 

### <a name="protocol-support"></a>Prise en charge de protocole

ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol). En outre, des protocoles personnalisés peuvent être créés.

### <a name="transports"></a>Transports

Le transport Forever Frame n’est pas pris en charge dans ASP.NET Core SignalR.

## <a name="differences-on-the-server"></a>Différences sur le serveur

Les bibliothèques côté serveur ASP.NET Core SignalR sont incluses dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) qui fait partie du modèle de l'**Application Web ASP.NET Core** pour les projets Razor et MVC.

ASP.NET Core SignalR est un intergiciel (middleware) ASP.NET Core, donc il doit être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de l'appel à la méthode [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) dans la méthode `Startup.Configure`.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a>Sessions rémanentes

Le modèle de montée en puissance parallèle de SignalR ASP.NET permet aux clients de se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs. Dans ASP.NET Core SignalR, le client doit interagir avec le même serveur pour la durée de la connexion. Pour la montée en puissance parallèle à l’aide de Redis, cela signifie que les sessions rémanentes sont requises. Pour la montée en puissance parallèle à l’aide [Service Azure SignalR](/azure/azure-signalr/), sessions rémanentes ne sont pas requises, car le service gère les connexions aux clients. 

### <a name="single-hub-per-connection"></a>Hub unique par connexion

Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié. Les connexions sont apportées directement à un hun unique, plutôt qu’à une connexion unique utilisée pour partager l’accès à plusieurs hubs.

### <a name="streaming"></a>Diffusion en continu

ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) à partir du hub au client.

### <a name="state"></a>État

La possibilité de passer un état arbitraire entre les clients et le hub (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression. Il n’existe aucun équivalent de proxy de hub pour le moment.

### <a name="persistentconnection-removal"></a>Suppression de PersistentConnection

Dans ASP.NET Core SignalR, le [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) classe a été supprimée. 

### <a name="globalhost"></a>GlobalHost

ASP.NET Core offre l’injection de dépendance (DI) intégrée au framework. Services peuvent utiliser l’injection de dépendances pour accéder à la [HubContext](xref:signalr/hubcontext). Le `GlobalHost` objet qui est utilisé dans ASP.NET SignalR pour obtenir un `HubContext` n’existe pas dans ASP.NET Core SignalR.

### <a name="hubpipeline"></a>HubPipeline

ASP.NET Core SignalR ne prend pas en charge `HubPipeline` modules.

## <a name="differences-on-the-client"></a>Différences sur le client

### <a name="typescript"></a>TypeScript

Le client d’ASP.NET Core SignalR est écrit en [TypeScript](https://www.typescriptlang.org/). Vous pouvez écrire en JavaScript ou TypeScript lorsque vous utilisez le [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Le client JavaScript est hébergé sur [npm](https://www.npmjs.com/)

Dans les versions précédentes, le client JavaScript était obtenu via un package NuGet dans Visual Studio. Pour les versions Core, le package npm [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript. Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**. Utiliser npm pour obtenir et installer le package npm `@aspnet/signalr`.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.

### <a name="internet-explorer-support"></a>Prise en charge d’Internet Explorer

ASP.NET Core SignalR requiert Microsoft Internet Explorer 11 ou version ultérieure (ASP.NET SignalR pris en charge de Microsoft Internet Explorer 8 et versions ultérieur).

### <a name="javascript-client-method-syntax"></a>Syntaxe de méthode JavaScript client

La syntaxe JavaScript a changé depuis la précédente version de SignalR. Au lieu d’utiliser l’objet `$connection`, créez une connexion en utilisant l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Utilisez la méthode [on](/javascript/api/@aspnet/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Après avoir créé la méthode du client, démarrez la connexion au hub. Chaînez une méthode [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) pour loguer ou gérer les erreurs.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serveurs proxy de hub

Les serveurs proxy de hub ne sont plus générés automatiquement. Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/%40aspnet/signalr/hubconnection#invoke) à l'API sous forme de chaîne.

### <a name="net-and-other-clients"></a>.NET et autres clients

Le package NuGet `Microsoft.AspNetCore.SignalR.Client` contient les bibliothèques de client .NET pour ASP.NET Core SignalR.

Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Différences de montée en puissance parallèle

ASP.NET SignalR prend en charge SQL Server et Redis. ASP.NET Core SignalR prend en charge le Service Azure SignalR et Redis.

### <a name="aspnet"></a>ASP.NET

* [Montée en puissance parallèle de SignalR avec Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Montée en puissance parallèle de SignalR avec Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [Montée en puissance parallèle de SignalR avec SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Service Azure SignalR](/azure/azure-signalr/)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Client .NET](xref:signalr/dotnet-client)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
