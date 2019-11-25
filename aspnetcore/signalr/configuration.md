---
title: Configuration de la SignalR ASP.NET Core
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963985"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="fac2a-103">Configuration de la SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fac2a-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="fac2a-104">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="fac2a-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="fac2a-105">ASP.NET Core SignalR prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="fac2a-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="fac2a-106">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="fac2a-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fac2a-107">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="fac2a-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="fac2a-108">`AddJsonProtocol` peut être ajouté après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fac2a-109">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="fac2a-110">La propriété [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) sur cet objet est un objet <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="fac2a-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="fac2a-111">Pour plus d’informations, consultez la [Documentation System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="fac2a-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="fac2a-112">Par exemple, pour configurer le sérialiseur de manière à ce qu’il ne change pas la casse des noms de propriétés, au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="fac2a-113">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="fac2a-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="fac2a-114">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="fac2a-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="fac2a-115">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="fac2a-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="fac2a-116">Basculer vers Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="fac2a-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="fac2a-117">Si vous avez besoin de fonctionnalités de `Newtonsoft.Json` qui ne sont pas prises en charge dans `System.Text.Json`, consultez [basculer vers Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="fac2a-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="fac2a-118">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol), qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-118">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fac2a-119">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-119">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="fac2a-120">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet JSON.net `JsonSerializerSettings` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="fac2a-120">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="fac2a-121">Pour plus d’informations, consultez la [documentation JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="fac2a-121">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="fac2a-122">Par exemple, pour configurer le sérialiseur afin d’utiliser les noms de propriété « casse Pascal », au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-122">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="fac2a-123">Dans le client .NET, la même méthode d’extension de `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="fac2a-123">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="fac2a-124">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="fac2a-124">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="fac2a-125">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="fac2a-125">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

::: moniker-end

### <a name="messagepack-serialization-options"></a><span data-ttu-id="fac2a-126">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="fac2a-126">MessagePack serialization options</span></span>

<span data-ttu-id="fac2a-127">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="fac2a-127">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="fac2a-128">Pour plus d’informations, consultez [MessagePack dans SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="fac2a-128">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="fac2a-129">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="fac2a-129">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="fac2a-130">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="fac2a-130">Configure server options</span></span>

<span data-ttu-id="fac2a-131">Le tableau suivant décrit les options de configuration des SignalR hubs :</span><span class="sxs-lookup"><span data-stu-id="fac2a-131">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="fac2a-132">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-132">Option</span></span> | <span data-ttu-id="fac2a-133">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-133">Default Value</span></span> | <span data-ttu-id="fac2a-134">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="fac2a-135">30 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-135">30 seconds</span></span> | <span data-ttu-id="fac2a-136">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="fac2a-136">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="fac2a-137">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="fac2a-137">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="fac2a-138">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-138">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="fac2a-139">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-139">15 seconds</span></span> | <span data-ttu-id="fac2a-140">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="fac2a-140">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="fac2a-141">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-141">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-142">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-142">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fac2a-143">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-143">15 seconds</span></span> | <span data-ttu-id="fac2a-144">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="fac2a-144">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="fac2a-145">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="fac2a-145">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="fac2a-146">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-146">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="fac2a-147">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="fac2a-147">All installed protocols</span></span> | <span data-ttu-id="fac2a-148">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="fac2a-148">Protocols supported by this hub.</span></span> <span data-ttu-id="fac2a-149">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="fac2a-149">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="fac2a-150">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-150">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fac2a-151">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="fac2a-151">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="fac2a-152">Nombre maximal d’éléments pouvant être mis en mémoire tampon pour les flux de téléchargement du client.</span><span class="sxs-lookup"><span data-stu-id="fac2a-152">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="fac2a-153">Si cette limite est atteinte, le traitement des appels est bloqué jusqu’à ce que le serveur traite les éléments de flux.</span><span class="sxs-lookup"><span data-stu-id="fac2a-153">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="fac2a-154">32 Ko</span><span class="sxs-lookup"><span data-stu-id="fac2a-154">32 KB</span></span> | <span data-ttu-id="fac2a-155">Taille maximale d’un seul message de concentrateur entrant.</span><span class="sxs-lookup"><span data-stu-id="fac2a-155">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="fac2a-156">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-156">Option</span></span> | <span data-ttu-id="fac2a-157">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-157">Default Value</span></span> | <span data-ttu-id="fac2a-158">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-158">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="fac2a-159">30 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-159">30 seconds</span></span> | <span data-ttu-id="fac2a-160">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="fac2a-160">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="fac2a-161">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="fac2a-161">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="fac2a-162">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-162">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="fac2a-163">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-163">15 seconds</span></span> | <span data-ttu-id="fac2a-164">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="fac2a-164">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="fac2a-165">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-165">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-166">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-166">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fac2a-167">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-167">15 seconds</span></span> | <span data-ttu-id="fac2a-168">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="fac2a-168">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="fac2a-169">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="fac2a-169">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="fac2a-170">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-170">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="fac2a-171">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="fac2a-171">All installed protocols</span></span> | <span data-ttu-id="fac2a-172">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="fac2a-172">Protocols supported by this hub.</span></span> <span data-ttu-id="fac2a-173">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="fac2a-173">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="fac2a-174">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-174">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fac2a-175">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="fac2a-175">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="fac2a-176">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-176">Option</span></span> | <span data-ttu-id="fac2a-177">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-177">Default Value</span></span> | <span data-ttu-id="fac2a-178">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-178">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="fac2a-179">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-179">15 seconds</span></span> | <span data-ttu-id="fac2a-180">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="fac2a-180">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="fac2a-181">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-181">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-182">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-182">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fac2a-183">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-183">15 seconds</span></span> | <span data-ttu-id="fac2a-184">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="fac2a-184">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="fac2a-185">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="fac2a-185">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="fac2a-186">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-186">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="fac2a-187">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="fac2a-187">All installed protocols</span></span> | <span data-ttu-id="fac2a-188">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="fac2a-188">Protocols supported by this hub.</span></span> <span data-ttu-id="fac2a-189">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="fac2a-189">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="fac2a-190">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-190">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fac2a-191">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="fac2a-191">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="fac2a-192">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-192">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="fac2a-193">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="fac2a-193">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="fac2a-194">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="fac2a-194">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fac2a-195">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-195">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="fac2a-196">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-196">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="fac2a-197">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-197">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="fac2a-198">Ces options sont configurées en passant un délégué à [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-198">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="fac2a-199">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="fac2a-199">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="fac2a-200">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-200">Option</span></span> | <span data-ttu-id="fac2a-201">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-201">Default Value</span></span> | <span data-ttu-id="fac2a-202">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-202">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="fac2a-203">32 Ko</span><span class="sxs-lookup"><span data-stu-id="fac2a-203">32 KB</span></span> | <span data-ttu-id="fac2a-204">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon avant d’appliquer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="fac2a-204">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="fac2a-205">L’augmentation de cette valeur permet au serveur de recevoir plus rapidement des messages plus volumineux sans appliquer la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-205">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="fac2a-206">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-206">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="fac2a-207">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-207">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="fac2a-208">32 Ko</span><span class="sxs-lookup"><span data-stu-id="fac2a-208">32 KB</span></span> | <span data-ttu-id="fac2a-209">Nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon avant d’observer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="fac2a-209">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="fac2a-210">L’augmentation de cette valeur permet au serveur de mettre plus rapidement en mémoire tampon des messages plus volumineux sans attendre la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-210">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="fac2a-211">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="fac2a-211">All Transports are enabled.</span></span> | <span data-ttu-id="fac2a-212">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="fac2a-212">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="fac2a-213">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fac2a-213">See below.</span></span> | <span data-ttu-id="fac2a-214">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="fac2a-214">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="fac2a-215">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fac2a-215">See below.</span></span> | <span data-ttu-id="fac2a-216">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="fac2a-216">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="fac2a-217">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-217">Option</span></span> | <span data-ttu-id="fac2a-218">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-218">Default Value</span></span> | <span data-ttu-id="fac2a-219">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-219">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="fac2a-220">32 Ko</span><span class="sxs-lookup"><span data-stu-id="fac2a-220">32 KB</span></span> | <span data-ttu-id="fac2a-221">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="fac2a-221">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="fac2a-222">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-222">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="fac2a-223">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-223">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="fac2a-224">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-224">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="fac2a-225">32 Ko</span><span class="sxs-lookup"><span data-stu-id="fac2a-225">32 KB</span></span> | <span data-ttu-id="fac2a-226">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="fac2a-226">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="fac2a-227">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-227">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="fac2a-228">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="fac2a-228">All Transports are enabled.</span></span> | <span data-ttu-id="fac2a-229">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="fac2a-229">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="fac2a-230">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fac2a-230">See below.</span></span> | <span data-ttu-id="fac2a-231">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="fac2a-231">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="fac2a-232">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fac2a-232">See below.</span></span> | <span data-ttu-id="fac2a-233">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="fac2a-233">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="fac2a-234">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="fac2a-234">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="fac2a-235">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-235">Option</span></span> | <span data-ttu-id="fac2a-236">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-236">Default Value</span></span> | <span data-ttu-id="fac2a-237">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-237">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="fac2a-238">90 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-238">90 seconds</span></span> | <span data-ttu-id="fac2a-239">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="fac2a-239">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="fac2a-240">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="fac2a-240">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="fac2a-241">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="fac2a-241">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="fac2a-242">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-242">Option</span></span> | <span data-ttu-id="fac2a-243">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-243">Default Value</span></span> | <span data-ttu-id="fac2a-244">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="fac2a-245">5 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-245">5 seconds</span></span> | <span data-ttu-id="fac2a-246">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="fac2a-246">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="fac2a-247">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="fac2a-247">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="fac2a-248">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="fac2a-248">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="fac2a-249">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="fac2a-249">Configure client options</span></span>

<span data-ttu-id="fac2a-250">Les options du client peuvent être configurées sur le type de `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fac2a-250">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="fac2a-251">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="fac2a-251">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="fac2a-252">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="fac2a-252">Configure logging</span></span>

