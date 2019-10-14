---
title: Configuration de Signalr ASP.NET Core
author: bradygaster
description: Découvrez comment configurer des applications ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246493"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="0c4ae-103">Configuration de Signalr ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c4ae-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="0c4ae-104">Options de sérialisation JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="0c4ae-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="0c4ae-105">ASP.NET Core Signalr prend en charge deux protocoles pour l’encodage des messages : [JSON](https://www.json.org/) et [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="0c4ae-106">Chaque protocole possède des options de configuration de la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c4ae-107">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="0c4ae-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="0c4ae-108">`AddJsonProtocol` peut être ajouté après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0c4ae-109">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="0c4ae-110">La propriété [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) sur cet objet est un objet `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="0c4ae-111">Pour plus d’informations, consultez la [Documentation System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="0c4ae-112">Par exemple, pour configurer le sérialiseur de manière à ce qu’il ne change pas la casse des noms de propriétés, au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="0c4ae-113">Dans le client .NET, la même méthode d’extension `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="0c4ae-114">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="0c4ae-115">Basculer vers Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="0c4ae-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="0c4ae-116">Si vous avez besoin de fonctionnalités de `Newtonsoft.Json` qui ne sont pas prises en charge dans `System.Text.Json`, consultez [basculer vers Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="0c4ae-117">La sérialisation JSON peut être configurée sur le serveur à l’aide de la méthode d’extension [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol), qui peut être ajoutée après [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="0c4ae-118">La méthode `AddJsonProtocol` prend un délégué qui reçoit un objet `options`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="0c4ae-119">La propriété [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) sur cet objet est un objet JSON.net `JsonSerializerSettings` qui peut être utilisé pour configurer la sérialisation des arguments et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="0c4ae-120">Pour plus d’informations, consultez la [documentation JSON.net](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="0c4ae-121">Par exemple, pour configurer le sérialiseur afin d’utiliser les noms de propriété « casse Pascal », au lieu des noms « la casse mixte » par défaut, utilisez le code suivant dans `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="0c4ae-122">Dans le client .NET, la même méthode d’extension `AddJsonProtocol` existe sur [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="0c4ae-123">L’espace de noms `Microsoft.Extensions.DependencyInjection` doit être importé pour résoudre la méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0c4ae-124">Il n’est pas possible de configurer la sérialisation JSON dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="0c4ae-125">Options de sérialisation MessagePack</span><span class="sxs-lookup"><span data-stu-id="0c4ae-125">MessagePack serialization options</span></span>

<span data-ttu-id="0c4ae-126">La sérialisation MessagePack peut être configurée en fournissant un délégué à l’appel [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="0c4ae-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="0c4ae-127">Pour plus d’informations, consultez [MessagePack dans signalr](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="0c4ae-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="0c4ae-128">Il n’est pas possible de configurer la sérialisation MessagePack dans le client JavaScript pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="0c4ae-129">Configurer les options de serveur</span><span class="sxs-lookup"><span data-stu-id="0c4ae-129">Configure server options</span></span>

<span data-ttu-id="0c4ae-130">Le tableau suivant décrit les options de configuration des hubs Signalr :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="0c4ae-131">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-131">Option</span></span> | <span data-ttu-id="0c4ae-132">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-132">Default Value</span></span> | <span data-ttu-id="0c4ae-133">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="0c4ae-134">30 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-134">30 seconds</span></span> | <span data-ttu-id="0c4ae-135">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="0c4ae-136">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="0c4ae-137">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="0c4ae-138">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-138">15 seconds</span></span> | <span data-ttu-id="0c4ae-139">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="0c4ae-140">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-141">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="0c4ae-142">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-142">15 seconds</span></span> | <span data-ttu-id="0c4ae-143">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="0c4ae-144">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="0c4ae-145">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="0c4ae-146">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="0c4ae-146">All installed protocols</span></span> | <span data-ttu-id="0c4ae-147">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-147">Protocols supported by this hub.</span></span> <span data-ttu-id="0c4ae-148">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="0c4ae-149">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="0c4ae-150">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="0c4ae-151">Nombre maximal d’éléments pouvant être mis en mémoire tampon pour les flux de téléchargement du client.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="0c4ae-152">Si cette limite est atteinte, le traitement des appels est bloqué jusqu’à ce que le serveur traite les éléments de flux.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="0c4ae-153">32 Ko</span><span class="sxs-lookup"><span data-stu-id="0c4ae-153">32 KB</span></span> | <span data-ttu-id="0c4ae-154">Taille maximale d’un seul message de concentrateur entrant.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="0c4ae-155">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-155">Option</span></span> | <span data-ttu-id="0c4ae-156">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-156">Default Value</span></span> | <span data-ttu-id="0c4ae-157">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="0c4ae-158">30 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-158">30 seconds</span></span> | <span data-ttu-id="0c4ae-159">Le serveur considère que le client est déconnecté s’il n’a pas reçu de message (y compris Keep-Alive) dans cet intervalle.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="0c4ae-160">Cela peut prendre plus de temps que cet intervalle de délai d’attente pour que le client soit marqué comme déconnecté, en raison de la façon dont il est implémenté.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="0c4ae-161">La valeur recommandée est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="0c4ae-162">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-162">15 seconds</span></span> | <span data-ttu-id="0c4ae-163">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="0c4ae-164">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-165">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="0c4ae-166">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-166">15 seconds</span></span> | <span data-ttu-id="0c4ae-167">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="0c4ae-168">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="0c4ae-169">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="0c4ae-170">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="0c4ae-170">All installed protocols</span></span> | <span data-ttu-id="0c4ae-171">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-171">Protocols supported by this hub.</span></span> <span data-ttu-id="0c4ae-172">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="0c4ae-173">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="0c4ae-174">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="0c4ae-175">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-175">Option</span></span> | <span data-ttu-id="0c4ae-176">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-176">Default Value</span></span> | <span data-ttu-id="0c4ae-177">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="0c4ae-178">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-178">15 seconds</span></span> | <span data-ttu-id="0c4ae-179">Si le client n’envoie pas de message d’établissement de liaison initial dans le cadre de cet intervalle de temps, la connexion est fermée.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="0c4ae-180">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-181">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="0c4ae-182">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-182">15 seconds</span></span> | <span data-ttu-id="0c4ae-183">Si le serveur n’a pas envoyé un message dans cet intervalle, un message ping est envoyé automatiquement pour maintenir la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="0c4ae-184">Lorsque vous modifiez `KeepAliveInterval`, modifiez le paramètre `ServerTimeout` / `serverTimeoutInMilliseconds` sur le client.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="0c4ae-185">La valeur recommandée `ServerTimeout` / `serverTimeoutInMilliseconds` est le double de la valeur `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="0c4ae-186">Tous les protocoles installés</span><span class="sxs-lookup"><span data-stu-id="0c4ae-186">All installed protocols</span></span> | <span data-ttu-id="0c4ae-187">Les protocoles pris en charge par ce hub.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-187">Protocols supported by this hub.</span></span> <span data-ttu-id="0c4ae-188">Par défaut, tous les protocoles inscrits sur le serveur sont autorisés, mais les protocoles peuvent être supprimés de cette liste pour désactiver des protocoles spécifiques pour certains hubs.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="0c4ae-189">Si `true`, les messages d’exception détaillés sont retournés aux clients lorsqu’une exception est levée dans une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="0c4ae-190">La valeur par défaut est `false`, car ces messages d’exception peuvent contenir des informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="0c4ae-191">Les options peuvent être configurées pour tous les hubs en fournissant un délégué d’options à l'appel `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="0c4ae-192">Les options d'un Hub spécifique remplacent les options globales fournies dans la méthode `AddSignalR` et peuvent être configurées à l’aide de : <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="0c4ae-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="0c4ae-193">Options de configuration HTTP avancées</span><span class="sxs-lookup"><span data-stu-id="0c4ae-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c4ae-194">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="0c4ae-195">Ces options sont configurées en passant un délégué à [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="0c4ae-196">Utilisez `HttpConnectionDispatcherOptions` pour configurer des paramètres avancés relatifs aux transports et à la gestion des tampons de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="0c4ae-197">Ces options sont configurées en passant un délégué à [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="0c4ae-198">Le tableau suivant décrit les options de configuration des options HTTP avancées de ASP.NET Core Signalr :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="0c4ae-199">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-199">Option</span></span> | <span data-ttu-id="0c4ae-200">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-200">Default Value</span></span> | <span data-ttu-id="0c4ae-201">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="0c4ae-202">32 Ko</span><span class="sxs-lookup"><span data-stu-id="0c4ae-202">32 KB</span></span> | <span data-ttu-id="0c4ae-203">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon avant d’appliquer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="0c4ae-204">L’augmentation de cette valeur permet au serveur de recevoir plus rapidement des messages plus volumineux sans appliquer la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="0c4ae-205">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="0c4ae-206">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="0c4ae-207">32 Ko</span><span class="sxs-lookup"><span data-stu-id="0c4ae-207">32 KB</span></span> | <span data-ttu-id="0c4ae-208">Nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon avant d’observer la contre-pression.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="0c4ae-209">L’augmentation de cette valeur permet au serveur de mettre plus rapidement en mémoire tampon des messages plus volumineux sans attendre la contre-pression, mais peut augmenter la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="0c4ae-210">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-210">All Transports are enabled.</span></span> | <span data-ttu-id="0c4ae-211">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="0c4ae-212">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-212">See below.</span></span> | <span data-ttu-id="0c4ae-213">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="0c4ae-214">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-214">See below.</span></span> | <span data-ttu-id="0c4ae-215">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="0c4ae-216">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-216">Option</span></span> | <span data-ttu-id="0c4ae-217">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-217">Default Value</span></span> | <span data-ttu-id="0c4ae-218">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="0c4ae-219">32 Ko</span><span class="sxs-lookup"><span data-stu-id="0c4ae-219">32 KB</span></span> | <span data-ttu-id="0c4ae-220">Nombre maximal d’octets reçus du client que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="0c4ae-221">L’augmentation de cette valeur permet au serveur de recevoir des messages plus volumineux, mais peut avoir un impact négatif sur la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="0c4ae-222">Les données sont collectées automatiquement à partir des attributs `Authorize` appliqués à la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="0c4ae-223">Liste d’objets [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) utilisés pour déterminer si un client est autorisé à se connecter au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="0c4ae-224">32 Ko</span><span class="sxs-lookup"><span data-stu-id="0c4ae-224">32 KB</span></span> | <span data-ttu-id="0c4ae-225">Le nombre maximal d’octets envoyés par l’application que le serveur met en mémoire tampon.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="0c4ae-226">L'augmentation de cette valeur permet au serveur d’envoyer des messages plus volumineux, mais peut affecter négativement la consommation de mémoire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="0c4ae-227">Tous les transports sont activés.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-227">All Transports are enabled.</span></span> | <span data-ttu-id="0c4ae-228">Énumération d’indicateurs de bits des valeurs `HttpTransportType` qui peuvent restreindre les transports qu’un client peut utiliser pour se connecter.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="0c4ae-229">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-229">See below.</span></span> | <span data-ttu-id="0c4ae-230">Options supplémentaires spécifiques au transport d’interrogation longue.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="0c4ae-231">Voir ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-231">See below.</span></span> | <span data-ttu-id="0c4ae-232">Options supplémentaires spécifiques au transport WebSockets.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="0c4ae-233">Le transport d’interrogation longue propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `LongPolling` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="0c4ae-234">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-234">Option</span></span> | <span data-ttu-id="0c4ae-235">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-235">Default Value</span></span> | <span data-ttu-id="0c4ae-236">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="0c4ae-237">90 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-237">90 seconds</span></span> | <span data-ttu-id="0c4ae-238">Durée maximale pendant laquelle le serveur attend qu’un message soit envoyé au client avant de mettre fin à une requête d’interrogation unique.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="0c4ae-239">Si vous réduisez cette valeur, le client émet de nouvelles demandes d’interrogation plus fréquemment.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="0c4ae-240">Le transport WebSocket propose des options supplémentaires qui peuvent être configurées à l’aide de la propriété `WebSockets` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="0c4ae-241">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-241">Option</span></span> | <span data-ttu-id="0c4ae-242">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-242">Default Value</span></span> | <span data-ttu-id="0c4ae-243">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="0c4ae-244">5 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-244">5 seconds</span></span> | <span data-ttu-id="0c4ae-245">Une fois le serveur fermé, si le client ne parvient pas à se fermer dans cet intervalle de temps, la connexion est interrompue.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="0c4ae-246">Un délégué qui peut être utilisé pour définir l'en-tête `Sec-WebSocket-Protocol` sur une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="0c4ae-247">Le délégué reçoit les valeurs demandées par le client en entrée et doit retourner la valeur souhaitée.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="0c4ae-248">Configurer les options du client</span><span class="sxs-lookup"><span data-stu-id="0c4ae-248">Configure client options</span></span>

<span data-ttu-id="0c4ae-249">Les options du client peuvent être configurées sur le type `HubConnectionBuilder` (disponible dans les clients .NET et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="0c4ae-250">Il est également disponible dans le client Java, mais la sous-classe `HttpHubConnectionBuilder` est ce qui contient les options de configuration du générateur, ainsi que sur le `HubConnection` lui-même.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="0c4ae-251">Configuration de la journalisation</span><span class="sxs-lookup"><span data-stu-id="0c4ae-251">Configure logging</span></span>

<span data-ttu-id="0c4ae-252">La journalisation est configurée dans le client .NET à l’aide de la méthode `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="0c4ae-253">Les fournisseurs de journalisation et les filtres peuvent être enregistrés de la même façon que sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="0c4ae-254">Pour plus d’informations, consultez la documentation relative [à la journalisation dans ASP.net Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="0c4ae-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="0c4ae-255">Pour pouvoir inscrire des fournisseurs de journalisation, vous devez installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="0c4ae-256">Consultez la section [fournisseurs de journalisation intégrés](xref:fundamentals/logging/index#built-in-logging-providers) de la documentation pour obtenir la liste complète.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="0c4ae-257">Par exemple, pour activer la journalisation de la Console, installez le package NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="0c4ae-258">Appelez la méthode d’extension `AddConsole` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="0c4ae-259">Dans le client JavaScript, il existe une méthode `configureLogging` similaire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="0c4ae-260">Fournissez une valeur `LogLevel` indiquant le niveau minimal de messages du journal à produire.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="0c4ae-261">Les journaux sont écrits dans la fenêtre de la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c4ae-262">Au lieu d’une valeur `LogLevel`, vous pouvez également fournir une valeur `string` représentant un nom de niveau de journal.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="0c4ae-263">Cela est utile lors de la configuration de la journalisation Signalr dans les environnements où vous n’avez pas accès aux constantes `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="0c4ae-264">Le tableau suivant répertorie les niveaux de journalisation disponibles.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-264">The following table lists the available log levels.</span></span> <span data-ttu-id="0c4ae-265">La valeur que vous fournissez à `configureLogging` définit le niveau de journalisation **minimale** qui sera enregistré.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="0c4ae-266">Les messages enregistrés à ce niveau, **ou les niveaux indiqués après celui-ci dans la table**, sont enregistrés.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="0c4ae-267">String</span><span class="sxs-lookup"><span data-stu-id="0c4ae-267">String</span></span>                      | <span data-ttu-id="0c4ae-268">LogLevel</span><span class="sxs-lookup"><span data-stu-id="0c4ae-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="0c4ae-269">`info` **ou** `information`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="0c4ae-270">`warn` **ou** `warning`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0c4ae-271">Pour désactiver entièrement la journalisation, spécifiez `signalR.LogLevel.None` dans la méthode `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="0c4ae-272">Pour plus d’informations sur la journalisation, consultez la [documentation relative aux diagnostics de signalr](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="0c4ae-273">Le client Java Signalr utilise la bibliothèque [SLF4J](https://www.slf4j.org/) pour la journalisation.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="0c4ae-274">Il s’agit d’une API de journalisation de haut niveau qui permet aux utilisateurs de la bibliothèque de choisir leur propre implémentation de journalisation spécifique en introduisant une dépendance de journalisation spécifique.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="0c4ae-275">L’extrait de code suivant montre comment utiliser `java.util.logging` avec le client Java Signalr.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="0c4ae-276">Si vous ne configurez pas la journalisation dans vos dépendances, SLF4J charge un journal de non-opération par défaut avec le message d’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="0c4ae-277">Cela peut être ignoré en toute sécurité.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="0c4ae-278">Configurer les transports autorisés</span><span class="sxs-lookup"><span data-stu-id="0c4ae-278">Configure allowed transports</span></span>

<span data-ttu-id="0c4ae-279">Les transports utilisés par Signalr peuvent être configurés dans l’appel `WithUrl` (`withUrl` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="0c4ae-280">Une opération or au niveau du bit des valeurs de `HttpTransportType` peut être utilisée pour restreindre le client à utiliser uniquement les transports spécifiés.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="0c4ae-281">Tous les transports sont activés par défaut.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-281">All transports are enabled by default.</span></span>

<span data-ttu-id="0c4ae-282">Par exemple, pour désactiver le transport des événements envoyés par le serveur, mais autoriser les connexions WebSocket et d’interrogation longue :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="0c4ae-283">Dans le client JavaScript, les transports sont configurés en définissant le champ `transport` sur l’objet d’options fourni à `withUrl` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0c4ae-284">Dans cette version du client Java, WebSockets est le seul transport disponible.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="0c4ae-285">Dans le client Java, le transport est sélectionné avec la méthode `withTransport` sur la `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="0c4ae-286">Le client Java utilise le transport WebSocket par défaut.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="0c4ae-287">Le client Java Signalr ne prend pas encore en charge le transport de secours.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="0c4ae-288">Configurer l’authentification du porteur</span><span class="sxs-lookup"><span data-stu-id="0c4ae-288">Configure bearer authentication</span></span>

<span data-ttu-id="0c4ae-289">Pour fournir des données d’authentification avec les demandes Signalr, utilisez l’option `AccessTokenProvider` (`accessTokenFactory` dans JavaScript) pour spécifier une fonction qui retourne le jeton d’accès souhaité.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="0c4ae-290">Dans le client .NET, ce jeton d’accès est transmis en tant que jeton « authentification du porteur » HTTP (à l’aide de l’en-tête `Authorization` avec un type de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="0c4ae-291">Dans le client JavaScript, le jeton d’accès est utilisé comme jeton du porteur, **sauf** dans certains cas où les API de navigateur restreignent la possibilité d’appliquer des en-têtes (en particulier dans les demandes d’événements envoyés par le serveur et WebSocket).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="0c4ae-292">Dans ce cas, le jeton d’accès est fourni sous la forme d’une valeur de chaîne de requête `access_token`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="0c4ae-293">Dans le client .NET, l'option `AccessTokenProvider` peut être spécifiée à l’aide du délégué d’options de `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="0c4ae-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="0c4ae-294">Dans le client JavaScript, le jeton d’accès est configuré en définissant le champ `accessTokenFactory` sur l’objet d’options dans `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="0c4ae-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="0c4ae-295">Dans le client Java Signalr, vous pouvez configurer un jeton de porteur à utiliser pour l’authentification en fournissant une fabrique de jetons d’accès au [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="0c4ae-296">Utilisez [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) pour fournir une [chaîne](https://github.com/ReactiveX/RxJava) [unique\<RxJava>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="0c4ae-297">Avec un appel à [Single. Defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), vous pouvez écrire une logique pour produire des jetons d’accès pour votre client.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="0c4ae-298">Configurer les options de délai d’attente et de conservation des connexions</span><span class="sxs-lookup"><span data-stu-id="0c4ae-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="0c4ae-299">Des options supplémentaires pour configurer le comportement de conservation et de délai d’expiration sont disponibles sur l'objet `HubConnection` proprement dit :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="0c4ae-300">.NET</span><span class="sxs-lookup"><span data-stu-id="0c4ae-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="0c4ae-301">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-301">Option</span></span> | <span data-ttu-id="0c4ae-302">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-302">Default value</span></span> | <span data-ttu-id="0c4ae-303">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="0c4ae-304">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-305">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-305">Timeout for server activity.</span></span> <span data-ttu-id="0c4ae-306">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="0c4ae-307">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="0c4ae-308">La valeur recommandée est un nombre au moins doubler la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="0c4ae-309">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-309">15 seconds</span></span> | <span data-ttu-id="0c4ae-310">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="0c4ae-311">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="0c4ae-312">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-313">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="0c4ae-314">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-314">15 seconds</span></span> | <span data-ttu-id="0c4ae-315">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="0c4ae-316">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="0c4ae-317">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="0c4ae-318">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0c4ae-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0c4ae-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="0c4ae-320">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-320">Option</span></span> | <span data-ttu-id="0c4ae-321">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-321">Default value</span></span> | <span data-ttu-id="0c4ae-322">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="0c4ae-323">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-324">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-324">Timeout for server activity.</span></span> <span data-ttu-id="0c4ae-325">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="0c4ae-326">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="0c4ae-327">La valeur recommandée est un nombre au moins doubler la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="0c4ae-328">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="0c4ae-329">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="0c4ae-330">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="0c4ae-331">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="0c4ae-332">Java</span><span class="sxs-lookup"><span data-stu-id="0c4ae-332">Java</span></span>](#tab/java)

| <span data-ttu-id="0c4ae-333">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-333">Option</span></span> | <span data-ttu-id="0c4ae-334">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-334">Default value</span></span> | <span data-ttu-id="0c4ae-335">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="0c4ae-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="0c4ae-337">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-338">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-338">Timeout for server activity.</span></span> <span data-ttu-id="0c4ae-339">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="0c4ae-340">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="0c4ae-341">La valeur recommandée est un nombre au moins doubler la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="0c4ae-342">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-342">15 seconds</span></span> | <span data-ttu-id="0c4ae-343">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="0c4ae-344">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="0c4ae-345">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-346">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="0c4ae-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="0c4ae-348">15 secondes (15 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-349">Détermine l’intervalle auquel le client envoie des messages ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="0c4ae-350">L’envoi d’un message à partir du client réinitialise le minuteur au début de l’intervalle.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="0c4ae-351">Si le client n’a pas envoyé de message dans le `ClientTimeoutInterval` défini sur le serveur, le serveur considère que le client est déconnecté.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="0c4ae-352">.NET</span><span class="sxs-lookup"><span data-stu-id="0c4ae-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="0c4ae-353">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-353">Option</span></span> | <span data-ttu-id="0c4ae-354">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-354">Default value</span></span> | <span data-ttu-id="0c4ae-355">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="0c4ae-356">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-357">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-357">Timeout for server activity.</span></span> <span data-ttu-id="0c4ae-358">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="0c4ae-359">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="0c4ae-360">La valeur recommandée est un nombre au moins doubler la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="0c4ae-361">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-361">15 seconds</span></span> | <span data-ttu-id="0c4ae-362">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="0c4ae-363">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `Closed` (`onclose` en JavaScript).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="0c4ae-364">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-365">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="0c4ae-366">Dans le Client .NET, les valeurs de délai d’attente sont spécifiées en tant que valeurs `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0c4ae-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0c4ae-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="0c4ae-368">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-368">Option</span></span> | <span data-ttu-id="0c4ae-369">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-369">Default value</span></span> | <span data-ttu-id="0c4ae-370">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="0c4ae-371">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-372">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-372">Timeout for server activity.</span></span> <span data-ttu-id="0c4ae-373">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onclose`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="0c4ae-374">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="0c4ae-375">La valeur recommandée est un nombre au moins doubler la valeur `KeepAliveInterval` du serveur pour permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="0c4ae-376">Java</span><span class="sxs-lookup"><span data-stu-id="0c4ae-376">Java</span></span>](#tab/java)

| <span data-ttu-id="0c4ae-377">Option</span><span class="sxs-lookup"><span data-stu-id="0c4ae-377">Option</span></span> | <span data-ttu-id="0c4ae-378">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-378">Default value</span></span> | <span data-ttu-id="0c4ae-379">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="0c4ae-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="0c4ae-381">30 secondes (30 000 millisecondes)</span><span class="sxs-lookup"><span data-stu-id="0c4ae-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="0c4ae-382">Délai d’expiration de l’activité du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-382">Timeout for server activity.</span></span> <span data-ttu-id="0c4ae-383">Si le serveur n’a pas envoyé de message dans cet intervalle, le client considère que le serveur est déconnecté et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="0c4ae-384">Cette valeur doit être suffisamment grande pour qu’un message ping soit envoyé à partir du serveur **et** reçu par le client dans l’intervalle de délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="0c4ae-385">La valeur recommandée est un nombre au moins deux fois la valeur `KeepAliveInterval` du serveur, afin de permettre l’arrivée des commandes ping.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="0c4ae-386">15 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-386">15 seconds</span></span> | <span data-ttu-id="0c4ae-387">Délai d’attente du protocole de transfert initial du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="0c4ae-388">Si le serveur n’envoie pas de réponse de négociation dans cet intervalle, le client annule le protocole de transfert et déclenche l’événement `onClose`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="0c4ae-389">Il s’agit d’un paramètre avancé qui ne doit être modifié que si des erreurs de dépassement de délai d’expiration de la liaison se produisent en raison d’une latence importante du réseau.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="0c4ae-390">Pour plus d’informations sur le processus d’établissement de liaison, consultez la [spécification du protocole signalr Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="0c4ae-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="0c4ae-391">Configurer des options supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0c4ae-391">Configure additional options</span></span>

<span data-ttu-id="0c4ae-392">Des options supplémentaires peuvent être configurées dans la méthode `WithUrl` (`withUrl` en JavaScript) sur `HubConnectionBuilder` ou sur les différentes API de configuration sur le `HttpHubConnectionBuilder` dans le client Java :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="0c4ae-393">.NET</span><span class="sxs-lookup"><span data-stu-id="0c4ae-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="0c4ae-394">Option .NET</span><span class="sxs-lookup"><span data-stu-id="0c4ae-394">.NET Option</span></span> |  <span data-ttu-id="0c4ae-395">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-395">Default value</span></span> | <span data-ttu-id="0c4ae-396">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="0c4ae-397">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="0c4ae-398">Affectez-lui la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="0c4ae-399">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="0c4ae-400">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="0c4ae-401">Empty</span><span class="sxs-lookup"><span data-stu-id="0c4ae-401">Empty</span></span> | <span data-ttu-id="0c4ae-402">Collection de certificats TLS à envoyer pour authentifier les demandes.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="0c4ae-403">Empty</span><span class="sxs-lookup"><span data-stu-id="0c4ae-403">Empty</span></span> | <span data-ttu-id="0c4ae-404">Collection de cookies HTTP à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="0c4ae-405">Empty</span><span class="sxs-lookup"><span data-stu-id="0c4ae-405">Empty</span></span> | <span data-ttu-id="0c4ae-406">Informations d’identification à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="0c4ae-407">5 secondes</span><span class="sxs-lookup"><span data-stu-id="0c4ae-407">5 seconds</span></span> | <span data-ttu-id="0c4ae-408">WebSocket uniquement.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-408">WebSockets only.</span></span> <span data-ttu-id="0c4ae-409">Durée d’attente maximale du client après la fermeture du serveur pour accuser réception de la demande de fermeture.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="0c4ae-410">Si le serveur n’accuse pas réception de la fermeture dans ce délai, le client se déconnecte.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="0c4ae-411">Empty</span><span class="sxs-lookup"><span data-stu-id="0c4ae-411">Empty</span></span> | <span data-ttu-id="0c4ae-412">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="0c4ae-413">Délégué qui peut être utilisé pour configurer ou remplacer le `HttpMessageHandler` utilisé pour envoyer des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="0c4ae-414">Non utilisé pour les connexions WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="0c4ae-415">Ce délégué doit retourner une valeur non null et il reçoit la valeur par défaut en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="0c4ae-416">Modifiez les paramètres de cette valeur par défaut et renvoyez-la, ou retournez une nouvelle instance `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="0c4ae-417">**Lors du remplacement du gestionnaire, veillez à copier les paramètres que vous souhaitez conserver à partir du gestionnaire fourni. dans le cas contraire, les options configurées (telles que les cookies et les en-têtes) ne s’appliqueront pas au nouveau gestionnaire.**</span><span class="sxs-lookup"><span data-stu-id="0c4ae-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="0c4ae-418">Proxy HTTP à utiliser lors de l’envoi de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="0c4ae-419">Définissez cette valeur booléenne pour envoyer les informations d’identification par défaut pour les requêtes HTTP et WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="0c4ae-420">Cela permet l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="0c4ae-421">Délégué qui peut être utilisé pour configurer des options WebSocket supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="0c4ae-422">Reçoit une instance de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) qui peut être utilisée pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="0c4ae-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0c4ae-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="0c4ae-424">Option JavaScript</span><span class="sxs-lookup"><span data-stu-id="0c4ae-424">JavaScript Option</span></span> | <span data-ttu-id="0c4ae-425">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-425">Default Value</span></span> | <span data-ttu-id="0c4ae-426">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="0c4ae-427">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="0c4ae-428">Affectez-lui la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="0c4ae-429">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="0c4ae-430">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="0c4ae-431">Java</span><span class="sxs-lookup"><span data-stu-id="0c4ae-431">Java</span></span>](#tab/java)

| <span data-ttu-id="0c4ae-432">Option Java</span><span class="sxs-lookup"><span data-stu-id="0c4ae-432">Java Option</span></span> | <span data-ttu-id="0c4ae-433">Valeur par défaut</span><span class="sxs-lookup"><span data-stu-id="0c4ae-433">Default Value</span></span> | <span data-ttu-id="0c4ae-434">Description</span><span class="sxs-lookup"><span data-stu-id="0c4ae-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="0c4ae-435">Fonction qui retourne une chaîne fournie en tant que jeton d’authentification du porteur dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="0c4ae-436">Affectez-lui la valeur `true` pour ignorer l’étape de négociation.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="0c4ae-437">**Pris en charge uniquement lorsque le transport WebSocket est le seul transport activé**.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="0c4ae-438">Ce paramètre ne peut pas être activé en cas d’utilisation du Service Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="0c4ae-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="0c4ae-440">Empty</span><span class="sxs-lookup"><span data-stu-id="0c4ae-440">Empty</span></span> | <span data-ttu-id="0c4ae-441">Mappage d’en-têtes HTTP supplémentaires à envoyer avec chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c4ae-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="0c4ae-442">Dans le client .NET, ces options peuvent être modifiées par le délégué d’options fourni à `WithUrl` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="0c4ae-443">Dans le client JavaScript, ces options peuvent être fournies dans un objet JavaScript fourni à `withUrl` :</span><span class="sxs-lookup"><span data-stu-id="0c4ae-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="0c4ae-444">Dans le client Java, ces options peuvent être configurées avec les méthodes sur le `HttpHubConnectionBuilder` retourné à partir de la `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="0c4ae-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="0c4ae-445">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0c4ae-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
