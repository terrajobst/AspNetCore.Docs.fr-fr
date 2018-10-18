---
title: Configuration d’ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: 855446003ae9d994854d4d8bb7d0f542a22734e4
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391100"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="24fa8-103">Configuration d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="24fa8-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="24fa8-104">Options de sérialisation de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="24fa8-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="24fa8-105">ASP.NET Core SignalR prend en charge deux protocoles d’encodage de messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="24fa8-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="24fa8-106">Chaque protocole a des options de configuration de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="24fa8-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="24fa8-107">Sérialisation JSON peut être configurée sur le serveur à l’aide de la [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="24fa8-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="24fa8-108">Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet.</span><span class="sxs-lookup"><span data-stu-id="24fa8-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="24fa8-109">Le [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="24fa8-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="24fa8-110">Consultez le [Documentation JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="24fa8-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="24fa8-111">Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « en casse Pascal », au lieu des noms « en casse mixte » par défaut, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="24fa8-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="24fa8-112">Dans le client .NET, les mêmes `AddJsonProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="24fa8-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="24fa8-113">Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="24fa8-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="24fa8-114">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24fa8-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="24fa8-115">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="24fa8-115">MessagePack serialization options</span></span>

<span data-ttu-id="24fa8-116">MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler.</span><span class="sxs-lookup"><span data-stu-id="24fa8-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="24fa8-117">Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="24fa8-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="24fa8-118">Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24fa8-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="24fa8-119">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="24fa8-119">Configure server options</span></span>

<span data-ttu-id="24fa8-120">Le tableau suivant décrit les options de configuration de concentrateurs SignalR :</span><span class="sxs-lookup"><span data-stu-id="24fa8-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="24fa8-121">Option</span><span class="sxs-lookup"><span data-stu-id="24fa8-121">Option</span></span> | <span data-ttu-id="24fa8-122">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24fa8-122">Default Value</span></span> | <span data-ttu-id="24fa8-123">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="24fa8-124">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24fa8-124">15 seconds</span></span> | <span data-ttu-id="24fa8-125">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="24fa8-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="24fa8-126">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="24fa8-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24fa8-127">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24fa8-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="24fa8-128">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24fa8-128">15 seconds</span></span> | <span data-ttu-id="24fa8-129">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="24fa8-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="24fa8-130">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="24fa8-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="24fa8-131">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="24fa8-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="24fa8-132">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="24fa8-132">All installed protocols</span></span> | <span data-ttu-id="24fa8-133">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="24fa8-133">Protocols supported by this hub.</span></span> <span data-ttu-id="24fa8-134">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="24fa8-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="24fa8-135">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="24fa8-136">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="24fa8-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="24fa8-137">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24fa8-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="24fa8-138">Options pour un seul hub remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="24fa8-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="24fa8-139">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liées aux transports et gestion de la mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="24fa8-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="24fa8-140">Ces options sont configurées en passant un délégué à [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="24fa8-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="24fa8-141">Option</span><span class="sxs-lookup"><span data-stu-id="24fa8-141">Option</span></span> | <span data-ttu-id="24fa8-142">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24fa8-142">Default Value</span></span> | <span data-ttu-id="24fa8-143">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="24fa8-144">32 KO</span><span class="sxs-lookup"><span data-stu-id="24fa8-144">32 KB</span></span> | <span data-ttu-id="24fa8-145">Le nombre maximal d’octets reçus à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="24fa8-146">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24fa8-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="24fa8-147">Données collectées à partir de la `Authorize` attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="24fa8-148">Une liste de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="24fa8-149">32 KO</span><span class="sxs-lookup"><span data-stu-id="24fa8-149">32 KB</span></span> | <span data-ttu-id="24fa8-150">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="24fa8-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="24fa8-151">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24fa8-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="24fa8-152">Tous les Transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="24fa8-152">All Transports are enabled.</span></span> | <span data-ttu-id="24fa8-153">Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="24fa8-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="24fa8-154">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24fa8-154">See below.</span></span> | <span data-ttu-id="24fa8-155">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="24fa8-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="24fa8-156">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24fa8-156">See below.</span></span> | <span data-ttu-id="24fa8-157">Options supplémentaires spécifiques au transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24fa8-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="24fa8-158">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="24fa8-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="24fa8-159">Option</span><span class="sxs-lookup"><span data-stu-id="24fa8-159">Option</span></span> | <span data-ttu-id="24fa8-160">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24fa8-160">Default Value</span></span> | <span data-ttu-id="24fa8-161">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="24fa8-162">90 secondes</span><span class="sxs-lookup"><span data-stu-id="24fa8-162">90 seconds</span></span> | <span data-ttu-id="24fa8-163">La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une demande d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="24fa8-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="24fa8-164">Diminution de cette valeur entraîne le client d’émettre de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="24fa8-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="24fa8-165">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="24fa8-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="24fa8-166">Option</span><span class="sxs-lookup"><span data-stu-id="24fa8-166">Option</span></span> | <span data-ttu-id="24fa8-167">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24fa8-167">Default Value</span></span> | <span data-ttu-id="24fa8-168">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="24fa8-169">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24fa8-169">5 seconds</span></span> | <span data-ttu-id="24fa8-170">Une fois que le serveur ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="24fa8-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="24fa8-171">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="24fa8-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="24fa8-172">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="24fa8-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="24fa8-173">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="24fa8-173">Configure client options</span></span>

<span data-ttu-id="24fa8-174">Options du client peuvent être configurées sur le `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript), de type, ainsi que dans le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="24fa8-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="24fa8-175">Configurer la journalisation</span><span class="sxs-lookup"><span data-stu-id="24fa8-175">Configure logging</span></span>

