---
title: Configuration d’ASP.NET Core SignalR
author: tdykstra
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: fac0226c939f4cf446c876b1c0b359d6c5b9dfd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095400"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="d6a16-103">Configuration d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d6a16-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="d6a16-104">Options de sérialisation de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="d6a16-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="d6a16-105">ASP.NET Core SignalR prend en charge deux protocoles d’encodage de messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="d6a16-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="d6a16-106">Chaque protocole a des options de configuration de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="d6a16-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="d6a16-107">Sérialisation JSON peut être configurée sur le serveur à l’aide de la [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d6a16-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d6a16-108">Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet.</span><span class="sxs-lookup"><span data-stu-id="d6a16-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="d6a16-109">Le [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="d6a16-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="d6a16-110">Consultez le [Documentation JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="d6a16-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="d6a16-111">Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « La casse Pascal », au lieu des noms de « la casse mixte » par défaut, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d6a16-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="d6a16-112">Dans le client .NET, les mêmes `AddJsonHubProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="d6a16-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="d6a16-113">Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="d6a16-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="d6a16-114">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="d6a16-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="d6a16-115">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="d6a16-115">MessagePack serialization options</span></span>

<span data-ttu-id="d6a16-116">MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler.</span><span class="sxs-lookup"><span data-stu-id="d6a16-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="d6a16-117">Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="d6a16-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a16-118">Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="d6a16-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="d6a16-119">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="d6a16-119">Configure server options</span></span>

<span data-ttu-id="d6a16-120">Le tableau suivant décrit les options de configuration de concentrateurs SignalR :</span><span class="sxs-lookup"><span data-stu-id="d6a16-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="d6a16-121">Option</span><span class="sxs-lookup"><span data-stu-id="d6a16-121">Option</span></span> | <span data-ttu-id="d6a16-122">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="d6a16-123">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="d6a16-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d6a16-124">Si le serveur n’a pas envoyé un message au sein de cet intervalle, un message ping est envoyé automatiquement pour maintenir ouverte la connexion.</span><span class="sxs-lookup"><span data-stu-id="d6a16-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="d6a16-125">Protocoles pris en charge par ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-125">Protocols supported by this hub.</span></span> <span data-ttu-id="d6a16-126">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais protocoles peuvent être supprimés de cette liste pour désactiver les protocoles spécifiques pour les hubs individuels.</span><span class="sxs-lookup"><span data-stu-id="d6a16-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="d6a16-127">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d6a16-128">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="d6a16-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="d6a16-129">Options peuvent être configurées pour tous les hubs en fournissant un délégué d’options pour le `AddSignalR` appeler dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d6a16-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="d6a16-130">Options pour un seul hub remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="d6a16-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="d6a16-131">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liées aux transports et gestion de la mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="d6a16-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="d6a16-132">Ces options sont configurées en passant un délégué à [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="d6a16-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="d6a16-133">Option</span><span class="sxs-lookup"><span data-stu-id="d6a16-133">Option</span></span> | <span data-ttu-id="d6a16-134">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="d6a16-135">Le nombre maximal d’octets reçus à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="d6a16-136">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="d6a16-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="d6a16-137">La valeur par défaut est 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="d6a16-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="d6a16-138">Une liste de [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="d6a16-139">Par défaut, il est rempli avec les valeurs de la `Authorize` attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="d6a16-140">Le nombre maximal d’octets envoyés par l’application qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="d6a16-141">Augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="d6a16-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="d6a16-142">La valeur par défaut est 32 Ko.</span><span class="sxs-lookup"><span data-stu-id="d6a16-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="d6a16-143">Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="d6a16-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="d6a16-144">Tous les transports sont activées par défaut.</span><span class="sxs-lookup"><span data-stu-id="d6a16-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="d6a16-145">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="d6a16-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="d6a16-146">Options supplémentaires spécifiques au transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d6a16-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="d6a16-147">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la `LongPolling` propriété :</span><span class="sxs-lookup"><span data-stu-id="d6a16-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="d6a16-148">Option</span><span class="sxs-lookup"><span data-stu-id="d6a16-148">Option</span></span> | <span data-ttu-id="d6a16-149">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="d6a16-150">La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une demande d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="d6a16-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="d6a16-151">Diminution de cette valeur entraîne le client d’émettre de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="d6a16-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="d6a16-152">La valeur par défaut est 90 secondes.</span><span class="sxs-lookup"><span data-stu-id="d6a16-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="d6a16-153">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la `WebSockets` propriété :</span><span class="sxs-lookup"><span data-stu-id="d6a16-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="d6a16-154">Option</span><span class="sxs-lookup"><span data-stu-id="d6a16-154">Option</span></span> | <span data-ttu-id="d6a16-155">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="d6a16-156">Une fois que le serveur ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="d6a16-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="d6a16-157">Un délégué qui peut être utilisé pour définir le `Sec-WebSocket-Protocol` en-tête sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="d6a16-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="d6a16-158">Le délégué reçoit les valeurs demandées par le client en tant qu’entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="d6a16-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="d6a16-159">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="d6a16-159">Configure client options</span></span>

<span data-ttu-id="d6a16-160">Options du client peuvent être configurées sur le `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript), de type, ainsi que dans le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="d6a16-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="d6a16-161">Configurer la journalisation</span><span class="sxs-lookup"><span data-stu-id="d6a16-161">Configure logging</span></span>

<span data-ttu-id="d6a16-162">La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d6a16-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="d6a16-163">Enregistrement des fournisseurs et les filtres peut être inscrits dans la même façon qu’ils se trouvent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="d6a16-164">Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="d6a16-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a16-165">Pour inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="d6a16-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="d6a16-166">Consultez le [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) section de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="d6a16-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="d6a16-167">Par exemple, pour activer la journalisation de la Console, installez le `Microsoft.Extensions.Logging.Console` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="d6a16-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="d6a16-168">Appelez le `AddConsole` méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="d6a16-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="d6a16-169">Dans le client JavaScript, un texte similaire `configureLogging` méthode existe.</span><span class="sxs-lookup"><span data-stu-id="d6a16-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="d6a16-170">Fournir un `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="d6a16-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="d6a16-171">Journaux sont écrits dans la fenêtre de console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="d6a16-172">Pour désactiver la journalisation entièrement, spécifiez `signalR.LogLevel.None` dans le `configureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d6a16-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="d6a16-173">Niveaux de journalisation disponibles pour le client JavaScript sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="d6a16-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="d6a16-174">Une des valeurs suivantes affectant le niveau de journal Active la journalisation des messages à **ou version ultérieure** ce niveau.</span><span class="sxs-lookup"><span data-stu-id="d6a16-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="d6a16-175">Niveau</span><span class="sxs-lookup"><span data-stu-id="d6a16-175">Level</span></span> | <span data-ttu-id="d6a16-176">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="d6a16-177">Aucun message n’est enregistré.</span><span class="sxs-lookup"><span data-stu-id="d6a16-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="d6a16-178">Messages qui indiquent un échec dans l’application entière.</span><span class="sxs-lookup"><span data-stu-id="d6a16-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="d6a16-179">Messages qui indiquent un échec dans l’opération en cours.</span><span class="sxs-lookup"><span data-stu-id="d6a16-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="d6a16-180">Messages qui indiquent un problème non irrécupérable.</span><span class="sxs-lookup"><span data-stu-id="d6a16-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="d6a16-181">Messages d’information.</span><span class="sxs-lookup"><span data-stu-id="d6a16-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="d6a16-182">Messages de diagnostic utiles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="d6a16-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="d6a16-183">Messages de diagnostic très détaillés conçus pour diagnostiquer les problèmes spécifiques.</span><span class="sxs-lookup"><span data-stu-id="d6a16-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="d6a16-184">Configurer les transports autorisées</span><span class="sxs-lookup"><span data-stu-id="d6a16-184">Configure allowed transports</span></span>

<span data-ttu-id="d6a16-185">Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d6a16-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="d6a16-186">Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client pour utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="d6a16-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="d6a16-187">Tous les transports sont activées par défaut.</span><span class="sxs-lookup"><span data-stu-id="d6a16-187">All transports are enabled by default.</span></span>

<span data-ttu-id="d6a16-188">Par exemple, pour désactiver le transport les événements, mais autoriser les connexions WebSocket et l’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="d6a16-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="d6a16-189">Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni pour `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d6a16-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="d6a16-190">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="d6a16-190">Configure bearer authentication</span></span>

<span data-ttu-id="d6a16-191">Pour fournir des données d’authentification, ainsi que les demandes de SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="d6a16-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="d6a16-192">Dans le Client .NET, ce jeton d’accès est transmis en tant que « Authentification du porteur » HTTP jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="d6a16-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="d6a16-193">Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton du porteur, **sauf** dans certains cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements et des WebSockets demandes).</span><span class="sxs-lookup"><span data-stu-id="d6a16-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="d6a16-194">Dans ce cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="d6a16-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="d6a16-195">Dans le client .NET, le `AccessTokenProvider` option peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="d6a16-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="d6a16-196">Dans le client JavaScript, le jeton d’accès est configuré en définissant le `accessTokenFactory` champ sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d6a16-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="d6a16-197">Configurer le délai d’expiration et les options keep-alive</span><span class="sxs-lookup"><span data-stu-id="d6a16-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="d6a16-198">Options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur le `HubConnection` objet proprement dit :</span><span class="sxs-lookup"><span data-stu-id="d6a16-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="d6a16-199">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="d6a16-199">.NET Option</span></span> | <span data-ttu-id="d6a16-200">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d6a16-200">JavaScript Option</span></span> | <span data-ttu-id="d6a16-201">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="d6a16-202">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-202">Timeout for server activity.</span></span> <span data-ttu-id="d6a16-203">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et le déclencheur le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d6a16-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="d6a16-204">Non configurable</span><span class="sxs-lookup"><span data-stu-id="d6a16-204">Not configurable</span></span> | <span data-ttu-id="d6a16-205">Délai d’attente pour la négociation initiale du serveur.</span><span class="sxs-lookup"><span data-stu-id="d6a16-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="d6a16-206">Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le négociation et déclencher le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d6a16-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="d6a16-207">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que `TimeSpan` valeurs.</span><span class="sxs-lookup"><span data-stu-id="d6a16-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="d6a16-208">Dans le client JavaScript, les valeurs de délai d’attente sont spécifiées sous forme de nombres.</span><span class="sxs-lookup"><span data-stu-id="d6a16-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="d6a16-209">Les nombres représentent les valeurs de temps en millisecondes.</span><span class="sxs-lookup"><span data-stu-id="d6a16-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="d6a16-210">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d6a16-210">Configure additional options</span></span>

