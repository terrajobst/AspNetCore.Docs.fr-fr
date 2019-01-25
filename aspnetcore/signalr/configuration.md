---
title: Configuration d’ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: bb18ba242584afa7181dcc19a5295f86996aeaa3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837518"
---
# <a name="aspnet-core-signalr-configuration"></a>Configuration d’ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Options de sérialisation de JSON/MessagePack

ASP.NET Core SignalR prend en charge deux protocoles d’encodage de messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html). Chaque protocole a des options de configuration de sérialisation.

Sérialisation JSON peut être configurée sur le serveur à l’aide de la [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode). Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet. Le [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour. Consultez le [Documentation JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.

Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « en casse Pascal », au lieu des noms « en casse mixte » par défaut, utilisez le code suivant :

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

Dans le client .NET, les mêmes `AddJsonProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.

### <a name="messagepack-serialization-options"></a>Options de sérialisation MessagePack

MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler. Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.

> [!NOTE]
> Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.

## <a name="configure-server-options"></a>Configurer les options de serveur

Le tableau suivant décrit les options de configuration de concentrateurs SignalR :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 secondes | Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée. Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves. Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 secondes | Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte. Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client. La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.  |
| `SupportedProtocols` | Tous les protocoles installés | Les protocoles pris en charge par ce hub. Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs. |
| `EnableDetailedErrors` | `false` | Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur. La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles. |

Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

Options pour un seul hub remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liées aux transports et gestion de la mémoire tampon. Ces options sont configurées en passant un délégué à [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KO | Le nombre maximal d’octets reçus à partir du client qui les mémoires tampons de serveur. Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire. |
| `AuthorizationData` | Données collectées à partir de la `Authorize` attributs appliqués à la classe de concentrateur. | Une liste de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur. |
| `TransportMaxBufferSize` | 32 KO | Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon. L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire. |
| `Transports` | Tous les Transports sont activés. | Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter. |
| `LongPolling` | Voir ci-dessous. | Options supplémentaires spécifiques au transport d’interrogation longue. |
| `WebSockets` | Voir ci-dessous. | Options supplémentaires spécifiques au transport WebSocket. |

Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 secondes | La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une demande d’interrogation unique. Diminution de cette valeur entraîne le client d’émettre de nouvelles demandes d’interrogation plus fréquemment. |

Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :

| Option | Valeur par défaut | Description |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 secondes | Une fois que le serveur ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est interrompue. |
| `SubProtocolSelector` | `null` | Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée. Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée. |

## <a name="configure-client-options"></a>Configurer les options du client

Options du client peuvent être configurées sur le `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript), de type, ainsi que dans le `HubConnection` lui-même.

### <a name="configure-logging"></a>Configurer la journalisation

La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode). Enregistrement des fournisseurs et les filtres peut être inscrits dans la même façon qu’ils se trouvent sur le serveur. Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index) documentation pour plus d’informations.

> [!NOTE]
> Pour inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires. Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.

Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`. Appelez la méthode d’extension `AddConsole` :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

Dans le client JavaScript, un texte similaire `configureLogging` méthode existe. Fournir un `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire. Journaux sont écrits dans la fenêtre de console du navigateur.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.

Niveaux de journalisation disponibles pour le client JavaScript sont répertoriées ci-dessous. Une des valeurs suivantes affectant le niveau de journal Active la journalisation des messages à **ou version ultérieure** ce niveau.

| Niveau | Description |
| ----- | ----------- |
| `None` | Aucun message n’est enregistré. |
| `Critical` | Messages qui indiquent un échec dans l’application entière. |
| `Error` | Messages qui indiquent un échec dans l’opération en cours. |
| `Warning` | Messages qui indiquent un problème non irrécupérable. |
| `Information` | Messages d’information. |
| `Debug` | Messages de diagnostic utiles pour le débogage. |
| `Trace` | Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques. |

