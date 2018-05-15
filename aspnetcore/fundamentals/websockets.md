---
title: Prise en charge des WebSockets dans ASP.NET Core
author: tdykstra
description: Découvrez comment commencer à utiliser les WebSockets dans ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: e744ab5b81ff85f48edb012a86b55003cc74929c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="8f29c-103">Prise en charge des WebSockets dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f29c-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="8f29c-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="8f29c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="8f29c-105">Cet article explique comment commencer avec les WebSockets dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8f29c-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="8f29c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) est un protocole qui autorise des canaux de communication persistants bidirectionnels sur les connexions TCP.</span><span class="sxs-lookup"><span data-stu-id="8f29c-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="8f29c-107">Il est utilisé dans les applications qui bénéficient de communications rapides et en temps réel, comme les applications de conversation, de tableau de bord et de jeu.</span><span class="sxs-lookup"><span data-stu-id="8f29c-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="8f29c-108">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8f29c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="8f29c-109">Pour plus d’informations, consultez la section [Étapes suivantes](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="8f29c-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f29c-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="8f29c-110">Prerequisites</span></span>

* <span data-ttu-id="8f29c-111">ASP.NET Core 1.1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="8f29c-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="8f29c-112">Tout système d’exploitation prenant en charge ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="8f29c-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="8f29c-113">Windows 7 / Windows Server 2008 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="8f29c-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="8f29c-114">Linux</span><span class="sxs-lookup"><span data-stu-id="8f29c-114">Linux</span></span>
  * <span data-ttu-id="8f29c-115">macOS</span><span class="sxs-lookup"><span data-stu-id="8f29c-115">macOS</span></span>
  
