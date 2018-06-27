---
title: Configuration de base SignalR ASP.NET
author: rachelappel
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961981"
---
# <a name="aspnet-core-signalr-configuration"></a>Configuration de base SignalR ASP.NET

## <a name="jsonmessagepack-serialization-options"></a>Options de sérialisation de JSON/MessagePack

ASP.NET Core SignalR prend en charge deux protocoles de codage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html). Chaque protocole possède des options de configuration de sérialisation.

Sérialisation JSON peut être configurée sur le serveur en utilisant la [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajouté après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode). Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet. Le [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour. Consultez le [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.

Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « PascalCase », au lieu de noms par défaut « camelCase », utilisez le code suivant :

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

Dans le client .NET, le même `AddJsonHubProtocol` méthode d’extension existe bien sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.

### <a name="messagepack-serialization-options"></a>Options de sérialisation MessagePack

MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler. Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.

> [!NOTE]
> Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.

## <a name="configure-server-options"></a>Configurer les options de serveur

Le tableau suivant décrit les options de configuration de concentrateurs SignalR :

| Option | Description |
| ------ | ----------- |
| `HandshakeTimeout` | Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée. |
| `KeepAliveInterval` | Si le serveur n’a pas envoyé un message au sein de cet intervalle, un message ping est envoyé automatiquement pour maintenir ouverte la connexion. |
| `SupportedProtocols` | Protocoles pris en charge par ce concentrateur. Par défaut, tous les protocoles inscrits sur le serveur sont autorisées, mais protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour les concentrateurs individuels. |
| `EnableDetailedErrors` | Si `true`détaillées des messages d’exception sont renvoyés aux clients lorsqu’une exception est levée dans une méthode de concentrateur. La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles. |

Options peuvent être configurées pour tous les hubs en fournissant un délégué d’options pour le `AddSignalR` appeler dans `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Options pour un concentrateur unique remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liés aux transports et gestion de la mémoire tampon. Ces options sont configurées en passant un délégué pour [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Option | Description |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | Le nombre maximal d’octets reçus par le client qui les mémoires tampons de serveur. L’augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire. La valeur par défaut est 32 Ko. |
| `AuthorizationData` | Une liste de [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur. Par défaut, il est rempli avec les valeurs de la `Authorize` attributs appliqués à la classe de concentrateur. |
| `TransportMaxBufferSize` | Le nombre maximal d’octets envoyés par l’application qui les mémoires tampons de serveur. L’augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire. La valeur par défaut est 32 Ko. |
| `Transports` | Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter. Tous les transports sont activées par défaut. |
| `LongPolling` | Options supplémentaires spécifiques au transport d’interrogation longue. |
| `WebSockets` | Options supplémentaires spécifiques au transport WebSocket. |

Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la `LongPolling` propriété :

| Option | Description |
| ------ | ----------- |
| `PollTimeout` | La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une requête d’interrogation unique. Diminution de cette valeur entraîne le client émettre de nouvelles demandes d’interrogation plus fréquemment. La valeur par défaut est 90 secondes. |

Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la `WebSockets` propriété :

| Option | Description |
| ------ | ----------- |
| `CloseTimeout` | Une fois que le serveur se ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est terminée. |
| `SubProtocolSelector` | Un délégué qui peut être utilisé pour définir le `Sec-WebSocket-Protocol` en-tête à une valeur personnalisée. Le délégué reçoit les valeurs demandées par le client en tant qu’entrée et doit retourner la valeur souhaitée. |

## <a name="configure-client-options"></a>Configurer les options du client

Options du client peuvent être configurées sur le `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript), de type, ainsi que dans le `HubConnection` lui-même.

### <a name="configure-logging"></a>Configurer la journalisation

La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode). Journalisation des fournisseurs et des filtres peut être inscrits de la même façon qu’ils se trouvent sur le serveur. Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation pour plus d’informations.

> [!NOTE]
> Pour inscrire des fournisseurs de journalisation, vous devez installer les packages. Consultez le [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) section de la documentation pour obtenir la liste complète.

Par exemple, pour activer la journalisation de la Console, installez le `Microsoft.Extensions.Logging.Console` package NuGet. Appelez le `AddConsole` méthode d’extension :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

Dans le client JavaScript, similaire `configureLogging` méthode existe. Fournir un `LogLevel` valeur indiquant le niveau minimal de produire des messages du journal. Les journaux sont écrits dans la fenêtre de console du navigateur.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Pour désactiver la journalisation entièrement, spécifiez `signalR.LogLevel.None` dans le `configureLogging` (méthode).

Niveaux de journalisation disponibles au client JavaScript sont répertoriées ci-dessous. Définir le niveau de journal à une de ces valeurs permet la journalisation des messages à **ou version ultérieure** ce niveau.

