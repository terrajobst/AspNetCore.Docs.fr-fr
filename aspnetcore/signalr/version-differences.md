---
title: Différences entre SignalR et ASP.NET Core SignalR
author: bradygaster
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: cca9a0cb0c46fc25eb5d1f7127d31fd3ab92f0b4
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880361"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="c3799-103">Différences entre ASP.NET SignalR et ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c3799-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="c3799-104">ASP.NET Core SignalR n’est pas compatible avec les clients ou les serveurs pour ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="c3799-105">Cet article détaille les fonctionnalités qui ont été supprimées ou modifiées dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="c3799-106">Comment identifier la version de SignalR</span><span class="sxs-lookup"><span data-stu-id="c3799-106">How to identify the SignalR version</span></span>

::: moniker range=">= aspnetcore-3.0"

|                      | <span data-ttu-id="c3799-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="c3799-107">ASP.NET SignalR</span></span> | <span data-ttu-id="c3799-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c3799-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="c3799-109">Package NuGet de serveur</span><span class="sxs-lookup"><span data-stu-id="c3799-109">Server NuGet package</span></span> | <span data-ttu-id="c3799-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="c3799-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="c3799-111">Aucun.</span><span class="sxs-lookup"><span data-stu-id="c3799-111">None.</span></span> <span data-ttu-id="c3799-112">Inclus dans l’infrastructure partagée [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) .</span><span class="sxs-lookup"><span data-stu-id="c3799-112">Included in the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) shared framework.</span></span> |
| <span data-ttu-id="c3799-113">Packages NuGet client</span><span class="sxs-lookup"><span data-stu-id="c3799-113">Client NuGet packages</span></span> | <span data-ttu-id="c3799-114">[Microsoft. AspNet.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="c3799-114">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="c3799-115">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="c3799-115">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="c3799-116">[Microsoft. AspNetCore.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="c3799-116">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="c3799-117">Package NPM du client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c3799-117">JavaScript client npm package</span></span> | [<span data-ttu-id="c3799-118">signalr</span><span class="sxs-lookup"><span data-stu-id="c3799-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) |
| <span data-ttu-id="c3799-119">Client Java</span><span class="sxs-lookup"><span data-stu-id="c3799-119">Java client</span></span> | <span data-ttu-id="c3799-120">[Dépôt github](https://github.com/SignalR/java-client) (déconseillé)</span><span class="sxs-lookup"><span data-stu-id="c3799-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="c3799-121">Package Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="c3799-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="c3799-122">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="c3799-122">Server app type</span></span> | <span data-ttu-id="c3799-123">ASP.NET (System. Web) ou auto-hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="c3799-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="c3799-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3799-124">ASP.NET Core</span></span> |
| <span data-ttu-id="c3799-125">Plateformes serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="c3799-125">Supported server platforms</span></span> | <span data-ttu-id="c3799-126">.NET Framework 4,5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c3799-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="c3799-127">.NET Core 3,0 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c3799-127">.NET Core 3.0 or later</span></span> |

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

|                      | <span data-ttu-id="c3799-128">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="c3799-128">ASP.NET SignalR</span></span> | <span data-ttu-id="c3799-129">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c3799-129">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="c3799-130">Package NuGet de serveur</span><span class="sxs-lookup"><span data-stu-id="c3799-130">Server NuGet package</span></span> | <span data-ttu-id="c3799-131">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="c3799-131">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="c3799-132">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.net Core)</span><span class="sxs-lookup"><span data-stu-id="c3799-132">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="c3799-133">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="c3799-133">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="c3799-134">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="c3799-134">(.NET Framework)</span></span> |
| <span data-ttu-id="c3799-135">Packages NuGet client</span><span class="sxs-lookup"><span data-stu-id="c3799-135">Client NuGet packages</span></span> | <span data-ttu-id="c3799-136">[Microsoft. AspNet.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="c3799-136">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="c3799-137">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="c3799-137">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="c3799-138">[Microsoft. AspNetCore.SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="c3799-138">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="c3799-139">Package NPM du client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c3799-139">JavaScript client npm package</span></span> | [<span data-ttu-id="c3799-140">signalr</span><span class="sxs-lookup"><span data-stu-id="c3799-140">signalr</span></span>](https://www.npmjs.com/package/signalr) | [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="c3799-141">Client Java</span><span class="sxs-lookup"><span data-stu-id="c3799-141">Java client</span></span> | <span data-ttu-id="c3799-142">[Dépôt github](https://github.com/SignalR/java-client) (déconseillé)</span><span class="sxs-lookup"><span data-stu-id="c3799-142">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="c3799-143">Package Maven [com. Microsoft. signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="c3799-143">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="c3799-144">Type d’application serveur</span><span class="sxs-lookup"><span data-stu-id="c3799-144">Server app type</span></span> | <span data-ttu-id="c3799-145">ASP.NET (System. Web) ou auto-hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="c3799-145">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="c3799-146">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3799-146">ASP.NET Core</span></span> |
| <span data-ttu-id="c3799-147">Plateformes serveur prises en charge</span><span class="sxs-lookup"><span data-stu-id="c3799-147">Supported server platforms</span></span> | <span data-ttu-id="c3799-148">.NET Framework 4,5 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c3799-148">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="c3799-149">.NET Framework 4.6.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="c3799-149">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="c3799-150">.NET Core 2,1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c3799-150">.NET Core 2.1 or later</span></span> |

::: moniker-end

## <a name="feature-differences"></a><span data-ttu-id="c3799-151">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="c3799-151">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="c3799-152">Reconnexions automatiques</span><span class="sxs-lookup"><span data-stu-id="c3799-152">Automatic reconnects</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c3799-153">Dans ASP.NET SignalR:</span><span class="sxs-lookup"><span data-stu-id="c3799-153">In ASP.NET SignalR:</span></span>

* <span data-ttu-id="c3799-154">Par défaut, SignalR tente de se reconnecter au serveur si la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="c3799-154">By default, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

<span data-ttu-id="c3799-155">Dans ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="c3799-155">In ASP.NET Core SignalR:</span></span>

* <span data-ttu-id="c3799-156">Les reconnexions automatiques s’abonnent à la fois avec le [client .net](xref:signalr/dotnet-client#automatically-reconnect) et le [client JavaScript](xref:signalr/javascript-client#automatically-reconnect):</span><span class="sxs-lookup"><span data-stu-id="c3799-156">Automatic reconnects are opt-in with both the [.NET client](xref:signalr/dotnet-client#automatically-reconnect) and the [JavaScript client](xref:signalr/javascript-client#automatically-reconnect):</span></span>

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c3799-157">Avant ASP.NET Core 3,0, SignalR ne prend pas en charge les reconnexions automatiques.</span><span class="sxs-lookup"><span data-stu-id="c3799-157">Prior to ASP.NET Core 3.0, SignalR doesn't support automatic reconnects.</span></span> <span data-ttu-id="c3799-158">Si le client est déconnecté, l’utilisateur doit démarrer explicitement une nouvelle connexion pour se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="c3799-158">If the client is disconnected, the user must explicitly start a new connection to reconnect.</span></span> <span data-ttu-id="c3799-159">Dans ASP.NET SignalR, SignalR tente de se reconnecter au serveur si la connexion est abandonnée.</span><span class="sxs-lookup"><span data-stu-id="c3799-159">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

::: moniker-end

### <a name="protocol-support"></a><span data-ttu-id="c3799-160">Prise en charge de protocole</span><span class="sxs-lookup"><span data-stu-id="c3799-160">Protocol support</span></span>

<span data-ttu-id="c3799-161">ASP.NET Core SignalR prend en charge JSON, ainsi qu’un nouveau protocole binaire basé sur [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="c3799-161">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="c3799-162">En outre, des protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="c3799-162">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="c3799-163">Transports</span><span class="sxs-lookup"><span data-stu-id="c3799-163">Transports</span></span>

<span data-ttu-id="c3799-164">Le transport de trame éternel n’est pas pris en charge dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-164">The Forever Frame transport isn't supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="c3799-165">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="c3799-165">Differences on the server</span></span>

<span data-ttu-id="c3799-166">Les ASP.NET Core SignalR bibliothèques côté serveur sont incluses dans [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), qui est utilisé dans le modèle d' **application Web ASP.net Core** pour les projets Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="c3799-166">The ASP.NET Core SignalR server-side libraries are included in [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), which is used in the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="c3799-167">ASP.NET Core SignalR est un intergiciel (middleware) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3799-167">ASP.NET Core SignalR is an ASP.NET Core middleware.</span></span> <span data-ttu-id="c3799-168">Elle doit être configurée en appelant <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddSignalR%2A> dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c3799-168">It must be configured by calling <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddSignalR%2A> in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c3799-169">Pour configurer le routage, mappez les itinéraires aux hubs à l’intérieur de l’appel de méthode <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c3799-169">To configure routing, map routes to hubs inside the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c3799-170">Pour configurer le routage, mappez les itinéraires aux hubs à l’intérieur de l’appel de méthode <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A> dans la méthode `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c3799-170">To configure routing, map routes to hubs inside the <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A> method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="c3799-171">Sessions rémanentes</span><span class="sxs-lookup"><span data-stu-id="c3799-171">Sticky sessions</span></span>

<span data-ttu-id="c3799-172">Le modèle ScaleOut pour ASP.NET SignalR permet aux clients de se reconnecter et d’envoyer des messages à n’importe quel serveur de la batterie.</span><span class="sxs-lookup"><span data-stu-id="c3799-172">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="c3799-173">Dans ASP.NET Core SignalR, le client doit interagir avec le même serveur pendant la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="c3799-173">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="c3799-174">Pour les ScaleOut utilisant des ReDim, cela signifie que les sessions rémanentes sont requises.</span><span class="sxs-lookup"><span data-stu-id="c3799-174">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="c3799-175">Pour ScaleOut à l’aide d' [Azure SignalR service](/azure/azure-signalr/), les sessions rémanentes ne sont pas nécessaires, car le service gère les connexions aux clients.</span><span class="sxs-lookup"><span data-stu-id="c3799-175">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions aren't required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="c3799-176">Concentrateur unique par connexion</span><span class="sxs-lookup"><span data-stu-id="c3799-176">Single hub per connection</span></span>

<span data-ttu-id="c3799-177">Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="c3799-177">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="c3799-178">Les connexions sont apportées directement à un hun unique, plutôt qu’à une connexion unique utilisée pour partager l’accès à plusieurs hubs.</span><span class="sxs-lookup"><span data-stu-id="c3799-178">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="c3799-179">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="c3799-179">Streaming</span></span>

<span data-ttu-id="c3799-180">ASP.NET Core SignalR prend désormais en charge la [diffusion en continu des données](xref:signalr/streaming) du concentrateur vers le client.</span><span class="sxs-lookup"><span data-stu-id="c3799-180">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="c3799-181">État</span><span class="sxs-lookup"><span data-stu-id="c3799-181">State</span></span>

<span data-ttu-id="c3799-182">La possibilité de passer un état arbitraire entre les clients et le Hub (souvent appelé `HubState`) a été supprimée, ainsi que la prise en charge des messages de progression.</span><span class="sxs-lookup"><span data-stu-id="c3799-182">The ability to pass arbitrary state between clients and the hub (often called `HubState`) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="c3799-183">Il n’existe aucun équivalent de proxy de Hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="c3799-183">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="c3799-184">Suppression de PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="c3799-184">PersistentConnection removal</span></span>

<span data-ttu-id="c3799-185">Dans ASP.NET Core SignalR, la classe [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="c3799-185">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="c3799-186">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="c3799-186">GlobalHost</span></span>

<span data-ttu-id="c3799-187">ASP.NET Core a intégré l’injection de dépendances dans le Framework.</span><span class="sxs-lookup"><span data-stu-id="c3799-187">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="c3799-188">Les services peuvent utiliser DI pour accéder à [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="c3799-188">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="c3799-189">L’objet `GlobalHost` utilisé dans ASP.NET SignalR pour obtenir un `HubContext` n’existe pas dans ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-189">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="c3799-190">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="c3799-190">HubPipeline</span></span>

<span data-ttu-id="c3799-191">ASP.NET Core SignalR ne prend pas en charge les modules `HubPipeline`.</span><span class="sxs-lookup"><span data-stu-id="c3799-191">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="c3799-192">Différences sur le client</span><span class="sxs-lookup"><span data-stu-id="c3799-192">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="c3799-193">TypeScript</span><span class="sxs-lookup"><span data-stu-id="c3799-193">TypeScript</span></span>

<span data-ttu-id="c3799-194">Le client ASP.NET Core SignalR est écrit dans la [machine à écrire](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="c3799-194">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="c3799-195">Vous pouvez écrire en JavaScript ou à la machine à écrire lors de l’utilisation du [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="c3799-195">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npm"></a><span data-ttu-id="c3799-196">Le client JavaScript est hébergé sur NPM</span><span class="sxs-lookup"><span data-stu-id="c3799-196">The JavaScript client is hosted at npm</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c3799-197">Dans les versions de ASP.NET, le client JavaScript a été obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3799-197">In ASP.NET versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="c3799-198">Dans les versions ASP.NET Core, le package NPM [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c3799-198">In the ASP.NET Core versions, the [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="c3799-199">Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c3799-199">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="c3799-200">Utiliser npm pour obtenir et installer le package npm `@microsoft/signalr`.</span><span class="sxs-lookup"><span data-stu-id="c3799-200">Use npm to obtain and install the `@microsoft/signalr` npm package.</span></span>

```console
npm init -y
npm install @microsoft/signalr
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c3799-201">Dans les versions de ASP.NET, le client JavaScript a été obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3799-201">In ASP.NET versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="c3799-202">Dans les versions ASP.NET Core, le package NPM [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c3799-202">In the ASP.NET Core versions, the [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="c3799-203">Ce package n’est pas inclus dans le modèle **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c3799-203">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="c3799-204">Utiliser npm pour obtenir et installer le package npm `@aspnet/signalr`.</span><span class="sxs-lookup"><span data-stu-id="c3799-204">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

::: moniker-end

### <a name="jquery"></a><span data-ttu-id="c3799-205">jQuery</span><span class="sxs-lookup"><span data-stu-id="c3799-205">jQuery</span></span>

<span data-ttu-id="c3799-206">La dépendance sur jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="c3799-206">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="c3799-207">Prise en charge d’Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c3799-207">Internet Explorer support</span></span>

<span data-ttu-id="c3799-208">ASP.NET Core SignalR nécessite Microsoft Internet Explorer 11 ou version ultérieure (ASP.NET SignalR pris en charge par Microsoft Internet Explorer 8 et versions ultérieures).</span><span class="sxs-lookup"><span data-stu-id="c3799-208">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="c3799-209">Syntaxe de méthode du client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c3799-209">JavaScript client method syntax</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c3799-210">La syntaxe JavaScript a été modifiée par rapport à la version ASP.NET de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-210">The JavaScript syntax has changed from the ASP.NET version of SignalR.</span></span> <span data-ttu-id="c3799-211">Au lieu d’utiliser l’objet `$connection`, créez une connexion en utilisant l'API [HubConnectionBuilder](/javascript/api/@aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="c3799-211">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/@aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="c3799-212">Utilisez la méthode [on](/javascript/api/@microsoft/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="c3799-212">Use the [on](/javascript/api/@microsoft/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c3799-213">La syntaxe JavaScript a été modifiée par rapport à la version ASP.NET de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-213">The JavaScript syntax has changed from the ASP.NET version of SignalR.</span></span> <span data-ttu-id="c3799-214">Au lieu d’utiliser l’objet `$connection`, créez une connexion en utilisant l'API [HubConnectionBuilder](/javascript/api/@microsoft/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="c3799-214">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/@microsoft/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="c3799-215">Utilisez la méthode [on](/javascript/api/@aspnet/signalr/HubConnection#on) pour spécifier les méthodes clientes que le hub peut appeler.</span><span class="sxs-lookup"><span data-stu-id="c3799-215">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

::: moniker-end

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = `${user} says ${msg}`;
    console.log(encodedMsg);
});
```

<span data-ttu-id="c3799-216">Après avoir créé la méthode du client, démarrez la connexion au hub.</span><span class="sxs-lookup"><span data-stu-id="c3799-216">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="c3799-217">Chaînez une méthode [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) pour loguer ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="c3799-217">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err));
```

### <a name="hub-proxies"></a><span data-ttu-id="c3799-218">Proxies Hub</span><span class="sxs-lookup"><span data-stu-id="c3799-218">Hub proxies</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c3799-219">Les serveurs proxy de hub ne sont plus générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="c3799-219">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="c3799-220">Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/@microsoft/signalr/hubconnection#invoke) à l'API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="c3799-220">Instead, the method name is passed into the [invoke](/javascript/api/@microsoft/signalr/hubconnection#invoke) API as a string.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c3799-221">Les serveurs proxy de hub ne sont plus générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="c3799-221">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="c3799-222">Au lieu de cela, le nom de méthode est passé dans l'[appel](/javascript/api/@aspnet/signalr/hubconnection#invoke) à l'API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="c3799-222">Instead, the method name is passed into the [invoke](/javascript/api/@aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

::: moniker-end

### <a name="net-and-other-clients"></a><span data-ttu-id="c3799-223">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="c3799-223">.NET and other clients</span></span>

<span data-ttu-id="c3799-224">[Microsoft. AspNetCore.SignalR. ](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)Le package NuGet client contient les bibliothèques clientes .net pour ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c3799-224">The [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="c3799-225">Utilisez la <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> pour créer et générer une instance d’une connexion à un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="c3799-225">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder> to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="c3799-226">Différences ScaleOut</span><span class="sxs-lookup"><span data-stu-id="c3799-226">Scaleout differences</span></span>

<span data-ttu-id="c3799-227">ASP.NET SignalR prend en charge les SQL Server et les ReDim.</span><span class="sxs-lookup"><span data-stu-id="c3799-227">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="c3799-228">ASP.NET Core SignalR prend en charge Azure SignalR service et les ReDim.</span><span class="sxs-lookup"><span data-stu-id="c3799-228">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="c3799-229">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c3799-229">ASP.NET</span></span>

* <span data-ttu-id="c3799-230">[SignalR ScaleOut avec Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="c3799-230">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="c3799-231">[SignalR ScaleOut avec Redims](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="c3799-231">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="c3799-232">[SignalR ScaleOut avec SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="c3799-232">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="c3799-233">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3799-233">ASP.NET Core</span></span>

* <span data-ttu-id="c3799-234">[Service SignalR Azure](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="c3799-234">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="c3799-235">Fond de panier ReDim</span><span class="sxs-lookup"><span data-stu-id="c3799-235">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="c3799-236">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c3799-236">Additional resources</span></span>

* [<span data-ttu-id="c3799-237">Hubs</span><span class="sxs-lookup"><span data-stu-id="c3799-237">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c3799-238">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c3799-238">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c3799-239">Client .NET</span><span class="sxs-lookup"><span data-stu-id="c3799-239">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c3799-240">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="c3799-240">Supported platforms</span></span>](xref:signalr/supported-platforms)
