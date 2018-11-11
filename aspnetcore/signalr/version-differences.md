---
title: Différences entre SignalR et ASP.NET Core SignalR
author: tdykstra
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 8f07647959b6ef815eed599703bdb1bfb446572f
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505750"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="9b660-103">Différences entre ASP.NET SignalR et ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="9b660-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="9b660-104">ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="9b660-105">Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="9b660-106">Comment identifier la version de SignalR</span><span class="sxs-lookup"><span data-stu-id="9b660-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="9b660-107">SignalR ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9b660-107">ASP.NET SignalR</span></span> | <span data-ttu-id="9b660-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="9b660-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="9b660-109">Package NuGet serveur</span><span class="sxs-lookup"><span data-stu-id="9b660-109">Server NuGet Package</span></span> | [<span data-ttu-id="9b660-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="9b660-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="9b660-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="9b660-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="9b660-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="9b660-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="9b660-113">Packages NuGet clients</span><span class="sxs-lookup"><span data-stu-id="9b660-113">Client NuGet Packages</span></span> | [<span data-ttu-id="9b660-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="9b660-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="9b660-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="9b660-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="9b660-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="9b660-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="9b660-117">Client npm Package</span><span class="sxs-lookup"><span data-stu-id="9b660-117">Client npm Package</span></span> | [<span data-ttu-id="9b660-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="9b660-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="9b660-119">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="9b660-119">Server App Type</span></span> | <span data-ttu-id="9b660-120">ASP.NET (System.Web) ou l’auto-hébergement OWIN</span><span class="sxs-lookup"><span data-stu-id="9b660-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="9b660-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b660-121">ASP.NET Core</span></span> |
| <span data-ttu-id="9b660-122">Plateformes serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="9b660-122">Supported Server Platforms</span></span> | <span data-ttu-id="9b660-123">.NET framework 4.5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="9b660-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="9b660-124">.NET Framework 4.6.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="9b660-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="9b660-125">.NET core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="9b660-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="9b660-126">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="9b660-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="9b660-127">Reconnexions automatiques</span><span class="sxs-lookup"><span data-stu-id="9b660-127">Automatic reconnects</span></span>

<span data-ttu-id="9b660-128">Reconnexions automatique ne sont pas pris en charge dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="9b660-129">Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’ils souhaitent se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="9b660-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="9b660-130">Dans ASP.NET SignalR, SignalR tente de se reconnecter au serveur si la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="9b660-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="9b660-131">Prise en charge de protocole</span><span class="sxs-lookup"><span data-stu-id="9b660-131">Protocol support</span></span>

<span data-ttu-id="9b660-132">ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="9b660-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="9b660-133">En outre, des protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="9b660-133">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="9b660-134">Transports</span><span class="sxs-lookup"><span data-stu-id="9b660-134">Transports</span></span>

<span data-ttu-id="9b660-135">Le transport Forever Frame n’est pas pris en charge dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-135">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="9b660-136">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="9b660-136">Differences on the server</span></span>

<span data-ttu-id="9b660-137">Les bibliothèques côté serveur ASP.NET Core SignalR sont incluses dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) qui fait partie du modèle de l'**Application Web ASP.NET Core** pour les projets Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="9b660-137">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="9b660-138">ASP.NET Core SignalR est un intergiciel (middleware) ASP.NET Core, donc il doit être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b660-138">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="9b660-139">Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de l'appel à la méthode [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9b660-139">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="9b660-140">Sessions rémanentes</span><span class="sxs-lookup"><span data-stu-id="9b660-140">Sticky sessions</span></span>

<span data-ttu-id="9b660-141">Le modèle de montée en puissance parallèle de SignalR ASP.NET permet aux clients de se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="9b660-141">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="9b660-142">Dans ASP.NET Core SignalR, le client doit interagir avec le même serveur pour la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="9b660-142">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="9b660-143">Pour la montée en puissance parallèle à l’aide de Redis, cela signifie que les sessions rémanentes sont requises.</span><span class="sxs-lookup"><span data-stu-id="9b660-143">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="9b660-144">Pour la montée en puissance parallèle à l’aide [Service Azure SignalR](/azure/azure-signalr/), sessions rémanentes ne sont pas requises, car le service gère les connexions aux clients.</span><span class="sxs-lookup"><span data-stu-id="9b660-144">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="9b660-145">Hub unique par connexion</span><span class="sxs-lookup"><span data-stu-id="9b660-145">Single hub per connection</span></span>

<span data-ttu-id="9b660-146">Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="9b660-146">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="9b660-147">Les connexions sont apportées directement à un hun unique, plutôt qu’à une connexion unique utilisée pour partager l’accès à plusieurs hubs.</span><span class="sxs-lookup"><span data-stu-id="9b660-147">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="9b660-148">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="9b660-148">Streaming</span></span>

<span data-ttu-id="9b660-149">ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) à partir du hub au client.</span><span class="sxs-lookup"><span data-stu-id="9b660-149">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="9b660-150">État</span><span class="sxs-lookup"><span data-stu-id="9b660-150">State</span></span>

<span data-ttu-id="9b660-151">La possibilité de passer un état arbitraire entre les clients et le hub (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression.</span><span class="sxs-lookup"><span data-stu-id="9b660-151">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="9b660-152">Il n’existe aucun équivalent de proxy de hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="9b660-152">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="globalhost"></a><span data-ttu-id="9b660-153">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="9b660-153">GlobalHost</span></span>

<span data-ttu-id="9b660-154">ASP.NET Core offre l’injection de dépendance (DI) intégrée au framework.</span><span class="sxs-lookup"><span data-stu-id="9b660-154">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="9b660-155">Services peuvent utiliser l’injection de dépendances pour accéder à la [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="9b660-155">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="9b660-156">Le `GlobalHost` objet qui est utilisé dans ASP.NET SignalR pour obtenir un `HubContext` n’existe pas dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-156">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="9b660-157">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="9b660-157">HubPipeline</span></span>

<span data-ttu-id="9b660-158">ASP.NET Core SignalR ne prend pas en charge `HubPipeline` modules.</span><span class="sxs-lookup"><span data-stu-id="9b660-158">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="9b660-159">Différences sur le client</span><span class="sxs-lookup"><span data-stu-id="9b660-159">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="9b660-160">TypeScript</span><span class="sxs-lookup"><span data-stu-id="9b660-160">TypeScript</span></span>

<span data-ttu-id="9b660-161">Le client d’ASP.NET Core SignalR est écrit en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="9b660-161">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="9b660-162">Vous pouvez écrire en JavaScript ou TypeScript lorsque vous utilisez le [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="9b660-162">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="9b660-163">Le client JavaScript est hébergé sur [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="9b660-163">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="9b660-164">Dans les versions précédentes, le client JavaScript était obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b660-164">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="9b660-165">Pour les versions Core, le package npm [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b660-165">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="9b660-166">Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9b660-166">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="9b660-167">Utiliser npm pour obtenir et installer le package npm `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="9b660-167">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="9b660-168">jQuery</span><span class="sxs-lookup"><span data-stu-id="9b660-168">jQuery</span></span>

<span data-ttu-id="9b660-169">La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="9b660-169">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="9b660-170">Prise en charge d’Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="9b660-170">Internet Explorer support</span></span>

<span data-ttu-id="9b660-171">ASP.NET Core SignalR requiert Microsoft Internet Explorer 11 ou version ultérieure (ASP.NET SignalR pris en charge de Microsoft Internet Explorer 8 et versions ultérieur).</span><span class="sxs-lookup"><span data-stu-id="9b660-171">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="9b660-172">Syntaxe de méthode JavaScript client</span><span class="sxs-lookup"><span data-stu-id="9b660-172">JavaScript client method syntax</span></span>

<span data-ttu-id="9b660-173">La syntaxe JavaScript a changé depuis la précédente version de SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-173">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="9b660-174">Au lieu d’utiliser l’objet `$connection`, créez une connexion en utilisant l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="9b660-174">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="9b660-175">Utilisez la méthode [on](/javascript/api/@aspnet/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="9b660-175">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="9b660-176">Après avoir créé la méthode du client, démarrez la connexion au hub.</span><span class="sxs-lookup"><span data-stu-id="9b660-176">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="9b660-177">Chaînez une méthode [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) pour loguer ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="9b660-177">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="9b660-178">Serveurs proxy de hub</span><span class="sxs-lookup"><span data-stu-id="9b660-178">Hub proxies</span></span>

<span data-ttu-id="9b660-179">Les serveurs proxy de hub ne sont plus générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="9b660-179">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="9b660-180">Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/%40aspnet/signalr/hubconnection#invoke) à l'API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="9b660-180">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="9b660-181">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="9b660-181">.NET and other clients</span></span>

<span data-ttu-id="9b660-182">Le package NuGet `Microsoft.AspNetCore.SignalR.Client` contient les bibliothèques de client .NET pour ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b660-182">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="9b660-183">Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un hub.</span><span class="sxs-lookup"><span data-stu-id="9b660-183">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="9b660-184">Différences de montée en puissance parallèle</span><span class="sxs-lookup"><span data-stu-id="9b660-184">Scaleout differences</span></span>

<span data-ttu-id="9b660-185">ASP.NET SignalR prend en charge SQL Server et Redis.</span><span class="sxs-lookup"><span data-stu-id="9b660-185">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="9b660-186">ASP.NET Core SignalR prend en charge le Service Azure SignalR et Redis.</span><span class="sxs-lookup"><span data-stu-id="9b660-186">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="9b660-187">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9b660-187">ASP.NET</span></span>

* [<span data-ttu-id="9b660-188">Montée en puissance parallèle de SignalR avec Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="9b660-188">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="9b660-189">Montée en puissance parallèle de SignalR avec Redis</span><span class="sxs-lookup"><span data-stu-id="9b660-189">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="9b660-190">Montée en puissance parallèle de SignalR avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="9b660-190">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="9b660-191">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b660-191">ASP.NET Core</span></span>

* [<span data-ttu-id="9b660-192">Service Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="9b660-192">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="9b660-193">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9b660-193">Additional resources</span></span>

* [<span data-ttu-id="9b660-194">Hubs</span><span class="sxs-lookup"><span data-stu-id="9b660-194">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9b660-195">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b660-195">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="9b660-196">Client .NET</span><span class="sxs-lookup"><span data-stu-id="9b660-196">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="9b660-197">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="9b660-197">Supported platforms</span></span>](xref:signalr/supported-platforms)