| Niveau | Description |
| ----- | ----------- |
| `None` | Aucun message n’est enregistrés. |
| `Critical` | Messages qui indiquent l’échec de l’application entière. |
| `Error` | Messages qui indiquent l’échec de l’opération en cours. |
| `Warning` | Messages qui indiquent un problème non fatale. |
| `Information` | Messages d’information. |
| `Debug` | Messages de diagnostic utiles pour le débogage. |
| `Trace` | Messages de diagnostic très détaillées conçus pour le diagnostic des problèmes spécifiques. |

### <a name="configure-allowed-transports"></a>Configurer les transports autorisées

Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` en JavaScript). Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisé pour limiter le client pour utiliser uniquement les transports spécifiés. Tous les transports sont activées par défaut.

Par exemple, pour désactiver le transport Server-Sent événements, mais autoriser les connexions WebSocket et l’interrogation longue :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

Dans le client JavaScript, les transports sont configurées en définissant le `transport` de l’objet d’options fournie à `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Configurer l’authentification du support

Pour fournir des données d’authentification, ainsi que les demandes SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` en JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité. Dans le Client .NET, ce jeton d’accès est transmis comme HTTP « Authentification de support » jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`). Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton de porteur, **sauf** dans de rares cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements Server-Sent et WebSockets demandes). Dans ces cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.

Dans le client .NET, le `AccessTokenProvider` option peut être spécifiée à l’aide du délégué d’options de `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

Dans le client JavaScript, le jeton d’accès est configuré en définissant le `accessTokenFactory` de l’objet d’options dans `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Configurer le délai d’attente et les options de persistance

Options supplémentaires pour la configuration du comportement keep-alive et délai d’attente sont disponibles sur le `HubConnection` objet lui-même :

| Option .NET | Option de JavaScript | Description |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Délai d’expiration pour l’activité du serveur. Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et le déclencheur le `Closed` événement (`onclose` en JavaScript). |
| `HandshakeTimeout` | N’est pas configurable | Délai d’attente pour la négociation initiale du serveur. Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de négociation et le déclencheur le `Closed` événement (`onclose` en JavaScript). |

Dans le Client .NET, les valeurs de délai d’attente sont spécifiés comme `TimeSpan` valeurs. Dans le client JavaScript, les valeurs de délai d’attente sont spécifiées sous forme de nombres. Les nombres représentent des valeurs d’heure en millisecondes.

### <a name="configure-additional-options"></a>Configurer des options supplémentaires

Options supplémentaires peuvent être configurées dans le `WithUrl` (`withUrl` en JavaScript) méthode sur `HubConnectionBuilder`:

| Option .NET | Option de JavaScript | Description |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification de support dans les demandes HTTP. |
| `SkipNegotiation` | `skipNegotiation` | Affectez la valeur `true` pour ignorer l’étape de négociation. **Prise en charge uniquement lorsque le transport WebSocket est le seul transport activé**. Ce paramètre ne peut pas être activé lors de l’utilisation du Service de SignalR Azure. |
| `ClientCertificates` | N’est pas configurable * | Collection de certificats TLS à envoyer authentifier les demandes. |
| `Cookies` | N’est pas configurable * | Une collection de cookies HTTP pour envoyer à chaque requête HTTP. |
| `Credentials` | N’est pas configurable * | Informations d’identification à envoyer avec chaque requête HTTP. |
| `CloseTimeout` | N’est pas configurable * | WebSockets. La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture. Si le serveur n’accuser réception de la fermeture dans ce délai, le client se déconnecte. |
| `Headers` | N’est pas configurable * | Un dictionnaire d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP. |
| `HttpMessageHandlerFactory` | N’est pas configurable * | Un délégué qui peut servir à configurer ou de remplacer le `HttpMessageHandler` permet d’envoyer des requêtes HTTP. Pas utilisé pour les connexions WebSocket. Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre. Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un tout nouveau `HttpMessageHandler` instance. |
| `Proxy` | N’est pas configurable * | Un proxy HTTP à utiliser lors de l’envoi des demandes HTTP. |
| `UseDefaultCredentials` | N’est pas configurable * | Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et le protocole WebSocket. Cela permet l’utilisation de l’authentification Windows. |
| `WebSocketConfiguration` | N’est pas configurable * | Délégué qui peut être utilisé pour configurer d’autres options de WebSocket. Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisé pour configurer les options. |

Options marquées d’un astérisque (*) ne sont pas configurables dans le client JavaScript, en raison des limitations dans l’Explorateur d’API.

Dans le Client .NET, ces options peuvent être modifiées par le délégué options fourni à `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

Dans le JavaScript Client, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
