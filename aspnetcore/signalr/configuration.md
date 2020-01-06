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
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358110"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="72252-103">Configuration de la SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72252-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="72252-104">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="72252-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="72252-105">ASP.NET Core SignalR prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="72252-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="72252-106">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="72252-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="72252-107">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="72252-108">`AddJsonProtocol` peut être ajouté après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72252-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="72252-109">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="72252-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="72252-110">La propriété [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) sur cet objet est un objet <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="72252-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="72252-111">Pour plus d’informations, consultez la [Documentation System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="72252-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="72252-112">Par exemple, pour configurer le sérialiseur de manière à ce qu’il ne change pas la casse des noms de propriétés, au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72252-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="72252-113">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="72252-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="72252-114">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="72252-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="72252-115">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="72252-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="72252-116">Basculer vers Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="72252-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="72252-117">Si vous avez besoin de fonctionnalités de `Newtonsoft.Json` qui ne sont pas prises en charge dans `System.Text.Json`, consultez [basculer vers Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="72252-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="72252-118">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="72252-118">MessagePack serialization options</span></span>

<span data-ttu-id="72252-119">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="72252-120">Pour plus d’informations, consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="72252-121">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="72252-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="72252-122">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="72252-122">Configure server options</span></span>

<span data-ttu-id="72252-123">Le tableau suivant décrit les options de configuration des SignalR hubs :</span><span class="sxs-lookup"><span data-stu-id="72252-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="72252-124">Option</span><span class="sxs-lookup"><span data-stu-id="72252-124">Option</span></span> | <span data-ttu-id="72252-125">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-125">Default Value</span></span> | <span data-ttu-id="72252-126">Description</span><span class="sxs-lookup"><span data-stu-id="72252-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="72252-127">30 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-127">30 seconds</span></span> | <span data-ttu-id="72252-128">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="72252-129">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="72252-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="72252-130">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="72252-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="72252-131">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-131">15 seconds</span></span> | <span data-ttu-id="72252-132">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="72252-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="72252-133">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-134">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72252-135">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-135">15 seconds</span></span> | <span data-ttu-id="72252-136">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="72252-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="72252-137">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="72252-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="72252-138">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="72252-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="72252-139">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="72252-139">All installed protocols</span></span> | <span data-ttu-id="72252-140">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="72252-140">Protocols supported by this hub.</span></span> <span data-ttu-id="72252-141">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="72252-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="72252-142">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="72252-143">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="72252-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="72252-144">Nombre maximal d’éléments pouvant être mis en mémoire tampon pour les flux de téléchargement du client.</span><span class="sxs-lookup"><span data-stu-id="72252-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="72252-145">Si cette limite est atteinte, le traitement des appels est bloqué jusqu’à ce que le serveur traite les éléments de flux.</span><span class="sxs-lookup"><span data-stu-id="72252-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="72252-146">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-146">32 KB</span></span> | <span data-ttu-id="72252-147">Taille maximale d’un seul message de concentrateur entrant.</span><span class="sxs-lookup"><span data-stu-id="72252-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="72252-148">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72252-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="72252-149">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="72252-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="72252-150">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="72252-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="72252-151">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="72252-152">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="72252-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="72252-153">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="72252-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="72252-154">Option</span><span class="sxs-lookup"><span data-stu-id="72252-154">Option</span></span> | <span data-ttu-id="72252-155">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-155">Default Value</span></span> | <span data-ttu-id="72252-156">Description</span><span class="sxs-lookup"><span data-stu-id="72252-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="72252-157">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-157">32 KB</span></span> | <span data-ttu-id="72252-158">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon avant d’appliquer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="72252-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="72252-159">L’augmentation de cette valeur permet au serveur de recevoir plus rapidement des messages plus volumineux sans appliquer la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="72252-160">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="72252-161">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="72252-162">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-162">32 KB</span></span> | <span data-ttu-id="72252-163">Nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon avant d’observer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="72252-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="72252-164">L’augmentation de cette valeur permet au serveur de mettre plus rapidement en mémoire tampon des messages plus volumineux sans attendre la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="72252-165">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="72252-165">All Transports are enabled.</span></span> | <span data-ttu-id="72252-166">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="72252-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="72252-167">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="72252-167">See below.</span></span> | <span data-ttu-id="72252-168">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="72252-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="72252-169">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="72252-169">See below.</span></span> | <span data-ttu-id="72252-170">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="72252-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="72252-171">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="72252-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="72252-172">Option</span><span class="sxs-lookup"><span data-stu-id="72252-172">Option</span></span> | <span data-ttu-id="72252-173">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-173">Default Value</span></span> | <span data-ttu-id="72252-174">Description</span><span class="sxs-lookup"><span data-stu-id="72252-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="72252-175">90 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-175">90 seconds</span></span> | <span data-ttu-id="72252-176">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="72252-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="72252-177">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="72252-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="72252-178">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="72252-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="72252-179">Option</span><span class="sxs-lookup"><span data-stu-id="72252-179">Option</span></span> | <span data-ttu-id="72252-180">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-180">Default Value</span></span> | <span data-ttu-id="72252-181">Description</span><span class="sxs-lookup"><span data-stu-id="72252-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="72252-182">5 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-182">5 seconds</span></span> | <span data-ttu-id="72252-183">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="72252-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="72252-184">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="72252-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="72252-185">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="72252-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="72252-186">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="72252-186">Configure client options</span></span>

<span data-ttu-id="72252-187">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="72252-188">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="72252-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="72252-189">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="72252-189">Configure logging</span></span>

<span data-ttu-id="72252-190">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="72252-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="72252-191">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="72252-192">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="72252-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="72252-193">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="72252-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="72252-194">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="72252-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="72252-195">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="72252-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="72252-196">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="72252-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="72252-197">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="72252-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="72252-198">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="72252-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="72252-199">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="72252-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="72252-200">Au lieu d’une valeur `LogLevel`, vous pouvez également fournir une valeur de `string` représentant un nom de niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="72252-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="72252-201">Cela est utile lors de la configuration de la journalisation SignalR dans les environnements où vous n’avez pas accès aux constantes `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="72252-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="72252-202">Le tableau suivant répertorie les niveaux de journalisation disponibles.</span><span class="sxs-lookup"><span data-stu-id="72252-202">The following table lists the available log levels.</span></span> <span data-ttu-id="72252-203">La valeur que vous fournissez à `configureLogging` définit le niveau de journalisation **minimale** qui sera enregistré.</span><span class="sxs-lookup"><span data-stu-id="72252-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="72252-204">Les messages enregistrés à ce niveau, **ou les niveaux indiqués après celui-ci dans la table**, sont enregistrés.</span><span class="sxs-lookup"><span data-stu-id="72252-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="72252-205">Chaîne</span><span class="sxs-lookup"><span data-stu-id="72252-205">String</span></span>                      | <span data-ttu-id="72252-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="72252-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="72252-207">`info` **ou** `information`</span><span class="sxs-lookup"><span data-stu-id="72252-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="72252-208">`warn` **ou** `warning`</span><span class="sxs-lookup"><span data-stu-id="72252-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="72252-209">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="72252-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="72252-210">Pour plus d’informations sur la journalisation, consultez la [documentationSignalR Diagnostics](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="72252-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="72252-211">Le client Java SignalR utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="72252-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="72252-212">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="72252-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="72252-213">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="72252-214">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="72252-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="72252-215">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="72252-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="72252-216">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="72252-216">Configure allowed transports</span></span>

<span data-ttu-id="72252-217">Les transports utilisés par SignalR peuvent être configurés dans l’appel de `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="72252-218">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="72252-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="72252-219">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="72252-219">All transports are enabled by default.</span></span>

<span data-ttu-id="72252-220">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="72252-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="72252-221">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="72252-222">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="72252-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="72252-223">Dans le client Java, le transport est sélectionné à l’aide de la méthode `withTransport` sur le `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="72252-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="72252-224">Le client Java utilise le transport WebSocket par défaut.</span><span class="sxs-lookup"><span data-stu-id="72252-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="72252-225">Le client Java SignalR ne prend pas encore en charge le transport de secours.</span><span class="sxs-lookup"><span data-stu-id="72252-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="72252-226">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="72252-226">Configure bearer authentication</span></span>

<span data-ttu-id="72252-227">Pour fournir des données d’authentification avec les demandes de SignalR, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` en JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="72252-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="72252-228">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="72252-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="72252-229">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="72252-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="72252-230">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="72252-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="72252-231">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="72252-232">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="72252-233">Dans le client Java SignalR, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="72252-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="72252-234">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="72252-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="72252-235">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="72252-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="72252-236">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="72252-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="72252-237">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="72252-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="72252-238">.NET</span><span class="sxs-lookup"><span data-stu-id="72252-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="72252-239">Option</span><span class="sxs-lookup"><span data-stu-id="72252-239">Option</span></span> | <span data-ttu-id="72252-240">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-240">Default value</span></span> | <span data-ttu-id="72252-241">Description</span><span class="sxs-lookup"><span data-stu-id="72252-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="72252-242">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-243">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-243">Timeout for server activity.</span></span> <span data-ttu-id="72252-244">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72252-245">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-246">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="72252-247">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-247">15 seconds</span></span> | <span data-ttu-id="72252-248">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="72252-249">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72252-250">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-251">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72252-252">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-252">15 seconds</span></span> | <span data-ttu-id="72252-253">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="72252-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="72252-254">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="72252-255">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="72252-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="72252-256">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="72252-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="72252-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="72252-258">Option</span><span class="sxs-lookup"><span data-stu-id="72252-258">Option</span></span> | <span data-ttu-id="72252-259">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-259">Default value</span></span> | <span data-ttu-id="72252-260">Description</span><span class="sxs-lookup"><span data-stu-id="72252-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="72252-261">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-262">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-262">Timeout for server activity.</span></span> <span data-ttu-id="72252-263">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="72252-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="72252-264">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-265">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="72252-266">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="72252-267">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="72252-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="72252-268">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="72252-269">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="72252-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="72252-270">Java</span><span class="sxs-lookup"><span data-stu-id="72252-270">Java</span></span>](#tab/java)

| <span data-ttu-id="72252-271">Option</span><span class="sxs-lookup"><span data-stu-id="72252-271">Option</span></span> | <span data-ttu-id="72252-272">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-272">Default value</span></span> | <span data-ttu-id="72252-273">Description</span><span class="sxs-lookup"><span data-stu-id="72252-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="72252-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="72252-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="72252-275">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-276">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-276">Timeout for server activity.</span></span> <span data-ttu-id="72252-277">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="72252-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="72252-278">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-279">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="72252-280">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-280">15 seconds</span></span> | <span data-ttu-id="72252-281">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="72252-282">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="72252-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="72252-283">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-284">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="72252-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="72252-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="72252-286">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="72252-287">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="72252-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="72252-288">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="72252-289">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="72252-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="72252-290">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72252-290">Configure additional options</span></span>

<span data-ttu-id="72252-291">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="72252-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="72252-292">.NET</span><span class="sxs-lookup"><span data-stu-id="72252-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="72252-293">Option .NET</span><span class="sxs-lookup"><span data-stu-id="72252-293">.NET Option</span></span> |  <span data-ttu-id="72252-294">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-294">Default value</span></span> | <span data-ttu-id="72252-295">Description</span><span class="sxs-lookup"><span data-stu-id="72252-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="72252-296">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="72252-297">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-298">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-299">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="72252-300">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-300">Empty</span></span> | <span data-ttu-id="72252-301">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="72252-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="72252-302">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-302">Empty</span></span> | <span data-ttu-id="72252-303">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="72252-304">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-304">Empty</span></span> | <span data-ttu-id="72252-305">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="72252-306">5 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-306">5 seconds</span></span> | <span data-ttu-id="72252-307">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="72252-307">WebSockets only.</span></span> <span data-ttu-id="72252-308">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="72252-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="72252-309">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="72252-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="72252-310">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-310">Empty</span></span> | <span data-ttu-id="72252-311">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="72252-312">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="72252-313">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72252-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="72252-314">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="72252-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="72252-315">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="72252-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="72252-316">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="72252-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="72252-317">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="72252-318">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72252-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="72252-319">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="72252-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="72252-320">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="72252-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="72252-321">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="72252-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="72252-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="72252-323">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-323">JavaScript Option</span></span> | <span data-ttu-id="72252-324">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-324">Default Value</span></span> | <span data-ttu-id="72252-325">Description</span><span class="sxs-lookup"><span data-stu-id="72252-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="72252-326">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="72252-327">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-328">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-329">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="72252-330">Java</span><span class="sxs-lookup"><span data-stu-id="72252-330">Java</span></span>](#tab/java)

| <span data-ttu-id="72252-331">Option Java</span><span class="sxs-lookup"><span data-stu-id="72252-331">Java Option</span></span> | <span data-ttu-id="72252-332">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-332">Default Value</span></span> | <span data-ttu-id="72252-333">Description</span><span class="sxs-lookup"><span data-stu-id="72252-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="72252-334">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="72252-335">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-336">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-337">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="72252-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="72252-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="72252-339">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-339">Empty</span></span> | <span data-ttu-id="72252-340">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="72252-341">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="72252-342">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="72252-343">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="72252-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="72252-344">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72252-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="72252-345">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="72252-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="72252-346">ASP.NET Core SignalR prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="72252-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="72252-347">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="72252-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="72252-348">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol), qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72252-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="72252-349">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="72252-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="72252-350">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet JSON.net `JsonSerializerSettings` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="72252-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="72252-351">Pour plus d’informations, voir la [documentation sur JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="72252-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="72252-352">Par exemple, pour configurer le sérialiseur afin d’utiliser les noms de propriété « casse Pascal », au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72252-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="72252-353">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="72252-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="72252-354">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="72252-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="72252-355">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="72252-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="72252-356">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="72252-356">MessagePack serialization options</span></span>

<span data-ttu-id="72252-357">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="72252-358">Pour plus d’informations, consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="72252-359">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="72252-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="72252-360">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="72252-360">Configure server options</span></span>

<span data-ttu-id="72252-361">Le tableau suivant décrit les options de configuration des SignalR hubs :</span><span class="sxs-lookup"><span data-stu-id="72252-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="72252-362">Option</span><span class="sxs-lookup"><span data-stu-id="72252-362">Option</span></span> | <span data-ttu-id="72252-363">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-363">Default Value</span></span> | <span data-ttu-id="72252-364">Description</span><span class="sxs-lookup"><span data-stu-id="72252-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="72252-365">30 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-365">30 seconds</span></span> | <span data-ttu-id="72252-366">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="72252-367">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="72252-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="72252-368">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="72252-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="72252-369">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-369">15 seconds</span></span> | <span data-ttu-id="72252-370">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="72252-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="72252-371">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-372">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72252-373">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-373">15 seconds</span></span> | <span data-ttu-id="72252-374">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="72252-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="72252-375">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="72252-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="72252-376">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="72252-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="72252-377">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="72252-377">All installed protocols</span></span> | <span data-ttu-id="72252-378">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="72252-378">Protocols supported by this hub.</span></span> <span data-ttu-id="72252-379">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="72252-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="72252-380">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="72252-381">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="72252-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="72252-382">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72252-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="72252-383">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="72252-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="72252-384">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="72252-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="72252-385">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="72252-386">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="72252-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="72252-387">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="72252-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="72252-388">Option</span><span class="sxs-lookup"><span data-stu-id="72252-388">Option</span></span> | <span data-ttu-id="72252-389">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-389">Default Value</span></span> | <span data-ttu-id="72252-390">Description</span><span class="sxs-lookup"><span data-stu-id="72252-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="72252-391">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-391">32 KB</span></span> | <span data-ttu-id="72252-392">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="72252-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="72252-393">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="72252-394">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="72252-395">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="72252-396">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-396">32 KB</span></span> | <span data-ttu-id="72252-397">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="72252-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="72252-398">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="72252-399">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="72252-399">All Transports are enabled.</span></span> | <span data-ttu-id="72252-400">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="72252-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="72252-401">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="72252-401">See below.</span></span> | <span data-ttu-id="72252-402">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="72252-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="72252-403">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="72252-403">See below.</span></span> | <span data-ttu-id="72252-404">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="72252-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="72252-405">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="72252-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="72252-406">Option</span><span class="sxs-lookup"><span data-stu-id="72252-406">Option</span></span> | <span data-ttu-id="72252-407">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-407">Default Value</span></span> | <span data-ttu-id="72252-408">Description</span><span class="sxs-lookup"><span data-stu-id="72252-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="72252-409">90 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-409">90 seconds</span></span> | <span data-ttu-id="72252-410">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="72252-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="72252-411">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="72252-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="72252-412">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="72252-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="72252-413">Option</span><span class="sxs-lookup"><span data-stu-id="72252-413">Option</span></span> | <span data-ttu-id="72252-414">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-414">Default Value</span></span> | <span data-ttu-id="72252-415">Description</span><span class="sxs-lookup"><span data-stu-id="72252-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="72252-416">5 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-416">5 seconds</span></span> | <span data-ttu-id="72252-417">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="72252-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="72252-418">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="72252-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="72252-419">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="72252-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="72252-420">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="72252-420">Configure client options</span></span>

<span data-ttu-id="72252-421">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="72252-422">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="72252-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="72252-423">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="72252-423">Configure logging</span></span>

<span data-ttu-id="72252-424">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="72252-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="72252-425">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="72252-426">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="72252-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="72252-427">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="72252-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="72252-428">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="72252-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="72252-429">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="72252-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="72252-430">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="72252-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="72252-431">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="72252-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="72252-432">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="72252-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="72252-433">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="72252-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="72252-434">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="72252-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="72252-435">Pour plus d’informations sur la journalisation, consultez la [documentationSignalR Diagnostics](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="72252-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="72252-436">Le client Java SignalR utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="72252-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="72252-437">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="72252-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="72252-438">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="72252-439">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="72252-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="72252-440">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="72252-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="72252-441">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="72252-441">Configure allowed transports</span></span>

<span data-ttu-id="72252-442">Les transports utilisés par SignalR peuvent être configurés dans l’appel de `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="72252-443">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="72252-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="72252-444">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="72252-444">All transports are enabled by default.</span></span>

<span data-ttu-id="72252-445">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="72252-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="72252-446">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="72252-447">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="72252-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="72252-448">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="72252-448">Configure bearer authentication</span></span>

<span data-ttu-id="72252-449">Pour fournir des données d’authentification avec les demandes de SignalR, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` en JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="72252-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="72252-450">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="72252-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="72252-451">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="72252-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="72252-452">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="72252-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="72252-453">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="72252-454">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="72252-455">Dans le client Java SignalR, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="72252-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="72252-456">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="72252-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="72252-457">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="72252-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="72252-458">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="72252-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="72252-459">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="72252-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="72252-460">.NET</span><span class="sxs-lookup"><span data-stu-id="72252-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="72252-461">Option</span><span class="sxs-lookup"><span data-stu-id="72252-461">Option</span></span> | <span data-ttu-id="72252-462">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-462">Default value</span></span> | <span data-ttu-id="72252-463">Description</span><span class="sxs-lookup"><span data-stu-id="72252-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="72252-464">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-465">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-465">Timeout for server activity.</span></span> <span data-ttu-id="72252-466">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72252-467">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-468">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="72252-469">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-469">15 seconds</span></span> | <span data-ttu-id="72252-470">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="72252-471">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72252-472">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-473">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72252-474">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-474">15 seconds</span></span> | <span data-ttu-id="72252-475">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="72252-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="72252-476">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="72252-477">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="72252-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="72252-478">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="72252-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="72252-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="72252-480">Option</span><span class="sxs-lookup"><span data-stu-id="72252-480">Option</span></span> | <span data-ttu-id="72252-481">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-481">Default value</span></span> | <span data-ttu-id="72252-482">Description</span><span class="sxs-lookup"><span data-stu-id="72252-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="72252-483">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-484">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-484">Timeout for server activity.</span></span> <span data-ttu-id="72252-485">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="72252-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="72252-486">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-487">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="72252-488">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="72252-489">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="72252-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="72252-490">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="72252-491">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="72252-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="72252-492">Java</span><span class="sxs-lookup"><span data-stu-id="72252-492">Java</span></span>](#tab/java)

| <span data-ttu-id="72252-493">Option</span><span class="sxs-lookup"><span data-stu-id="72252-493">Option</span></span> | <span data-ttu-id="72252-494">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-494">Default value</span></span> | <span data-ttu-id="72252-495">Description</span><span class="sxs-lookup"><span data-stu-id="72252-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="72252-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="72252-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="72252-497">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-498">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-498">Timeout for server activity.</span></span> <span data-ttu-id="72252-499">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="72252-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="72252-500">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-501">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="72252-502">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-502">15 seconds</span></span> | <span data-ttu-id="72252-503">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="72252-504">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="72252-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="72252-505">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-506">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="72252-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="72252-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="72252-508">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="72252-509">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="72252-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="72252-510">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="72252-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="72252-511">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="72252-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="72252-512">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72252-512">Configure additional options</span></span>

<span data-ttu-id="72252-513">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="72252-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="72252-514">.NET</span><span class="sxs-lookup"><span data-stu-id="72252-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="72252-515">Option .NET</span><span class="sxs-lookup"><span data-stu-id="72252-515">.NET Option</span></span> |  <span data-ttu-id="72252-516">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-516">Default value</span></span> | <span data-ttu-id="72252-517">Description</span><span class="sxs-lookup"><span data-stu-id="72252-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="72252-518">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="72252-519">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-520">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-521">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="72252-522">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-522">Empty</span></span> | <span data-ttu-id="72252-523">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="72252-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="72252-524">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-524">Empty</span></span> | <span data-ttu-id="72252-525">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="72252-526">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-526">Empty</span></span> | <span data-ttu-id="72252-527">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="72252-528">5 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-528">5 seconds</span></span> | <span data-ttu-id="72252-529">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="72252-529">WebSockets only.</span></span> <span data-ttu-id="72252-530">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="72252-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="72252-531">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="72252-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="72252-532">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-532">Empty</span></span> | <span data-ttu-id="72252-533">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="72252-534">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="72252-535">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72252-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="72252-536">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="72252-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="72252-537">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="72252-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="72252-538">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="72252-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="72252-539">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="72252-540">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72252-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="72252-541">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="72252-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="72252-542">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="72252-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="72252-543">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="72252-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="72252-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="72252-545">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-545">JavaScript Option</span></span> | <span data-ttu-id="72252-546">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-546">Default Value</span></span> | <span data-ttu-id="72252-547">Description</span><span class="sxs-lookup"><span data-stu-id="72252-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="72252-548">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="72252-549">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-550">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-551">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="72252-552">Java</span><span class="sxs-lookup"><span data-stu-id="72252-552">Java</span></span>](#tab/java)

| <span data-ttu-id="72252-553">Option Java</span><span class="sxs-lookup"><span data-stu-id="72252-553">Java Option</span></span> | <span data-ttu-id="72252-554">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-554">Default Value</span></span> | <span data-ttu-id="72252-555">Description</span><span class="sxs-lookup"><span data-stu-id="72252-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="72252-556">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="72252-557">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-558">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-559">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="72252-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="72252-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="72252-561">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-561">Empty</span></span> | <span data-ttu-id="72252-562">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="72252-563">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="72252-564">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="72252-565">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="72252-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="72252-566">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72252-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="72252-567">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="72252-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="72252-568">ASP.NET Core SignalR prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="72252-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="72252-569">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="72252-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="72252-570">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol), qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72252-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="72252-571">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="72252-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="72252-572">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet JSON.net `JsonSerializerSettings` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="72252-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="72252-573">Pour plus d’informations, voir la [documentation sur JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="72252-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="72252-574">Par exemple, pour configurer le sérialiseur afin d’utiliser les noms de propriété « casse Pascal », au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="72252-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="72252-575">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="72252-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="72252-576">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="72252-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="72252-577">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="72252-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="72252-578">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="72252-578">MessagePack serialization options</span></span>

<span data-ttu-id="72252-579">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="72252-580">Pour plus d’informations, consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="72252-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="72252-581">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="72252-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="72252-582">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="72252-582">Configure server options</span></span>

<span data-ttu-id="72252-583">Le tableau suivant décrit les options de configuration des SignalR hubs :</span><span class="sxs-lookup"><span data-stu-id="72252-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="72252-584">Option</span><span class="sxs-lookup"><span data-stu-id="72252-584">Option</span></span> | <span data-ttu-id="72252-585">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-585">Default Value</span></span> | <span data-ttu-id="72252-586">Description</span><span class="sxs-lookup"><span data-stu-id="72252-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="72252-587">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-587">15 seconds</span></span> | <span data-ttu-id="72252-588">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="72252-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="72252-589">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-590">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72252-591">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-591">15 seconds</span></span> | <span data-ttu-id="72252-592">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="72252-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="72252-593">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="72252-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="72252-594">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="72252-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="72252-595">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="72252-595">All installed protocols</span></span> | <span data-ttu-id="72252-596">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="72252-596">Protocols supported by this hub.</span></span> <span data-ttu-id="72252-597">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="72252-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="72252-598">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="72252-599">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="72252-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="72252-600">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72252-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="72252-601">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="72252-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="72252-602">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="72252-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="72252-603">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="72252-604">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="72252-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="72252-605">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="72252-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="72252-606">Option</span><span class="sxs-lookup"><span data-stu-id="72252-606">Option</span></span> | <span data-ttu-id="72252-607">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-607">Default Value</span></span> | <span data-ttu-id="72252-608">Description</span><span class="sxs-lookup"><span data-stu-id="72252-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="72252-609">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-609">32 KB</span></span> | <span data-ttu-id="72252-610">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="72252-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="72252-611">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="72252-612">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="72252-613">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="72252-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="72252-614">32 Ko</span><span class="sxs-lookup"><span data-stu-id="72252-614">32 KB</span></span> | <span data-ttu-id="72252-615">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="72252-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="72252-616">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="72252-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="72252-617">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="72252-617">All Transports are enabled.</span></span> | <span data-ttu-id="72252-618">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="72252-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="72252-619">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="72252-619">See below.</span></span> | <span data-ttu-id="72252-620">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="72252-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="72252-621">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="72252-621">See below.</span></span> | <span data-ttu-id="72252-622">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="72252-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="72252-623">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="72252-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="72252-624">Option</span><span class="sxs-lookup"><span data-stu-id="72252-624">Option</span></span> | <span data-ttu-id="72252-625">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-625">Default Value</span></span> | <span data-ttu-id="72252-626">Description</span><span class="sxs-lookup"><span data-stu-id="72252-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="72252-627">90 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-627">90 seconds</span></span> | <span data-ttu-id="72252-628">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="72252-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="72252-629">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="72252-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="72252-630">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="72252-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="72252-631">Option</span><span class="sxs-lookup"><span data-stu-id="72252-631">Option</span></span> | <span data-ttu-id="72252-632">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-632">Default Value</span></span> | <span data-ttu-id="72252-633">Description</span><span class="sxs-lookup"><span data-stu-id="72252-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="72252-634">5 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-634">5 seconds</span></span> | <span data-ttu-id="72252-635">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="72252-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="72252-636">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="72252-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="72252-637">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="72252-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="72252-638">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="72252-638">Configure client options</span></span>

<span data-ttu-id="72252-639">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="72252-640">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="72252-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="72252-641">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="72252-641">Configure logging</span></span>

<span data-ttu-id="72252-642">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="72252-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="72252-643">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="72252-644">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="72252-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="72252-645">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="72252-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="72252-646">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="72252-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="72252-647">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="72252-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="72252-648">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="72252-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="72252-649">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="72252-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="72252-650">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="72252-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="72252-651">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="72252-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="72252-652">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="72252-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="72252-653">Pour plus d’informations sur la journalisation, consultez la [documentationSignalR Diagnostics](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="72252-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="72252-654">Le client Java SignalR utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="72252-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="72252-655">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="72252-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="72252-656">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="72252-657">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="72252-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="72252-658">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="72252-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="72252-659">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="72252-659">Configure allowed transports</span></span>

<span data-ttu-id="72252-660">Les transports utilisés par SignalR peuvent être configurés dans l’appel de `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="72252-661">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="72252-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="72252-662">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="72252-662">All transports are enabled by default.</span></span>

<span data-ttu-id="72252-663">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="72252-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="72252-664">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="72252-665">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="72252-665">Configure bearer authentication</span></span>

<span data-ttu-id="72252-666">Pour fournir des données d’authentification avec les demandes de SignalR, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` en JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="72252-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="72252-667">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="72252-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="72252-668">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="72252-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="72252-669">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="72252-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="72252-670">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="72252-671">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="72252-672">Dans le client Java SignalR, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="72252-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="72252-673">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="72252-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="72252-674">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="72252-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="72252-675">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="72252-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="72252-676">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="72252-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="72252-677">.NET</span><span class="sxs-lookup"><span data-stu-id="72252-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="72252-678">Option</span><span class="sxs-lookup"><span data-stu-id="72252-678">Option</span></span> | <span data-ttu-id="72252-679">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-679">Default value</span></span> | <span data-ttu-id="72252-680">Description</span><span class="sxs-lookup"><span data-stu-id="72252-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="72252-681">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-682">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-682">Timeout for server activity.</span></span> <span data-ttu-id="72252-683">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72252-684">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-685">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="72252-686">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-686">15 seconds</span></span> | <span data-ttu-id="72252-687">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="72252-688">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72252-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="72252-689">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-690">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="72252-691">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="72252-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="72252-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="72252-693">Option</span><span class="sxs-lookup"><span data-stu-id="72252-693">Option</span></span> | <span data-ttu-id="72252-694">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-694">Default value</span></span> | <span data-ttu-id="72252-695">Description</span><span class="sxs-lookup"><span data-stu-id="72252-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="72252-696">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-697">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-697">Timeout for server activity.</span></span> <span data-ttu-id="72252-698">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="72252-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="72252-699">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-700">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="72252-701">Java</span><span class="sxs-lookup"><span data-stu-id="72252-701">Java</span></span>](#tab/java)

| <span data-ttu-id="72252-702">Option</span><span class="sxs-lookup"><span data-stu-id="72252-702">Option</span></span> | <span data-ttu-id="72252-703">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-703">Default value</span></span> | <span data-ttu-id="72252-704">Description</span><span class="sxs-lookup"><span data-stu-id="72252-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="72252-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="72252-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="72252-706">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="72252-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="72252-707">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-707">Timeout for server activity.</span></span> <span data-ttu-id="72252-708">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="72252-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="72252-709">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="72252-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="72252-710">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur, afin de laisser le temps de recevoir les commandes ping.</span><span class="sxs-lookup"><span data-stu-id="72252-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="72252-711">15 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-711">15 seconds</span></span> | <span data-ttu-id="72252-712">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="72252-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="72252-713">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="72252-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="72252-714">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="72252-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="72252-715">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="72252-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="72252-716">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72252-716">Configure additional options</span></span>

<span data-ttu-id="72252-717">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="72252-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="72252-718">.NET</span><span class="sxs-lookup"><span data-stu-id="72252-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="72252-719">Option .NET</span><span class="sxs-lookup"><span data-stu-id="72252-719">.NET Option</span></span> |  <span data-ttu-id="72252-720">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-720">Default value</span></span> | <span data-ttu-id="72252-721">Description</span><span class="sxs-lookup"><span data-stu-id="72252-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="72252-722">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="72252-723">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-724">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-725">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="72252-726">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-726">Empty</span></span> | <span data-ttu-id="72252-727">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="72252-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="72252-728">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-728">Empty</span></span> | <span data-ttu-id="72252-729">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="72252-730">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-730">Empty</span></span> | <span data-ttu-id="72252-731">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="72252-732">5 secondes</span><span class="sxs-lookup"><span data-stu-id="72252-732">5 seconds</span></span> | <span data-ttu-id="72252-733">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="72252-733">WebSockets only.</span></span> <span data-ttu-id="72252-734">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="72252-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="72252-735">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="72252-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="72252-736">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-736">Empty</span></span> | <span data-ttu-id="72252-737">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="72252-738">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="72252-739">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72252-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="72252-740">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="72252-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="72252-741">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="72252-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="72252-742">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="72252-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="72252-743">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="72252-744">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72252-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="72252-745">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="72252-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="72252-746">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="72252-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="72252-747">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="72252-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="72252-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="72252-749">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="72252-749">JavaScript Option</span></span> | <span data-ttu-id="72252-750">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-750">Default Value</span></span> | <span data-ttu-id="72252-751">Description</span><span class="sxs-lookup"><span data-stu-id="72252-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="72252-752">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="72252-753">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-754">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-755">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="72252-756">Java</span><span class="sxs-lookup"><span data-stu-id="72252-756">Java</span></span>](#tab/java)

| <span data-ttu-id="72252-757">Option Java</span><span class="sxs-lookup"><span data-stu-id="72252-757">Java Option</span></span> | <span data-ttu-id="72252-758">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="72252-758">Default Value</span></span> | <span data-ttu-id="72252-759">Description</span><span class="sxs-lookup"><span data-stu-id="72252-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="72252-760">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="72252-761">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="72252-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72252-762">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="72252-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72252-763">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72252-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="72252-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="72252-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="72252-765">Empty</span><span class="sxs-lookup"><span data-stu-id="72252-765">Empty</span></span> | <span data-ttu-id="72252-766">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="72252-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="72252-767">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="72252-768">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72252-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="72252-769">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="72252-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="72252-770">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="72252-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end