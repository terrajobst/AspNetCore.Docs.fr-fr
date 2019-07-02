---
title: Configuration d’ASP.NET Core SignalR
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 662565e537fa0eb13ed80e558949740739a63558
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500383"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="436d6-103">Configuration d’ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="436d6-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="436d6-104">Options de sérialisation de JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="436d6-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="436d6-105">ASP.NET Core SignalR prend en charge deux protocoles d’encodage de messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="436d6-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="436d6-106">Chaque protocole a des options de configuration de sérialisation.</span><span class="sxs-lookup"><span data-stu-id="436d6-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="436d6-107">Sérialisation JSON peut être configurée sur le serveur à l’aide de la [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) méthode d’extension, qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="436d6-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="436d6-108">Le `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet.</span><span class="sxs-lookup"><span data-stu-id="436d6-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="436d6-109">Le [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriété sur cet objet est un JSON.NET `JsonSerializerSettings` objet qui peut être utilisé pour configurer la sérialisation d’arguments et valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="436d6-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="436d6-110">Consultez le [Documentation JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="436d6-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="436d6-111">Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « en casse Pascal », au lieu des noms « en casse mixte » par défaut, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="436d6-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="436d6-112">Dans le client .NET, les mêmes `AddJsonProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="436d6-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="436d6-113">Le `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="436d6-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="436d6-114">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="436d6-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="436d6-115">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="436d6-115">MessagePack serialization options</span></span>

<span data-ttu-id="436d6-116">MessagePack sérialisation peut être configurée en fournissant un délégué à la [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) appeler.</span><span class="sxs-lookup"><span data-stu-id="436d6-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="436d6-117">Consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="436d6-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="436d6-118">Il n’est pas possible de configurer la sérialisation de MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="436d6-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="436d6-119">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="436d6-119">Configure server options</span></span>