* <span data-ttu-id="8f29c-116">Si l’application s’exécute sur Windows avec IIS :</span><span class="sxs-lookup"><span data-stu-id="8f29c-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="8f29c-117">Windows 8/Windows Server 2012 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="8f29c-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="8f29c-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="8f29c-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="8f29c-119">WebSocket doit être activé dans IIS (consultez la section [Prise en charge d’IIS/IIS Express](#iisiis-express-support).)</span><span class="sxs-lookup"><span data-stu-id="8f29c-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="8f29c-120">Si l’application s’exécute sur [HTTP.sys](xref:fundamentals/servers/httpsys) :</span><span class="sxs-lookup"><span data-stu-id="8f29c-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="8f29c-121">Windows 8/Windows Server 2012 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="8f29c-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="8f29c-122">Pour les navigateurs pris en charge, consultez https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="8f29c-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="8f29c-123">Quand utiliser WebSocket</span><span class="sxs-lookup"><span data-stu-id="8f29c-123">When to use WebSockets</span></span>

<span data-ttu-id="8f29c-124">Utilisez WebSocket pour travailler directement avec une connexion de socket.</span><span class="sxs-lookup"><span data-stu-id="8f29c-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="8f29c-125">Par exemple, utilisez WebSocket de façon à obtenir les meilleures performances possibles pour un jeu en temps réel.</span><span class="sxs-lookup"><span data-stu-id="8f29c-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="8f29c-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) fournit un modèle d’application plus avancé pour les fonctionnalités temps réel, mais il s’exécute seulement sur ASP.NET 4.x, et pas sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8f29c-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="8f29c-127">Une version ASP.NET Core de SignalR est prévue avec la publication d’ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8f29c-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="8f29c-128">Consultez [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="8f29c-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="8f29c-129">D’ici là, vous pouvez utiliser WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8f29c-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="8f29c-130">Cependant, des fonctionnalités fournies par SignalR doivent être fournies et prises en charge par le développeur.</span><span class="sxs-lookup"><span data-stu-id="8f29c-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="8f29c-131">Exemple :</span><span class="sxs-lookup"><span data-stu-id="8f29c-131">For example:</span></span>

* <span data-ttu-id="8f29c-132">Prise en charge d’un plus grand nombre de versions de navigateur à l’aide d’un retour automatique à d’autres méthodes de transport.</span><span class="sxs-lookup"><span data-stu-id="8f29c-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="8f29c-133">Reconnexion automatique en cas de perte de connexion.</span><span class="sxs-lookup"><span data-stu-id="8f29c-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="8f29c-134">Prise en charge des clients appelant des méthodes sur le serveur, ou vice versa.</span><span class="sxs-lookup"><span data-stu-id="8f29c-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="8f29c-135">Prise en charge de la mise à l’échelle sur plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="8f29c-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="8f29c-136">Utilisation</span><span class="sxs-lookup"><span data-stu-id="8f29c-136">How to use it</span></span>

* <span data-ttu-id="8f29c-137">Installez le package [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="8f29c-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="8f29c-138">Configurez l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="8f29c-138">Configure the middleware.</span></span>
* <span data-ttu-id="8f29c-139">Acceptez les requêtes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8f29c-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="8f29c-140">Envoyez et recevez des messages.</span><span class="sxs-lookup"><span data-stu-id="8f29c-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="8f29c-141">Configurer l’intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="8f29c-141">Configure the middleware</span></span>

<span data-ttu-id="8f29c-142">Ajoutez le middleware WebSocket dans la méthode `Configure` de la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="8f29c-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="8f29c-143">Les paramètres suivants peuvent être configurés :</span><span class="sxs-lookup"><span data-stu-id="8f29c-143">The following settings can be configured:</span></span>

* <span data-ttu-id="8f29c-144">`KeepAliveInterval` : fréquence d’envoi de frames de « ping » au client pour garantir que les proxys maintiennent la connexion ouverte.</span><span class="sxs-lookup"><span data-stu-id="8f29c-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="8f29c-145">`ReceiveBufferSize` : taille de la mémoire tampon utilisée pour recevoir des données.</span><span class="sxs-lookup"><span data-stu-id="8f29c-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="8f29c-146">Seuls les utilisateurs avancés peuvent être amenés à changer ce paramètre, pour l’optimisation des performances en fonction de la taille des données.</span><span class="sxs-lookup"><span data-stu-id="8f29c-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="8f29c-147">Accepter les requêtes WebSocket</span><span class="sxs-lookup"><span data-stu-id="8f29c-147">Accept WebSocket requests</span></span>

<span data-ttu-id="8f29c-148">Ultérieurement dans le cycle de vie de la requête (plus loin dans la méthode `Configure` ou dans une action MVC, par exemple), vérifiez s’il s’agit d’une requête WebSocket et acceptez-la.</span><span class="sxs-lookup"><span data-stu-id="8f29c-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="8f29c-149">L’exemple suivant est extrait d’une partie ultérieure de la méthode `Configure` :</span><span class="sxs-lookup"><span data-stu-id="8f29c-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="8f29c-150">Une requête WebSocket peut figurer sur toute URL, mais cet exemple de code accepte uniquement les requêtes pour `/ws`.</span><span class="sxs-lookup"><span data-stu-id="8f29c-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="8f29c-151">Envoyer et recevoir des messages</span><span class="sxs-lookup"><span data-stu-id="8f29c-151">Send and receive messages</span></span>

<span data-ttu-id="8f29c-152">La méthode `AcceptWebSocketAsync` met à niveau la connexion TCP vers une connexion WebSocket et fournit un objet [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="8f29c-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="8f29c-153">Utilisez l’objet `WebSocket` pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="8f29c-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="8f29c-154">Le code montré précédemment qui accepte la requête WebSocket passe l’objet `WebSocket` à une méthode `Echo`.</span><span class="sxs-lookup"><span data-stu-id="8f29c-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="8f29c-155">Le code reçoit un message et renvoie immédiatement le même message.</span><span class="sxs-lookup"><span data-stu-id="8f29c-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="8f29c-156">Les messages sont envoyés et reçus dans une boucle jusqu’à ce que le client ferme la connexion :</span><span class="sxs-lookup"><span data-stu-id="8f29c-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="8f29c-157">Quand vous acceptez le WebSocket avant de commencer cette boucle, le pipeline de middlewares se termine.</span><span class="sxs-lookup"><span data-stu-id="8f29c-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="8f29c-158">À la fermeture du socket, le pipeline se déroule.</span><span class="sxs-lookup"><span data-stu-id="8f29c-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="8f29c-159">Autrement dit, la requête cesse d’avancer dans le pipeline quand le WebSocket est accepté.</span><span class="sxs-lookup"><span data-stu-id="8f29c-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="8f29c-160">Quand la boucle est terminée et que le socket est fermé, la requête recule dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="8f29c-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="8f29c-161">Prise en charge d’IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="8f29c-161">IIS/IIS Express support</span></span>

<span data-ttu-id="8f29c-162">Windows Server 2012 ou ultérieur et Windows 8 ou ultérieur avec IIS/IIS Express 8 ou ultérieure prennent en charge le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8f29c-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="8f29c-163">Pour activer la prise en charge du protocole WebSocket sur Windows Server 2012 ou ultérieur :</span><span class="sxs-lookup"><span data-stu-id="8f29c-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="8f29c-164">Utilisez l’Assistant **Ajouter des rôles et des fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="8f29c-165">Sélectionnez **Installation basée sur un rôle ou une fonctionnalité**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="8f29c-166">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-166">Select **Next**.</span></span>
1. <span data-ttu-id="8f29c-167">Sélectionnez le serveur approprié (le serveur local est sélectionné par défaut).</span><span class="sxs-lookup"><span data-stu-id="8f29c-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="8f29c-168">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-168">Select **Next**.</span></span>
1. <span data-ttu-id="8f29c-169">Développez **Serveur web (IIS)** dans l’arborescence **Rôles**, développez **Serveur Web**, puis développez **Développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="8f29c-170">Sélectionnez **Protocole WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="8f29c-171">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-171">Select **Next**.</span></span>
1. <span data-ttu-id="8f29c-172">Si vous n’avez pas besoin d’autres fonctionnalités, sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="8f29c-173">Cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-173">Select **Install**.</span></span>
1. <span data-ttu-id="8f29c-174">Une fois l’installation terminée, sélectionnez **Fermer** pour quitter l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="8f29c-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="8f29c-175">Pour activer la prise en charge du protocole WebSocket sur Windows 8 ou ultérieur :</span><span class="sxs-lookup"><span data-stu-id="8f29c-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="8f29c-176">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="8f29c-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="8f29c-177">Ouvrez les nœuds suivants : **Internet Information Services** > **Services World Wide Web** > **Fonctionnalités de développement d’applications**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="8f29c-178">Sélectionnez la fonctionnalité **Protocole WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="8f29c-179">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f29c-179">Select **OK**.</span></span>

<span data-ttu-id="8f29c-180">**Désactiver WebSocket lors de l’utilisation de socket.io sur node.js**</span><span class="sxs-lookup"><span data-stu-id="8f29c-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="8f29c-181">Si vous utilisez la prise en charge de WebSocket dans [socket.io](https://socket.io/) sur [Node.js](https://nodejs.org/), désactivez le module WebSocket IIS par défaut en utilisant l’élément `webSocket` dans *web.config* ou dans *applicationHost.config*. Si cette étape n’est pas effectuée, le module WebSocket IIS tente de gérer la communication WebSocket, au lieu de celle de Node.js et de l’application.</span><span class="sxs-lookup"><span data-stu-id="8f29c-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="8f29c-182">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="8f29c-182">Next steps</span></span>

<span data-ttu-id="8f29c-183">[L’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) qui accompagne cet article est une application d’écho.</span><span class="sxs-lookup"><span data-stu-id="8f29c-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="8f29c-184">Elle comporte une page web qui établit des connexions WebSocket, et le serveur renvoie au client les messages qu’il reçoit.</span><span class="sxs-lookup"><span data-stu-id="8f29c-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="8f29c-185">Exécutez l’application à partir d’une invite de commandes (elle n’est pas configurée pour s’exécuter à partir de Visual Studio avec IIS Express) et accédez à http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="8f29c-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="8f29c-186">La page web affiche l’état de la connexion en haut à gauche :</span><span class="sxs-lookup"><span data-stu-id="8f29c-186">The web page shows the connection status in the upper left:</span></span>

![État initial de la page web](websockets/_static/start.png)

<span data-ttu-id="8f29c-188">Sélectionnez **Connect** (Connexion) pour envoyer une requête WebSocket à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="8f29c-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="8f29c-189">Entrez un message de test, puis sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="8f29c-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="8f29c-190">Quand vous avez terminé, sélectionnez **Close Socket** (Fermer le socket).</span><span class="sxs-lookup"><span data-stu-id="8f29c-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="8f29c-191">La section **Communication Log** (Journal des communications) signale chaque action d’ouverture, d’envoi et de fermeture en temps réel.</span><span class="sxs-lookup"><span data-stu-id="8f29c-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![État initial de la page web](websockets/_static/end.png)
