---
title: Différences entre SignalR et ASP.NET Core SignalR
author: tdykstra
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 3cec37719b743b3c805ada77249f526278e44599
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234603"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="b10ec-103">Différences entre ASP.NET SignalR et ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b10ec-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="b10ec-104">ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="b10ec-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="b10ec-105">Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="b10ec-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="b10ec-106">Comment identifier la version de SignalR</span><span class="sxs-lookup"><span data-stu-id="b10ec-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="b10ec-107">SignalR ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b10ec-107">ASP.NET SignalR</span></span> | <span data-ttu-id="b10ec-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b10ec-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="b10ec-109">Package NuGet serveur</span><span class="sxs-lookup"><span data-stu-id="b10ec-109">Server NuGet Package</span></span> | [<span data-ttu-id="b10ec-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="b10ec-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="b10ec-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="b10ec-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="b10ec-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="b10ec-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="b10ec-113">Packages NuGet clients</span><span class="sxs-lookup"><span data-stu-id="b10ec-113">Client NuGet Packages</span></span> | [<span data-ttu-id="b10ec-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="b10ec-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="b10ec-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="b10ec-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="b10ec-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="b10ec-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="b10ec-117">Client npm Package</span><span class="sxs-lookup"><span data-stu-id="b10ec-117">Client npm Package</span></span> | [<span data-ttu-id="b10ec-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="b10ec-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="b10ec-119">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="b10ec-119">Server App Type</span></span> | <span data-ttu-id="b10ec-120">ASP.NET (System.Web) ou l’auto-hébergement OWIN</span><span class="sxs-lookup"><span data-stu-id="b10ec-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="b10ec-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b10ec-121">ASP.NET Core</span></span> |
| <span data-ttu-id="b10ec-122">Plateformes serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="b10ec-122">Supported Server Platforms</span></span> | <span data-ttu-id="b10ec-123">.NET framework 4.5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="b10ec-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="b10ec-124">.NET Framework 4.6.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="b10ec-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="b10ec-125">.NET core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="b10ec-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="b10ec-126">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="b10ec-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="b10ec-127">Reconnexions automatiques</span><span class="sxs-lookup"><span data-stu-id="b10ec-127">Automatic reconnects</span></span>

<span data-ttu-id="b10ec-128">Reconnexions automatique ne sont pas pris en charge dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="b10ec-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="b10ec-129">Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’ils souhaitent se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="b10ec-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="b10ec-130">Dans ASP.NET SignalR, SignalR tente de se reconnecter au serveur si la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="b10ec-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="b10ec-131">Prise en charge de protocole</span><span class="sxs-lookup"><span data-stu-id="b10ec-131">Protocol support</span></span>

<span data-ttu-id="b10ec-132">ASP.NET Core SignalR prend en charge JSON, mais aussi un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="b10ec-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="b10ec-133">En outre, des protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="b10ec-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="b10ec-134">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="b10ec-134">Differences on the server</span></span>

<span data-ttu-id="b10ec-135">Les bibliothèques côté serveur ASP.NET Core SignalR sont incluses dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) qui fait partie du modèle de l'**Application Web ASP.NET Core** pour les projets Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="b10ec-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="b10ec-136">ASP.NET Core SignalR est un intergiciel (middleware) ASP.NET Core, donc il doit être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b10ec-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="b10ec-137">Pour configurer le routage, mapper les itinéraires aux hubs à l’intérieur de l'appel à la méthode [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b10ec-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="b10ec-138">Sessions rémanentes</span><span class="sxs-lookup"><span data-stu-id="b10ec-138">Sticky sessions</span></span>

<span data-ttu-id="b10ec-139">Le modèle de montée en puissance parallèle de SignalR ASP.NET permet aux clients de se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="b10ec-139">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="b10ec-140">Dans ASP.NET Core SignalR, le client doit interagir avec le même serveur pour la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="b10ec-140">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="b10ec-141">Pour la montée en puissance parallèle à l’aide de Redis, cela signifie que les sessions rémanentes sont requises.</span><span class="sxs-lookup"><span data-stu-id="b10ec-141">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="b10ec-142">Pour la montée en puissance parallèle à l’aide [Service Azure SignalR](/azure/azure-signalr/), sessions rémanentes ne sont pas requises, car le service gère les connexions aux clients.</span><span class="sxs-lookup"><span data-stu-id="b10ec-142">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="b10ec-143">Hub unique par connexion</span><span class="sxs-lookup"><span data-stu-id="b10ec-143">Single hub per connection</span></span>

<span data-ttu-id="b10ec-144">Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="b10ec-144">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="b10ec-145">Les connexions sont apportées directement à un hun unique, plutôt qu’à une connexion unique utilisée pour partager l’accès à plusieurs hubs.</span><span class="sxs-lookup"><span data-stu-id="b10ec-145">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="b10ec-146">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="b10ec-146">Streaming</span></span>

<span data-ttu-id="b10ec-147">ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) à partir du hub au client.</span><span class="sxs-lookup"><span data-stu-id="b10ec-147">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="b10ec-148">État</span><span class="sxs-lookup"><span data-stu-id="b10ec-148">State</span></span>

<span data-ttu-id="b10ec-149">La possibilité de passer un état arbitraire entre les clients et le hub (souvent appelé HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression.</span><span class="sxs-lookup"><span data-stu-id="b10ec-149">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="b10ec-150">Il n’existe aucun équivalent de proxy de hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="b10ec-150">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="b10ec-151">Différences sur le client</span><span class="sxs-lookup"><span data-stu-id="b10ec-151">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="b10ec-152">TypeScript</span><span class="sxs-lookup"><span data-stu-id="b10ec-152">TypeScript</span></span>

<span data-ttu-id="b10ec-153">Le client d’ASP.NET Core SignalR est écrit en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="b10ec-153">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="b10ec-154">Vous pouvez écrire en JavaScript ou TypeScript lorsque vous utilisez le [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="b10ec-154">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="b10ec-155">Le client JavaScript est hébergé sur [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="b10ec-155">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="b10ec-156">Dans les versions précédentes, le client JavaScript était obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b10ec-156">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="b10ec-157">Pour les versions Core, le package npm [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b10ec-157">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="b10ec-158">Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b10ec-158">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="b10ec-159">Utiliser npm pour obtenir et installer le package npm `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="b10ec-159">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="b10ec-160">jQuery</span><span class="sxs-lookup"><span data-stu-id="b10ec-160">jQuery</span></span>

<span data-ttu-id="b10ec-161">La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="b10ec-161">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="b10ec-162">Syntaxe de méthode JavaScript client</span><span class="sxs-lookup"><span data-stu-id="b10ec-162">JavaScript client method syntax</span></span>

<span data-ttu-id="b10ec-163">La syntaxe JavaScript a changé depuis la précédente version de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b10ec-163">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="b10ec-164">Au lieu d’utiliser l’objet `$connection`, créez une connexion en utilisant l'API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b10ec-164">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="b10ec-165">Utilisez la méthode [on](/javascript/api/@aspnet/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="b10ec-165">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="b10ec-166">Après avoir créé la méthode du client, démarrez la connexion au hub.</span><span class="sxs-lookup"><span data-stu-id="b10ec-166">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="b10ec-167">Chaînez une méthode [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) pour loguer ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="b10ec-167">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="b10ec-168">Serveurs proxy de hub</span><span class="sxs-lookup"><span data-stu-id="b10ec-168">Hub proxies</span></span>

<span data-ttu-id="b10ec-169">Les serveurs proxy de hub ne sont plus générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="b10ec-169">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="b10ec-170">Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/%40aspnet/signalr/hubconnection#invoke) à l'API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="b10ec-170">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="b10ec-171">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="b10ec-171">.NET and other clients</span></span>

<span data-ttu-id="b10ec-172">Le package NuGet `Microsoft.AspNetCore.SignalR.Client` contient les bibliothèques de client .NET pour ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="b10ec-172">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="b10ec-173">Utilisez le [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un hub.</span><span class="sxs-lookup"><span data-stu-id="b10ec-173">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="b10ec-174">Différences de montée en puissance parallèle</span><span class="sxs-lookup"><span data-stu-id="b10ec-174">Scaleout differences</span></span>

<span data-ttu-id="b10ec-175">ASP.NET SignalR prend en charge SQL Server et Redis.</span><span class="sxs-lookup"><span data-stu-id="b10ec-175">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="b10ec-176">ASP.NET Core SignalR prend en charge le Service Azure SignalR et Redis.</span><span class="sxs-lookup"><span data-stu-id="b10ec-176">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="b10ec-177">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b10ec-177">ASP.NET</span></span>

* [<span data-ttu-id="b10ec-178">Montée en puissance parallèle de SignalR avec Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="b10ec-178">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="b10ec-179">Montée en puissance parallèle de SignalR avec Redis</span><span class="sxs-lookup"><span data-stu-id="b10ec-179">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="b10ec-180">Montée en puissance parallèle de SignalR avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="b10ec-180">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="b10ec-181">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b10ec-181">ASP.NET Core</span></span>

* [<span data-ttu-id="b10ec-182">Service Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="b10ec-182">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="b10ec-183">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b10ec-183">Additional resources</span></span>

* [<span data-ttu-id="b10ec-184">Hubs</span><span class="sxs-lookup"><span data-stu-id="b10ec-184">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b10ec-185">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b10ec-185">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b10ec-186">Client .NET</span><span class="sxs-lookup"><span data-stu-id="b10ec-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b10ec-187">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="b10ec-187">Supported platforms</span></span>](xref:signalr/supported-platforms)
