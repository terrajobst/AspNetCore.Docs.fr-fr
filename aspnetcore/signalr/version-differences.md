---
title: Différences entre Signalr et ASP.NET Core Signalr
author: bradygaster
description: Différences entre Signalr et ASP.NET Core Signalr
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746535"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="ba89e-103">Différences entre ASP.NET Signalr et ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="ba89e-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="ba89e-104">ASP.NET Core Signalr n’est pas compatible avec les clients ou les serveurs pour ASP.NET Signalr.</span><span class="sxs-lookup"><span data-stu-id="ba89e-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="ba89e-105">Cet article détaille les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core Signalr.</span><span class="sxs-lookup"><span data-stu-id="ba89e-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="ba89e-106">Comment identifier la version de Signalr</span><span class="sxs-lookup"><span data-stu-id="ba89e-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="ba89e-107">SignalR ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba89e-107">ASP.NET SignalR</span></span> | <span data-ttu-id="ba89e-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ba89e-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="ba89e-109">Package NuGet serveur</span><span class="sxs-lookup"><span data-stu-id="ba89e-109">Server NuGet Package</span></span> | [<span data-ttu-id="ba89e-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ba89e-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="ba89e-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.net Core)</span><span class="sxs-lookup"><span data-stu-id="ba89e-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="ba89e-112">[Microsoft. AspNetCore. signalr](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="ba89e-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="ba89e-113">Packages NuGet client</span><span class="sxs-lookup"><span data-stu-id="ba89e-113">Client NuGet Packages</span></span> | [<span data-ttu-id="ba89e-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="ba89e-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="ba89e-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="ba89e-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="ba89e-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="ba89e-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="ba89e-117">Package NPM client</span><span class="sxs-lookup"><span data-stu-id="ba89e-117">Client npm Package</span></span> | [<span data-ttu-id="ba89e-118">signalr</span><span class="sxs-lookup"><span data-stu-id="ba89e-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="ba89e-119">Client Java</span><span class="sxs-lookup"><span data-stu-id="ba89e-119">Java Client</span></span> | <span data-ttu-id="ba89e-120">[Dépôt github](https://github.com/SignalR/java-client) déconseillé</span><span class="sxs-lookup"><span data-stu-id="ba89e-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="ba89e-121">Package Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="ba89e-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="ba89e-122">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="ba89e-122">Server App Type</span></span> | <span data-ttu-id="ba89e-123">ASP.NET (System. Web) ou auto-hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="ba89e-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="ba89e-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba89e-124">ASP.NET Core</span></span> |
| <span data-ttu-id="ba89e-125">Plateformes serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="ba89e-125">Supported Server Platforms</span></span> | <span data-ttu-id="ba89e-126">.NET Framework 4,5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ba89e-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="ba89e-127">.NET Framework 4.6.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="ba89e-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="ba89e-128">.NET Core 2,1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ba89e-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="ba89e-129">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="ba89e-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="ba89e-130">Reconnexions automatiques</span><span class="sxs-lookup"><span data-stu-id="ba89e-130">Automatic reconnects</span></span>

<span data-ttu-id="ba89e-131">Les reconnexions automatiques ne sont pas prises en charge dans ASP.NET Core Signalr.</span><span class="sxs-lookup"><span data-stu-id="ba89e-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="ba89e-132">Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’il souhaite se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="ba89e-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="ba89e-133">Dans ASP.NET Signalr, signaler tente de se reconnecter au serveur si la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="ba89e-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="ba89e-134">Prise en charge de protocole</span><span class="sxs-lookup"><span data-stu-id="ba89e-134">Protocol support</span></span>

<span data-ttu-id="ba89e-135">ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="ba89e-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="ba89e-136">En outre, des protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="ba89e-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="ba89e-137">Transports</span><span class="sxs-lookup"><span data-stu-id="ba89e-137">Transports</span></span>

<span data-ttu-id="ba89e-138">Le transport de trame éternel n’est pas pris en charge dans ASP.NET Core Signalr.</span><span class="sxs-lookup"><span data-stu-id="ba89e-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="ba89e-139">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="ba89e-139">Differences on the server</span></span>

<span data-ttu-id="ba89e-140">Les bibliothèques côté serveur ASP.NET Core SignalR sont incluses dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) qui fait partie du modèle de l'**Application Web ASP.NET Core** pour les projets Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="ba89e-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="ba89e-141">ASP.NET Core Signalr est un intergiciel ASP.NET Core. il doit donc être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ba89e-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ba89e-142">Pour configurer le routage, mappez les itinéraires aux hubs à l’intérieur de l’appel de la méthode [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ba89e-142">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ba89e-143">Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de l'appel à la méthode [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ba89e-143">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="ba89e-144">Sessions rémanentes</span><span class="sxs-lookup"><span data-stu-id="ba89e-144">Sticky sessions</span></span>

<span data-ttu-id="ba89e-145">Le modèle ScaleOut pour ASP.NET Signalr permet aux clients de se reconnecter et d’envoyer des messages à n’importe quel serveur de la batterie.</span><span class="sxs-lookup"><span data-stu-id="ba89e-145">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="ba89e-146">Dans ASP.NET Core Signalr, le client doit interagir avec le même serveur pendant la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="ba89e-146">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="ba89e-147">Pour les ScaleOut utilisant des ReDim, cela signifie que les sessions rémanentes sont requises.</span><span class="sxs-lookup"><span data-stu-id="ba89e-147">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="ba89e-148">Pour ScaleOut à l’aide du [service Azure signalr](/azure/azure-signalr/), les sessions rémanentes ne sont pas nécessaires, car le service gère les connexions aux clients.</span><span class="sxs-lookup"><span data-stu-id="ba89e-148">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="ba89e-149">Concentrateur unique par connexion</span><span class="sxs-lookup"><span data-stu-id="ba89e-149">Single hub per connection</span></span>

<span data-ttu-id="ba89e-150">Dans ASP.NET Core Signalr, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="ba89e-150">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="ba89e-151">Les connexions sont apportées directement à un hun unique, plutôt qu’à une connexion unique utilisée pour partager l’accès à plusieurs hubs.</span><span class="sxs-lookup"><span data-stu-id="ba89e-151">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="ba89e-152">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="ba89e-152">Streaming</span></span>

<span data-ttu-id="ba89e-153">ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) à partir du hub au client.</span><span class="sxs-lookup"><span data-stu-id="ba89e-153">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="ba89e-154">État</span><span class="sxs-lookup"><span data-stu-id="ba89e-154">State</span></span>

<span data-ttu-id="ba89e-155">La possibilité de passer un état arbitraire entre les clients et le hub (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression.</span><span class="sxs-lookup"><span data-stu-id="ba89e-155">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="ba89e-156">Il n’existe aucun équivalent de proxy de Hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="ba89e-156">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="ba89e-157">Suppression de PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="ba89e-157">PersistentConnection removal</span></span>

<span data-ttu-id="ba89e-158">Dans ASP.NET Core Signalr, la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="ba89e-158">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="ba89e-159">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="ba89e-159">GlobalHost</span></span>

<span data-ttu-id="ba89e-160">ASP.NET Core a intégré l’injection de dépendances dans le Framework.</span><span class="sxs-lookup"><span data-stu-id="ba89e-160">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="ba89e-161">Les services peuvent utiliser DI pour accéder à [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="ba89e-161">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="ba89e-162">L' `GlobalHost` objet utilisé dans ASP.net signalr pour obtenir un `HubContext` n’existe pas dans ASP.net Core signalr.</span><span class="sxs-lookup"><span data-stu-id="ba89e-162">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="ba89e-163">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="ba89e-163">HubPipeline</span></span>

<span data-ttu-id="ba89e-164">ASP.net Core signalr ne prend pas en `HubPipeline` charge les modules.</span><span class="sxs-lookup"><span data-stu-id="ba89e-164">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="ba89e-165">Différences sur le client</span><span class="sxs-lookup"><span data-stu-id="ba89e-165">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="ba89e-166">TypeScript</span><span class="sxs-lookup"><span data-stu-id="ba89e-166">TypeScript</span></span>

<span data-ttu-id="ba89e-167">Le client d’ASP.NET Core SignalR est écrit en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="ba89e-167">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="ba89e-168">Vous pouvez écrire en JavaScript ou à la machine à écrire lors de l’utilisation du [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="ba89e-168">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="ba89e-169">Le client JavaScript est hébergé sur [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ba89e-169">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="ba89e-170">Dans les versions précédentes, le client JavaScript était obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba89e-170">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="ba89e-171">Pour les versions Core, le package npm [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ba89e-171">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="ba89e-172">Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ba89e-172">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ba89e-173">Utiliser npm pour obtenir et installer le package npm `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="ba89e-173">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="ba89e-174">jQuery</span><span class="sxs-lookup"><span data-stu-id="ba89e-174">jQuery</span></span>

<span data-ttu-id="ba89e-175">La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="ba89e-175">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="ba89e-176">Prise en charge d’Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="ba89e-176">Internet Explorer support</span></span>

<span data-ttu-id="ba89e-177">ASP.NET Core Signalr requiert Microsoft Internet Explorer 11 ou une version ultérieure (ASP.NET Signalr est pris en charge par Microsoft Internet Explorer 8 et versions ultérieures).</span><span class="sxs-lookup"><span data-stu-id="ba89e-177">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="ba89e-178">Syntaxe de méthode du client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ba89e-178">JavaScript client method syntax</span></span>

<span data-ttu-id="ba89e-179">La syntaxe JavaScript a changé depuis la précédente version de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ba89e-179">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="ba89e-180">Au lieu d’utiliser l’objet `$connection`, créez une connexion en utilisant l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ba89e-180">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="ba89e-181">Utilisez la méthode [on](/javascript/api/@aspnet/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="ba89e-181">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="ba89e-182">Après avoir créé la méthode du client, démarrez la connexion au hub.</span><span class="sxs-lookup"><span data-stu-id="ba89e-182">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="ba89e-183">Chaînez une méthode [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) pour loguer ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="ba89e-183">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="ba89e-184">Proxies Hub</span><span class="sxs-lookup"><span data-stu-id="ba89e-184">Hub proxies</span></span>

<span data-ttu-id="ba89e-185">Les serveurs proxy de hub ne sont plus générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="ba89e-185">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="ba89e-186">Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/%40aspnet/signalr/hubconnection#invoke) à l'API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="ba89e-186">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="ba89e-187">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="ba89e-187">.NET and other clients</span></span>

<span data-ttu-id="ba89e-188">Le package NuGet `Microsoft.AspNetCore.SignalR.Client` contient les bibliothèques de client .NET pour ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="ba89e-188">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="ba89e-189">Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un hub.</span><span class="sxs-lookup"><span data-stu-id="ba89e-189">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="ba89e-190">Différences ScaleOut</span><span class="sxs-lookup"><span data-stu-id="ba89e-190">Scaleout differences</span></span>

<span data-ttu-id="ba89e-191">ASP.NET Signalr prend en charge les SQL Server et les ReDim.</span><span class="sxs-lookup"><span data-stu-id="ba89e-191">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="ba89e-192">ASP.NET Core Signalr prend en charge le service et les ReDim Azure Signalr.</span><span class="sxs-lookup"><span data-stu-id="ba89e-192">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="ba89e-193">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ba89e-193">ASP.NET</span></span>

* [<span data-ttu-id="ba89e-194">Signalr ScaleOut avec Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ba89e-194">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="ba89e-195">Signalr ScaleOut avec Redims</span><span class="sxs-lookup"><span data-stu-id="ba89e-195">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="ba89e-196">Signalr ScaleOut avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="ba89e-196">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="ba89e-197">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba89e-197">ASP.NET Core</span></span>

* [<span data-ttu-id="ba89e-198">Service Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="ba89e-198">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="ba89e-199">Fond de panier ReDim</span><span class="sxs-lookup"><span data-stu-id="ba89e-199">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="ba89e-200">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ba89e-200">Additional resources</span></span>

* [<span data-ttu-id="ba89e-201">Hubs</span><span class="sxs-lookup"><span data-stu-id="ba89e-201">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ba89e-202">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ba89e-202">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ba89e-203">Client .NET</span><span class="sxs-lookup"><span data-stu-id="ba89e-203">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ba89e-204">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="ba89e-204">Supported platforms</span></span>](xref:signalr/supported-platforms)