<span data-ttu-id="24fa8-176">La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="24fa8-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="24fa8-177">Enregistrement des fournisseurs et les filtres peut être inscrits dans la même façon qu’ils se trouvent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="24fa8-178">Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="24fa8-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="24fa8-179">Pour inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="24fa8-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="24fa8-180">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="24fa8-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="24fa8-181">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="24fa8-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="24fa8-182">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="24fa8-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="24fa8-183">Dans le client JavaScript, un texte similaire `configureLogging` méthode existe.</span><span class="sxs-lookup"><span data-stu-id="24fa8-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="24fa8-184">Fournir un `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="24fa8-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="24fa8-185">Journaux sont écrits dans la fenêtre de console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="24fa8-186">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24fa8-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="24fa8-187">Niveaux de journalisation disponibles pour le client JavaScript sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24fa8-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="24fa8-188">Une des valeurs suivantes affectant le niveau de journal Active la journalisation des messages à **ou version ultérieure** ce niveau.</span><span class="sxs-lookup"><span data-stu-id="24fa8-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="24fa8-189">Niveau</span><span class="sxs-lookup"><span data-stu-id="24fa8-189">Level</span></span> | <span data-ttu-id="24fa8-190">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="24fa8-191">Aucun message n’est enregistré.</span><span class="sxs-lookup"><span data-stu-id="24fa8-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="24fa8-192">Messages qui indiquent un échec dans l’application entière.</span><span class="sxs-lookup"><span data-stu-id="24fa8-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="24fa8-193">Messages qui indiquent un échec dans l’opération en cours.</span><span class="sxs-lookup"><span data-stu-id="24fa8-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="24fa8-194">Messages qui indiquent un problème non irrécupérable.</span><span class="sxs-lookup"><span data-stu-id="24fa8-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="24fa8-195">Messages d’information.</span><span class="sxs-lookup"><span data-stu-id="24fa8-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="24fa8-196">Messages de diagnostic utiles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="24fa8-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="24fa8-197">Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques.</span><span class="sxs-lookup"><span data-stu-id="24fa8-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="24fa8-198">Configurer les transports autorisées</span><span class="sxs-lookup"><span data-stu-id="24fa8-198">Configure allowed transports</span></span>

<span data-ttu-id="24fa8-199">Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24fa8-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="24fa8-200">Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client pour utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="24fa8-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="24fa8-201">Tous les transports sont activées par défaut.</span><span class="sxs-lookup"><span data-stu-id="24fa8-201">All transports are enabled by default.</span></span>

<span data-ttu-id="24fa8-202">Par exemple, pour désactiver le transport les événements, mais autoriser les connexions WebSocket et l’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="24fa8-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="24fa8-203">Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni pour `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24fa8-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="24fa8-204">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="24fa8-204">Configure bearer authentication</span></span>

