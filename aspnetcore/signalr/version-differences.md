---
title: Différences entre SignalR et ASP.NET Core SignalR
author: rachelappel
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090062"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Différences entre SignalR et ASP.NET Core SignalR

ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR. Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans la base ASP.NET SignalR.

## <a name="feature-differences"></a>Différences de fonctionnalités

### <a name="automatic-reconnects"></a>Se reconnecte automatique

Se reconnecte automatique n’est plus prises en charge. Auparavant, SignalR a tenté de se reconnecter au serveur si la connexion a été supprimée. Maintenant, si le client est déconnecté l’utilisateur doit démarrer explicitement une nouvelle connexion, s’ils souhaitent se reconnecter.

### <a name="protocol-support"></a>Prise en charge du protocole

ASP.NET Core SignalR prend en charge JSON, ainsi que d’un nouveau protocole binaire selon [MessagePack](xref:signalr/messagepackhubprotocol). En outre, les protocoles personnalisés peuvent être créés.

## <a name="differences-on-the-server"></a>Différences sur le serveur

Les bibliothèques de côté serveur SignalR sont inclus dans le `Microsoft.AspNetCore.App` package qui fait partie de la **Application ASP.NET Core Web** modèle pour les projets Razor et MVC.

SignalR étant un intergiciel (middleware) ASP.NET Core, elle doit être configurée en appelant `AddSignalR` dans `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Pour configurer le routage, mapper les itinéraires aux concentrateurs à l’intérieur de la `UseSignalR` l’appel de méthode le `Startup.Configure` (méthode).

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sessions rémanentes maintenant

En raison de la montée en puissance parallèle travaillé dans les versions précédentes de SignalR, les clients peuvent se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs. En raison de modifications pour le modèle de montée en puissance parallèle, ainsi que ne prend ne pas en charge se reconnecte, il est plus pris en charge. À présent, une fois que le client se connecte au serveur, il doit interagir avec le même serveur pour la durée de la connexion.

### <a name="single-hub-per-connection"></a>Concentrateur unique par connexion

Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié. Les connexions sont effectuées directement à un concentrateur unique, plutôt que d’une connexion unique utilisé pour partager l’accès à plusieurs concentrateurs.

### <a name="streaming"></a>Diffusion en continu

Prend en charge SignalR [diffusion en continu de données](xref:signalr/streaming) à partir du concentrateur au client.

### <a name="state"></a>État

La possibilité de passer d’un état arbitraire entre les clients et le concentrateur (souvent appelés HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression. Il n’existe aucun équivalent de proxy de hub pour le moment.

## <a name="differences-on-the-client"></a>Différences sur le client

### <a name="typescript"></a>TypeScript

La version de ASP.NET Core du SignalR est écrit [TypeScript](https://www.typescriptlang.org/). Vous pouvez écrire en JavaScript ou TypeScript lors de l’utilisation du [client JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Le client JavaScript est hébergé à [npm](https://www.npmjs.com/)

Dans les versions précédentes, le client JavaScript a été obtenu via un package NuGet dans Visual Studio. Pour les versions principales, le [ @aspnet/signalr package npm](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript. Ce package n’est pas inclus dans le **Application ASP.NET Core Web** modèle. Utilisez npm pour obtenir et installer le `@aspnet/signalr` package npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

La dépendance vis-à-vis de jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.

### <a name="javascript-client-method-syntax"></a>Syntaxe de la méthode JavaScript client

La syntaxe JavaScript a changé depuis la version précédente de SignalR. Au lieu d’utiliser le `$connection` d’objet, créez une connexion en utilisant le `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Utilisez `connection.on` pour spécifier les méthodes de client que le concentrateur peut appeler.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Après avoir créé la méthode du client, démarrez la connexion du concentrateur. Chaîne un `catch` méthode pour se connecter ou gérer les erreurs.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serveurs proxy de hub

Serveurs proxy de Hub est générés ne sont plus automatiquement. Au lieu de cela, le nom de la méthode a été passé dans le `invoke` API sous forme de chaîne.

### <a name="net-and-other-clients"></a>.NET et autres clients

Le `Microsoft.AspNetCore.SignalR.Client` package NuGet contient les bibliothèques clientes .NET pour ASP.NET Core SignalR.

Utilisez le `HubConnectionBuilder` pour créer et générer une instance d’une connexion à un concentrateur.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Hubs](xref:signalr/hubs)
* [Client JavaScript](xref:signalr/javascript-client)
* [Client .NET](xref:signalr/dotnet-client)
* [Plateformes prises en charge](xref:signalr/supported-platforms)