<span data-ttu-id="436d6-120">Le tableau suivant décrit les options de configuration de concentrateurs SignalR :</span><span class="sxs-lookup"><span data-stu-id="436d6-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="436d6-121">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-121">Option</span></span> | <span data-ttu-id="436d6-122">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-122">Default Value</span></span> | <span data-ttu-id="436d6-123">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="436d6-124">30 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-124">30 seconds</span></span> | <span data-ttu-id="436d6-125">Le serveur considère le client déconnecté si elle n’a pas reçu un message (y compris keep-alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="436d6-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="436d6-126">Il peut prendre plu de cet intervalle de délai d’attente à réellement être identifiée comme étant déconnectée, en raison de la façon dont ceci est implémenté par le client.</span><span class="sxs-lookup"><span data-stu-id="436d6-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="436d6-127">La valeur recommandée est double la `KeepAliveInterval` valeur.</span><span class="sxs-lookup"><span data-stu-id="436d6-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="436d6-128">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-128">15 seconds</span></span> | <span data-ttu-id="436d6-129">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="436d6-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="436d6-130">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="436d6-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="436d6-131">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="436d6-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="436d6-132">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-132">15 seconds</span></span> | <span data-ttu-id="436d6-133">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="436d6-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="436d6-134">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="436d6-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="436d6-135">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="436d6-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="436d6-136">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="436d6-136">All installed protocols</span></span> | <span data-ttu-id="436d6-137">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="436d6-137">Protocols supported by this hub.</span></span> <span data-ttu-id="436d6-138">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="436d6-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="436d6-139">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="436d6-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="436d6-140">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="436d6-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="436d6-141">Le nombre maximal d’éléments qui peuvent être mis en mémoire tampon pour le client les flux de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="436d6-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="436d6-142">Si cette limite est atteinte, le traitement des appels est bloqué jusqu'à ce que le serveur traite les éléments de flux de données.</span><span class="sxs-lookup"><span data-stu-id="436d6-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="436d6-143">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-143">Option</span></span> | <span data-ttu-id="436d6-144">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-144">Default Value</span></span> | <span data-ttu-id="436d6-145">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="436d6-146">30 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-146">30 seconds</span></span> | <span data-ttu-id="436d6-147">Le serveur considère le client déconnecté si elle n’a pas reçu un message (y compris keep-alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="436d6-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="436d6-148">Il peut prendre plu de cet intervalle de délai d’attente à réellement être identifiée comme étant déconnectée, en raison de la façon dont ceci est implémenté par le client.</span><span class="sxs-lookup"><span data-stu-id="436d6-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="436d6-149">La valeur recommandée est double la `KeepAliveInterval` valeur.</span><span class="sxs-lookup"><span data-stu-id="436d6-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="436d6-150">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-150">15 seconds</span></span> | <span data-ttu-id="436d6-151">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="436d6-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="436d6-152">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="436d6-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="436d6-153">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="436d6-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="436d6-154">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-154">15 seconds</span></span> | <span data-ttu-id="436d6-155">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="436d6-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="436d6-156">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="436d6-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="436d6-157">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="436d6-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="436d6-158">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="436d6-158">All installed protocols</span></span> | <span data-ttu-id="436d6-159">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="436d6-159">Protocols supported by this hub.</span></span> <span data-ttu-id="436d6-160">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="436d6-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="436d6-161">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="436d6-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="436d6-162">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="436d6-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="436d6-163">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-163">Option</span></span> | <span data-ttu-id="436d6-164">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-164">Default Value</span></span> | <span data-ttu-id="436d6-165">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="436d6-166">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-166">15 seconds</span></span> | <span data-ttu-id="436d6-167">Si le client n’envoie un message de négociation initiale au sein de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="436d6-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="436d6-168">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="436d6-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="436d6-169">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="436d6-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="436d6-170">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-170">15 seconds</span></span> | <span data-ttu-id="436d6-171">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="436d6-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="436d6-172">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="436d6-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="436d6-173">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="436d6-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="436d6-174">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="436d6-174">All installed protocols</span></span> | <span data-ttu-id="436d6-175">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="436d6-175">Protocols supported by this hub.</span></span> <span data-ttu-id="436d6-176">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="436d6-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="436d6-177">Si `true`et détaillée des messages d’exception sont retournées aux clients quand une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="436d6-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="436d6-178">La valeur par défaut est `false`, que ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="436d6-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="436d6-179">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="436d6-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="436d6-180">Options pour un seul hub remplacent les options globales fournies dans `AddSignalR` et peuvent être configurés à l’aide de <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="436d6-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="436d6-181">Options de configuration avancées HTTP</span><span class="sxs-lookup"><span data-stu-id="436d6-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="436d6-182">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés liées aux transports et gestion de la mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="436d6-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="436d6-183">Ces options sont configurées en passant un délégué à [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="436d6-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="436d6-184">Le tableau suivant décrit les options de configuration des options de HTTP d’ASP.NET Core SignalR avancées :</span><span class="sxs-lookup"><span data-stu-id="436d6-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="436d6-185">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-185">Option</span></span> | <span data-ttu-id="436d6-186">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-186">Default Value</span></span> | <span data-ttu-id="436d6-187">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="436d6-188">32 KO</span><span class="sxs-lookup"><span data-stu-id="436d6-188">32 KB</span></span> | <span data-ttu-id="436d6-189">Le nombre maximal d’octets reçus à partir du client qui les mémoires tampons de serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="436d6-190">Augmentation de cette valeur permet au serveur recevoir des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="436d6-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="436d6-191">Données collectées à partir de la `Authorize` attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="436d6-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="436d6-192">Une liste de [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objets utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="436d6-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="436d6-193">32 KO</span><span class="sxs-lookup"><span data-stu-id="436d6-193">32 KB</span></span> | <span data-ttu-id="436d6-194">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="436d6-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="436d6-195">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="436d6-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="436d6-196">Tous les Transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="436d6-196">All Transports are enabled.</span></span> | <span data-ttu-id="436d6-197">Masque de bits de `HttpTransportType` valeurs qui restreignent les transports, un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="436d6-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="436d6-198">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="436d6-198">See below.</span></span> | <span data-ttu-id="436d6-199">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="436d6-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="436d6-200">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="436d6-200">See below.</span></span> | <span data-ttu-id="436d6-201">Options supplémentaires spécifiques au transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="436d6-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="436d6-202">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="436d6-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="436d6-203">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-203">Option</span></span> | <span data-ttu-id="436d6-204">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-204">Default Value</span></span> | <span data-ttu-id="436d6-205">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="436d6-206">90 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-206">90 seconds</span></span> | <span data-ttu-id="436d6-207">La quantité maximale de temps le serveur attend un message à envoyer au client avant de mettre fin à une demande d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="436d6-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="436d6-208">Diminution de cette valeur entraîne le client d’émettre de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="436d6-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="436d6-209">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="436d6-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="436d6-210">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-210">Option</span></span> | <span data-ttu-id="436d6-211">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-211">Default Value</span></span> | <span data-ttu-id="436d6-212">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="436d6-213">5 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-213">5 seconds</span></span> | <span data-ttu-id="436d6-214">Une fois que le serveur ferme, si le client ne parvient pas à fermer au sein de cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="436d6-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="436d6-215">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="436d6-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="436d6-216">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="436d6-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="436d6-217">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="436d6-217">Configure client options</span></span>

<span data-ttu-id="436d6-218">Options du client peuvent être configurées sur le `HubConnectionBuilder` type (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="436d6-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="436d6-219">Il est également disponible dans le client Java, mais le `HttpHubConnectionBuilder` sous-classe est ce que contient les options de configuration Générateur de rapports, ainsi que dans le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="436d6-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="436d6-220">Configurer la journalisation</span><span class="sxs-lookup"><span data-stu-id="436d6-220">Configure logging</span></span>

<span data-ttu-id="436d6-221">La journalisation est configurée dans le Client .NET à l’aide de la `ConfigureLogging` (méthode).</span><span class="sxs-lookup"><span data-stu-id="436d6-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="436d6-222">Enregistrement des fournisseurs et les filtres peut être inscrits dans la même façon qu’ils se trouvent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="436d6-223">Consultez le [journalisation dans ASP.NET Core](xref:fundamentals/logging/index) documentation pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="436d6-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="436d6-224">Pour inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="436d6-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="436d6-225">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="436d6-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="436d6-226">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="436d6-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="436d6-227">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="436d6-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="436d6-228">Dans le client JavaScript, un texte similaire `configureLogging` méthode existe.</span><span class="sxs-lookup"><span data-stu-id="436d6-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="436d6-229">Fournir un `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="436d6-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="436d6-230">Journaux sont écrits dans la fenêtre de console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="436d6-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="436d6-231">Au lieu d’un `LogLevel` valeur, vous pouvez également fournir un `string` valeur représentant un nom de niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="436d6-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="436d6-232">Cela est utile lors de la configuration SignalR journalisation dans les environnements où vous n’avez pas accès à la `LogLevel` constantes.</span><span class="sxs-lookup"><span data-stu-id="436d6-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="436d6-233">Le tableau suivant répertorie les niveaux de journal disponibles.</span><span class="sxs-lookup"><span data-stu-id="436d6-233">The following table lists the available log levels.</span></span> <span data-ttu-id="436d6-234">La valeur que vous fournissez à `configureLogging` définit le **minimale** niveau qui est enregistré de journal.</span><span class="sxs-lookup"><span data-stu-id="436d6-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="436d6-235">Les messages enregistrés à ce niveau, **ou les niveaux répertorié après lui dans la table**, seront enregistrés.</span><span class="sxs-lookup"><span data-stu-id="436d6-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="436d6-236">string</span><span class="sxs-lookup"><span data-stu-id="436d6-236">string</span></span> | <span data-ttu-id="436d6-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="436d6-237">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="436d6-238">`"info"` **Ou** `"information"`</span><span class="sxs-lookup"><span data-stu-id="436d6-238">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="436d6-239">`"warn"` **Ou** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="436d6-239">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="436d6-240">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="436d6-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="436d6-241">Pour plus d’informations sur la journalisation, consultez le [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="436d6-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="436d6-242">Le client SignalR Java utilise le [SLF4J](https://www.slf4j.org/) bibliothèque pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="436d6-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="436d6-243">C’est une API de haut niveau de journalisation qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifiques en apportant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="436d6-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="436d6-244">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="436d6-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="436d6-245">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un enregistreur d’événements non opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="436d6-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="436d6-246">Cela peut être ignoré sans risque.</span><span class="sxs-lookup"><span data-stu-id="436d6-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="436d6-247">Configurer les transports autorisées</span><span class="sxs-lookup"><span data-stu-id="436d6-247">Configure allowed transports</span></span>

<span data-ttu-id="436d6-248">Les transports utilisés par SignalR peuvent être configurés dans le `WithUrl` appeler (`withUrl` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="436d6-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="436d6-249">Une opération de bits OR des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client pour utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="436d6-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="436d6-250">Tous les transports sont activées par défaut.</span><span class="sxs-lookup"><span data-stu-id="436d6-250">All transports are enabled by default.</span></span>

<span data-ttu-id="436d6-251">Par exemple, pour désactiver le transport les événements, mais autoriser les connexions WebSocket et l’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="436d6-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="436d6-252">Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni pour `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="436d6-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="436d6-253">Dans cette version de Java client WebSocket est le transport uniquement disponible.</span><span class="sxs-lookup"><span data-stu-id="436d6-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="436d6-254">Dans le client Java, le transport est sélectionné avec la `withTransport` méthode sur le `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="436d6-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="436d6-255">Les valeurs par défaut du client Java pour l’utilisation du transport WebSocket.</span><span class="sxs-lookup"><span data-stu-id="436d6-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="436d6-256">Le client SignalR Java ne prend en charge encore transport de secours.</span><span class="sxs-lookup"><span data-stu-id="436d6-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="436d6-257">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="436d6-257">Configure bearer authentication</span></span>

<span data-ttu-id="436d6-258">Pour fournir des données d’authentification, ainsi que les demandes de SignalR, utilisez le `AccessTokenProvider` option (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="436d6-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="436d6-259">Dans le Client .NET, ce jeton d’accès est transmis en tant que « Authentification du porteur » HTTP jeton (à l’aide du `Authorization` en-tête avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="436d6-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="436d6-260">Dans le client JavaScript, le jeton d’accès est utilisé comme un jeton du porteur, **sauf** dans certains cas où le navigateur API restreindre la possibilité d’appliquer les en-têtes des (plus précisément, les événements et des WebSockets demandes).</span><span class="sxs-lookup"><span data-stu-id="436d6-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="436d6-261">Dans ce cas, le jeton d’accès est fourni en tant que valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="436d6-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="436d6-262">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="436d6-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="436d6-263">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="436d6-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="436d6-264">Dans le client SignalR Java, vous pouvez configurer un jeton du porteur à utiliser pour l’authentification en fournissant une fabrique de jeton d’accès à la [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="436d6-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="436d6-265">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir un [RxJava](https://github.com/ReactiveX/RxJava) [unique\<chaîne >](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="436d6-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="436d6-266">Avec un appel à [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire la logique pour générer des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="436d6-266">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="436d6-267">Configurer le délai d’expiration et les options keep-alive</span><span class="sxs-lookup"><span data-stu-id="436d6-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="436d6-268">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="436d6-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="436d6-269">.NET</span><span class="sxs-lookup"><span data-stu-id="436d6-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="436d6-270">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-270">Option</span></span> | <span data-ttu-id="436d6-271">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-271">Default value</span></span> | <span data-ttu-id="436d6-272">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="436d6-273">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="436d6-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="436d6-274">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-274">Timeout for server activity.</span></span> <span data-ttu-id="436d6-275">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="436d6-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="436d6-276">Cette valeur doit être suffisamment grande pour un message ping à envoyer à partir du serveur **et** reçues par le client dans l’intervalle de délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="436d6-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="436d6-277">La valeur recommandée est un nombre au moins deux fois sur le serveur `KeepAliveInterval` valeur, de prévoir un délai pour les tests ping arrivée.</span><span class="sxs-lookup"><span data-stu-id="436d6-277">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="436d6-278">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-278">15 seconds</span></span> | <span data-ttu-id="436d6-279">Délai d’attente pour la négociation initiale du serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="436d6-280">Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le protocole de négociation et déclencheurs le `Closed` événement (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="436d6-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="436d6-281">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="436d6-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="436d6-282">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="436d6-282">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="436d6-283">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="436d6-283">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="436d6-284">JavaScript</span><span class="sxs-lookup"><span data-stu-id="436d6-284">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="436d6-285">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-285">Option</span></span> | <span data-ttu-id="436d6-286">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-286">Default value</span></span> | <span data-ttu-id="436d6-287">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-287">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="436d6-288">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="436d6-288">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="436d6-289">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-289">Timeout for server activity.</span></span> <span data-ttu-id="436d6-290">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `onclose` événement.</span><span class="sxs-lookup"><span data-stu-id="436d6-290">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="436d6-291">Cette valeur doit être suffisamment grande pour un message ping à envoyer à partir du serveur **et** reçues par le client dans l’intervalle de délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="436d6-291">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="436d6-292">La valeur recommandée est un nombre au moins deux fois sur le serveur `KeepAliveInterval` valeur, de prévoir un délai pour les tests ping arrivée.</span><span class="sxs-lookup"><span data-stu-id="436d6-292">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="436d6-293">Java</span><span class="sxs-lookup"><span data-stu-id="436d6-293">Java</span></span>](#tab/java)

| <span data-ttu-id="436d6-294">Option</span><span class="sxs-lookup"><span data-stu-id="436d6-294">Option</span></span> | <span data-ttu-id="436d6-295">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-295">Default value</span></span> | <span data-ttu-id="436d6-296">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-296">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="436d6-297">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="436d6-297">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="436d6-298">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="436d6-298">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="436d6-299">Délai d’expiration pour l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-299">Timeout for server activity.</span></span> <span data-ttu-id="436d6-300">Si le serveur n’a pas envoyé un message dans cet intervalle, le client considère que le serveur déconnecté et les déclencheurs le `onClose` événement.</span><span class="sxs-lookup"><span data-stu-id="436d6-300">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="436d6-301">Cette valeur doit être suffisamment grande pour un message ping à envoyer à partir du serveur **et** reçues par le client dans l’intervalle de délai d’attente.</span><span class="sxs-lookup"><span data-stu-id="436d6-301">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="436d6-302">La valeur recommandée est un nombre au moins deux fois sur le serveur `KeepAliveInterval` valeur, de prévoir un délai pour les tests ping arrivée.</span><span class="sxs-lookup"><span data-stu-id="436d6-302">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="436d6-303">15 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-303">15 seconds</span></span> | <span data-ttu-id="436d6-304">Délai d’attente pour la négociation initiale du serveur.</span><span class="sxs-lookup"><span data-stu-id="436d6-304">Timeout for initial server handshake.</span></span> <span data-ttu-id="436d6-305">Si le serveur n’envoie une réponse de négociation dans cet intervalle, le client annule le protocole de négociation et déclencheurs le `onClose` événement.</span><span class="sxs-lookup"><span data-stu-id="436d6-305">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="436d6-306">Il s’agit d’un paramètre avancé qui doit uniquement être modifié en cas d’erreurs de délai d’expiration de la négociation en raison de la latence du réseau graves.</span><span class="sxs-lookup"><span data-stu-id="436d6-306">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="436d6-307">Pour plus d’informations sur le processus de négociation, consultez le [spécification du protocole SignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="436d6-307">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="436d6-308">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="436d6-308">Configure additional options</span></span>

<span data-ttu-id="436d6-309">Options supplémentaires peuvent être configurées dans le `WithUrl` (`withUrl` dans JavaScript) méthode sur `HubConnectionBuilder` ou sur les API de configuration différents sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="436d6-309">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="436d6-310">.NET</span><span class="sxs-lookup"><span data-stu-id="436d6-310">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="436d6-311">Option de .NET</span><span class="sxs-lookup"><span data-stu-id="436d6-311">.NET Option</span></span> |  <span data-ttu-id="436d6-312">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-312">Default value</span></span> | <span data-ttu-id="436d6-313">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-313">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="436d6-314">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-314">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="436d6-315">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="436d6-315">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="436d6-316">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="436d6-316">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="436d6-317">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="436d6-317">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="436d6-318">Empty</span><span class="sxs-lookup"><span data-stu-id="436d6-318">Empty</span></span> | <span data-ttu-id="436d6-319">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="436d6-319">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="436d6-320">Empty</span><span class="sxs-lookup"><span data-stu-id="436d6-320">Empty</span></span> | <span data-ttu-id="436d6-321">Une collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-321">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="436d6-322">Empty</span><span class="sxs-lookup"><span data-stu-id="436d6-322">Empty</span></span> | <span data-ttu-id="436d6-323">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-323">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="436d6-324">5 secondes</span><span class="sxs-lookup"><span data-stu-id="436d6-324">5 seconds</span></span> | <span data-ttu-id="436d6-325">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="436d6-325">WebSockets only.</span></span> <span data-ttu-id="436d6-326">La quantité maximale de temps le client attend après la fermeture du serveur accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="436d6-326">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="436d6-327">Si le serveur n’accuse pas la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="436d6-327">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="436d6-328">Empty</span><span class="sxs-lookup"><span data-stu-id="436d6-328">Empty</span></span> | <span data-ttu-id="436d6-329">Une carte d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-329">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="436d6-330">Un délégué qui peut être utilisé pour configurer ou de remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-330">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="436d6-331">Pas utilisé pour les connexions de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="436d6-331">Not used for WebSocket connections.</span></span> <span data-ttu-id="436d6-332">Ce délégué doit retourner une valeur non null, et qu’il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="436d6-332">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="436d6-333">Modifier les paramètres sur cette valeur par défaut et retourner ou retourner un nouveau `HttpMessageHandler` instance.</span><span class="sxs-lookup"><span data-stu-id="436d6-333">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="436d6-334">**Lorsque vous en remplaçant le gestionnaire veillez à copier les paramètres que vous souhaitez empêcher le gestionnaire fourni, sinon, les options configurées (par exemple, les Cookies et les en-têtes) ne s’appliquent pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="436d6-334">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="436d6-335">Un proxy HTTP à utiliser lors de l’envoi des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-335">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="436d6-336">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les demandes HTTP et WebSockets.</span><span class="sxs-lookup"><span data-stu-id="436d6-336">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="436d6-337">Cela permet d’utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="436d6-337">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="436d6-338">Délégué qui peut être utilisé pour configurer des options supplémentaires de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="436d6-338">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="436d6-339">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="436d6-339">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="436d6-340">JavaScript</span><span class="sxs-lookup"><span data-stu-id="436d6-340">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="436d6-341">Option de JavaScript</span><span class="sxs-lookup"><span data-stu-id="436d6-341">JavaScript Option</span></span> | <span data-ttu-id="436d6-342">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-342">Default Value</span></span> | <span data-ttu-id="436d6-343">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-343">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="436d6-344">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-344">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="436d6-345">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="436d6-345">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="436d6-346">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="436d6-346">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="436d6-347">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="436d6-347">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="436d6-348">Java</span><span class="sxs-lookup"><span data-stu-id="436d6-348">Java</span></span>](#tab/java)

| <span data-ttu-id="436d6-349">Option de Java</span><span class="sxs-lookup"><span data-stu-id="436d6-349">Java Option</span></span> | <span data-ttu-id="436d6-350">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="436d6-350">Default Value</span></span> | <span data-ttu-id="436d6-351">Description</span><span class="sxs-lookup"><span data-stu-id="436d6-351">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="436d6-352">Une fonction qui retourne une chaîne qui est fournie comme un jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-352">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="436d6-353">Affectez la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="436d6-353">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="436d6-354">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="436d6-354">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="436d6-355">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="436d6-355">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="436d6-356">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="436d6-356">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="436d6-357">Empty</span><span class="sxs-lookup"><span data-stu-id="436d6-357">Empty</span></span> | <span data-ttu-id="436d6-358">Une carte d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="436d6-358">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="436d6-359">Dans le Client .NET, ces options peuvent être modifiées par l’options du délégué fourni `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="436d6-359">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="436d6-360">Dans le JavaScript Client, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="436d6-360">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="436d6-361">Dans le client Java, ces options peuvent être configurées avec les méthodes sur le `HttpHubConnectionBuilder` retourné à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="436d6-361">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="436d6-362">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="436d6-362">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
