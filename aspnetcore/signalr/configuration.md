---
title: Configuration de la SignalR ASP.NET Core
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658633"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="24635-103">Configuration de Signalr ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24635-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="24635-104">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="24635-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="24635-105">ASP.NET Core Signalr prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="24635-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="24635-106">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="24635-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="24635-107">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="24635-108">`AddJsonProtocol` peut être ajouté après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24635-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="24635-109">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="24635-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="24635-110">La propriété [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) sur cet objet est un objet <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="24635-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="24635-111">Pour plus d’informations, consultez la [Documentation System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="24635-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="24635-112">Par exemple, pour configurer le sérialiseur de manière à ce qu’il ne change pas la casse des noms de propriétés, au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="24635-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="24635-113">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="24635-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="24635-114">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="24635-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="24635-115">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24635-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="24635-116">Basculer vers Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="24635-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="24635-117">Si vous avez besoin de fonctionnalités de `Newtonsoft.Json` qui ne sont pas prises en charge dans `System.Text.Json`, consultez [basculer vers Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="24635-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="24635-118">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="24635-118">MessagePack serialization options</span></span>

<span data-ttu-id="24635-119">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="24635-120">Pour plus d’informations, consultez [MessagePack dans signalr](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="24635-121">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24635-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="24635-122">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="24635-122">Configure server options</span></span>

<span data-ttu-id="24635-123">Le tableau suivant décrit les options de configuration des hubs Signalr :</span><span class="sxs-lookup"><span data-stu-id="24635-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="24635-124">Option</span><span class="sxs-lookup"><span data-stu-id="24635-124">Option</span></span> | <span data-ttu-id="24635-125">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-125">Default Value</span></span> | <span data-ttu-id="24635-126">Description</span><span class="sxs-lookup"><span data-stu-id="24635-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="24635-127">30 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-127">30 seconds</span></span> | <span data-ttu-id="24635-128">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="24635-129">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="24635-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="24635-130">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="24635-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="24635-131">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-131">15 seconds</span></span> | <span data-ttu-id="24635-132">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="24635-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="24635-133">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-134">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="24635-135">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-135">15 seconds</span></span> | <span data-ttu-id="24635-136">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="24635-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="24635-137">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="24635-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="24635-138">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="24635-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="24635-139">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="24635-139">All installed protocols</span></span> | <span data-ttu-id="24635-140">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="24635-140">Protocols supported by this hub.</span></span> <span data-ttu-id="24635-141">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="24635-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="24635-142">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="24635-143">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="24635-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="24635-144">Nombre maximal d’éléments pouvant être mis en mémoire tampon pour les flux de téléchargement du client.</span><span class="sxs-lookup"><span data-stu-id="24635-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="24635-145">Si cette limite est atteinte, le traitement des appels est bloqué jusqu’à ce que le serveur traite les éléments de flux.</span><span class="sxs-lookup"><span data-stu-id="24635-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="24635-146">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-146">32 KB</span></span> | <span data-ttu-id="24635-147">Taille maximale d’un seul message de concentrateur entrant.</span><span class="sxs-lookup"><span data-stu-id="24635-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="24635-148">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24635-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="24635-149">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="24635-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="24635-150">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="24635-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="24635-151">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="24635-152">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="24635-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="24635-153">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core Signalr :</span><span class="sxs-lookup"><span data-stu-id="24635-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="24635-154">Option</span><span class="sxs-lookup"><span data-stu-id="24635-154">Option</span></span> | <span data-ttu-id="24635-155">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-155">Default Value</span></span> | <span data-ttu-id="24635-156">Description</span><span class="sxs-lookup"><span data-stu-id="24635-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="24635-157">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-157">32 KB</span></span> | <span data-ttu-id="24635-158">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon avant d’appliquer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="24635-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="24635-159">L’augmentation de cette valeur permet au serveur de recevoir plus rapidement des messages plus volumineux sans appliquer la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="24635-160">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="24635-161">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="24635-162">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-162">32 KB</span></span> | <span data-ttu-id="24635-163">Nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon avant d’observer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="24635-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="24635-164">L’augmentation de cette valeur permet au serveur de mettre plus rapidement en mémoire tampon des messages plus volumineux sans attendre la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="24635-165">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="24635-165">All Transports are enabled.</span></span> | <span data-ttu-id="24635-166">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="24635-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="24635-167">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24635-167">See below.</span></span> | <span data-ttu-id="24635-168">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="24635-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="24635-169">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24635-169">See below.</span></span> | <span data-ttu-id="24635-170">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="24635-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="24635-171">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="24635-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="24635-172">Option</span><span class="sxs-lookup"><span data-stu-id="24635-172">Option</span></span> | <span data-ttu-id="24635-173">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-173">Default Value</span></span> | <span data-ttu-id="24635-174">Description</span><span class="sxs-lookup"><span data-stu-id="24635-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="24635-175">90 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-175">90 seconds</span></span> | <span data-ttu-id="24635-176">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="24635-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="24635-177">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="24635-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="24635-178">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="24635-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="24635-179">Option</span><span class="sxs-lookup"><span data-stu-id="24635-179">Option</span></span> | <span data-ttu-id="24635-180">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-180">Default Value</span></span> | <span data-ttu-id="24635-181">Description</span><span class="sxs-lookup"><span data-stu-id="24635-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="24635-182">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-182">5 seconds</span></span> | <span data-ttu-id="24635-183">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="24635-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="24635-184">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="24635-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="24635-185">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="24635-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="24635-186">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="24635-186">Configure client options</span></span>

<span data-ttu-id="24635-187">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="24635-188">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="24635-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="24635-189">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="24635-189">Configure logging</span></span>

<span data-ttu-id="24635-190">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24635-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="24635-191">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="24635-192">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="24635-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="24635-193">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="24635-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="24635-194">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="24635-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="24635-195">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="24635-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="24635-196">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="24635-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="24635-197">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="24635-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="24635-198">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="24635-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="24635-199">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="24635-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="24635-200">Au lieu d’une valeur `LogLevel`, vous pouvez également fournir une valeur de `string` représentant un nom de niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="24635-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="24635-201">Cela est utile lors de la configuration de la journalisation Signalr dans les environnements où vous n’avez pas accès aux constantes de `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="24635-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="24635-202">Le tableau suivant répertorie les niveaux de journalisation disponibles.</span><span class="sxs-lookup"><span data-stu-id="24635-202">The following table lists the available log levels.</span></span> <span data-ttu-id="24635-203">La valeur que vous fournissez à `configureLogging` définit le niveau de journalisation **minimale** qui sera enregistré.</span><span class="sxs-lookup"><span data-stu-id="24635-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="24635-204">Les messages enregistrés à ce niveau, **ou les niveaux indiqués après celui-ci dans la table**, sont enregistrés.</span><span class="sxs-lookup"><span data-stu-id="24635-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="24635-205">String</span><span class="sxs-lookup"><span data-stu-id="24635-205">String</span></span>                      | <span data-ttu-id="24635-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="24635-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="24635-207">`info` **ou** `information`</span><span class="sxs-lookup"><span data-stu-id="24635-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="24635-208">`warn` **ou** `warning`</span><span class="sxs-lookup"><span data-stu-id="24635-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="24635-209">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24635-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="24635-210">Pour plus d’informations sur la journalisation, consultez la [documentation relative aux diagnostics de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="24635-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="24635-211">Le client Java Signalr utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="24635-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="24635-212">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="24635-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="24635-213">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java Signalr.</span><span class="sxs-lookup"><span data-stu-id="24635-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="24635-214">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="24635-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="24635-215">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="24635-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="24635-216">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="24635-216">Configure allowed transports</span></span>

<span data-ttu-id="24635-217">Les transports utilisés par Signalr peuvent être configurés dans l’appel `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="24635-218">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="24635-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="24635-219">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="24635-219">All transports are enabled by default.</span></span>

<span data-ttu-id="24635-220">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="24635-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="24635-221">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="24635-222">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="24635-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="24635-223">Dans le client Java, le transport est sélectionné à l’aide de la méthode `withTransport` sur le `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24635-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="24635-224">Le client Java utilise le transport WebSocket par défaut.</span><span class="sxs-lookup"><span data-stu-id="24635-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="24635-225">Le client Java Signalr ne prend pas encore en charge le transport de secours.</span><span class="sxs-lookup"><span data-stu-id="24635-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="24635-226">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="24635-226">Configure bearer authentication</span></span>

<span data-ttu-id="24635-227">Pour fournir des données d’authentification avec les demandes Signalr, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="24635-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="24635-228">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="24635-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="24635-229">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="24635-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="24635-230">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="24635-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="24635-231">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="24635-232">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="24635-233">Dans le client Java Signalr, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="24635-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="24635-234">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="24635-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="24635-235">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="24635-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="24635-236">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="24635-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="24635-237">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="24635-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="24635-238">.NET</span><span class="sxs-lookup"><span data-stu-id="24635-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="24635-239">Option</span><span class="sxs-lookup"><span data-stu-id="24635-239">Option</span></span> | <span data-ttu-id="24635-240">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-240">Default value</span></span> | <span data-ttu-id="24635-241">Description</span><span class="sxs-lookup"><span data-stu-id="24635-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="24635-242">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-243">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-243">Timeout for server activity.</span></span> <span data-ttu-id="24635-244">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24635-245">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-246">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="24635-247">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-247">15 seconds</span></span> | <span data-ttu-id="24635-248">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="24635-249">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24635-250">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-251">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="24635-252">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-252">15 seconds</span></span> | <span data-ttu-id="24635-253">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="24635-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="24635-254">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="24635-255">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="24635-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="24635-256">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="24635-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="24635-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="24635-258">Option</span><span class="sxs-lookup"><span data-stu-id="24635-258">Option</span></span> | <span data-ttu-id="24635-259">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-259">Default value</span></span> | <span data-ttu-id="24635-260">Description</span><span class="sxs-lookup"><span data-stu-id="24635-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="24635-261">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-262">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-262">Timeout for server activity.</span></span> <span data-ttu-id="24635-263">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="24635-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="24635-264">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-265">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="24635-266">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="24635-267">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="24635-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="24635-268">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="24635-269">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="24635-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="24635-270">Java</span><span class="sxs-lookup"><span data-stu-id="24635-270">Java</span></span>](#tab/java)

| <span data-ttu-id="24635-271">Option</span><span class="sxs-lookup"><span data-stu-id="24635-271">Option</span></span> | <span data-ttu-id="24635-272">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-272">Default value</span></span> | <span data-ttu-id="24635-273">Description</span><span class="sxs-lookup"><span data-stu-id="24635-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="24635-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="24635-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="24635-275">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-276">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-276">Timeout for server activity.</span></span> <span data-ttu-id="24635-277">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="24635-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="24635-278">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-279">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="24635-280">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-280">15 seconds</span></span> | <span data-ttu-id="24635-281">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="24635-282">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="24635-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="24635-283">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-284">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="24635-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="24635-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="24635-286">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="24635-287">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="24635-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="24635-288">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="24635-289">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="24635-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="24635-290">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24635-290">Configure additional options</span></span>

<span data-ttu-id="24635-291">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="24635-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="24635-292">.NET</span><span class="sxs-lookup"><span data-stu-id="24635-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="24635-293">Option .NET</span><span class="sxs-lookup"><span data-stu-id="24635-293">.NET Option</span></span> |  <span data-ttu-id="24635-294">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-294">Default value</span></span> | <span data-ttu-id="24635-295">Description</span><span class="sxs-lookup"><span data-stu-id="24635-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="24635-296">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="24635-297">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-298">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-299">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="24635-300">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-300">Empty</span></span> | <span data-ttu-id="24635-301">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="24635-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="24635-302">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-302">Empty</span></span> | <span data-ttu-id="24635-303">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="24635-304">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-304">Empty</span></span> | <span data-ttu-id="24635-305">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="24635-306">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-306">5 seconds</span></span> | <span data-ttu-id="24635-307">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="24635-307">WebSockets only.</span></span> <span data-ttu-id="24635-308">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="24635-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="24635-309">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="24635-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="24635-310">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-310">Empty</span></span> | <span data-ttu-id="24635-311">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="24635-312">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="24635-313">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24635-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="24635-314">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="24635-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="24635-315">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="24635-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="24635-316">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="24635-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="24635-317">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="24635-318">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24635-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="24635-319">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="24635-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="24635-320">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="24635-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="24635-321">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="24635-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="24635-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="24635-323">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-323">JavaScript Option</span></span> | <span data-ttu-id="24635-324">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-324">Default Value</span></span> | <span data-ttu-id="24635-325">Description</span><span class="sxs-lookup"><span data-stu-id="24635-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="24635-326">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="24635-327">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-328">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-329">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="24635-330">Java</span><span class="sxs-lookup"><span data-stu-id="24635-330">Java</span></span>](#tab/java)

| <span data-ttu-id="24635-331">Option Java</span><span class="sxs-lookup"><span data-stu-id="24635-331">Java Option</span></span> | <span data-ttu-id="24635-332">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-332">Default Value</span></span> | <span data-ttu-id="24635-333">Description</span><span class="sxs-lookup"><span data-stu-id="24635-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="24635-334">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="24635-335">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-336">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-337">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="24635-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="24635-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="24635-339">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-339">Empty</span></span> | <span data-ttu-id="24635-340">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="24635-341">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="24635-342">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="24635-343">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="24635-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="24635-344">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24635-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="24635-345">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="24635-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="24635-346">ASP.NET Core Signalr prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="24635-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="24635-347">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="24635-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="24635-348">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol), qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24635-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="24635-349">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="24635-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="24635-350">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet JSON.net `JsonSerializerSettings` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="24635-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="24635-351">Pour plus d’informations, voir la [documentation sur JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="24635-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="24635-352">Par exemple, pour configurer le sérialiseur afin d’utiliser les noms de propriété « casse Pascal », au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="24635-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="24635-353">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="24635-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="24635-354">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="24635-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="24635-355">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24635-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="24635-356">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="24635-356">MessagePack serialization options</span></span>

<span data-ttu-id="24635-357">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="24635-358">Pour plus d’informations, consultez [MessagePack dans signalr](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="24635-359">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24635-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="24635-360">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="24635-360">Configure server options</span></span>

<span data-ttu-id="24635-361">Le tableau suivant décrit les options de configuration des hubs Signalr :</span><span class="sxs-lookup"><span data-stu-id="24635-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="24635-362">Option</span><span class="sxs-lookup"><span data-stu-id="24635-362">Option</span></span> | <span data-ttu-id="24635-363">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-363">Default Value</span></span> | <span data-ttu-id="24635-364">Description</span><span class="sxs-lookup"><span data-stu-id="24635-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="24635-365">30 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-365">30 seconds</span></span> | <span data-ttu-id="24635-366">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="24635-367">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="24635-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="24635-368">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="24635-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="24635-369">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-369">15 seconds</span></span> | <span data-ttu-id="24635-370">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="24635-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="24635-371">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-372">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="24635-373">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-373">15 seconds</span></span> | <span data-ttu-id="24635-374">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="24635-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="24635-375">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="24635-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="24635-376">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="24635-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="24635-377">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="24635-377">All installed protocols</span></span> | <span data-ttu-id="24635-378">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="24635-378">Protocols supported by this hub.</span></span> <span data-ttu-id="24635-379">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="24635-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="24635-380">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="24635-381">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="24635-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="24635-382">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24635-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="24635-383">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="24635-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="24635-384">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="24635-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="24635-385">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="24635-386">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="24635-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="24635-387">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core Signalr :</span><span class="sxs-lookup"><span data-stu-id="24635-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="24635-388">Option</span><span class="sxs-lookup"><span data-stu-id="24635-388">Option</span></span> | <span data-ttu-id="24635-389">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-389">Default Value</span></span> | <span data-ttu-id="24635-390">Description</span><span class="sxs-lookup"><span data-stu-id="24635-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="24635-391">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-391">32 KB</span></span> | <span data-ttu-id="24635-392">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="24635-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="24635-393">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="24635-394">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="24635-395">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="24635-396">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-396">32 KB</span></span> | <span data-ttu-id="24635-397">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="24635-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="24635-398">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="24635-399">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="24635-399">All Transports are enabled.</span></span> | <span data-ttu-id="24635-400">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="24635-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="24635-401">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24635-401">See below.</span></span> | <span data-ttu-id="24635-402">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="24635-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="24635-403">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24635-403">See below.</span></span> | <span data-ttu-id="24635-404">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="24635-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="24635-405">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="24635-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="24635-406">Option</span><span class="sxs-lookup"><span data-stu-id="24635-406">Option</span></span> | <span data-ttu-id="24635-407">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-407">Default Value</span></span> | <span data-ttu-id="24635-408">Description</span><span class="sxs-lookup"><span data-stu-id="24635-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="24635-409">90 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-409">90 seconds</span></span> | <span data-ttu-id="24635-410">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="24635-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="24635-411">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="24635-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="24635-412">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="24635-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="24635-413">Option</span><span class="sxs-lookup"><span data-stu-id="24635-413">Option</span></span> | <span data-ttu-id="24635-414">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-414">Default Value</span></span> | <span data-ttu-id="24635-415">Description</span><span class="sxs-lookup"><span data-stu-id="24635-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="24635-416">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-416">5 seconds</span></span> | <span data-ttu-id="24635-417">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="24635-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="24635-418">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="24635-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="24635-419">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="24635-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="24635-420">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="24635-420">Configure client options</span></span>

<span data-ttu-id="24635-421">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="24635-422">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="24635-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="24635-423">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="24635-423">Configure logging</span></span>

<span data-ttu-id="24635-424">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24635-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="24635-425">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="24635-426">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="24635-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="24635-427">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="24635-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="24635-428">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="24635-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="24635-429">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="24635-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="24635-430">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="24635-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="24635-431">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="24635-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="24635-432">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="24635-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="24635-433">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="24635-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="24635-434">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24635-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="24635-435">Pour plus d’informations sur la journalisation, consultez la [documentation relative aux diagnostics de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="24635-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="24635-436">Le client Java Signalr utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="24635-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="24635-437">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="24635-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="24635-438">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java Signalr.</span><span class="sxs-lookup"><span data-stu-id="24635-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="24635-439">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="24635-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="24635-440">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="24635-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="24635-441">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="24635-441">Configure allowed transports</span></span>

<span data-ttu-id="24635-442">Les transports utilisés par Signalr peuvent être configurés dans l’appel `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="24635-443">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="24635-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="24635-444">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="24635-444">All transports are enabled by default.</span></span>

<span data-ttu-id="24635-445">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="24635-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="24635-446">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="24635-447">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="24635-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="24635-448">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="24635-448">Configure bearer authentication</span></span>

<span data-ttu-id="24635-449">Pour fournir des données d’authentification avec les demandes Signalr, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="24635-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="24635-450">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="24635-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="24635-451">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="24635-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="24635-452">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="24635-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="24635-453">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="24635-454">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="24635-455">Dans le client Java Signalr, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="24635-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="24635-456">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="24635-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="24635-457">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="24635-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="24635-458">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="24635-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="24635-459">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="24635-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="24635-460">.NET</span><span class="sxs-lookup"><span data-stu-id="24635-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="24635-461">Option</span><span class="sxs-lookup"><span data-stu-id="24635-461">Option</span></span> | <span data-ttu-id="24635-462">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-462">Default value</span></span> | <span data-ttu-id="24635-463">Description</span><span class="sxs-lookup"><span data-stu-id="24635-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="24635-464">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-465">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-465">Timeout for server activity.</span></span> <span data-ttu-id="24635-466">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24635-467">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-468">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="24635-469">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-469">15 seconds</span></span> | <span data-ttu-id="24635-470">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="24635-471">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24635-472">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-473">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="24635-474">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-474">15 seconds</span></span> | <span data-ttu-id="24635-475">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="24635-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="24635-476">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="24635-477">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="24635-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="24635-478">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="24635-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="24635-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="24635-480">Option</span><span class="sxs-lookup"><span data-stu-id="24635-480">Option</span></span> | <span data-ttu-id="24635-481">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-481">Default value</span></span> | <span data-ttu-id="24635-482">Description</span><span class="sxs-lookup"><span data-stu-id="24635-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="24635-483">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-484">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-484">Timeout for server activity.</span></span> <span data-ttu-id="24635-485">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="24635-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="24635-486">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-487">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="24635-488">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="24635-489">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="24635-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="24635-490">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="24635-491">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="24635-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="24635-492">Java</span><span class="sxs-lookup"><span data-stu-id="24635-492">Java</span></span>](#tab/java)

| <span data-ttu-id="24635-493">Option</span><span class="sxs-lookup"><span data-stu-id="24635-493">Option</span></span> | <span data-ttu-id="24635-494">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-494">Default value</span></span> | <span data-ttu-id="24635-495">Description</span><span class="sxs-lookup"><span data-stu-id="24635-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="24635-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="24635-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="24635-497">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-498">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-498">Timeout for server activity.</span></span> <span data-ttu-id="24635-499">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="24635-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="24635-500">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-501">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="24635-502">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-502">15 seconds</span></span> | <span data-ttu-id="24635-503">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="24635-504">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="24635-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="24635-505">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-506">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="24635-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="24635-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="24635-508">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="24635-509">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="24635-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="24635-510">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="24635-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="24635-511">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="24635-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="24635-512">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24635-512">Configure additional options</span></span>

<span data-ttu-id="24635-513">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="24635-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="24635-514">.NET</span><span class="sxs-lookup"><span data-stu-id="24635-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="24635-515">Option .NET</span><span class="sxs-lookup"><span data-stu-id="24635-515">.NET Option</span></span> |  <span data-ttu-id="24635-516">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-516">Default value</span></span> | <span data-ttu-id="24635-517">Description</span><span class="sxs-lookup"><span data-stu-id="24635-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="24635-518">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="24635-519">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-520">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-521">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="24635-522">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-522">Empty</span></span> | <span data-ttu-id="24635-523">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="24635-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="24635-524">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-524">Empty</span></span> | <span data-ttu-id="24635-525">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="24635-526">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-526">Empty</span></span> | <span data-ttu-id="24635-527">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="24635-528">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-528">5 seconds</span></span> | <span data-ttu-id="24635-529">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="24635-529">WebSockets only.</span></span> <span data-ttu-id="24635-530">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="24635-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="24635-531">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="24635-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="24635-532">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-532">Empty</span></span> | <span data-ttu-id="24635-533">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="24635-534">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="24635-535">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24635-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="24635-536">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="24635-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="24635-537">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="24635-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="24635-538">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="24635-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="24635-539">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="24635-540">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24635-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="24635-541">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="24635-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="24635-542">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="24635-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="24635-543">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="24635-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="24635-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="24635-545">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-545">JavaScript Option</span></span> | <span data-ttu-id="24635-546">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-546">Default Value</span></span> | <span data-ttu-id="24635-547">Description</span><span class="sxs-lookup"><span data-stu-id="24635-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="24635-548">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="24635-549">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-550">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-551">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="24635-552">Java</span><span class="sxs-lookup"><span data-stu-id="24635-552">Java</span></span>](#tab/java)

| <span data-ttu-id="24635-553">Option Java</span><span class="sxs-lookup"><span data-stu-id="24635-553">Java Option</span></span> | <span data-ttu-id="24635-554">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-554">Default Value</span></span> | <span data-ttu-id="24635-555">Description</span><span class="sxs-lookup"><span data-stu-id="24635-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="24635-556">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="24635-557">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-558">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-559">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="24635-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="24635-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="24635-561">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-561">Empty</span></span> | <span data-ttu-id="24635-562">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="24635-563">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="24635-564">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="24635-565">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="24635-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="24635-566">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24635-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="24635-567">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="24635-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="24635-568">ASP.NET Core Signalr prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="24635-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="24635-569">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="24635-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="24635-570">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol), qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24635-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="24635-571">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="24635-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="24635-572">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet JSON.net `JsonSerializerSettings` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="24635-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="24635-573">Pour plus d’informations, voir la [documentation sur JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="24635-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="24635-574">Par exemple, pour configurer le sérialiseur afin d’utiliser les noms de propriété « casse Pascal », au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="24635-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="24635-575">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="24635-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="24635-576">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="24635-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="24635-577">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24635-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="24635-578">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="24635-578">MessagePack serialization options</span></span>

<span data-ttu-id="24635-579">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="24635-580">Pour plus d’informations, consultez [MessagePack dans signalr](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="24635-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="24635-581">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="24635-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="24635-582">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="24635-582">Configure server options</span></span>

<span data-ttu-id="24635-583">Le tableau suivant décrit les options de configuration des hubs Signalr :</span><span class="sxs-lookup"><span data-stu-id="24635-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="24635-584">Option</span><span class="sxs-lookup"><span data-stu-id="24635-584">Option</span></span> | <span data-ttu-id="24635-585">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-585">Default Value</span></span> | <span data-ttu-id="24635-586">Description</span><span class="sxs-lookup"><span data-stu-id="24635-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="24635-587">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-587">15 seconds</span></span> | <span data-ttu-id="24635-588">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="24635-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="24635-589">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-590">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="24635-591">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-591">15 seconds</span></span> | <span data-ttu-id="24635-592">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="24635-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="24635-593">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="24635-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="24635-594">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="24635-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="24635-595">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="24635-595">All installed protocols</span></span> | <span data-ttu-id="24635-596">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="24635-596">Protocols supported by this hub.</span></span> <span data-ttu-id="24635-597">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="24635-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="24635-598">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="24635-599">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="24635-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="24635-600">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24635-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="24635-601">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="24635-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="24635-602">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="24635-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="24635-603">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="24635-604">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="24635-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="24635-605">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core Signalr :</span><span class="sxs-lookup"><span data-stu-id="24635-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="24635-606">Option</span><span class="sxs-lookup"><span data-stu-id="24635-606">Option</span></span> | <span data-ttu-id="24635-607">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-607">Default Value</span></span> | <span data-ttu-id="24635-608">Description</span><span class="sxs-lookup"><span data-stu-id="24635-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="24635-609">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-609">32 KB</span></span> | <span data-ttu-id="24635-610">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="24635-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="24635-611">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="24635-612">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="24635-613">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="24635-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="24635-614">32 Ko</span><span class="sxs-lookup"><span data-stu-id="24635-614">32 KB</span></span> | <span data-ttu-id="24635-615">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="24635-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="24635-616">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="24635-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="24635-617">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="24635-617">All Transports are enabled.</span></span> | <span data-ttu-id="24635-618">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="24635-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="24635-619">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24635-619">See below.</span></span> | <span data-ttu-id="24635-620">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="24635-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="24635-621">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="24635-621">See below.</span></span> | <span data-ttu-id="24635-622">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="24635-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="24635-623">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="24635-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="24635-624">Option</span><span class="sxs-lookup"><span data-stu-id="24635-624">Option</span></span> | <span data-ttu-id="24635-625">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-625">Default Value</span></span> | <span data-ttu-id="24635-626">Description</span><span class="sxs-lookup"><span data-stu-id="24635-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="24635-627">90 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-627">90 seconds</span></span> | <span data-ttu-id="24635-628">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="24635-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="24635-629">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="24635-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="24635-630">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="24635-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="24635-631">Option</span><span class="sxs-lookup"><span data-stu-id="24635-631">Option</span></span> | <span data-ttu-id="24635-632">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-632">Default Value</span></span> | <span data-ttu-id="24635-633">Description</span><span class="sxs-lookup"><span data-stu-id="24635-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="24635-634">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-634">5 seconds</span></span> | <span data-ttu-id="24635-635">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="24635-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="24635-636">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="24635-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="24635-637">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="24635-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="24635-638">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="24635-638">Configure client options</span></span>

<span data-ttu-id="24635-639">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="24635-640">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="24635-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="24635-641">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="24635-641">Configure logging</span></span>

<span data-ttu-id="24635-642">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24635-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="24635-643">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="24635-644">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="24635-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="24635-645">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="24635-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="24635-646">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="24635-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="24635-647">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="24635-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="24635-648">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="24635-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="24635-649">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="24635-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="24635-650">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="24635-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="24635-651">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="24635-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="24635-652">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="24635-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="24635-653">Pour plus d’informations sur la journalisation, consultez la [documentation relative aux diagnostics de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="24635-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="24635-654">Le client Java Signalr utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="24635-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="24635-655">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="24635-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="24635-656">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java Signalr.</span><span class="sxs-lookup"><span data-stu-id="24635-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="24635-657">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="24635-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="24635-658">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="24635-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="24635-659">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="24635-659">Configure allowed transports</span></span>

<span data-ttu-id="24635-660">Les transports utilisés par Signalr peuvent être configurés dans l’appel `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="24635-661">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="24635-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="24635-662">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="24635-662">All transports are enabled by default.</span></span>

<span data-ttu-id="24635-663">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="24635-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="24635-664">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="24635-665">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="24635-665">Configure bearer authentication</span></span>

<span data-ttu-id="24635-666">Pour fournir des données d’authentification avec les demandes Signalr, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="24635-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="24635-667">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="24635-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="24635-668">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="24635-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="24635-669">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="24635-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="24635-670">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="24635-671">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="24635-672">Dans le client Java Signalr, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="24635-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="24635-673">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="24635-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="24635-674">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="24635-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="24635-675">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="24635-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="24635-676">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="24635-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="24635-677">.NET</span><span class="sxs-lookup"><span data-stu-id="24635-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="24635-678">Option</span><span class="sxs-lookup"><span data-stu-id="24635-678">Option</span></span> | <span data-ttu-id="24635-679">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-679">Default value</span></span> | <span data-ttu-id="24635-680">Description</span><span class="sxs-lookup"><span data-stu-id="24635-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="24635-681">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-682">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-682">Timeout for server activity.</span></span> <span data-ttu-id="24635-683">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24635-684">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-685">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="24635-686">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-686">15 seconds</span></span> | <span data-ttu-id="24635-687">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="24635-688">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="24635-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="24635-689">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-690">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="24635-691">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="24635-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="24635-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="24635-693">Option</span><span class="sxs-lookup"><span data-stu-id="24635-693">Option</span></span> | <span data-ttu-id="24635-694">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-694">Default value</span></span> | <span data-ttu-id="24635-695">Description</span><span class="sxs-lookup"><span data-stu-id="24635-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="24635-696">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-697">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-697">Timeout for server activity.</span></span> <span data-ttu-id="24635-698">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="24635-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="24635-699">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-700">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="24635-701">Java</span><span class="sxs-lookup"><span data-stu-id="24635-701">Java</span></span>](#tab/java)

| <span data-ttu-id="24635-702">Option</span><span class="sxs-lookup"><span data-stu-id="24635-702">Option</span></span> | <span data-ttu-id="24635-703">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-703">Default value</span></span> | <span data-ttu-id="24635-704">Description</span><span class="sxs-lookup"><span data-stu-id="24635-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="24635-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="24635-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="24635-706">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="24635-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="24635-707">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-707">Timeout for server activity.</span></span> <span data-ttu-id="24635-708">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="24635-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="24635-709">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="24635-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="24635-710">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur, afin de laisser le temps de recevoir les commandes ping.</span><span class="sxs-lookup"><span data-stu-id="24635-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="24635-711">15 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-711">15 seconds</span></span> | <span data-ttu-id="24635-712">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="24635-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="24635-713">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="24635-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="24635-714">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="24635-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="24635-715">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="24635-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="24635-716">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24635-716">Configure additional options</span></span>

<span data-ttu-id="24635-717">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="24635-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="24635-718">.NET</span><span class="sxs-lookup"><span data-stu-id="24635-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="24635-719">Option .NET</span><span class="sxs-lookup"><span data-stu-id="24635-719">.NET Option</span></span> |  <span data-ttu-id="24635-720">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-720">Default value</span></span> | <span data-ttu-id="24635-721">Description</span><span class="sxs-lookup"><span data-stu-id="24635-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="24635-722">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="24635-723">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-724">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-725">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="24635-726">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-726">Empty</span></span> | <span data-ttu-id="24635-727">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="24635-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="24635-728">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-728">Empty</span></span> | <span data-ttu-id="24635-729">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="24635-730">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-730">Empty</span></span> | <span data-ttu-id="24635-731">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="24635-732">5 secondes</span><span class="sxs-lookup"><span data-stu-id="24635-732">5 seconds</span></span> | <span data-ttu-id="24635-733">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="24635-733">WebSockets only.</span></span> <span data-ttu-id="24635-734">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="24635-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="24635-735">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="24635-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="24635-736">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-736">Empty</span></span> | <span data-ttu-id="24635-737">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="24635-738">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="24635-739">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24635-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="24635-740">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="24635-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="24635-741">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="24635-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="24635-742">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="24635-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="24635-743">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="24635-744">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24635-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="24635-745">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="24635-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="24635-746">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="24635-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="24635-747">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="24635-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="24635-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="24635-749">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="24635-749">JavaScript Option</span></span> | <span data-ttu-id="24635-750">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-750">Default Value</span></span> | <span data-ttu-id="24635-751">Description</span><span class="sxs-lookup"><span data-stu-id="24635-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="24635-752">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="24635-753">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-754">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-755">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="24635-756">Java</span><span class="sxs-lookup"><span data-stu-id="24635-756">Java</span></span>](#tab/java)

| <span data-ttu-id="24635-757">Option Java</span><span class="sxs-lookup"><span data-stu-id="24635-757">Java Option</span></span> | <span data-ttu-id="24635-758">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="24635-758">Default Value</span></span> | <span data-ttu-id="24635-759">Description</span><span class="sxs-lookup"><span data-stu-id="24635-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="24635-760">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="24635-761">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="24635-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="24635-762">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="24635-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="24635-763">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="24635-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="24635-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="24635-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="24635-765">Empty</span><span class="sxs-lookup"><span data-stu-id="24635-765">Empty</span></span> | <span data-ttu-id="24635-766">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="24635-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="24635-767">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="24635-768">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="24635-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="24635-769">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="24635-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="24635-770">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="24635-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end