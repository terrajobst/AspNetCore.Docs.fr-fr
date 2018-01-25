---
title: Prise en charge de WebSocket dans ASP.NET Core
author: tdykstra
description: En savoir plus sur la prise en main WebSockets dans ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 6f335376c72cd0c68f4667cf0e661a25bf448980
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="253ea-103">Introduction à WebSockets dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="253ea-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="253ea-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Andrew Stanton-infirmière](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="253ea-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="253ea-105">Cet article explique comment démarrer avec le protocole WebSocket dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="253ea-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="253ea-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP.</span><span class="sxs-lookup"><span data-stu-id="253ea-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="253ea-107">Il est utilisé pour les applications telles que la conversation, téléscripteurs, jeux, n’importe où vous souhaitez les fonctionnalités en temps réel dans une application web.</span><span class="sxs-lookup"><span data-stu-id="253ea-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="253ea-108">[Afficher ou télécharger l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="253ea-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="253ea-109">Consultez le [étapes](#next-steps) section pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="253ea-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="253ea-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="253ea-110">Prerequisites</span></span>

* <span data-ttu-id="253ea-111">ASP.NET Core 1.1 (ne s’exécute pas sur la version 1.0)</span><span class="sxs-lookup"><span data-stu-id="253ea-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="253ea-112">Un système d’exploitation ASP.NET Core s’exécute sur :</span><span class="sxs-lookup"><span data-stu-id="253ea-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="253ea-113">Windows 7 / Windows Server 2008 et versions ultérieur</span><span class="sxs-lookup"><span data-stu-id="253ea-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="253ea-114">Linux</span><span class="sxs-lookup"><span data-stu-id="253ea-114">Linux</span></span>
  * <span data-ttu-id="253ea-115">macOS</span><span class="sxs-lookup"><span data-stu-id="253ea-115">macOS</span></span>

* <span data-ttu-id="253ea-116">**Exception**: Si votre application s’exécute sur Windows avec IIS, ou avec WebListener, vous devez utiliser :</span><span class="sxs-lookup"><span data-stu-id="253ea-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="253ea-117">Windows 8 / Windows Server 2012 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="253ea-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="253ea-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="253ea-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="253ea-119">WebSocket doit être activé dans IIS</span><span class="sxs-lookup"><span data-stu-id="253ea-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="253ea-120">Pour les navigateurs pris en charge, consultez http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="253ea-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="253ea-121">Quand l'utiliser</span><span class="sxs-lookup"><span data-stu-id="253ea-121">When to use it</span></span>

<span data-ttu-id="253ea-122">Utiliser des WebSockets lorsque vous avez besoin travailler directement avec une connexion de socket.</span><span class="sxs-lookup"><span data-stu-id="253ea-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="253ea-123">Par exemple, vous devrez peut-être les meilleures performances possibles pour un jeu en temps réel.</span><span class="sxs-lookup"><span data-stu-id="253ea-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="253ea-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fournit un riche modèle d’application de la fonctionnalité en temps réel, mais il s’exécute uniquement sur ASP.NET, pas ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="253ea-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="253ea-125">Une version de base de SignalR est en cours de développement ; pour suivre sa progression, consultez le [référentiel GitHub pour SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="253ea-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="253ea-126">Si vous ne souhaitez pas attendre que SignalR Core, vous pouvez utiliser des WebSockets directement maintenant.</span><span class="sxs-lookup"><span data-stu-id="253ea-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="253ea-127">Toutefois, vous devrez peut-être développer les fonctionnalités SignalR fournirait, telles que :</span><span class="sxs-lookup"><span data-stu-id="253ea-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="253ea-128">Prise en charge pour un plus grand nombre de versions de navigateur à l’aide de secours automatique de méthodes de transport secondaire.</span><span class="sxs-lookup"><span data-stu-id="253ea-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="253ea-129">Reconnexion automatique lors de la connexion.</span><span class="sxs-lookup"><span data-stu-id="253ea-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="253ea-130">Prise en charge pour les clients de l’appel de méthodes sur le serveur, ou vice versa.</span><span class="sxs-lookup"><span data-stu-id="253ea-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="253ea-131">Prise en charge pour la mise à l’échelle sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="253ea-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="253ea-132">Utilisation</span><span class="sxs-lookup"><span data-stu-id="253ea-132">How to use it</span></span>

* <span data-ttu-id="253ea-133">Installer le [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span><span class="sxs-lookup"><span data-stu-id="253ea-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="253ea-134">Configurer l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="253ea-134">Configure the middleware.</span></span>
* <span data-ttu-id="253ea-135">Accepter les demandes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="253ea-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="253ea-136">Envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="253ea-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="253ea-137">Configurer l’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="253ea-137">Configure the middleware</span></span>

<span data-ttu-id="253ea-138">Ajouter l’intergiciel (middleware) WebSockets dans le `Configure` méthode de la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="253ea-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="253ea-139">Les paramètres suivants peuvent être configurés :</span><span class="sxs-lookup"><span data-stu-id="253ea-139">The following settings can be configured:</span></span>

* <span data-ttu-id="253ea-140">`KeepAliveInterval`-La fréquence à laquelle envoyer des trames de « ping » pour le client, pour vous assurer de proxys maintiennent ouverte la connexion.</span><span class="sxs-lookup"><span data-stu-id="253ea-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="253ea-141">`ReceiveBufferSize`-La taille de la mémoire tampon utilisée pour recevoir des données.</span><span class="sxs-lookup"><span data-stu-id="253ea-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="253ea-142">Seuls les utilisateurs expérimentés devrez modifier ce paramètre, pour le réglage des performances basée sur la taille de leurs données.</span><span class="sxs-lookup"><span data-stu-id="253ea-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="253ea-143">Accepter les demandes de WebSocket</span><span class="sxs-lookup"><span data-stu-id="253ea-143">Accept WebSocket requests</span></span>

<span data-ttu-id="253ea-144">Plus tard dans le cycle de vie de demande (plus loin dans le `Configure` méthode ou dans une action MVC, par exemple) vérifier s’il est une demande WebSocket et accepter la demande WebSocket.</span><span class="sxs-lookup"><span data-stu-id="253ea-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="253ea-145">Cet exemple provient plus tard dans le `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="253ea-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="253ea-146">Une demande WebSocket peut provenir sur n’importe quelle URL, mais cet exemple de code n’accepte que les demandes de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="253ea-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="253ea-147">Envoyer et recevoir des messages</span><span class="sxs-lookup"><span data-stu-id="253ea-147">Send and receive messages</span></span>

<span data-ttu-id="253ea-148">Le `AcceptWebSocketAsync` méthode met à niveau de la connexion TCP pour une connexion WebSocket et vous donne un [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objet.</span><span class="sxs-lookup"><span data-stu-id="253ea-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="253ea-149">Utilisez l’objet WebSocket pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="253ea-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="253ea-150">Le code illustré précédemment et qui accepte la demande WebSocket passe le `WebSocket` de l’objet à un `Echo` méthode ; c' est ici le `Echo` (méthode).</span><span class="sxs-lookup"><span data-stu-id="253ea-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="253ea-151">Le code reçoit un message et envoie immédiatement le même message.</span><span class="sxs-lookup"><span data-stu-id="253ea-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="253ea-152">Il reste dans une boucle cela jusqu'à ce que le client ferme la connexion.</span><span class="sxs-lookup"><span data-stu-id="253ea-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="253ea-153">Lorsque vous acceptez le WebSocket avant de commencer cette boucle, le pipeline de l’intergiciel (middleware) se termine.</span><span class="sxs-lookup"><span data-stu-id="253ea-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="253ea-154">La fermeture du socket, le pipeline se déroule.</span><span class="sxs-lookup"><span data-stu-id="253ea-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="253ea-155">Autrement dit, l’arrêt de demande de déplacement vers l’avant dans le pipeline lorsque vous acceptez un WebSocket, tel qu’il serait lorsque vous atteignez une action MVC, par exemple.</span><span class="sxs-lookup"><span data-stu-id="253ea-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="253ea-156">Mais lorsque vous terminez cette boucle et que vous fermez le socket, la demande se poursuit sauvegarder le pipeline.</span><span class="sxs-lookup"><span data-stu-id="253ea-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="253ea-157">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="253ea-157">Next steps</span></span>

<span data-ttu-id="253ea-158">Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) qui accompagne cette article est une application simple écho.</span><span class="sxs-lookup"><span data-stu-id="253ea-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="253ea-159">Il a une page web qui établit des connexions de WebSocket, et le serveur renvoie simplement au client les messages qu’il reçoit.</span><span class="sxs-lookup"><span data-stu-id="253ea-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="253ea-160">Exécuter à partir d’une invite de commandes (il n'a pas configuré pour exécuter à partir de Visual Studio avec IIS Express) et accédez à http://localhost : 5000.</span><span class="sxs-lookup"><span data-stu-id="253ea-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="253ea-161">La page web affiche l’état de la connexion en haut à gauche :</span><span class="sxs-lookup"><span data-stu-id="253ea-161">The web page shows connection status at the upper left:</span></span>

![État initial de la page web](websockets/_static/start.png)

<span data-ttu-id="253ea-163">Sélectionnez **connexion** pour envoyer une demande WebSocket à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="253ea-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="253ea-164">Entrez un message de test et sélectionnez **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="253ea-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="253ea-165">Lorsque vous avez terminé, sélectionnez **Socket fermer**.</span><span class="sxs-lookup"><span data-stu-id="253ea-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="253ea-166">Le **Communication journal** section signale chaque ouverture, d’envoi et l’action Fermer tel qu’il se produit.</span><span class="sxs-lookup"><span data-stu-id="253ea-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![État initial de la page web](websockets/_static/end.png)