<span data-ttu-id="d6a16-211">Options supplémentaires peuvent être configurées dans le `WithUrl` (`withUrl` dans JavaScript) méthode sur `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d6a16-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="d6a16-212">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="d6a16-212">.NET Option</span></span> | <span data-ttu-id="d6a16-213">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d6a16-213">JavaScript Option</span></span> | <span data-ttu-id="d6a16-214">Description</span><span class="sxs-lookup"><span data-stu-id="d6a16-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="d6a16-215">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6a16-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="d6a16-216">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="d6a16-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d6a16-217">**Pris en charge uniquement lorsque le transport WebSocket est le transport est activé uniquement**.</span><span class="sxs-lookup"><span data-stu-id="d6a16-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d6a16-218">Ce paramètre ne peut pas être activé lors de l’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d6a16-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="d6a16-219">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-219">Not configurable \*</span></span> | <span data-ttu-id="d6a16-220">Collection de certificats TLS à envoyer authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="d6a16-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="d6a16-221">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-221">Not configurable \*</span></span> | <span data-ttu-id="d6a16-222">Une collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6a16-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="d6a16-223">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-223">Not configurable \*</span></span> | <span data-ttu-id="d6a16-224">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6a16-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="d6a16-225">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-225">Not configurable \*</span></span> | <span data-ttu-id="d6a16-226">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d6a16-226">WebSockets only.</span></span> <span data-ttu-id="d6a16-227">La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="d6a16-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="d6a16-228">Si le serveur n’accuse pas la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="d6a16-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="d6a16-229">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-229">Not configurable \*</span></span> | <span data-ttu-id="d6a16-230">Un dictionnaire d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6a16-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="d6a16-231">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-231">Not configurable \*</span></span> | <span data-ttu-id="d6a16-232">Un délégué qui peut être utilisé pour configurer ou de remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6a16-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="d6a16-233">Pas utilisé pour les connexions de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d6a16-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="d6a16-234">Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="d6a16-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="d6a16-235">Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un complètement nouveau `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="d6a16-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="d6a16-236">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-236">Not configurable \*</span></span> | <span data-ttu-id="d6a16-237">Un proxy HTTP à utiliser lors de l’envoi des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d6a16-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="d6a16-238">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-238">Not configurable \*</span></span> | <span data-ttu-id="d6a16-239">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d6a16-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="d6a16-240">Cela permet d’utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="d6a16-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="d6a16-241">Non configurable \*</span><span class="sxs-lookup"><span data-stu-id="d6a16-241">Not configurable \*</span></span> | <span data-ttu-id="d6a16-242">Délégué qui peut être utilisé pour configurer des options supplémentaires de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d6a16-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="d6a16-243">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisé pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="d6a16-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="d6a16-244">Options marquées d’un astérisque (\*) ne sont pas configurables dans le client JavaScript, en raison des limitations dans le navigateur d’API.</span><span class="sxs-lookup"><span data-stu-id="d6a16-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="d6a16-245">Dans le Client .NET, ces options peuvent être modifiées par l’options du délégué fourni `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="d6a16-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="d6a16-246">Dans le JavaScript Client, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d6a16-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="d6a16-247">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d6a16-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
