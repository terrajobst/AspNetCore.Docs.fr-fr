---
title: Configuration de Signalr ASP.NET Core
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 156ffac83fbdf61fd88ad8acc307c2c701c46bca
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773933"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="65a92-103">Configuration de Signalr ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65a92-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="65a92-104">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="65a92-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="65a92-105">ASP.NET Core Signalr prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="65a92-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="65a92-106">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="65a92-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="65a92-107">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , qui peut être `Startup.ConfigureServices` ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans votre méthode.</span><span class="sxs-lookup"><span data-stu-id="65a92-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="65a92-108">La `AddJsonProtocol` méthode prend un délégué qui reçoit un `options` objet.</span><span class="sxs-lookup"><span data-stu-id="65a92-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="65a92-109">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet `JsonSerializerSettings` JSON.net qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="65a92-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="65a92-110">Pour plus d’informations, consultez la [documentation de JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm) .</span><span class="sxs-lookup"><span data-stu-id="65a92-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="65a92-111">Par exemple, pour configurer le sérialiseur pour utiliser des noms de propriété « en casse Pascal », au lieu des noms « en casse mixte » par défaut, utilisez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="65a92-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="65a92-112">Dans le client .net, la même `AddJsonProtocol` méthode d’extension existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="65a92-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="65a92-113">L' `Microsoft.Extensions.DependencyInjection` espace de noms doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="65a92-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="65a92-114">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="65a92-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="65a92-115">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="65a92-115">MessagePack serialization options</span></span>

<span data-ttu-id="65a92-116">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="65a92-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="65a92-117">Pour plus d’informations, consultez [MessagePack dans signalr](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="65a92-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="65a92-118">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="65a92-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="65a92-119">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="65a92-119">Configure server options</span></span>

<span data-ttu-id="65a92-120">Le tableau suivant décrit les options de configuration des hubs Signalr :</span><span class="sxs-lookup"><span data-stu-id="65a92-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="65a92-121">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-121">Option</span></span> | <span data-ttu-id="65a92-122">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-122">Default Value</span></span> | <span data-ttu-id="65a92-123">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="65a92-124">30 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-124">30 seconds</span></span> | <span data-ttu-id="65a92-125">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="65a92-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="65a92-126">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="65a92-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="65a92-127">La valeur recommandée est le double `KeepAliveInterval` de la valeur.</span><span class="sxs-lookup"><span data-stu-id="65a92-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="65a92-128">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-128">15 seconds</span></span> | <span data-ttu-id="65a92-129">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="65a92-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="65a92-130">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-131">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="65a92-132">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-132">15 seconds</span></span> | <span data-ttu-id="65a92-133">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="65a92-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="65a92-134">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="65a92-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="65a92-135">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="65a92-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="65a92-136">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="65a92-136">All installed protocols</span></span> | <span data-ttu-id="65a92-137">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="65a92-137">Protocols supported by this hub.</span></span> <span data-ttu-id="65a92-138">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="65a92-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="65a92-139">Si `true`la valeur est, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="65a92-140">La valeur par `false`défaut est, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="65a92-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="65a92-141">Nombre maximal d’éléments pouvant être mis en mémoire tampon pour les flux de téléchargement du client.</span><span class="sxs-lookup"><span data-stu-id="65a92-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="65a92-142">Si cette limite est atteinte, le traitement des appels est bloqué jusqu’à ce que le serveur traite les éléments de flux.</span><span class="sxs-lookup"><span data-stu-id="65a92-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="65a92-143">32 Ko</span><span class="sxs-lookup"><span data-stu-id="65a92-143">32 KB</span></span> | <span data-ttu-id="65a92-144">Taille maximale d’un seul message de concentrateur entrant.</span><span class="sxs-lookup"><span data-stu-id="65a92-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="65a92-145">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-145">Option</span></span> | <span data-ttu-id="65a92-146">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-146">Default Value</span></span> | <span data-ttu-id="65a92-147">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="65a92-148">30 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-148">30 seconds</span></span> | <span data-ttu-id="65a92-149">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="65a92-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="65a92-150">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="65a92-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="65a92-151">La valeur recommandée est le double `KeepAliveInterval` de la valeur.</span><span class="sxs-lookup"><span data-stu-id="65a92-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="65a92-152">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-152">15 seconds</span></span> | <span data-ttu-id="65a92-153">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="65a92-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="65a92-154">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-155">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="65a92-156">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-156">15 seconds</span></span> | <span data-ttu-id="65a92-157">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="65a92-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="65a92-158">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="65a92-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="65a92-159">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="65a92-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="65a92-160">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="65a92-160">All installed protocols</span></span> | <span data-ttu-id="65a92-161">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="65a92-161">Protocols supported by this hub.</span></span> <span data-ttu-id="65a92-162">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="65a92-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="65a92-163">Si `true`la valeur est, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="65a92-164">La valeur par `false`défaut est, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="65a92-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="65a92-165">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-165">Option</span></span> | <span data-ttu-id="65a92-166">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-166">Default Value</span></span> | <span data-ttu-id="65a92-167">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="65a92-168">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-168">15 seconds</span></span> | <span data-ttu-id="65a92-169">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="65a92-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="65a92-170">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-171">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="65a92-172">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-172">15 seconds</span></span> | <span data-ttu-id="65a92-173">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="65a92-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="65a92-174">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="65a92-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="65a92-175">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="65a92-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="65a92-176">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="65a92-176">All installed protocols</span></span> | <span data-ttu-id="65a92-177">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="65a92-177">Protocols supported by this hub.</span></span> <span data-ttu-id="65a92-178">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="65a92-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="65a92-179">Si `true`la valeur est, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="65a92-180">La valeur par `false`défaut est, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="65a92-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="65a92-181">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="65a92-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="65a92-182">Les options d’un seul Hub remplacent les options globales `AddSignalR` fournies dans et peuvent être <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>configurées à l’aide de :</span><span class="sxs-lookup"><span data-stu-id="65a92-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="65a92-183">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="65a92-183">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="65a92-184">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="65a92-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="65a92-185">Ces options sont configurées en passant un délégué [à\<MapHub T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) dans. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="65a92-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="65a92-186">Utilisez `HttpConnectionDispatcherOptions` pour configurer les paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="65a92-186">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="65a92-187">Ces options sont configurées en passant un délégué [à\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="65a92-187">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