<span data-ttu-id="24fa8-205">Pour fournir des données d’authentification, ainsi que les demandes de SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="24fa8-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="24fa8-206">Dans le Client .NET, ce jeton d’accès est transmis en tant que « Authentification du porteur » HTTP jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="24fa8-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="24fa8-207">Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton du porteur, **sauf** dans certains cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements et des WebSockets demandes).</span><span class="sxs-lookup"><span data-stu-id="24fa8-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="24fa8-208">Dans ce cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="24fa8-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="24fa8-209">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24fa8-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="24fa8-210">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24fa8-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="24fa8-211">Configurer le délai d’expiration et les options keep-alive</span><span class="sxs-lookup"><span data-stu-id="24fa8-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="24fa8-212">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="24fa8-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="24fa8-213">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="24fa8-213">.NET Option</span></span> | <span data-ttu-id="24fa8-214">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="24fa8-214">JavaScript Option</span></span> | <span data-ttu-id="24fa8-215">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24fa8-215">Default Value</span></span> | <span data-ttu-id="24fa8-216">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="24fa8-217">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24fa8-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24fa8-218">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-218">Timeout for server activity.</span></span> <span data-ttu-id="24fa8-219">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24fa8-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24fa8-220">Cette valeur doit être suffisamment grande pour un message ping à envoyer à partir du serveur **et** reçues par le client dans l’intervalle de délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="24fa8-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24fa8-221">La valeur recommandée est un nombre au moins deux fois sur le serveur `KeepAliveInterval` valeur, de prévoir un délai pour les tests ping arrivée.</span><span class="sxs-lookup"><span data-stu-id="24fa8-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="24fa8-222">Non configurable</span><span class="sxs-lookup"><span data-stu-id="24fa8-222">Not configurable</span></span> | <span data-ttu-id="24fa8-223">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24fa8-223">15 seconds</span></span> | <span data-ttu-id="24fa8-224">Délai d’attente pour la négociation initiale du serveur.</span><span class="sxs-lookup"><span data-stu-id="24fa8-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="24fa8-225">Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le protocole de négociation et déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24fa8-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24fa8-226">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="24fa8-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24fa8-227">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24fa8-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="24fa8-228">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="24fa8-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="24fa8-229">Dans le client JavaScript, les valeurs de délai d’attente sont spécifiées en tant que nombre indiquant la durée en millisecondes.</span><span class="sxs-lookup"><span data-stu-id="24fa8-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="24fa8-230">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24fa8-230">Configure additional options</span></span>

<span data-ttu-id="24fa8-231">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="24fa8-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="24fa8-232">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="24fa8-232">.NET Option</span></span> | <span data-ttu-id="24fa8-233">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="24fa8-233">JavaScript Option</span></span> | <span data-ttu-id="24fa8-234">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24fa8-234">Default Value</span></span> | <span data-ttu-id="24fa8-235">Description</span><span class="sxs-lookup"><span data-stu-id="24fa8-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="24fa8-236">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24fa8-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="24fa8-237">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24fa8-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24fa8-238">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24fa8-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24fa8-239">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24fa8-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="24fa8-240">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-240">Not configurable \*</span></span> | <span data-ttu-id="24fa8-241">Empty</span><span class="sxs-lookup"><span data-stu-id="24fa8-241">Empty</span></span> | <span data-ttu-id="24fa8-242">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="24fa8-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="24fa8-243">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-243">Not configurable \*</span></span> | <span data-ttu-id="24fa8-244">Empty</span><span class="sxs-lookup"><span data-stu-id="24fa8-244">Empty</span></span> | <span data-ttu-id="24fa8-245">Une collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24fa8-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="24fa8-246">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-246">Not configurable \*</span></span> | <span data-ttu-id="24fa8-247">Empty</span><span class="sxs-lookup"><span data-stu-id="24fa8-247">Empty</span></span> | <span data-ttu-id="24fa8-248">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24fa8-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="24fa8-249">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-249">Not configurable \*</span></span> | <span data-ttu-id="24fa8-250">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24fa8-250">5 seconds</span></span> | <span data-ttu-id="24fa8-251">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="24fa8-251">WebSockets only.</span></span> <span data-ttu-id="24fa8-252">La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="24fa8-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="24fa8-253">Si le serveur n’accuse pas la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="24fa8-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="24fa8-254">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-254">Not configurable \*</span></span> | <span data-ttu-id="24fa8-255">Empty</span><span class="sxs-lookup"><span data-stu-id="24fa8-255">Empty</span></span> | <span data-ttu-id="24fa8-256">Un dictionnaire d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24fa8-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="24fa8-257">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="24fa8-258">Un délégué qui peut être utilisé pour configurer ou de remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24fa8-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="24fa8-259">Pas utilisé pour les connexions de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24fa8-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="24fa8-260">Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="24fa8-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="24fa8-261">Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un nouveau `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="24fa8-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="24fa8-262">**Lorsque vous en remplaçant le gestionnaire veillez à copier les paramètres que vous souhaitez empêcher le gestionnaire fourni, sinon, les options configurées (par exemple, les Cookies et les en-têtes) ne s’appliquent pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="24fa8-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="24fa8-263">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="24fa8-264">Un proxy HTTP à utiliser lors de l’envoi des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24fa8-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="24fa8-265">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="24fa8-266">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et WebSockets.</span><span class="sxs-lookup"><span data-stu-id="24fa8-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="24fa8-267">Cela permet d’utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="24fa8-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="24fa8-268">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="24fa8-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="24fa8-269">Délégué qui peut être utilisé pour configurer des options supplémentaires de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24fa8-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="24fa8-270">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="24fa8-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="24fa8-271">Options marquées d’un astérisque (\*) ne sont pas configurables dans le client JavaScript, en raison des limitations dans le navigateur d’API.</span><span class="sxs-lookup"><span data-stu-id="24fa8-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="24fa8-272">Dans le Client .NET, ces options peuvent être modifiées par l’options du délégué fourni `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24fa8-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="24fa8-273">Dans le JavaScript Client, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24fa8-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="24fa8-274">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24fa8-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
