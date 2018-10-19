---
title: Différences entre SignalR et ASP.NET Core SignalR
author: tdykstra
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: ea2de2606a99de70fa645c0c42303525fea0a44e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325534"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Différences entre ASP.NET SignalR et ASP.NET Core SignalR

ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR. Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.

## <a name="how-to-identify-the-signalr-version"></a>Comment identifier la version de SignalR

|                      | SignalR ASP.NET | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Package NuGet de serveur | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Packages NuGet clients | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Client npm Package | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Type d’application serveur | ASP.NET (System.Web) ou l’auto-hébergement OWIN | ASP.NET Core |
| Plateformes de serveur pris en charge | .NET framework 4.5 ou version ultérieure | .NET Framework 4.6.1 ou ultérieur<br>.NET core 2.1 ou version ultérieure |

## <a name="feature-differences"></a>Différences de fonctionnalités

### <a name="automatic-reconnects"></a>Reconnexions automatique

Reconnexions automatique ne sont pas pris en charge dans ASP.NET Core SignalR. Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’ils souhaitent se reconnecter. Dans ASP.NET SignalR, SignalR tente de se reconnecter au serveur si la connexion est abandonnée. 

### <a name="protocol-support"></a>Prise en charge du protocole

ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire selon [MessagePack](xref:signalr/messagepackhubprotocol). En outre, les protocoles personnalisés peuvent être créés.

## <a name="differences-on-the-server"></a>Différences sur le serveur

Les bibliothèques de côté serveur ASP.NET Core SignalR sont incluses dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) package qui fait partie de la **Application Web ASP.NET Core** modèle pour Razor et MVC projets.

ASP.NET Core SignalR est un intergiciel (middleware) ASP.NET Core, donc il doit être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de la [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) appel de méthode dans le `Startup.Configure` (méthode).

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sessions rémanentes désormais requises

En raison de la montée en puissance travaillé dans ASP.NET SignalR, les clients peuvent se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs. En raison de modifications pour le modèle de montée en puissance, mais aussi ne prenant ne pas en charge se reconnecte, il est n’est plus pris en charge. Une fois que le client se connecte au serveur, il doit interagir avec le même serveur pour la durée de la connexion.

### <a name="single-hub-per-connection"></a>Hub unique par connexion

Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié. Les connexions sont apportées directement à un concentrateur unique, plutôt que d’une connexion unique utilisé pour partager l’accès à plusieurs concentrateurs.

### <a name="streaming"></a>Diffusion en continu

Prend en charge ASP.NET Core SignalR [diffusion en continu des données](xref:signalr/streaming) à partir du concentrateur au client.

### <a name="state"></a>État

La possibilité de passer l’état arbitraire entre les clients et le concentrateur (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression. Il n’existe aucun équivalent de proxy de hub pour le moment.

## <a name="differences-on-the-client"></a>Connaître les différences entre le client

### <a name="typescript"></a>TypeScript

Le client d’ASP.NET Core SignalR est écrit [TypeScript](https://www.typescriptlang.org/). Vous pouvez écrire en JavaScript ou TypeScript lorsque vous utilisez le [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Le client JavaScript est hébergé à [npm](https://www.npmjs.com/)

Dans les versions précédentes, le client JavaScript a été obtenu via un package NuGet dans Visual Studio. Pour les versions Core, le [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) package npm contient les bibliothèques JavaScript. Ce package n’est pas inclus dans le **Application Web ASP.NET Core** modèle. Utiliser npm pour obtenir et installer le `@aspnet/signalr` package npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.

### <a name="javascript-client-method-syntax"></a>Syntaxe de méthode JavaScript client

La syntaxe de JavaScript a changé depuis la précédente version de SignalR. Au lieu d’utiliser le `$connection` d’objet, de créer une connexion en utilisant le [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Utilisez le [sur](/javascript/api/@aspnet/signalr/HubConnection#on) méthode pour spécifier les méthodes de client que le hub peut appeler.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Après avoir créé la méthode du client, démarrez la connexion du concentrateur. Chaîne un [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) méthode pour vous connecter ou gérer les erreurs.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serveurs proxy de hub

Serveurs proxy de Hub est générées ne sont plus automatiquement. Au lieu de cela, le nom de méthode est passé dans le [appeler](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API sous forme de chaîne.

### <a name="net-and-other-clients"></a>.NET et autres clients

Le `Microsoft.AspNetCore.SignalR.Client` package NuGet contient les bibliothèques de client .NET pour ASP.NET Core SignalR.

Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un concentrateur.

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