::: moniker-end

<span data-ttu-id="65a92-188">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core Signalr :</span><span class="sxs-lookup"><span data-stu-id="65a92-188">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="65a92-189">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-189">Option</span></span> | <span data-ttu-id="65a92-190">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-190">Default Value</span></span> | <span data-ttu-id="65a92-191">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-191">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="65a92-192">32 Ko</span><span class="sxs-lookup"><span data-stu-id="65a92-192">32 KB</span></span> | <span data-ttu-id="65a92-193">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon avant d’appliquer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="65a92-193">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="65a92-194">L’augmentation de cette valeur permet au serveur de recevoir plus rapidement des messages plus volumineux sans appliquer la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="65a92-194">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="65a92-195">Données collectées automatiquement à `Authorize` partir des attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-195">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="65a92-196">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-196">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="65a92-197">32 Ko</span><span class="sxs-lookup"><span data-stu-id="65a92-197">32 KB</span></span> | <span data-ttu-id="65a92-198">Nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon avant d’observer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="65a92-198">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="65a92-199">L’augmentation de cette valeur permet au serveur de mettre plus rapidement en mémoire tampon des messages plus volumineux sans attendre la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="65a92-199">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="65a92-200">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="65a92-200">All Transports are enabled.</span></span> | <span data-ttu-id="65a92-201">Énumération de bits indicateurs `HttpTransportType` des valeurs qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="65a92-201">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="65a92-202">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="65a92-202">See below.</span></span> | <span data-ttu-id="65a92-203">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="65a92-203">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="65a92-204">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="65a92-204">See below.</span></span> | <span data-ttu-id="65a92-205">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="65a92-205">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="65a92-206">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-206">Option</span></span> | <span data-ttu-id="65a92-207">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-207">Default Value</span></span> | <span data-ttu-id="65a92-208">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-208">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="65a92-209">32 Ko</span><span class="sxs-lookup"><span data-stu-id="65a92-209">32 KB</span></span> | <span data-ttu-id="65a92-210">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="65a92-210">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="65a92-211">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="65a92-211">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="65a92-212">Données collectées automatiquement à `Authorize` partir des attributs appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-212">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="65a92-213">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-213">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="65a92-214">32 Ko</span><span class="sxs-lookup"><span data-stu-id="65a92-214">32 KB</span></span> | <span data-ttu-id="65a92-215">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="65a92-215">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="65a92-216">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="65a92-216">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="65a92-217">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="65a92-217">All Transports are enabled.</span></span> | <span data-ttu-id="65a92-218">Énumération de bits indicateurs `HttpTransportType` des valeurs qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="65a92-218">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="65a92-219">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="65a92-219">See below.</span></span> | <span data-ttu-id="65a92-220">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="65a92-220">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="65a92-221">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="65a92-221">See below.</span></span> | <span data-ttu-id="65a92-222">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="65a92-222">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="65a92-223">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="65a92-223">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="65a92-224">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-224">Option</span></span> | <span data-ttu-id="65a92-225">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-225">Default Value</span></span> | <span data-ttu-id="65a92-226">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-226">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="65a92-227">90 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-227">90 seconds</span></span> | <span data-ttu-id="65a92-228">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="65a92-228">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="65a92-229">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="65a92-229">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="65a92-230">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="65a92-230">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="65a92-231">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-231">Option</span></span> | <span data-ttu-id="65a92-232">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-232">Default Value</span></span> | <span data-ttu-id="65a92-233">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-233">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="65a92-234">5 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-234">5 seconds</span></span> | <span data-ttu-id="65a92-235">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="65a92-235">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="65a92-236">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="65a92-236">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="65a92-237">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="65a92-237">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="65a92-238">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="65a92-238">Configure client options</span></span>

