---
title: Différences entre SignalR et ASP.NET Core SignalR
author: tdykstra
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 2f3458f27fd7f22339751e0734dd8c5da709a3c0
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340119"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="ad801-103">Différences entre ASP.NET SignalR et ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ad801-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="ad801-104">ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad801-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="ad801-105">Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad801-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="ad801-106">Comment identifier la version de SignalR</span><span class="sxs-lookup"><span data-stu-id="ad801-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="ad801-107">SignalR ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad801-107">ASP.NET SignalR</span></span> | <span data-ttu-id="ad801-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ad801-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="ad801-109">Package NuGet de serveur</span><span class="sxs-lookup"><span data-stu-id="ad801-109">Server NuGet Package</span></span> | [<span data-ttu-id="ad801-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ad801-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="ad801-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ad801-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="ad801-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="ad801-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="ad801-113">Packages NuGet clients</span><span class="sxs-lookup"><span data-stu-id="ad801-113">Client NuGet Packages</span></span> | [<span data-ttu-id="ad801-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="ad801-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="ad801-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="ad801-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="ad801-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="ad801-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="ad801-117">Client npm Package</span><span class="sxs-lookup"><span data-stu-id="ad801-117">Client npm Package</span></span> | [<span data-ttu-id="ad801-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="ad801-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="ad801-119">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="ad801-119">Server App Type</span></span> | <span data-ttu-id="ad801-120">ASP.NET (System.Web) ou l’auto-hébergement OWIN</span><span class="sxs-lookup"><span data-stu-id="ad801-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="ad801-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad801-121">ASP.NET Core</span></span> |
| <span data-ttu-id="ad801-122">Plateformes de serveur pris en charge</span><span class="sxs-lookup"><span data-stu-id="ad801-122">Supported Server Platforms</span></span> | <span data-ttu-id="ad801-123">.NET framework 4.5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad801-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="ad801-124">.NET Framework 4.6.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="ad801-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="ad801-125">.NET core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ad801-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="ad801-126">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="ad801-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="ad801-127">Reconnexions automatique</span><span class="sxs-lookup"><span data-stu-id="ad801-127">Automatic reconnects</span></span>

<span data-ttu-id="ad801-128">Reconnexions automatique ne sont plus prises en charge.</span><span class="sxs-lookup"><span data-stu-id="ad801-128">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="ad801-129">Auparavant, SignalR a tenté de se reconnecter au serveur si la connexion a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="ad801-129">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="ad801-130">Maintenant, si le client est déconnecté l’utilisateur doit démarrer explicitement une nouvelle connexion s’ils souhaitent se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="ad801-130">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="ad801-131">Prise en charge du protocole</span><span class="sxs-lookup"><span data-stu-id="ad801-131">Protocol support</span></span>

<span data-ttu-id="ad801-132">ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire selon [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="ad801-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="ad801-133">En outre, les protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="ad801-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="ad801-134">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="ad801-134">Differences on the server</span></span>

<span data-ttu-id="ad801-135">Les bibliothèques de côté serveur ASP.NET Core SignalR sont incluses dans le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) package qui fait partie de la **Application Web ASP.NET Core** modèle pour Razor et MVC projets.</span><span class="sxs-lookup"><span data-stu-id="ad801-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="ad801-136">ASP.NET Core SignalR est un intergiciel (middleware) ASP.NET Core, donc il doit être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ad801-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="ad801-137">Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de la [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) appel de méthode dans le `Startup.Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="ad801-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="ad801-138">Sessions rémanentes désormais requises</span><span class="sxs-lookup"><span data-stu-id="ad801-138">Sticky sessions now required</span></span>

<span data-ttu-id="ad801-139">En raison de la montée en puissance travaillé dans ASP.NET SignalR, les clients peuvent se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="ad801-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="ad801-140">En raison de modifications pour le modèle de montée en puissance, mais aussi ne prenant ne pas en charge se reconnecte, il est n’est plus pris en charge.</span><span class="sxs-lookup"><span data-stu-id="ad801-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="ad801-141">Une fois que le client se connecte au serveur, il doit interagir avec le même serveur pour la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="ad801-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="ad801-142">Hub unique par connexion</span><span class="sxs-lookup"><span data-stu-id="ad801-142">Single hub per connection</span></span>

<span data-ttu-id="ad801-143">Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="ad801-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="ad801-144">Les connexions sont apportées directement à un concentrateur unique, plutôt que d’une connexion unique utilisé pour partager l’accès à plusieurs concentrateurs.</span><span class="sxs-lookup"><span data-stu-id="ad801-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="ad801-145">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="ad801-145">Streaming</span></span>

<span data-ttu-id="ad801-146">Prend en charge ASP.NET Core SignalR [diffusion en continu des données](xref:signalr/streaming) à partir du concentrateur au client.</span><span class="sxs-lookup"><span data-stu-id="ad801-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="ad801-147">État</span><span class="sxs-lookup"><span data-stu-id="ad801-147">State</span></span>

<span data-ttu-id="ad801-148">La possibilité de passer l’état arbitraire entre les clients et le concentrateur (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression.</span><span class="sxs-lookup"><span data-stu-id="ad801-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="ad801-149">Il n’existe aucun équivalent de proxy de hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="ad801-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="ad801-150">Connaître les différences entre le client</span><span class="sxs-lookup"><span data-stu-id="ad801-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="ad801-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="ad801-151">TypeScript</span></span>

<span data-ttu-id="ad801-152">Le client d’ASP.NET Core SignalR est écrit [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="ad801-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="ad801-153">Vous pouvez écrire en JavaScript ou TypeScript lorsque vous utilisez le [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="ad801-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="ad801-154">Le client JavaScript est hébergé à [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ad801-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="ad801-155">Dans les versions précédentes, le client JavaScript a été obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad801-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="ad801-156">Pour les versions Core, le [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) package npm contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ad801-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="ad801-157">Ce package n’est pas inclus dans le **Application Web ASP.NET Core** modèle.</span><span class="sxs-lookup"><span data-stu-id="ad801-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ad801-158">Utiliser npm pour obtenir et installer le `@aspnet/signalr` package npm.</span><span class="sxs-lookup"><span data-stu-id="ad801-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="ad801-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="ad801-159">jQuery</span></span>

<span data-ttu-id="ad801-160">La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="ad801-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="ad801-161">Syntaxe de méthode JavaScript client</span><span class="sxs-lookup"><span data-stu-id="ad801-161">JavaScript client method syntax</span></span>

<span data-ttu-id="ad801-162">La syntaxe de JavaScript a changé depuis la précédente version de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad801-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="ad801-163">Au lieu d’utiliser le `$connection` d’objet, de créer une connexion en utilisant le [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="ad801-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="ad801-164">Utilisez le [sur](/javascript/api/@aspnet/signalr/HubConnection#on) méthode pour spécifier les méthodes de client que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="ad801-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="ad801-165">Après avoir créé la méthode du client, démarrez la connexion du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ad801-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="ad801-166">Chaîne un [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) méthode pour vous connecter ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="ad801-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="ad801-167">Serveurs proxy de hub</span><span class="sxs-lookup"><span data-stu-id="ad801-167">Hub proxies</span></span>

<span data-ttu-id="ad801-168">Serveurs proxy de Hub est générées ne sont plus automatiquement.</span><span class="sxs-lookup"><span data-stu-id="ad801-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="ad801-169">Au lieu de cela, le nom de méthode est passé dans le [appeler](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="ad801-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="ad801-170">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="ad801-170">.NET and other clients</span></span>

<span data-ttu-id="ad801-171">Le `Microsoft.AspNetCore.SignalR.Client` package NuGet contient les bibliothèques de client .NET pour ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad801-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="ad801-172">Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ad801-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="ad801-173">Différences de montée en puissance parallèle</span><span class="sxs-lookup"><span data-stu-id="ad801-173">Scaleout differences</span></span>

<span data-ttu-id="ad801-174">ASP.NET SignalR prend en charge SQL Server et Redis.</span><span class="sxs-lookup"><span data-stu-id="ad801-174">ASP.NET SignalR supports both SQL Server and Redis.</span></span> <span data-ttu-id="ad801-175">ASP.NET Core SignalR prend en charge le Service Azure SignalR et Redis.</span><span class="sxs-lookup"><span data-stu-id="ad801-175">ASP.NET Core SignalR supports both Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="ad801-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ad801-176">ASP.NET</span></span>

* [<span data-ttu-id="ad801-177">Montée en puissance parallèle de SignalR avec Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ad801-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="ad801-178">Montée en puissance parallèle de SignalR avec Redis</span><span class="sxs-lookup"><span data-stu-id="ad801-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="ad801-179">Montée en puissance parallèle de SignalR avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="ad801-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="ad801-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad801-180">ASP.NET Core</span></span>

* [<span data-ttu-id="ad801-181">Service Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="ad801-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="ad801-182">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ad801-182">Additional resources</span></span>

* [<span data-ttu-id="ad801-183">Hubs</span><span class="sxs-lookup"><span data-stu-id="ad801-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ad801-184">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="ad801-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ad801-185">Client .NET</span><span class="sxs-lookup"><span data-stu-id="ad801-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ad801-186">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="ad801-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