<span data-ttu-id="fac2a-253">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-253">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="fac2a-254">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-254">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="fac2a-255">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="fac2a-255">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="fac2a-256">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="fac2a-256">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="fac2a-257">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="fac2a-257">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="fac2a-258">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-258">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="fac2a-259">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="fac2a-259">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="fac2a-260">Dans le client JavaScript, il existe une méthode de `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-260">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="fac2a-261">Fournissez une valeur de `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="fac2a-261">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="fac2a-262">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-262">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fac2a-263">Au lieu d’une valeur `LogLevel`, vous pouvez également fournir une valeur de `string` représentant un nom de niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="fac2a-263">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="fac2a-264">Cela est utile lors de la configuration de la journalisation SignalR dans les environnements où vous n’avez pas accès aux constantes `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-264">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="fac2a-265">Le tableau suivant répertorie les niveaux de journalisation disponibles.</span><span class="sxs-lookup"><span data-stu-id="fac2a-265">The following table lists the available log levels.</span></span> <span data-ttu-id="fac2a-266">La valeur que vous fournissez à `configureLogging` définit le niveau de journalisation **minimale** qui sera enregistré.</span><span class="sxs-lookup"><span data-stu-id="fac2a-266">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="fac2a-267">Les messages enregistrés à ce niveau, **ou les niveaux indiqués après celui-ci dans la table**, sont enregistrés.</span><span class="sxs-lookup"><span data-stu-id="fac2a-267">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="fac2a-268">String</span><span class="sxs-lookup"><span data-stu-id="fac2a-268">String</span></span>                      | <span data-ttu-id="fac2a-269">LogLevel</span><span class="sxs-lookup"><span data-stu-id="fac2a-269">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="fac2a-270">`info` **ou** `information`</span><span class="sxs-lookup"><span data-stu-id="fac2a-270">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="fac2a-271">`warn` **ou** `warning`</span><span class="sxs-lookup"><span data-stu-id="fac2a-271">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="fac2a-272">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-272">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="fac2a-273">Pour plus d’informations sur la journalisation, consultez la [documentationSignalR Diagnostics](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="fac2a-273">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="fac2a-274">Le client Java SignalR utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="fac2a-274">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="fac2a-275">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="fac2a-275">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="fac2a-276">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="fac2a-276">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="fac2a-277">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="fac2a-277">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="fac2a-278">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="fac2a-278">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="fac2a-279">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="fac2a-279">Configure allowed transports</span></span>

<span data-ttu-id="fac2a-280">Les transports utilisés par SignalR peuvent être configurés dans l’appel de `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fac2a-280">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="fac2a-281">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="fac2a-281">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="fac2a-282">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="fac2a-282">All transports are enabled by default.</span></span>

<span data-ttu-id="fac2a-283">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="fac2a-283">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="fac2a-284">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-284">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fac2a-285">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="fac2a-285">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="fac2a-286">Dans le client Java, le transport est sélectionné à l’aide de la méthode `withTransport` sur le `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-286">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="fac2a-287">Le client Java utilise le transport WebSocket par défaut.</span><span class="sxs-lookup"><span data-stu-id="fac2a-287">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="fac2a-288">Le client Java SignalR ne prend pas encore en charge le transport de secours.</span><span class="sxs-lookup"><span data-stu-id="fac2a-288">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="fac2a-289">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="fac2a-289">Configure bearer authentication</span></span>

<span data-ttu-id="fac2a-290">Pour fournir des données d’authentification avec les demandes de SignalR, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` en JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="fac2a-290">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="fac2a-291">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="fac2a-291">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="fac2a-292">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="fac2a-292">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="fac2a-293">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-293">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="fac2a-294">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-294">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="fac2a-295">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-295">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="fac2a-296">Dans le client Java SignalR, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="fac2a-296">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="fac2a-297">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="fac2a-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="fac2a-298">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="fac2a-298">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="fac2a-299">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="fac2a-299">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="fac2a-300">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="fac2a-300">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="fac2a-301">.NET</span><span class="sxs-lookup"><span data-stu-id="fac2a-301">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="fac2a-302">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-302">Option</span></span> | <span data-ttu-id="fac2a-303">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-303">Default value</span></span> | <span data-ttu-id="fac2a-304">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-304">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="fac2a-305">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-305">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-306">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-306">Timeout for server activity.</span></span> <span data-ttu-id="fac2a-307">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fac2a-307">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fac2a-308">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="fac2a-308">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fac2a-309">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-309">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="fac2a-310">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-310">15 seconds</span></span> | <span data-ttu-id="fac2a-311">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-311">Timeout for initial server handshake.</span></span> <span data-ttu-id="fac2a-312">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fac2a-312">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fac2a-313">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-313">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-314">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-314">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fac2a-315">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-315">15 seconds</span></span> | <span data-ttu-id="fac2a-316">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-316">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="fac2a-317">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="fac2a-317">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="fac2a-318">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="fac2a-318">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="fac2a-319">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-319">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="fac2a-320">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fac2a-320">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="fac2a-321">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-321">Option</span></span> | <span data-ttu-id="fac2a-322">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-322">Default value</span></span> | <span data-ttu-id="fac2a-323">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-323">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="fac2a-324">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-324">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-325">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-325">Timeout for server activity.</span></span> <span data-ttu-id="fac2a-326">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-326">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="fac2a-327">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="fac2a-327">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fac2a-328">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-328">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="fac2a-329">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-329">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="fac2a-330">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-330">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="fac2a-331">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="fac2a-331">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="fac2a-332">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="fac2a-332">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="fac2a-333">Java</span><span class="sxs-lookup"><span data-stu-id="fac2a-333">Java</span></span>](#tab/java)

| <span data-ttu-id="fac2a-334">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-334">Option</span></span> | <span data-ttu-id="fac2a-335">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-335">Default value</span></span> | <span data-ttu-id="fac2a-336">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-336">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="fac2a-337">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="fac2a-337">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="fac2a-338">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-338">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-339">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-339">Timeout for server activity.</span></span> <span data-ttu-id="fac2a-340">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-340">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="fac2a-341">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="fac2a-341">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fac2a-342">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-342">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="fac2a-343">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-343">15 seconds</span></span> | <span data-ttu-id="fac2a-344">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-344">Timeout for initial server handshake.</span></span> <span data-ttu-id="fac2a-345">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-345">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="fac2a-346">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-346">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-347">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-347">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="fac2a-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="fac2a-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="fac2a-349">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-349">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-350">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-350">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="fac2a-351">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="fac2a-351">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="fac2a-352">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="fac2a-352">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="fac2a-353">.NET</span><span class="sxs-lookup"><span data-stu-id="fac2a-353">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="fac2a-354">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-354">Option</span></span> | <span data-ttu-id="fac2a-355">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-355">Default value</span></span> | <span data-ttu-id="fac2a-356">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-356">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="fac2a-357">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-357">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-358">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-358">Timeout for server activity.</span></span> <span data-ttu-id="fac2a-359">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fac2a-359">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fac2a-360">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="fac2a-360">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fac2a-361">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-361">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="fac2a-362">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-362">15 seconds</span></span> | <span data-ttu-id="fac2a-363">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-363">Timeout for initial server handshake.</span></span> <span data-ttu-id="fac2a-364">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` dans JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fac2a-364">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="fac2a-365">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-365">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-366">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-366">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="fac2a-367">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-367">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="fac2a-368">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fac2a-368">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="fac2a-369">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-369">Option</span></span> | <span data-ttu-id="fac2a-370">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-370">Default value</span></span> | <span data-ttu-id="fac2a-371">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-371">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="fac2a-372">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-372">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-373">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-373">Timeout for server activity.</span></span> <span data-ttu-id="fac2a-374">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-374">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="fac2a-375">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="fac2a-375">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fac2a-376">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-376">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="fac2a-377">Java</span><span class="sxs-lookup"><span data-stu-id="fac2a-377">Java</span></span>](#tab/java)

| <span data-ttu-id="fac2a-378">Option</span><span class="sxs-lookup"><span data-stu-id="fac2a-378">Option</span></span> | <span data-ttu-id="fac2a-379">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-379">Default value</span></span> | <span data-ttu-id="fac2a-380">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-380">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="fac2a-381">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="fac2a-381">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="fac2a-382">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="fac2a-382">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="fac2a-383">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-383">Timeout for server activity.</span></span> <span data-ttu-id="fac2a-384">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-384">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="fac2a-385">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="fac2a-385">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="fac2a-386">La valeur recommandée est un nombre au moins double de la valeur de `KeepAliveInterval` du serveur, afin de laisser le temps de recevoir les commandes ping.</span><span class="sxs-lookup"><span data-stu-id="fac2a-386">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="fac2a-387">15 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-387">15 seconds</span></span> | <span data-ttu-id="fac2a-388">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="fac2a-388">Timeout for initial server handshake.</span></span> <span data-ttu-id="fac2a-389">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-389">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="fac2a-390">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="fac2a-390">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="fac2a-391">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocoleSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="fac2a-391">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="fac2a-392">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fac2a-392">Configure additional options</span></span>

<span data-ttu-id="fac2a-393">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="fac2a-393">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="fac2a-394">.NET</span><span class="sxs-lookup"><span data-stu-id="fac2a-394">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="fac2a-395">Option .NET</span><span class="sxs-lookup"><span data-stu-id="fac2a-395">.NET Option</span></span> |  <span data-ttu-id="fac2a-396">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-396">Default value</span></span> | <span data-ttu-id="fac2a-397">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-397">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="fac2a-398">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-398">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="fac2a-399">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="fac2a-399">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fac2a-400">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="fac2a-400">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fac2a-401">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="fac2a-401">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="fac2a-402">Empty</span><span class="sxs-lookup"><span data-stu-id="fac2a-402">Empty</span></span> | <span data-ttu-id="fac2a-403">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="fac2a-403">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="fac2a-404">Empty</span><span class="sxs-lookup"><span data-stu-id="fac2a-404">Empty</span></span> | <span data-ttu-id="fac2a-405">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-405">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="fac2a-406">Empty</span><span class="sxs-lookup"><span data-stu-id="fac2a-406">Empty</span></span> | <span data-ttu-id="fac2a-407">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-407">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="fac2a-408">5 secondes</span><span class="sxs-lookup"><span data-stu-id="fac2a-408">5 seconds</span></span> | <span data-ttu-id="fac2a-409">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="fac2a-409">WebSockets only.</span></span> <span data-ttu-id="fac2a-410">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="fac2a-410">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="fac2a-411">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="fac2a-411">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="fac2a-412">Empty</span><span class="sxs-lookup"><span data-stu-id="fac2a-412">Empty</span></span> | <span data-ttu-id="fac2a-413">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-413">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="fac2a-414">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-414">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="fac2a-415">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fac2a-415">Not used for WebSocket connections.</span></span> <span data-ttu-id="fac2a-416">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="fac2a-416">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="fac2a-417">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="fac2a-417">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="fac2a-418">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="fac2a-418">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="fac2a-419">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-419">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="fac2a-420">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fac2a-420">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="fac2a-421">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="fac2a-421">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="fac2a-422">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="fac2a-422">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="fac2a-423">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="fac2a-423">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="fac2a-424">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fac2a-424">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="fac2a-425">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="fac2a-425">JavaScript Option</span></span> | <span data-ttu-id="fac2a-426">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-426">Default Value</span></span> | <span data-ttu-id="fac2a-427">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-427">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="fac2a-428">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-428">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="fac2a-429">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="fac2a-429">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fac2a-430">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="fac2a-430">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fac2a-431">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="fac2a-431">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="fac2a-432">Java</span><span class="sxs-lookup"><span data-stu-id="fac2a-432">Java</span></span>](#tab/java)

| <span data-ttu-id="fac2a-433">Option Java</span><span class="sxs-lookup"><span data-stu-id="fac2a-433">Java Option</span></span> | <span data-ttu-id="fac2a-434">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="fac2a-434">Default Value</span></span> | <span data-ttu-id="fac2a-435">Description</span><span class="sxs-lookup"><span data-stu-id="fac2a-435">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="fac2a-436">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-436">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="fac2a-437">Définissez cette valeur sur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="fac2a-437">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fac2a-438">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="fac2a-438">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fac2a-439">Ce paramètre ne peut pas être activé lors de l’utilisation du service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="fac2a-439">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="fac2a-440">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="fac2a-440">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="fac2a-441">Empty</span><span class="sxs-lookup"><span data-stu-id="fac2a-441">Empty</span></span> | <span data-ttu-id="fac2a-442">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="fac2a-442">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="fac2a-443">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-443">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="fac2a-444">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fac2a-444">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="fac2a-445">Dans le client Java, ces options peuvent être configurées avec les méthodes sur la `HttpHubConnectionBuilder` retournée à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="fac2a-445">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="fac2a-446">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fac2a-446">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