<span data-ttu-id="65a92-239">Les options du client peuvent être configurées sur le `HubConnectionBuilder` type (disponible dans les clients .net et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="65a92-239">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="65a92-240">Il est également disponible dans le client Java, mais la `HttpHubConnectionBuilder` sous-classe est ce qui contient les options de configuration du générateur, ainsi `HubConnection` que sur le lui-même.</span><span class="sxs-lookup"><span data-stu-id="65a92-240">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="65a92-241">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="65a92-241">Configure logging</span></span>

<span data-ttu-id="65a92-242">La journalisation est configurée dans le client `ConfigureLogging` .net à l’aide de la méthode.</span><span class="sxs-lookup"><span data-stu-id="65a92-242">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="65a92-243">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-243">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="65a92-244">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="65a92-244">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="65a92-245">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="65a92-245">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="65a92-246">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="65a92-246">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="65a92-247">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="65a92-247">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="65a92-248">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="65a92-248">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="65a92-249">Dans le client JavaScript, il existe `configureLogging` une méthode similaire.</span><span class="sxs-lookup"><span data-stu-id="65a92-249">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="65a92-250">Indiquez une `LogLevel` valeur indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="65a92-250">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="65a92-251">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="65a92-251">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="65a92-252">Au lieu d' `LogLevel` une valeur, vous pouvez également fournir `string` une valeur représentant un nom de niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="65a92-252">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="65a92-253">Cela est utile lors de la configuration de la journalisation signalr dans les `LogLevel` environnements où vous n’avez pas accès aux constantes.</span><span class="sxs-lookup"><span data-stu-id="65a92-253">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="65a92-254">Le tableau suivant répertorie les niveaux de journalisation disponibles.</span><span class="sxs-lookup"><span data-stu-id="65a92-254">The following table lists the available log levels.</span></span> <span data-ttu-id="65a92-255">La valeur que vous fournissez pour `configureLogging` définir le niveau de journalisation **minimale** qui sera enregistré.</span><span class="sxs-lookup"><span data-stu-id="65a92-255">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="65a92-256">Les messages enregistrés à ce niveau, **ou les niveaux indiqués après celui-ci dans la table**, sont enregistrés.</span><span class="sxs-lookup"><span data-stu-id="65a92-256">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="65a92-257">String</span><span class="sxs-lookup"><span data-stu-id="65a92-257">String</span></span>                      | <span data-ttu-id="65a92-258">LogLevel</span><span class="sxs-lookup"><span data-stu-id="65a92-258">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="65a92-259">`info`**ou**`information`</span><span class="sxs-lookup"><span data-stu-id="65a92-259">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="65a92-260">`warn`**ou**`warning`</span><span class="sxs-lookup"><span data-stu-id="65a92-260">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="65a92-261">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="65a92-261">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="65a92-262">Pour plus d’informations sur la journalisation, consultez la [documentation relative aux diagnostics de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="65a92-262">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="65a92-263">Le client Java Signalr utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="65a92-263">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="65a92-264">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="65a92-264">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="65a92-265">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java signalr.</span><span class="sxs-lookup"><span data-stu-id="65a92-265">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="65a92-266">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="65a92-266">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="65a92-267">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="65a92-267">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="65a92-268">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="65a92-268">Configure allowed transports</span></span>

<span data-ttu-id="65a92-269">Les transports utilisés par signalr peuvent être configurés dans l' `WithUrl` appel (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="65a92-269">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="65a92-270">Une opération or au niveau du bit des `HttpTransportType` valeurs de peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="65a92-270">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="65a92-271">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="65a92-271">All transports are enabled by default.</span></span>

<span data-ttu-id="65a92-272">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="65a92-272">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="65a92-273">Dans le client JavaScript, les transports sont configurés en définissant le `transport` champ sur l’objet d’options fourni à : `withUrl`</span><span class="sxs-lookup"><span data-stu-id="65a92-273">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="65a92-274">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="65a92-274">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="65a92-275">Dans le client Java, le transport est sélectionné à l' `withTransport` aide de la `HttpHubConnectionBuilder`méthode sur le.</span><span class="sxs-lookup"><span data-stu-id="65a92-275">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="65a92-276">Le client Java utilise le transport WebSocket par défaut.</span><span class="sxs-lookup"><span data-stu-id="65a92-276">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="65a92-277">Le client Java Signalr ne prend pas encore en charge le transport de secours.</span><span class="sxs-lookup"><span data-stu-id="65a92-277">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="65a92-278">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="65a92-278">Configure bearer authentication</span></span>

<span data-ttu-id="65a92-279">Pour fournir des données d’authentification avec les demandes signalr, `AccessTokenProvider` utilisez l'`accessTokenFactory` option (en JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="65a92-279">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="65a92-280">Dans le client .net, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » http ( `Authorization` à l’aide de l' `Bearer`en-tête de type).</span><span class="sxs-lookup"><span data-stu-id="65a92-280">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="65a92-281">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="65a92-281">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="65a92-282">Dans ce cas, le jeton d’accès est fourni sous la forme d' `access_token`une valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="65a92-282">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="65a92-283">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="65a92-283">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="65a92-284">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="65a92-284">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="65a92-285">Dans le client Java Signalr, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="65a92-285">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="65a92-286">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="65a92-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="65a92-287">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="65a92-287">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="65a92-288">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="65a92-288">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="65a92-289">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="65a92-289">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="65a92-290">.NET</span><span class="sxs-lookup"><span data-stu-id="65a92-290">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="65a92-291">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-291">Option</span></span> | <span data-ttu-id="65a92-292">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-292">Default value</span></span> | <span data-ttu-id="65a92-293">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-293">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="65a92-294">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-294">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="65a92-295">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-295">Timeout for server activity.</span></span> <span data-ttu-id="65a92-296">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté `Closed` et déclenche`onclose` l’événement (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="65a92-296">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="65a92-297">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="65a92-297">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="65a92-298">La valeur recommandée est un nombre au moins double de la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-298">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="65a92-299">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-299">15 seconds</span></span> | <span data-ttu-id="65a92-300">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-300">Timeout for initial server handshake.</span></span> <span data-ttu-id="65a92-301">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche `Closed` l’événement`onclose` (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="65a92-301">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="65a92-302">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-302">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-303">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-303">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="65a92-304">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-304">15 seconds</span></span> | <span data-ttu-id="65a92-305">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-305">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="65a92-306">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="65a92-306">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="65a92-307">Si le client n’a pas envoyé de message `ClientTimeoutInterval` dans le jeu sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="65a92-307">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="65a92-308">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="65a92-308">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="65a92-309">JavaScript</span><span class="sxs-lookup"><span data-stu-id="65a92-309">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="65a92-310">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-310">Option</span></span> | <span data-ttu-id="65a92-311">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-311">Default value</span></span> | <span data-ttu-id="65a92-312">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-312">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="65a92-313">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-313">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="65a92-314">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-314">Timeout for server activity.</span></span> <span data-ttu-id="65a92-315">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté `onclose` et déclenche l’événement.</span><span class="sxs-lookup"><span data-stu-id="65a92-315">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="65a92-316">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="65a92-316">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="65a92-317">La valeur recommandée est un nombre au moins double de la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-317">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="65a92-318">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-318">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="65a92-319">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-319">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="65a92-320">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="65a92-320">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="65a92-321">Si le client n’a pas envoyé de message `ClientTimeoutInterval` dans le jeu sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="65a92-321">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="65a92-322">Java</span><span class="sxs-lookup"><span data-stu-id="65a92-322">Java</span></span>](#tab/java)

