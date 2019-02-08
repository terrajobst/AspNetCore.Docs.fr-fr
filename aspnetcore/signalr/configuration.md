---
title: Configuration d’ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/07/2019
uid: signalr/configuration
ms.openlocfilehash: f5449a15743c1f38c550fe30945bdc19f069e3f5
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55958113"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="ac96f-103">Configuration d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ac96f-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ac96f-104">Options de sérialisation de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="ac96f-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ac96f-105">ASP.NET Core SignalR prend en charge deux protocoles d’encodage de messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ac96f-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ac96f-106">Chaque protocole a des options de configuration de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="ac96f-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ac96f-107">Sérialisation JSON peut être configurée sur le serveur à l’aide de la [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ac96f-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ac96f-108">Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet.</span><span class="sxs-lookup"><span data-stu-id="ac96f-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ac96f-109">Le [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="ac96f-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ac96f-110">Consultez le [Documentation JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="ac96f-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="ac96f-111">Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « en casse Pascal », au lieu des noms « en casse mixte » par défaut, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ac96f-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="ac96f-112">Dans le client .NET, les mêmes `AddJsonProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ac96f-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ac96f-113">Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="ac96f-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ac96f-114">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="ac96f-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ac96f-115">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="ac96f-115">MessagePack serialization options</span></span>

<span data-ttu-id="ac96f-116">MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler.</span><span class="sxs-lookup"><span data-stu-id="ac96f-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ac96f-117">Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="ac96f-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ac96f-118">Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="ac96f-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ac96f-119">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="ac96f-119">Configure server options</span></span>

<span data-ttu-id="ac96f-120">Le tableau suivant décrit les options de configuration de concentrateurs SignalR :</span><span class="sxs-lookup"><span data-stu-id="ac96f-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="ac96f-121">Option</span><span class="sxs-lookup"><span data-stu-id="ac96f-121">Option</span></span> | <span data-ttu-id="ac96f-122">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="ac96f-122">Default Value</span></span> | <span data-ttu-id="ac96f-123">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ac96f-124">30 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-124">30 seconds</span></span> | <span data-ttu-id="ac96f-125">Le serveur considère le client déconnecté si elle n’a pas reçu un message (y compris keep-alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="ac96f-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ac96f-126">Il peut prendre plu de cet intervalle de délai d’attente à réellement être identifiée comme étant déconnectée, en raison de la façon dont ceci est implémenté par le client.</span><span class="sxs-lookup"><span data-stu-id="ac96f-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ac96f-127">La valeur recommandée est double la `KeepAliveInterval` valeur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ac96f-128">15 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-128">15 seconds</span></span> | <span data-ttu-id="ac96f-129">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="ac96f-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ac96f-130">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="ac96f-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac96f-131">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ac96f-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ac96f-132">15 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-132">15 seconds</span></span> | <span data-ttu-id="ac96f-133">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="ac96f-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ac96f-134">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="ac96f-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ac96f-135">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ac96f-136">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="ac96f-136">All installed protocols</span></span> | <span data-ttu-id="ac96f-137">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="ac96f-137">Protocols supported by this hub.</span></span> <span data-ttu-id="ac96f-138">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="ac96f-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ac96f-139">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ac96f-140">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="ac96f-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="ac96f-141">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ac96f-142">Options pour un seul hub remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="ac96f-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ac96f-143">Options de configuration avancées HTTP</span><span class="sxs-lookup"><span data-stu-id="ac96f-143">Advanced HTTP configuration options</span></span>

<span data-ttu-id="ac96f-144">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liées aux transports et gestion de la mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="ac96f-144">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ac96f-145">Ces options sont configurées en passant un délégué à [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-145">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) => 
    {
        var desiredTransports = 
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) => 
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="ac96f-146">Le tableau suivant décrit les options de configuration des options de HTTP d’ASP.NET Core SignalR avancées :</span><span class="sxs-lookup"><span data-stu-id="ac96f-146">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="ac96f-147">Option</span><span class="sxs-lookup"><span data-stu-id="ac96f-147">Option</span></span> | <span data-ttu-id="ac96f-148">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="ac96f-148">Default Value</span></span> | <span data-ttu-id="ac96f-149">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-149">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ac96f-150">32 KO</span><span class="sxs-lookup"><span data-stu-id="ac96f-150">32 KB</span></span> | <span data-ttu-id="ac96f-151">Le nombre maximal d’octets reçus à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-151">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ac96f-152">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="ac96f-152">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ac96f-153">Données collectées à partir de la `Authorize` attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-153">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ac96f-154">Une liste de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-154">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ac96f-155">32 KO</span><span class="sxs-lookup"><span data-stu-id="ac96f-155">32 KB</span></span> | <span data-ttu-id="ac96f-156">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="ac96f-156">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ac96f-157">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="ac96f-157">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ac96f-158">Tous les Transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="ac96f-158">All Transports are enabled.</span></span> | <span data-ttu-id="ac96f-159">Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="ac96f-159">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ac96f-160">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ac96f-160">See below.</span></span> | <span data-ttu-id="ac96f-161">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="ac96f-161">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ac96f-162">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ac96f-162">See below.</span></span> | <span data-ttu-id="ac96f-163">Options supplémentaires spécifiques au transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ac96f-163">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ac96f-164">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="ac96f-164">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ac96f-165">Option</span><span class="sxs-lookup"><span data-stu-id="ac96f-165">Option</span></span> | <span data-ttu-id="ac96f-166">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="ac96f-166">Default Value</span></span> | <span data-ttu-id="ac96f-167">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ac96f-168">90 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-168">90 seconds</span></span> | <span data-ttu-id="ac96f-169">La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une demande d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="ac96f-169">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ac96f-170">Diminution de cette valeur entraîne le client d’émettre de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="ac96f-170">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ac96f-171">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="ac96f-171">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ac96f-172">Option</span><span class="sxs-lookup"><span data-stu-id="ac96f-172">Option</span></span> | <span data-ttu-id="ac96f-173">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="ac96f-173">Default Value</span></span> | <span data-ttu-id="ac96f-174">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ac96f-175">5 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-175">5 seconds</span></span> | <span data-ttu-id="ac96f-176">Une fois que le serveur ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="ac96f-176">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ac96f-177">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="ac96f-177">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ac96f-178">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="ac96f-178">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ac96f-179">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="ac96f-179">Configure client options</span></span>

<span data-ttu-id="ac96f-180">Options du client peuvent être configurées sur le `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript), de type, ainsi que dans le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="ac96f-180">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ac96f-181">Configurer la journalisation</span><span class="sxs-lookup"><span data-stu-id="ac96f-181">Configure logging</span></span>

<span data-ttu-id="ac96f-182">La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ac96f-182">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ac96f-183">Enregistrement des fournisseurs et les filtres peut être inscrits dans la même façon qu’ils se trouvent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-183">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ac96f-184">Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="ac96f-184">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ac96f-185">Pour inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="ac96f-185">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ac96f-186">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="ac96f-186">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ac96f-187">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-187">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ac96f-188">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="ac96f-188">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ac96f-189">Dans le client JavaScript, un texte similaire `configureLogging` méthode existe.</span><span class="sxs-lookup"><span data-stu-id="ac96f-189">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ac96f-190">Fournir un `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="ac96f-190">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ac96f-191">Journaux sont écrits dans la fenêtre de console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-191">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ac96f-192">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-192">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ac96f-193">Niveaux de journalisation disponibles pour le client JavaScript sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ac96f-193">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="ac96f-194">Une des valeurs suivantes affectant le niveau de journal Active la journalisation des messages à **ou version ultérieure** ce niveau.</span><span class="sxs-lookup"><span data-stu-id="ac96f-194">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="ac96f-195">Niveau</span><span class="sxs-lookup"><span data-stu-id="ac96f-195">Level</span></span> | <span data-ttu-id="ac96f-196">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-196">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="ac96f-197">Aucun message n’est enregistré.</span><span class="sxs-lookup"><span data-stu-id="ac96f-197">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="ac96f-198">Messages qui indiquent un échec dans l’application entière.</span><span class="sxs-lookup"><span data-stu-id="ac96f-198">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="ac96f-199">Messages qui indiquent un échec dans l’opération en cours.</span><span class="sxs-lookup"><span data-stu-id="ac96f-199">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="ac96f-200">Messages qui indiquent un problème non irrécupérable.</span><span class="sxs-lookup"><span data-stu-id="ac96f-200">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="ac96f-201">Messages d’information.</span><span class="sxs-lookup"><span data-stu-id="ac96f-201">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="ac96f-202">Messages de diagnostic utiles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="ac96f-202">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="ac96f-203">Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques.</span><span class="sxs-lookup"><span data-stu-id="ac96f-203">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="ac96f-204">Configurer les transports autorisées</span><span class="sxs-lookup"><span data-stu-id="ac96f-204">Configure allowed transports</span></span>

<span data-ttu-id="ac96f-205">Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ac96f-205">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ac96f-206">Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client pour utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="ac96f-206">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ac96f-207">Tous les transports sont activées par défaut.</span><span class="sxs-lookup"><span data-stu-id="ac96f-207">All transports are enabled by default.</span></span>

<span data-ttu-id="ac96f-208">Par exemple, pour désactiver le transport les événements, mais autoriser les connexions WebSocket et l’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="ac96f-208">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ac96f-209">Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni pour `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ac96f-209">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ac96f-210">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="ac96f-210">Configure bearer authentication</span></span>

<span data-ttu-id="ac96f-211">Pour fournir des données d’authentification, ainsi que les demandes de SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="ac96f-211">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ac96f-212">Dans le Client .NET, ce jeton d’accès est transmis en tant que « Authentification du porteur » HTTP jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="ac96f-212">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ac96f-213">Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton du porteur, **sauf** dans certains cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements et des WebSockets demandes).</span><span class="sxs-lookup"><span data-stu-id="ac96f-213">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ac96f-214">Dans ce cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-214">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ac96f-215">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ac96f-215">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ac96f-216">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ac96f-216">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ac96f-217">Configurer le délai d’expiration et les options keep-alive</span><span class="sxs-lookup"><span data-stu-id="ac96f-217">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ac96f-218">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="ac96f-218">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="ac96f-219">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="ac96f-219">.NET Option</span></span> | <span data-ttu-id="ac96f-220">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ac96f-220">JavaScript Option</span></span> | <span data-ttu-id="ac96f-221">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="ac96f-221">Default Value</span></span> | <span data-ttu-id="ac96f-222">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-222">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="ac96f-223">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="ac96f-223">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ac96f-224">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-224">Timeout for server activity.</span></span> <span data-ttu-id="ac96f-225">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ac96f-225">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ac96f-226">Cette valeur doit être suffisamment grande pour un message ping à envoyer à partir du serveur **et** reçues par le client dans l’intervalle de délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="ac96f-226">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ac96f-227">La valeur recommandée est un nombre au moins deux fois sur le serveur `KeepAliveInterval` valeur, de prévoir un délai pour les tests ping arrivée.</span><span class="sxs-lookup"><span data-stu-id="ac96f-227">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ac96f-228">Non configurable</span><span class="sxs-lookup"><span data-stu-id="ac96f-228">Not configurable</span></span> | <span data-ttu-id="ac96f-229">15 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-229">15 seconds</span></span> | <span data-ttu-id="ac96f-230">Délai d’attente pour la négociation initiale du serveur.</span><span class="sxs-lookup"><span data-stu-id="ac96f-230">Timeout for initial server handshake.</span></span> <span data-ttu-id="ac96f-231">Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le protocole de négociation et déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ac96f-231">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ac96f-232">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="ac96f-232">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ac96f-233">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ac96f-233">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ac96f-234">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="ac96f-234">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="ac96f-235">Dans le client JavaScript, les valeurs de délai d’attente sont spécifiées en tant que nombre indiquant la durée en millisecondes.</span><span class="sxs-lookup"><span data-stu-id="ac96f-235">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="ac96f-236">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ac96f-236">Configure additional options</span></span>

