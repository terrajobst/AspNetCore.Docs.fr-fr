---
title: Différences entre SignalR et ASP.NET Core SignalR
author: bradygaster
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963723"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="8dcbb-103">Différences entre ASP.NET SignalR et ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8dcbb-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="8dcbb-104">ASP.NET Core SignalR n’est pas compatible avec les clients ou les serveurs pour ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="8dcbb-105">Cet article détaille les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="8dcbb-106">Comment identifier la version de SignalR</span><span class="sxs-lookup"><span data-stu-id="8dcbb-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="8dcbb-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="8dcbb-107">ASP.NET SignalR</span></span> | <span data-ttu-id="8dcbb-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8dcbb-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="8dcbb-109">Package NuGet de serveur</span><span class="sxs-lookup"><span data-stu-id="8dcbb-109">Server NuGet Package</span></span> | <span data-ttu-id="8dcbb-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="8dcbb-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.net Core)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="8dcbb-112">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="8dcbb-113">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-113">(.NET Framework)</span></span> |
| <span data-ttu-id="8dcbb-114">Packages NuGet client</span><span class="sxs-lookup"><span data-stu-id="8dcbb-114">Client NuGet Packages</span></span> | <span data-ttu-id="8dcbb-115">[Microsoft. AspNet.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="8dcbb-116">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="8dcbb-117">[Microsoft. AspNetCore.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="8dcbb-118">Package NPM client</span><span class="sxs-lookup"><span data-stu-id="8dcbb-118">Client npm Package</span></span> | [<span data-ttu-id="8dcbb-119">signalr</span><span class="sxs-lookup"><span data-stu-id="8dcbb-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="8dcbb-120">Client Java</span><span class="sxs-lookup"><span data-stu-id="8dcbb-120">Java Client</span></span> | <span data-ttu-id="8dcbb-121">[Dépôt github](https://github.com/SignalR/java-client) (déconseillé)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="8dcbb-122">Package Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="8dcbb-123">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="8dcbb-123">Server App Type</span></span> | <span data-ttu-id="8dcbb-124">ASP.NET (System. Web) ou auto-hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="8dcbb-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="8dcbb-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8dcbb-125">ASP.NET Core</span></span> |
| <span data-ttu-id="8dcbb-126">Plateformes serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="8dcbb-126">Supported Server Platforms</span></span> | <span data-ttu-id="8dcbb-127">.NET Framework 4,5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="8dcbb-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="8dcbb-128">.NET Framework 4.6.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="8dcbb-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="8dcbb-129">.NET Core 2,1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="8dcbb-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="8dcbb-130">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="8dcbb-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="8dcbb-131">Reconnexions automatiques</span><span class="sxs-lookup"><span data-stu-id="8dcbb-131">Automatic reconnects</span></span>

<span data-ttu-id="8dcbb-132">Les reconnexions automatiques ne sont pas prises en charge dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="8dcbb-133">Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion s’il souhaite se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="8dcbb-134">Dans ASP.NET SignalR, SignalR tente de se reconnecter au serveur si la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="8dcbb-135">Prise en charge du protocole</span><span class="sxs-lookup"><span data-stu-id="8dcbb-135">Protocol support</span></span>

<span data-ttu-id="8dcbb-136">ASP.NET Core SignalR prend en charge JSON, ainsi qu’un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="8dcbb-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="8dcbb-137">En outre, des protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="8dcbb-138">Transports</span><span class="sxs-lookup"><span data-stu-id="8dcbb-138">Transports</span></span>

<span data-ttu-id="8dcbb-139">Le transport de trame Forever n’est pas pris en charge dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="8dcbb-140">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="8dcbb-140">Differences on the server</span></span>

<span data-ttu-id="8dcbb-141">Les ASP.NET Core SignalR bibliothèques côté serveur sont incluses dans le package de packages [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) qui fait partie du modèle d' **application Web ASP.net Core** pour les projets Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="8dcbb-142">ASP.NET Core SignalR est un intergiciel ASP.NET Core, il doit être configuré en appelant [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8dcbb-143">Pour configurer le routage, mappez les itinéraires aux hubs à l’intérieur de l’appel de la méthode [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="8dcbb-144">Pour configurer le routage, mappez les itinéraires aux hubs à l’intérieur de l’appel de la méthode [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="8dcbb-145">Sessions rémanentes</span><span class="sxs-lookup"><span data-stu-id="8dcbb-145">Sticky sessions</span></span>

<span data-ttu-id="8dcbb-146">Le modèle ScaleOut pour ASP.NET SignalR permet aux clients de se reconnecter et d’envoyer des messages à n’importe quel serveur de la batterie.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="8dcbb-147">Dans ASP.NET Core SignalR, le client doit interagir avec le même serveur pendant la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="8dcbb-148">Pour les ScaleOut utilisant des ReDim, cela signifie que les sessions rémanentes sont requises.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="8dcbb-149">Pour ScaleOut à l’aide d' [Azure SignalR service](/azure/azure-signalr/), les sessions rémanentes ne sont pas nécessaires, car le service gère les connexions aux clients.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="8dcbb-150">Concentrateur unique par connexion</span><span class="sxs-lookup"><span data-stu-id="8dcbb-150">Single hub per connection</span></span>

<span data-ttu-id="8dcbb-151">Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="8dcbb-152">Les connexions sont établies directement sur un concentrateur unique, au lieu d’une connexion unique utilisée pour partager l’accès à plusieurs hubs.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="8dcbb-153">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="8dcbb-153">Streaming</span></span>

<span data-ttu-id="8dcbb-154">ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) du concentrateur vers le client.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="8dcbb-155">État</span><span class="sxs-lookup"><span data-stu-id="8dcbb-155">State</span></span>

<span data-ttu-id="8dcbb-156">La possibilité de passer un état arbitraire entre les clients et le Hub (souvent appelé HubState) a été supprimée, ainsi que la prise en charge des messages de progression.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="8dcbb-157">Il n’existe aucun équivalent de proxy de Hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="8dcbb-158">Suppression de PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="8dcbb-158">PersistentConnection removal</span></span>

<span data-ttu-id="8dcbb-159">Dans ASP.NET Core SignalR, la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="8dcbb-160">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="8dcbb-160">GlobalHost</span></span>

<span data-ttu-id="8dcbb-161">ASP.NET Core a intégré l’injection de dépendances dans le Framework.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="8dcbb-162">Les services peuvent utiliser DI pour accéder à [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="8dcbb-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="8dcbb-163">L’objet `GlobalHost` utilisé dans ASP.NET SignalR pour obtenir un `HubContext` n’existe pas dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="8dcbb-164">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="8dcbb-164">HubPipeline</span></span>

<span data-ttu-id="8dcbb-165">ASP.NET Core SignalR ne prend pas en charge les modules `HubPipeline`.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="8dcbb-166">Différences sur le client</span><span class="sxs-lookup"><span data-stu-id="8dcbb-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="8dcbb-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="8dcbb-167">TypeScript</span></span>

<span data-ttu-id="8dcbb-168">Le client ASP.NET Core SignalR est écrit dans la [machine à écrire](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="8dcbb-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="8dcbb-169">Vous pouvez écrire en JavaScript ou à la machine à écrire lors de l’utilisation du [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="8dcbb-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="8dcbb-170">Le client JavaScript est hébergé sur [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="8dcbb-171">Dans les versions précédentes, le client JavaScript a été obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="8dcbb-172">Pour les versions principales, le package NPM [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="8dcbb-173">Ce package n’est pas inclus dans le modèle d' **application Web ASP.net Core** .</span><span class="sxs-lookup"><span data-stu-id="8dcbb-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8dcbb-174">Utilisez NPM pour obtenir et installer le package NPM `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="8dcbb-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="8dcbb-175">jQuery</span></span>

<span data-ttu-id="8dcbb-176">La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="8dcbb-177">Prise en charge d’Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="8dcbb-177">Internet Explorer support</span></span>

<span data-ttu-id="8dcbb-178">ASP.NET Core SignalR nécessite Microsoft Internet Explorer 11 ou version ultérieure (ASP.NET SignalR pris en charge par Microsoft Internet Explorer 8 et versions ultérieures).</span><span class="sxs-lookup"><span data-stu-id="8dcbb-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="8dcbb-179">Syntaxe de méthode du client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8dcbb-179">JavaScript client method syntax</span></span>

<span data-ttu-id="8dcbb-180">La syntaxe JavaScript a été modifiée par rapport à la version précédente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="8dcbb-181">Au lieu d’utiliser l’objet `$connection`, créez une connexion à l’aide de l’API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="8dcbb-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="8dcbb-182">Utilisez la méthode [on](/javascript/api/@aspnet/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="8dcbb-183">Après la création de la méthode cliente, démarrez la connexion Hub.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="8dcbb-184">Chaînez une méthode [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) pour consigner ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="8dcbb-185">Proxies Hub</span><span class="sxs-lookup"><span data-stu-id="8dcbb-185">Hub proxies</span></span>

<span data-ttu-id="8dcbb-186">Les proxys de concentrateur ne sont plus générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="8dcbb-187">Au lieu de cela, le nom de la méthode est passé à l’API [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) en tant que chaîne.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="8dcbb-188">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="8dcbb-188">.NET and other clients</span></span>

<span data-ttu-id="8dcbb-189">Le package NuGet `Microsoft.AspNetCore.SignalR.Client` contient les bibliothèques clientes .NET pour les SignalRASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="8dcbb-190">Utilisez [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) pour créer et générer une instance d’une connexion à un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="8dcbb-191">Différences ScaleOut</span><span class="sxs-lookup"><span data-stu-id="8dcbb-191">Scaleout differences</span></span>

<span data-ttu-id="8dcbb-192">ASP.NET SignalR prend en charge les SQL Server et les ReDim.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="8dcbb-193">ASP.NET Core SignalR prend en charge Azure SignalR service et les ReDim.</span><span class="sxs-lookup"><span data-stu-id="8dcbb-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="8dcbb-194">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8dcbb-194">ASP.NET</span></span>

* <span data-ttu-id="8dcbb-195">[SignalR ScaleOut avec Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="8dcbb-196">[SignalR ScaleOut avec Redims](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="8dcbb-197">[SignalR ScaleOut avec SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="8dcbb-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8dcbb-198">ASP.NET Core</span></span>

* <span data-ttu-id="8dcbb-199">[Service SignalR Azure](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="8dcbb-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="8dcbb-200">Fond de panier ReDim</span><span class="sxs-lookup"><span data-stu-id="8dcbb-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="8dcbb-201">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8dcbb-201">Additional resources</span></span>

* [<span data-ttu-id="8dcbb-202">Hubs</span><span class="sxs-lookup"><span data-stu-id="8dcbb-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8dcbb-203">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="8dcbb-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8dcbb-204">Client .NET</span><span class="sxs-lookup"><span data-stu-id="8dcbb-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="8dcbb-205">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="8dcbb-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