| <span data-ttu-id="65a92-323">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-323">Option</span></span> | <span data-ttu-id="65a92-324">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-324">Default value</span></span> | <span data-ttu-id="65a92-325">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-325">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="65a92-326">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="65a92-326">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="65a92-327">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-327">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="65a92-328">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-328">Timeout for server activity.</span></span> <span data-ttu-id="65a92-329">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté `onClose` et déclenche l’événement.</span><span class="sxs-lookup"><span data-stu-id="65a92-329">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="65a92-330">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="65a92-330">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="65a92-331">La valeur recommandée est un nombre au moins double de la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-331">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="65a92-332">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-332">15 seconds</span></span> | <span data-ttu-id="65a92-333">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-333">Timeout for initial server handshake.</span></span> <span data-ttu-id="65a92-334">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche `onClose` l’événement.</span><span class="sxs-lookup"><span data-stu-id="65a92-334">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="65a92-335">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-335">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-336">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-336">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="65a92-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="65a92-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="65a92-338">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-338">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="65a92-339">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-339">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="65a92-340">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="65a92-340">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="65a92-341">Si le client n’a pas envoyé de message `ClientTimeoutInterval` dans le jeu sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="65a92-341">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="65a92-342">.NET</span><span class="sxs-lookup"><span data-stu-id="65a92-342">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="65a92-343">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-343">Option</span></span> | <span data-ttu-id="65a92-344">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-344">Default value</span></span> | <span data-ttu-id="65a92-345">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-345">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="65a92-346">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-346">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="65a92-347">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-347">Timeout for server activity.</span></span> <span data-ttu-id="65a92-348">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté `Closed` et déclenche`onclose` l’événement (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="65a92-348">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="65a92-349">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="65a92-349">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="65a92-350">La valeur recommandée est un nombre au moins double de la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-350">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="65a92-351">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-351">15 seconds</span></span> | <span data-ttu-id="65a92-352">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-352">Timeout for initial server handshake.</span></span> <span data-ttu-id="65a92-353">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche `Closed` l’événement`onclose` (en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="65a92-353">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="65a92-354">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-354">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-355">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-355">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="65a92-356">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="65a92-356">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="65a92-357">JavaScript</span><span class="sxs-lookup"><span data-stu-id="65a92-357">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="65a92-358">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-358">Option</span></span> | <span data-ttu-id="65a92-359">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-359">Default value</span></span> | <span data-ttu-id="65a92-360">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-360">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="65a92-361">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-361">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="65a92-362">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-362">Timeout for server activity.</span></span> <span data-ttu-id="65a92-363">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté `onclose` et déclenche l’événement.</span><span class="sxs-lookup"><span data-stu-id="65a92-363">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="65a92-364">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="65a92-364">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="65a92-365">La valeur recommandée est un nombre au moins double de la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-365">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="65a92-366">Java</span><span class="sxs-lookup"><span data-stu-id="65a92-366">Java</span></span>](#tab/java)

| <span data-ttu-id="65a92-367">Option</span><span class="sxs-lookup"><span data-stu-id="65a92-367">Option</span></span> | <span data-ttu-id="65a92-368">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-368">Default value</span></span> | <span data-ttu-id="65a92-369">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-369">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="65a92-370">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="65a92-370">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="65a92-371">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="65a92-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="65a92-372">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-372">Timeout for server activity.</span></span> <span data-ttu-id="65a92-373">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté `onClose` et déclenche l’événement.</span><span class="sxs-lookup"><span data-stu-id="65a92-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="65a92-374">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="65a92-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="65a92-375">La valeur recommandée est un nombre au moins double de la valeur `KeepAliveInterval` du serveur, pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="65a92-375">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="65a92-376">15 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-376">15 seconds</span></span> | <span data-ttu-id="65a92-377">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="65a92-377">Timeout for initial server handshake.</span></span> <span data-ttu-id="65a92-378">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche `onClose` l’événement.</span><span class="sxs-lookup"><span data-stu-id="65a92-378">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="65a92-379">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="65a92-379">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="65a92-380">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="65a92-380">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="65a92-381">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="65a92-381">Configure additional options</span></span>

<span data-ttu-id="65a92-382">Des options supplémentaires peuvent être configurées `WithUrl` dans`withUrl` la méthode (en JavaScript `HubConnectionBuilder` ) sur ou sur les différentes API de `HttpHubConnectionBuilder` configuration sur le dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="65a92-382">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="65a92-383">.NET</span><span class="sxs-lookup"><span data-stu-id="65a92-383">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="65a92-384">Option .NET</span><span class="sxs-lookup"><span data-stu-id="65a92-384">.NET Option</span></span> |  <span data-ttu-id="65a92-385">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-385">Default value</span></span> | <span data-ttu-id="65a92-386">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-386">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="65a92-387">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-387">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="65a92-388">Affectez la valeur pour ignorer l’étape de négociation. `true`</span><span class="sxs-lookup"><span data-stu-id="65a92-388">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="65a92-389">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="65a92-389">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="65a92-390">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="65a92-390">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="65a92-391">Empty</span><span class="sxs-lookup"><span data-stu-id="65a92-391">Empty</span></span> | <span data-ttu-id="65a92-392">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="65a92-392">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="65a92-393">Empty</span><span class="sxs-lookup"><span data-stu-id="65a92-393">Empty</span></span> | <span data-ttu-id="65a92-394">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-394">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="65a92-395">Empty</span><span class="sxs-lookup"><span data-stu-id="65a92-395">Empty</span></span> | <span data-ttu-id="65a92-396">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-396">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="65a92-397">5 secondes</span><span class="sxs-lookup"><span data-stu-id="65a92-397">5 seconds</span></span> | <span data-ttu-id="65a92-398">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="65a92-398">WebSockets only.</span></span> <span data-ttu-id="65a92-399">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="65a92-399">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="65a92-400">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="65a92-400">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="65a92-401">Empty</span><span class="sxs-lookup"><span data-stu-id="65a92-401">Empty</span></span> | <span data-ttu-id="65a92-402">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-402">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="65a92-403">Délégué qui peut être utilisé pour configurer ou remplacer le utilisé `HttpMessageHandler` pour envoyer des requêtes http.</span><span class="sxs-lookup"><span data-stu-id="65a92-403">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="65a92-404">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="65a92-404">Not used for WebSocket connections.</span></span> <span data-ttu-id="65a92-405">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="65a92-405">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="65a92-406">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou `HttpMessageHandler` retournez une nouvelle instance.</span><span class="sxs-lookup"><span data-stu-id="65a92-406">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="65a92-407">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="65a92-407">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="65a92-408">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-408">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="65a92-409">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="65a92-409">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="65a92-410">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="65a92-410">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="65a92-411">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="65a92-411">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="65a92-412">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="65a92-412">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="65a92-413">JavaScript</span><span class="sxs-lookup"><span data-stu-id="65a92-413">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="65a92-414">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="65a92-414">JavaScript Option</span></span> | <span data-ttu-id="65a92-415">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-415">Default Value</span></span> | <span data-ttu-id="65a92-416">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-416">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="65a92-417">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-417">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="65a92-418">Affectez la valeur pour ignorer l’étape de négociation. `true`</span><span class="sxs-lookup"><span data-stu-id="65a92-418">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="65a92-419">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="65a92-419">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="65a92-420">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="65a92-420">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="65a92-421">Java</span><span class="sxs-lookup"><span data-stu-id="65a92-421">Java</span></span>](#tab/java)

| <span data-ttu-id="65a92-422">Option Java</span><span class="sxs-lookup"><span data-stu-id="65a92-422">Java Option</span></span> | <span data-ttu-id="65a92-423">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="65a92-423">Default Value</span></span> | <span data-ttu-id="65a92-424">Description</span><span class="sxs-lookup"><span data-stu-id="65a92-424">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="65a92-425">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-425">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="65a92-426">Affectez la valeur pour ignorer l’étape de négociation. `true`</span><span class="sxs-lookup"><span data-stu-id="65a92-426">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="65a92-427">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="65a92-427">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="65a92-428">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="65a92-428">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="65a92-429">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="65a92-429">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="65a92-430">Empty</span><span class="sxs-lookup"><span data-stu-id="65a92-430">Empty</span></span> | <span data-ttu-id="65a92-431">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="65a92-431">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="65a92-432">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni `WithUrl`à :</span><span class="sxs-lookup"><span data-stu-id="65a92-432">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="65a92-433">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="65a92-433">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="65a92-434">Dans le client Java, ces options peuvent être configurées avec les méthodes sur `HttpHubConnectionBuilder` le retourné à partir de l’option`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="65a92-434">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="65a92-435">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="65a92-435">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