<span data-ttu-id="ac96f-237">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="ac96f-237">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="ac96f-238">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="ac96f-238">.NET Option</span></span> | <span data-ttu-id="ac96f-239">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ac96f-239">JavaScript Option</span></span> | <span data-ttu-id="ac96f-240">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="ac96f-240">Default Value</span></span> | <span data-ttu-id="ac96f-241">Description</span><span class="sxs-lookup"><span data-stu-id="ac96f-241">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="ac96f-242">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac96f-242">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="ac96f-243">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="ac96f-243">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ac96f-244">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="ac96f-244">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ac96f-245">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac96f-245">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ac96f-246">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-246">Not configurable \*</span></span> | <span data-ttu-id="ac96f-247">Empty</span><span class="sxs-lookup"><span data-stu-id="ac96f-247">Empty</span></span> | <span data-ttu-id="ac96f-248">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="ac96f-248">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ac96f-249">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-249">Not configurable \*</span></span> | <span data-ttu-id="ac96f-250">Empty</span><span class="sxs-lookup"><span data-stu-id="ac96f-250">Empty</span></span> | <span data-ttu-id="ac96f-251">Une collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac96f-251">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ac96f-252">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-252">Not configurable \*</span></span> | <span data-ttu-id="ac96f-253">Empty</span><span class="sxs-lookup"><span data-stu-id="ac96f-253">Empty</span></span> | <span data-ttu-id="ac96f-254">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac96f-254">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ac96f-255">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-255">Not configurable \*</span></span> | <span data-ttu-id="ac96f-256">5 secondes</span><span class="sxs-lookup"><span data-stu-id="ac96f-256">5 seconds</span></span> | <span data-ttu-id="ac96f-257">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ac96f-257">WebSockets only.</span></span> <span data-ttu-id="ac96f-258">La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="ac96f-258">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ac96f-259">Si le serveur n’accuse pas la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="ac96f-259">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ac96f-260">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-260">Not configurable \*</span></span> | <span data-ttu-id="ac96f-261">Empty</span><span class="sxs-lookup"><span data-stu-id="ac96f-261">Empty</span></span> | <span data-ttu-id="ac96f-262">Un dictionnaire d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac96f-262">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="ac96f-263">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="ac96f-264">Un délégué qui peut être utilisé pour configurer ou de remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac96f-264">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ac96f-265">Pas utilisé pour les connexions de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ac96f-265">Not used for WebSocket connections.</span></span> <span data-ttu-id="ac96f-266">Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="ac96f-266">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ac96f-267">Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un nouveau `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="ac96f-267">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ac96f-268">**Lorsque vous en remplaçant le gestionnaire veillez à copier les paramètres que vous souhaitez empêcher le gestionnaire fourni, sinon, les options configurées (par exemple, les Cookies et les en-têtes) ne s’appliquent pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="ac96f-268">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="ac96f-269">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-269">Not configurable \*</span></span> | `null` | <span data-ttu-id="ac96f-270">Un proxy HTTP à utiliser lors de l’envoi des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac96f-270">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="ac96f-271">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-271">Not configurable \*</span></span> | `false` | <span data-ttu-id="ac96f-272">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ac96f-272">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ac96f-273">Cela permet d’utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="ac96f-273">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="ac96f-274">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="ac96f-274">Not configurable \*</span></span> | `null` | <span data-ttu-id="ac96f-275">Délégué qui peut être utilisé pour configurer des options supplémentaires de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ac96f-275">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ac96f-276">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="ac96f-276">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="ac96f-277">Options marquées d’un astérisque (\*) ne sont pas configurables dans le client JavaScript, en raison des limitations dans le navigateur d’API.</span><span class="sxs-lookup"><span data-stu-id="ac96f-277">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="ac96f-278">Dans le Client .NET, ces options peuvent être modifiées par l’options du délégué fourni `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ac96f-278">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ac96f-279">Dans le JavaScript Client, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ac96f-279">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ac96f-280">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ac96f-280">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
