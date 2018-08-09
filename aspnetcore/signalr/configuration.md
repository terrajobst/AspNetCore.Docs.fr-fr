---
title: Configuration d’ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/31/2018
uid: signalr/configuration
ms.openlocfilehash: eac1202828edbcd295d7e52aa424cd625ee70e34
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722462"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="3f4ee-103">Configuration d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3f4ee-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="3f4ee-104">Options de sérialisation de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="3f4ee-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="3f4ee-105">ASP.NET Core SignalR prend en charge deux protocoles d’encodage de messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="3f4ee-106">Chaque protocole a des options de configuration de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="3f4ee-107">Sérialisation JSON peut être configurée sur le serveur à l’aide de la [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3f4ee-108">Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="3f4ee-109">Le [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="3f4ee-110">Consultez le [Documentation JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="3f4ee-111">Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « La casse Pascal », au lieu des noms de « la casse mixte » par défaut, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="3f4ee-112">Dans le client .NET, les mêmes `AddJsonHubProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="3f4ee-113">Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="3f4ee-114">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="3f4ee-115">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="3f4ee-115">MessagePack serialization options</span></span>

<span data-ttu-id="3f4ee-116">MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="3f4ee-117">Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="3f4ee-118">Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="3f4ee-119">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="3f4ee-119">Configure server options</span></span>

<span data-ttu-id="3f4ee-120">Le tableau suivant décrit les options de configuration de concentrateurs SignalR :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="3f4ee-121">Option</span><span class="sxs-lookup"><span data-stu-id="3f4ee-121">Option</span></span> | <span data-ttu-id="3f4ee-122">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3f4ee-122">Default Value</span></span> | <span data-ttu-id="3f4ee-123">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="3f4ee-124">15 secondes</span><span class="sxs-lookup"><span data-stu-id="3f4ee-124">15 seconds</span></span> | <span data-ttu-id="3f4ee-125">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="3f4ee-126">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3f4ee-127">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3f4ee-128">15 secondes</span><span class="sxs-lookup"><span data-stu-id="3f4ee-128">15 seconds</span></span> | <span data-ttu-id="3f4ee-129">Si le serveur n’a pas envoyé un message au sein de cet intervalle, un message ping est envoyé automatiquement pour maintenir ouverte la connexion.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="3f4ee-130">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="3f4ee-130">All installed protocols</span></span> | <span data-ttu-id="3f4ee-131">Protocoles pris en charge par ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-131">Protocols supported by this hub.</span></span> <span data-ttu-id="3f4ee-132">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais protocoles peuvent être supprimés de cette liste pour désactiver les protocoles spécifiques pour les hubs individuels.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-132">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3f4ee-133">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-133">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="3f4ee-134">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-134">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="3f4ee-135">Options peuvent être configurées pour tous les hubs en fournissant un délégué d’options pour le `AddSignalR` appeler dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-135">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="3f4ee-136">Options pour un seul hub remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="3f4ee-136">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="3f4ee-137">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liées aux transports et gestion de la mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-137">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="3f4ee-138">Ces options sont configurées en passant un délégué à [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-138">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="3f4ee-139">Option</span><span class="sxs-lookup"><span data-stu-id="3f4ee-139">Option</span></span> | <span data-ttu-id="3f4ee-140">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3f4ee-140">Default Value</span></span> | <span data-ttu-id="3f4ee-141">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-141">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="3f4ee-142">32 KO</span><span class="sxs-lookup"><span data-stu-id="3f4ee-142">32 KB</span></span> | <span data-ttu-id="3f4ee-143">Le nombre maximal d’octets reçus à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-143">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="3f4ee-144">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-144">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="3f4ee-145">Données collectées à partir de la `Authorize` attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-145">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="3f4ee-146">Une liste de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-146">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="3f4ee-147">32 KO</span><span class="sxs-lookup"><span data-stu-id="3f4ee-147">32 KB</span></span> | <span data-ttu-id="3f4ee-148">Le nombre maximal d’octets envoyés par l’application qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-148">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="3f4ee-149">Augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-149">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="3f4ee-150">Tous les Transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-150">All Transports are enabled.</span></span> | <span data-ttu-id="3f4ee-151">Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-151">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="3f4ee-152">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-152">See below.</span></span> | <span data-ttu-id="3f4ee-153">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-153">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="3f4ee-154">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-154">See below.</span></span> | <span data-ttu-id="3f4ee-155">Options supplémentaires spécifiques au transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-155">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="3f4ee-156">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la `LongPolling` propriété :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-156">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="3f4ee-157">Option</span><span class="sxs-lookup"><span data-stu-id="3f4ee-157">Option</span></span> | <span data-ttu-id="3f4ee-158">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3f4ee-158">Default Value</span></span> | <span data-ttu-id="3f4ee-159">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-159">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="3f4ee-160">90 secondes</span><span class="sxs-lookup"><span data-stu-id="3f4ee-160">90 seconds</span></span> | <span data-ttu-id="3f4ee-161">La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une demande d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-161">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="3f4ee-162">Diminution de cette valeur entraîne le client d’émettre de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-162">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="3f4ee-163">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la `WebSockets` propriété :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-163">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="3f4ee-164">Option</span><span class="sxs-lookup"><span data-stu-id="3f4ee-164">Option</span></span> | <span data-ttu-id="3f4ee-165">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3f4ee-165">Default Value</span></span> | <span data-ttu-id="3f4ee-166">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-166">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="3f4ee-167">5 secondes</span><span class="sxs-lookup"><span data-stu-id="3f4ee-167">5 seconds</span></span> | <span data-ttu-id="3f4ee-168">Une fois que le serveur ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-168">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="3f4ee-169">Un délégué qui peut être utilisé pour définir le `Sec-WebSocket-Protocol` en-tête sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-169">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="3f4ee-170">Le délégué reçoit les valeurs demandées par le client en tant qu’entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-170">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="3f4ee-171">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="3f4ee-171">Configure client options</span></span>

<span data-ttu-id="3f4ee-172">Options du client peuvent être configurées sur le `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript), de type, ainsi que dans le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-172">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="3f4ee-173">Configurer la journalisation</span><span class="sxs-lookup"><span data-stu-id="3f4ee-173">Configure logging</span></span>

<span data-ttu-id="3f4ee-174">La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-174">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="3f4ee-175">Enregistrement des fournisseurs et les filtres peut être inscrits dans la même façon qu’ils se trouvent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-175">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="3f4ee-176">Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-176">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="3f4ee-177">Pour inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-177">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="3f4ee-178">Consultez le [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) section de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-178">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="3f4ee-179">Par exemple, pour activer la journalisation de la Console, installez le `Microsoft.Extensions.Logging.Console` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-179">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="3f4ee-180">Appelez le `AddConsole` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-180">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="3f4ee-181">Dans le client JavaScript, un texte similaire `configureLogging` méthode existe.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-181">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="3f4ee-182">Fournir un `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-182">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="3f4ee-183">Journaux sont écrits dans la fenêtre de console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-183">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="3f4ee-184">Pour désactiver la journalisation entièrement, spécifiez `signalR.LogLevel.None` dans le `configureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-184">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="3f4ee-185">Niveaux de journalisation disponibles pour le client JavaScript sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-185">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="3f4ee-186">Une des valeurs suivantes affectant le niveau de journal Active la journalisation des messages à **ou version ultérieure** ce niveau.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-186">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="3f4ee-187">Niveau</span><span class="sxs-lookup"><span data-stu-id="3f4ee-187">Level</span></span> | <span data-ttu-id="3f4ee-188">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-188">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="3f4ee-189">Aucun message n’est enregistré.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-189">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="3f4ee-190">Messages qui indiquent un échec dans l’application entière.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-190">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="3f4ee-191">Messages qui indiquent un échec dans l’opération en cours.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-191">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="3f4ee-192">Messages qui indiquent un problème non irrécupérable.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-192">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="3f4ee-193">Messages d’information.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-193">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="3f4ee-194">Messages de diagnostic utiles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-194">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="3f4ee-195">Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-195">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="3f4ee-196">Configurer les transports autorisées</span><span class="sxs-lookup"><span data-stu-id="3f4ee-196">Configure allowed transports</span></span>

<span data-ttu-id="3f4ee-197">Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-197">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="3f4ee-198">Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client pour utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-198">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="3f4ee-199">Tous les transports sont activées par défaut.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-199">All transports are enabled by default.</span></span>

<span data-ttu-id="3f4ee-200">Par exemple, pour désactiver le transport les événements, mais autoriser les connexions WebSocket et l’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-200">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="3f4ee-201">Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni pour `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3f4ee-201">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="3f4ee-202">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="3f4ee-202">Configure bearer authentication</span></span>

<span data-ttu-id="3f4ee-203">Pour fournir des données d’authentification, ainsi que les demandes de SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-203">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="3f4ee-204">Dans le Client .NET, ce jeton d’accès est transmis en tant que « Authentification du porteur » HTTP jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-204">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="3f4ee-205">Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton du porteur, **sauf** dans certains cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements et des WebSockets demandes).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-205">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="3f4ee-206">Dans ce cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-206">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="3f4ee-207">Dans le client .NET, le `AccessTokenProvider` option peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3f4ee-207">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="3f4ee-208">Dans le client JavaScript, le jeton d’accès est configuré en définissant le `accessTokenFactory` champ sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3f4ee-208">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="3f4ee-209">Configurer le délai d’expiration et les options keep-alive</span><span class="sxs-lookup"><span data-stu-id="3f4ee-209">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="3f4ee-210">Options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur le `HubConnection` objet proprement dit :</span><span class="sxs-lookup"><span data-stu-id="3f4ee-210">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="3f4ee-211">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="3f4ee-211">.NET Option</span></span> | <span data-ttu-id="3f4ee-212">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3f4ee-212">JavaScript Option</span></span> | <span data-ttu-id="3f4ee-213">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3f4ee-213">Default Value</span></span> | <span data-ttu-id="3f4ee-214">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-214">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="3f4ee-215">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="3f4ee-215">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3f4ee-216">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-216">Timeout for server activity.</span></span> <span data-ttu-id="3f4ee-217">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-217">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="3f4ee-218">Non configurable</span><span class="sxs-lookup"><span data-stu-id="3f4ee-218">Not configurable</span></span> | <span data-ttu-id="3f4ee-219">15 secondes</span><span class="sxs-lookup"><span data-stu-id="3f4ee-219">15 seconds</span></span> | <span data-ttu-id="3f4ee-220">Délai d’attente pour la négociation initiale du serveur.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-220">Timeout for initial server handshake.</span></span> <span data-ttu-id="3f4ee-221">Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le protocole de négociation et déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-221">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3f4ee-222">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-222">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3f4ee-223">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3f4ee-223">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="3f4ee-224">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que `TimeSpan` valeurs.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-224">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="3f4ee-225">Dans le client JavaScript, les valeurs de délai d’attente sont spécifiées en tant que nombre indiquant la durée en millisecondes.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-225">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="3f4ee-226">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3f4ee-226">Configure additional options</span></span>

<span data-ttu-id="3f4ee-227">Options supplémentaires peuvent être configurées dans le `WithUrl` (`withUrl` dans JavaScript) méthode sur `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3f4ee-227">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="3f4ee-228">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="3f4ee-228">.NET Option</span></span> | <span data-ttu-id="3f4ee-229">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3f4ee-229">JavaScript Option</span></span> | <span data-ttu-id="3f4ee-230">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="3f4ee-230">Default Value</span></span> | <span data-ttu-id="3f4ee-231">Description</span><span class="sxs-lookup"><span data-stu-id="3f4ee-231">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="3f4ee-232">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-232">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="3f4ee-233">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-233">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3f4ee-234">**Pris en charge uniquement lorsque le transport WebSocket est le transport est activé uniquement**.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-234">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3f4ee-235">Ce paramètre ne peut pas être activé lors de l’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-235">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="3f4ee-236">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-236">Not configurable \*</span></span> | <span data-ttu-id="3f4ee-237">Empty</span><span class="sxs-lookup"><span data-stu-id="3f4ee-237">Empty</span></span> | <span data-ttu-id="3f4ee-238">Collection de certificats TLS à envoyer authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-238">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="3f4ee-239">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-239">Not configurable \*</span></span> | <span data-ttu-id="3f4ee-240">Empty</span><span class="sxs-lookup"><span data-stu-id="3f4ee-240">Empty</span></span> | <span data-ttu-id="3f4ee-241">Une collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-241">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="3f4ee-242">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-242">Not configurable \*</span></span> | <span data-ttu-id="3f4ee-243">Empty</span><span class="sxs-lookup"><span data-stu-id="3f4ee-243">Empty</span></span> | <span data-ttu-id="3f4ee-244">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-244">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="3f4ee-245">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-245">Not configurable \*</span></span> | <span data-ttu-id="3f4ee-246">5 secondes</span><span class="sxs-lookup"><span data-stu-id="3f4ee-246">5 seconds</span></span> | <span data-ttu-id="3f4ee-247">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-247">WebSockets only.</span></span> <span data-ttu-id="3f4ee-248">La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-248">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="3f4ee-249">Si le serveur n’accuse pas la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-249">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="3f4ee-250">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-250">Not configurable \*</span></span> | <span data-ttu-id="3f4ee-251">Empty</span><span class="sxs-lookup"><span data-stu-id="3f4ee-251">Empty</span></span> | <span data-ttu-id="3f4ee-252">Un dictionnaire d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-252">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="3f4ee-253">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-253">Not configurable \*</span></span> | `null` | <span data-ttu-id="3f4ee-254">Un délégué qui peut être utilisé pour configurer ou de remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-254">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="3f4ee-255">Pas utilisé pour les connexions de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-255">Not used for WebSocket connections.</span></span> <span data-ttu-id="3f4ee-256">Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-256">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="3f4ee-257">Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un nouveau `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-257">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="3f4ee-258">**Lorsque vous en remplaçant le gestionnaire veillez à copier les paramètres que vous souhaitez empêcher le gestionnaire fourni, sinon, les options configurées (par exemple, les Cookies et les en-têtes) ne s’appliquent pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="3f4ee-258">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="3f4ee-259">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-259">Not configurable \*</span></span> | `null` | <span data-ttu-id="3f4ee-260">Un proxy HTTP à utiliser lors de l’envoi des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-260">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="3f4ee-261">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-261">Not configurable \*</span></span> | `false` | <span data-ttu-id="3f4ee-262">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-262">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="3f4ee-263">Cela permet d’utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-263">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="3f4ee-264">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="3f4ee-264">Not configurable \*</span></span> | `null` | <span data-ttu-id="3f4ee-265">Délégué qui peut être utilisé pour configurer des options supplémentaires de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-265">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="3f4ee-266">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisé pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-266">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="3f4ee-267">Options marquées d’un astérisque (\*) ne sont pas configurables dans le client JavaScript, en raison des limitations dans le navigateur d’API.</span><span class="sxs-lookup"><span data-stu-id="3f4ee-267">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="3f4ee-268">Dans le Client .NET, ces options peuvent être modifiées par l’options du délégué fourni `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3f4ee-268">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="3f4ee-269">Dans le JavaScript Client, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3f4ee-269">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="3f4ee-270">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3f4ee-270">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
