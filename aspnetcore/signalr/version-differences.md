---
title: Différences entre Signalr et ASP.NET Core Signalr
author: bradygaster
description: Différences entre Signalr et ASP.NET Core Signalr
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746535"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Différences entre ASP.NET Signalr et ASP.NET Core Signalr

ASP.NET Core Signalr n’est pas compatible avec les clients ou les serveurs pour ASP.NET Signalr. Cet article détaille les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core Signalr.

## <a name="how-to-identify-the-signalr-version"></a>Comment identifier la version de Signalr

|                      | SignalR ASP.NET | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Package NuGet serveur | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.net Core)<br>[Microsoft. AspNetCore. signalr](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Packages NuGet client | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Package NPM client | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Client Java | [Dépôt github](https://github.com/SignalR/java-client) déconseillé  | Package Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Type d’application serveur | ASP.NET (System. Web) ou auto-hôte OWIN | ASP.NET Core |
| Plateformes serveur prises en charge | .NET Framework 4,5 ou version ultérieure | .NET Framework 4.6.1 ou ultérieur<br>.NET Core 2,1 ou version ultérieure |

## <a name="feature-differences"></a>Différences de fonctionnalités

### <a name="automatic-reconnects"></a>Reconnexions automatiques

Les reconnexions automatiques ne sont pas prises en charge dans ASP.NET Core Signalr. Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’il souhaite se reconnecter. Dans ASP.NET Signalr, signaler tente de se reconnecter au serveur si la connexion est abandonnée.

### <a name="protocol-support"></a>Prise en charge de protocole

ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol). En outre, des protocoles personnalisés peuvent être créés.

### <a name="transports"></a>Transports

Le transport de trame éternel n’est pas pris en charge dans ASP.NET Core Signalr.

## <a name="differences-on-the-server"></a>Différences sur le serveur

Les bibliothèques côté serveur ASP.NET Core SignalR sont incluses dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) qui fait partie du modèle de l'**Application Web ASP.NET Core** pour les projets Razor et MVC.

ASP.NET Core Signalr est un intergiciel ASP.NET Core. il doit donc être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Pour configurer le routage, mappez les itinéraires aux hubs à l’intérieur de l’appel de la méthode [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) dans la méthode `Startup.Configure`.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de l'appel à la méthode [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) dans la méthode `Startup.Configure`.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Sessions rémanentes

Le modèle ScaleOut pour ASP.NET Signalr permet aux clients de se reconnecter et d’envoyer des messages à n’importe quel serveur de la batterie. Dans ASP.NET Core Signalr, le client doit interagir avec le même serveur pendant la durée de la connexion. Pour les ScaleOut utilisant des ReDim, cela signifie que les sessions rémanentes sont requises. Pour ScaleOut à l’aide du [service Azure signalr](/azure/azure-signalr/), les sessions rémanentes ne sont pas nécessaires, car le service gère les connexions aux clients.

### <a name="single-hub-per-connection"></a>Concentrateur unique par connexion

Dans ASP.NET Core Signalr, le modèle de connexion a été simplifié. Les connexions sont apportées directement à un hun unique, plutôt qu’à une connexion unique utilisée pour partager l’accès à plusieurs hubs.

### <a name="streaming"></a>Diffusion en continu

ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) à partir du hub au client.

### <a name="state"></a>État

La possibilité de passer un état arbitraire entre les clients et le hub (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression. Il n’existe aucun équivalent de proxy de Hub pour le moment.

### <a name="persistentconnection-removal"></a>Suppression de PersistentConnection

Dans ASP.NET Core Signalr, la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) a été supprimée.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core a intégré l’injection de dépendances dans le Framework. Les services peuvent utiliser DI pour accéder à [HubContext](xref:signalr/hubcontext). L' `GlobalHost` objet utilisé dans ASP.net signalr pour obtenir un `HubContext` n’existe pas dans ASP.net Core signalr.

### <a name="hubpipeline"></a>HubPipeline

ASP.net Core signalr ne prend pas en `HubPipeline` charge les modules.

## <a name="differences-on-the-client"></a>Différences sur le client

### <a name="typescript"></a>TypeScript

Le client d’ASP.NET Core SignalR est écrit en [TypeScript](https://www.typescriptlang.org/). Vous pouvez écrire en JavaScript ou à la machine à écrire lors de l’utilisation du [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Le client JavaScript est hébergé sur [npm](https://www.npmjs.com/)

Dans les versions précédentes, le client JavaScript était obtenu via un package NuGet dans Visual Studio. Pour les versions Core, le package npm [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript. Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**. Utiliser npm pour obtenir et installer le package npm `@aspnet/signalr`.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.

### <a name="internet-explorer-support"></a>Prise en charge d’Internet Explorer

ASP.NET Core Signalr requiert Microsoft Internet Explorer 11 ou une version ultérieure (ASP.NET Signalr est pris en charge par Microsoft Internet Explorer 8 et versions ultérieures).

### <a name="javascript-client-method-syntax"></a>Syntaxe de méthode du client JavaScript

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

### <a name="hub-proxies"></a>Proxies Hub

Les serveurs proxy de hub ne sont plus générés automatiquement. Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/%40aspnet/signalr/hubconnection#invoke) à l'API sous forme de chaîne.

### <a name="net-and-other-clients"></a>.NET et autres clients

Le package NuGet `Microsoft.AspNetCore.SignalR.Client` contient les bibliothèques de client .NET pour ASP.NET Core SignalR.

Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Différences ScaleOut

ASP.NET Signalr prend en charge les SQL Server et les ReDim. ASP.NET Core Signalr prend en charge le service et les ReDim Azure Signalr.

### <a name="aspnet"></a>ASP.NET

* [Signalr ScaleOut avec Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Signalr ScaleOut avec Redims](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [Signalr ScaleOut avec SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Service Azure SignalR](/azure/azure-signalr/)
* [Fond de panier ReDim](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Client .NET](xref:signalr/dotnet-client)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
