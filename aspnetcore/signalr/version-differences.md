---
title: Différences entre SignalR et ASP.NET Core SignalR
author: rachelappel
description: Différences entre SignalR et ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090062"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="f42b5-103">Différences entre SignalR et ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f42b5-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="f42b5-104">ASP.NET Core SignalR n’est pas compatible avec les clients ou serveurs pour ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="f42b5-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="f42b5-105">Cet article décrit en détail les fonctionnalités qui ont été supprimées ou modifiées dans la base ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="f42b5-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="f42b5-106">Différences de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="f42b5-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="f42b5-107">Se reconnecte automatique</span><span class="sxs-lookup"><span data-stu-id="f42b5-107">Automatic reconnects</span></span>

<span data-ttu-id="f42b5-108">Se reconnecte automatique n’est plus prises en charge.</span><span class="sxs-lookup"><span data-stu-id="f42b5-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="f42b5-109">Auparavant, SignalR a tenté de se reconnecter au serveur si la connexion a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="f42b5-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="f42b5-110">Maintenant, si le client est déconnecté l’utilisateur doit démarrer explicitement une nouvelle connexion, s’ils souhaitent se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="f42b5-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="f42b5-111">Prise en charge du protocole</span><span class="sxs-lookup"><span data-stu-id="f42b5-111">Protocol support</span></span>