### <a name="configure-allowed-transports"></a>Configurer les transports autorisées

Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` dans JavaScript). Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client pour utiliser uniquement les transports spécifiés. Tous les transports sont activées par défaut.

Par exemple, pour désactiver le transport les événements, mais autoriser les connexions WebSocket et l’interrogation longue :

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni pour `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Configurer l’authentification du porteur

Pour fournir des données d’authentification, ainsi que les demandes de SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité. Dans le Client .NET, ce jeton d’accès est transmis en tant que « Authentification du porteur » HTTP jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`). Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton du porteur, **sauf** dans certains cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements et des WebSockets demandes). Dans ce cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.

Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:

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

### <a name="configure-timeout-and-keep-alive-options"></a>Configurer le délai d’expiration et les options keep-alive

Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :

| Option de .NET | Option de JavaScript | Valeur par défaut | Description |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | 30 secondes (30 000 millisecondes) | Délai d’expiration pour l’activité du serveur. Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `Closed` événement (`onclose` dans JavaScript). Cette valeur doit être suffisamment grande pour un message ping à envoyer à partir du serveur **et** reçues par le client dans l’intervalle de délai d’attente. La valeur recommandée est un nombre au moins deux fois sur le serveur `KeepAliveInterval` valeur, de prévoir un délai pour les tests ping arrivée. |
| `HandshakeTimeout` | Non configurable | 15 secondes | Délai d’attente pour la négociation initiale du serveur. Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le protocole de négociation et déclencheurs le `Closed` événement (`onclose` dans JavaScript). Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves. Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`. Dans le client JavaScript, les valeurs de délai d’attente sont spécifiées en tant que nombre indiquant la durée en millisecondes.

### <a name="configure-additional-options"></a>Configurer des options supplémentaires

Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder`:

| Option de .NET | Option de JavaScript | Valeur par défaut | Description |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP. |
| `SkipNegotiation` | `skipNegotiation` | `false` | Affectez la valeur `true` pour ignorer l’étape de négociation. **Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**. Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR. |
| `ClientCertificates` | Non configurable * | Empty | Collection de certificats TLS à envoyer pour authentifier les demandes. |
| `Cookies` | Non configurable * | Empty | Une collection de cookies HTTP à envoyer avec chaque requête HTTP. |
| `Credentials` | Non configurable * | Empty | Informations d’identification à envoyer avec chaque requête HTTP. |
| `CloseTimeout` | Non configurable * | 5 secondes | WebSockets. La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture. Si le serveur n’accuse pas la fermeture dans ce délai, le client se déconnecte. |
| `Headers` | Non configurable * | Empty | Un dictionnaire d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP. |
| `HttpMessageHandlerFactory` | Non configurable * | `null` | Un délégué qui peut être utilisé pour configurer ou de remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP. Pas utilisé pour les connexions de WebSocket. Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre. Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un nouveau `HttpMessageHandler` instance. **Lorsque vous en remplaçant le gestionnaire veillez à copier les paramètres que vous souhaitez empêcher le gestionnaire fourni, sinon, les options configurées (par exemple, les Cookies et les en-têtes) ne s’appliquent pas au nouveau gestionnaire.** |
| `Proxy` | Non configurable * | `null` | Un proxy HTTP à utiliser lors de l’envoi des requêtes HTTP. |
| `UseDefaultCredentials` | Non configurable * | `false` | Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et WebSockets. Cela permet d’utiliser l’authentification Windows. |
| `WebSocketConfiguration` | Non configurable * | `null` | Délégué qui peut être utilisé pour configurer des options supplémentaires de WebSocket. Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options. |

Options marquées d’un astérisque (*) ne sont pas configurables dans le client JavaScript, en raison des limitations dans le navigateur d’API.

Dans le Client .NET, ces options peuvent être modifiées par l’options du délégué fourni `WithUrl`:

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
    })
    .build();
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