<span data-ttu-id="f42b5-112">ASP.NET Core SignalR prend en charge JSON, ainsi que d’un nouveau protocole binaire selon [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="f42b5-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="f42b5-113">En outre, les protocoles personnalisés peuvent être créés.</span><span class="sxs-lookup"><span data-stu-id="f42b5-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="f42b5-114">Différences sur le serveur</span><span class="sxs-lookup"><span data-stu-id="f42b5-114">Differences on the server</span></span>

<span data-ttu-id="f42b5-115">Les bibliothèques de côté serveur SignalR sont inclus dans le `Microsoft.AspNetCore.App` package qui fait partie de la **Application ASP.NET Core Web** modèle pour les projets Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="f42b5-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="f42b5-116">SignalR étant un intergiciel (middleware) ASP.NET Core, elle doit être configurée en appelant `AddSignalR` dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f42b5-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="f42b5-117">Pour configurer le routage, mapper les itinéraires aux concentrateurs à l’intérieur de la `UseSignalR` l’appel de méthode le `Startup.Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f42b5-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="f42b5-118">Sessions rémanentes maintenant</span><span class="sxs-lookup"><span data-stu-id="f42b5-118">Sticky sessions now required</span></span>

<span data-ttu-id="f42b5-119">En raison de la montée en puissance parallèle travaillé dans les versions précédentes de SignalR, les clients peuvent se reconnecter et envoyer des messages à n’importe quel serveur dans la batterie de serveurs.</span><span class="sxs-lookup"><span data-stu-id="f42b5-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="f42b5-120">En raison de modifications pour le modèle de montée en puissance parallèle, ainsi que ne prend ne pas en charge se reconnecte, il est plus pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f42b5-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="f42b5-121">À présent, une fois que le client se connecte au serveur, il doit interagir avec le même serveur pour la durée de la connexion.</span><span class="sxs-lookup"><span data-stu-id="f42b5-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="f42b5-122">Concentrateur unique par connexion</span><span class="sxs-lookup"><span data-stu-id="f42b5-122">Single hub per connection</span></span>

<span data-ttu-id="f42b5-123">Dans ASP.NET Core SignalR, le modèle de connexion a été simplifié.</span><span class="sxs-lookup"><span data-stu-id="f42b5-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="f42b5-124">Les connexions sont effectuées directement à un concentrateur unique, plutôt que d’une connexion unique utilisé pour partager l’accès à plusieurs concentrateurs.</span><span class="sxs-lookup"><span data-stu-id="f42b5-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="f42b5-125">Diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="f42b5-125">Streaming</span></span>

<span data-ttu-id="f42b5-126">Prend en charge SignalR [diffusion en continu de données](xref:signalr/streaming) à partir du concentrateur au client.</span><span class="sxs-lookup"><span data-stu-id="f42b5-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="f42b5-127">État</span><span class="sxs-lookup"><span data-stu-id="f42b5-127">State</span></span>

<span data-ttu-id="f42b5-128">La possibilité de passer d’un état arbitraire entre les clients et le concentrateur (souvent appelés HubState) a été supprimée, ainsi que la prise en charge pour les messages de progression.</span><span class="sxs-lookup"><span data-stu-id="f42b5-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="f42b5-129">Il n’existe aucun équivalent de proxy de hub pour le moment.</span><span class="sxs-lookup"><span data-stu-id="f42b5-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="f42b5-130">Différences sur le client</span><span class="sxs-lookup"><span data-stu-id="f42b5-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="f42b5-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="f42b5-131">TypeScript</span></span>

<span data-ttu-id="f42b5-132">La version de ASP.NET Core du SignalR est écrit [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="f42b5-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="f42b5-133">Vous pouvez écrire en JavaScript ou TypeScript lors de l’utilisation du [client JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="f42b5-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="f42b5-134">Le client JavaScript est hébergé à [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f42b5-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="f42b5-135">Dans les versions précédentes, le client JavaScript a été obtenu via un package NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f42b5-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="f42b5-136">Pour les versions principales, le [ @aspnet/signalr package npm](https://www.npmjs.com/package/@aspnet/signalr) contient les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f42b5-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="f42b5-137">Ce package n’est pas inclus dans le **Application ASP.NET Core Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="f42b5-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="f42b5-138">Utilisez npm pour obtenir et installer le `@aspnet/signalr` package npm.</span><span class="sxs-lookup"><span data-stu-id="f42b5-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="f42b5-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="f42b5-139">jQuery</span></span>

<span data-ttu-id="f42b5-140">La dépendance vis-à-vis de jQuery a été supprimée, mais les projets peuvent toujours utiliser jQuery.</span><span class="sxs-lookup"><span data-stu-id="f42b5-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="f42b5-141">Syntaxe de la méthode JavaScript client</span><span class="sxs-lookup"><span data-stu-id="f42b5-141">JavaScript client method syntax</span></span>

<span data-ttu-id="f42b5-142">La syntaxe JavaScript a changé depuis la version précédente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f42b5-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="f42b5-143">Au lieu d’utiliser le `$connection` d’objet, créez une connexion en utilisant le `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="f42b5-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="f42b5-144">Utilisez `connection.on` pour spécifier les méthodes de client que le concentrateur peut appeler.</span><span class="sxs-lookup"><span data-stu-id="f42b5-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="f42b5-145">Après avoir créé la méthode du client, démarrez la connexion du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f42b5-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="f42b5-146">Chaîne un `catch` méthode pour se connecter ou gérer les erreurs.</span><span class="sxs-lookup"><span data-stu-id="f42b5-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="f42b5-147">Serveurs proxy de hub</span><span class="sxs-lookup"><span data-stu-id="f42b5-147">Hub proxies</span></span>

<span data-ttu-id="f42b5-148">Serveurs proxy de Hub est générés ne sont plus automatiquement.</span><span class="sxs-lookup"><span data-stu-id="f42b5-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="f42b5-149">Au lieu de cela, le nom de la méthode a été passé dans le `invoke` API sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f42b5-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="f42b5-150">.NET et autres clients</span><span class="sxs-lookup"><span data-stu-id="f42b5-150">.NET and other clients</span></span>

<span data-ttu-id="f42b5-151">Le `Microsoft.AspNetCore.SignalR.Client` package NuGet contient les bibliothèques clientes .NET pour ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="f42b5-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="f42b5-152">Utilisez le `HubConnectionBuilder` pour créer et générer une instance d’une connexion à un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f42b5-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="f42b5-153">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f42b5-153">Additional resources</span></span>

* [<span data-ttu-id="f42b5-154">Hubs</span><span class="sxs-lookup"><span data-stu-id="f42b5-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f42b5-155">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="f42b5-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="f42b5-156">Client .NET</span><span class="sxs-lookup"><span data-stu-id="f42b5-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f42b5-157">Plateformes prises en charge</span><span class="sxs-lookup"><span data-stu-id="f42b5-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
